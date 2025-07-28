# مثال Closure در React

در React خیلی وقت‌ها نیاز داریم متغیرهای state یا props را درون توابع callback (مثلاً `onClick`، `setInterval` یا hookهای سفارشی) نگه داریم. اینجاست که Closure وارد می‌شود.

```jsx
import { useState, useEffect, useCallback } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  // این تابع یک closure است که count و setCount را از رندر جاری می‌گیرد
  const handleClick = () => {
    alert(`شمارش: ${count}`);
  };

  // مثال استال کلیژر: در useEffect آرایه‌ی deps خالی است،
  // callback فقط در اولین رندر بسته می‌شود و count را 0 می‌گیرد
  useEffect(() => {
    const id = setInterval(() => {
      // اینجا به countِ آن لحظهٔ اول رندر دسترسی دارد و افزایش نمی‌یابد
      console.log('tick:', count);
    }, 1000);
    return () => clearInterval(id);
  }, []);

  // راهکار: استفاده از functional update برای بی‌اثر کردن استال کلیژر
  useEffect(() => {
    const id = setInterval(() => {
      setCount(c => c + 1); // c جدید را همیشه از آخرین state می‌گیرد
    }, 1000);
    return () => clearInterval(id);
  }, []);

  return (
    <div>
      <button onClick={handleClick}>نمایش شمارش</button>
      <p>شمارش فعلی: {count}</p>
    </div>
  );
}
```

- `handleClick` در هر رندر یک تابع جدید می‌سازد که **snapshot** آن لحظه از `count` را در خود دارد.  
- اگر آرایهٔ dependency در `useEffect` خالی باشد، callback فقط یک‌بار بسته (closed) می‌شود و به state بعدی دسترسی پیدا نمی‌کند.  
- با Functional Updates (`setCount(c => c+1)`) مشکل stale closure رفع می‌شود چون مستقیماً آخرین مقدار state را می‌گیریم، نه مقداری که در closure بسته شده.

---

# آیا state های Hook خودِ React جزو Closure هستند؟

- **متغیرهای state** (مثلاً `count`) درون تابع کامپوننت فقط یک **مقدار** (snapshot) هستند که در آن لحظهٔ رندر در دسترس است.  
- React این مقادیر را خارج از تابع (در internal Hook list) نگه می‌دارد و در هر رندر جدید، مقدارِ به‌روز را به کامپوننت تزریق می‌کند.  
- پس خودِ state در یک closure قرار ندارد؛ بلکه React آن را مدیریت می‌کند و به‌عنوان پارامتر به تابع کامپوننت می‌دهد.

اما:

- هر تابع یا callback که درون کامپوننت تعریف می‌شود، **closure** است و می‌تواند آن snapshot از state را «درون خود» نگه دارد.  
- حتی تابعی که از `useCallback` برمی‌گردد، closure به `count` یا هر متغیر دیگری در آرایهٔ deps دارد.

در نتیجه:

1. state خودِ React در closure جا نمی‌گیرد—بلکه React برای مدیریت آن به‌صورت جدا ذخیره‌اش می‌کند.  
2. اما تمام توابعی که در کامپوننت تعریف می‌کنی، closure هستند و snapshot آن رندر را می‌گیرند.

---

اگر دوست داری نمونه‌های بیشتری از استال کلیژر (`stale closure`) یا استراتژی‌های پیشرفته مثل `useRef` برای حفظ مقادیر بین رندرها ببینیم، بگو تا بیشتر عمیق بشیم!



---//////////////////---

----------------//////////////خوب با این حساب وقتی که eventHandler یا هر تابعی که تعریف میکنیم کلاژر هست باید memory leak صورت بگیره، وضعیت این توابع بعد از حذف یه کامپوننت و unmount شدن کامپوننت چطوریه ما خودمون باید کلین کنیم یا خوده ریکت هدنل میکنه؟//////////////---------------------

# مدیریت حافظه و پاک‌سازی Closureها در React

هر تابع (مثل event handler یا callback) که درون یک کامپوننت تعریف می‌شود، یک **closure** است که snapshot از متغیرهای رندر فعلی را حفظ می‌کند. اما به‌خاطر مکانیزم React و نحوهٔ رندر/آنمَونت، خودِ این closureها لزوماً منجر به نشت حافظه نمی‌شوند—مگر اینکه رفرنس‌هایی به منابع خارجی (مثل تایمر، subscription یا event listener روی window/document) نگه دارید و آن‌ها را در cleanup آزاد نکنید.

