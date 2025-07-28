# Event Delegation در Vanilla JavaScript

وقتی تعداد زیادی المان فرزند دارید (مثلاً صدها آیتم در یک لیست) و قصد دارید روی همهٔ آن‌ها یک listener یکسان نصب کنید، استفاده از Event Delegation کارایی و خوانایی کد را به‌طرز چشمگیری بهبود می‌بخشد.

## 1. مفهوم پایه  
- به‌جای افزودن listener به هر آیتم، یک listener واحد روی والد (parent) می‌گذاریم.  
- با بررسی `event.target` یا متد `closest()` مشخص می‌کنیم کدام فرزند رویداد را ایجاد کرده.  

## 2. پیاده‌سازی نمونه  

```html
<ul id="todo-list">
  <li data-id="1">خرید مایحتاج</li>
  <li data-id="2">ورزش</li>
  <li data-id="3">مطالعه</li>
</ul>
```

```js
const list = document.getElementById("todo-list");

list.addEventListener("click", (event) => {
  // از closest برای یافتن نزدیک‌ترین <li> به رویداد استفاده می‌کنیم
  const item = event.target.closest("li");
  
  if (!item || !list.contains(item)) return; 
  console.log("کاربر روی آیتم با آیدی", item.dataset.id, "کلیک کرد");
});
```

- `event.target` دقیقاً نشان‌دهندهٔ عنصری است که کلیک روی آن رخ داده.  
- `closest("li")` اجازه می‌دهد اگر کاربر روی یک span یا strong داخل li کلیک کرد هم li والد را بیابیم.  

---

# Event Delegation در React

React خود مدیریت Synthetic Eventها را بر عهده می‌گیرد و تقریباً همهٔ رویدادها را روی ریشهٔ سند (document) دِلیگیت می‌کند. اما وقتی نیاز به کنترل دقیق یا اضافه‌کردن لیسنر مستقیم روی یک DOM node داریم، یا می‌خواهیم منطق delegation را درون یک کامپوننت بنویسیم، همچنان اصول مشابهی را دنبال می‌کنیم.

## 1. دلیگیت پیش‌فرض React  
- React یک سیستم Synthetic Event دارد که همهٔ listenerهای شما (مثل `onClick` در JSX) را پشت صحنه روی document یا root container نصب می‌کند.  
- این باعث می‌شود React در یک نقطه مرکزی رویدادها را دریافت، نرمالایز و بعد به کامپوننت‌های هدف منتقل کند.  

## 2. دلیگیت با JSX  
اگر بخواهید delegation را درون یک کامپوننت پیاده کنید:

```jsx
function TodoList({ items, onItemClick }) {
  return (
    <ul onClick={(e) => {
      const li = e.target.closest("li");
      if (!li) return;
      onItemClick(li.dataset.id);
    }}>
      {items.map(item => (
        <li key={item.id} data-id={item.id}>{item.text}</li>
      ))}
    </ul>
  );
}
```

- متد `e.target.closest("li")` دقیقا مثل Vanilla کار می‌کند.  
- تفاوت اصلی این است که handler شما یک SyntheticEvent دریافت می‌کند که برخی ویژگی‌های بومی مرورگر (مثل event pooling در نسخه‌های قدیمی) را پوشش می‌دهد.  

---

# پاک‌سازی (Cleanup) برای listenerهای مستقیم DOM

گاهی نیاز داریم مستقیماً با `ref` به یک المان دسترسی پیدا کرده و `addEventListener` روی آن بگذاریم، مثلاً رویداد خارج از محدوده‌ی React یا کتابخانهٔ ثالث:

```jsx
import { useEffect, useRef } from "react";

function ClickOutside({ onOutsideClick }) {
  const containerRef = useRef(null);

  useEffect(() => {
    function handleClick(event) {
      if (!containerRef.current.contains(event.target)) {
        onOutsideClick();
      }
    }

    document.addEventListener("click", handleClick);

    return () => {
      // پاک‌سازی listener
      document.removeEventListener("click", handleClick);
    };
  }, [onOutsideClick]);

  return <div ref={containerRef}>…</div>;
}
```

- هر `useEffect` اگر یک تابع بازگشتی برگرداند، هنگام unmount یا قبل از اجرای مجدد اثر (وقتی dependency تغییر کند) آن را فراخوانی می‌کند.  
- این تضمین می‌کند listenerهای اضافه‌شده دوباره نشوند یا پس از حذف کامپوننت در DOM باقی نمانند.  

---

# جلوگیری از نشت حافظه و Re-subscribe مکرر

وقتی callbackها در dependency array تغییر می‌کنند، effect دوباره اجرا می‌شود و ممکن است چندین listener هم‌زمان فعال گردد. برای ثابت نگه‌داشتن ارجاع تابع می‌توانیم از `useCallback` یا یک ref واسط استفاده کنیم.

## 1. با useCallback

```jsx
import { useCallback, useEffect } from "react";

function MyComponent({ onEvent }) {
  const stableHandler = useCallback((e) => {
    // پردازش e
    onEvent(e);
  }, [onEvent]);

  useEffect(() => {
    window.addEventListener("resize", stableHandler);
    return () => {
      window.removeEventListener("resize", stableHandler);
    };
  }, [stableHandler]);

  return <div>…</div>;
}
```

- `useCallback` باعث می‌شود `stableHandler` تنها وقتی `onEvent` تغییر کند ساخته شود.  
- در نتیجه effect وابسته به `stableHandler` هم فقط در آن شرایط رِان می‌شود.

## 2. با Ref واسط

اگر نتوانیم یا نخواهیم از useCallback استفاده کنیم، الگوی زیر کمک می‌کند:

```jsx
import { useEffect, useRef } from "react";

function MyComponent({ onEvent }) {
  const handlerRef = useRef(onEvent);

  // همواره جدیدترین onEvent را در ref می‌ریزیم
  useEffect(() => {
    handlerRef.current = onEvent;
  }, [onEvent]);

  useEffect(() => {
    const listener = (e) => handlerRef.current(e);
    window.addEventListener("resize", listener);
    return () => window.removeEventListener("resize", listener);
  }, []); // فقط یک‌بار اجرا می‌شود

  return <div>…</div>;
}
```

- اولین effect همواره `handlerRef.current` را با آخرین prop به‌روز می‌کند.  
- دومین effect فقط یک بار اجرا و listener را نصب می‌کند؛ اما هنگام رویداد، مستقل از referential change، همیشه آخرین `onEvent` را صدا می‌زند.

---

با درک عمیق این الگوها در Vanilla و React، می‌توانید event delegation را بهینه، قابل نگهداری و عاری از نشت حافظه پیاده کنید.