---

## ۱. پاک‌سازی خودکار React

- React به‌تنهایی closureهای درون JSX یا توابع عادی را پاک نمی‌کند؛ اما وقتی کامپوننت آن‌mount‌ می‌شود:
  - آرایش داخلی state و props آن پاک می‌شود.
  - تمام closureهایی که فقط به state/props اشاره دارند و جای دیگری رفرنس نشده‌اند، پس از از بین رفتن component unreferenced می‌شوند و GC آن‌ها را جمع‌آوری می‌کند.

- **اما** اگر:
  - درون closure به اشتراک با منابع خارجی رفرنس داده‌اید (timer, subscription, event listener)،  
  - یا خود closure را به جایی بیرون از کامپوننت (مثلاً به window، global variable یا بیرون یک hook) پاس داده‌اید,  
  آنگاه تا زمانی که آن resource آزاد نشده، closure در حافظه باقی می‌ماند.

---

## ۲. پاک‌سازی دستی با useEffect

برای جلوگیری از نشت حافظه، همیشه منابع خارجی را در بلوک cleanup داخل `useEffect` آزاد کنید:

```jsx
import { useEffect } from 'react';

function MyComponent() {
  useEffect(() => {
    function onScroll() {
      console.log(window.scrollY);
    }
    window.addEventListener('scroll', onScroll);

    // Cleanup: حذف event listener هنگام unmount
    return () => {
      window.removeEventListener('scroll', onScroll);
    };
  }, []); // دیپندنسی خالی یعنی فقط یک‌بار هنگام mount/unmount
}
```

نکات کلیدی:

- هر منبعی که داخل `useEffect` یا خارج از آن می‌سازید (timer, subscription),  
- حتماً در تابع بازگشتی `return` آن را لغو یا پاک کنید.

---

## ۳. پاک‌سازی تایمرها و subscriptionها

```jsx
import { useEffect } from 'react';

function TimerComponent() {
  useEffect(() => {
    const id = setInterval(() => {
      console.log('tick');
    }, 1000);

    return () => {
      clearInterval(id);
    };
  }, []);
}
```

اگر تایمر پاک نشود، پس از unmount تا پایان عمر صفحه در حال کار باقی می‌ماند.

---

## ۴. Event Handlerهای DOM و Ref

وقتی listener را به یک DOM element در JSX اختصاص می‌دهید (مثل `<button onClick={handler}>`)، React خودش آن را موقع unmount پاک می‌کند.  

اما در مواقعی که مستقیماً از `addEventListener` روی window/document استفاده می‌کنید یا از Ref برای دسترسی به المان DOM می‌گیرید:

```jsx
import { useEffect, useRef } from 'react';

function ClickOutside({ onOutsideClick }) {
  const ref = useRef();
  useEffect(() => {
    function handleClick(e) {
      if (!ref.current.contains(e.target)) {
        onOutsideClick();
      }
    }
    document.addEventListener('click', handleClick);
    return () => {
      document.removeEventListener('click', handleClick);
    };
  }, [onOutsideClick]);

  return <div ref={ref}>بخش داخلی</div>;
}
```

در این مثال مسئول حذف listener خود شما هستید.

---

## ۵. خلاصهٔ راهکارها برای جلوگیری از حافظهٔ نشت

- منابع خارجی (timers, subscriptions, event listeners) را همیشه در `useEffect` بسازید و در cleanup حذف کنید.  
- از آرایهٔ dependency مناسب استفاده کنید تا cleanup در زمان مناسب انجام شود.  
- از Hookهای آماده (مثل `useInterval`, `useEventListener`) استفاده کنید که خودشان cleanup را مدیریت می‌کنند.  
- اگر callbackها را به جای state و props فقط برای یک رندر ایجاد می‌کنید، نگران نشت نباشید—GC آن‌ها را پس از unmount پاک می‌کند.

---

با این رویکردها، حتی اگر ده‌ها closure در کامپوننت ایجاد شود، نیازی به پاک‌سازی دستی همه نیست و فقط منابع خارجی و listenerهایی که React مدیریت نمی‌کند را پاک کنید.



