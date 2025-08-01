# Stack Trace چیست؟

Stack Trace یا ردیابی پشته، گزارشی است از توالی فراخوانی توابع (call stack) که منجر به بروز یک خطا شده است. این گزارش کمک می‌کند دقیقا بدانید خطا در کدام تابع، در چه فایل و در چه خطی رخ داده است.

---

## معنای Call Stack و ارتباط آن با Stack Trace

Call Stack بخشی از حافظه است که هر بار تابعی فراخوانی می‌شود، اطلاعاتی مثل نام تابع و مکان بازگشت (return address) را ذخیره می‌کند.  
وقتی خطایی پرتاب (throw) می‌شود و هندلچی (catch) در همان سطح وجود نداشته باشد، فریم‌های پشته برای چاپ گزارش می‌شوند. این همان Stack Trace است.

---

## اجزای معمول یک Stack Trace

- نام تابع یا متد (Function Name)  
- مسیر فایل یا ماژول (File Path)  
- شماره خط و گاهی شماره ستون (Line:Column)  
- ترتیب صعودی یا نزولی فراخوانی‌ها  

مثال ساختاری (pseudo):
```
Error: Something went wrong
    at doWork (utils.js:10:15)
    at main (index.js:5:3)
    at Object.<anonymous> (index.js:9:1)
```

---

## مثال عملی در JavaScript

```js
function divide(a, b) {
  if (b === 0) throw new Error("تقسیم بر صفر مجاز نیست");
  return a / b;
}

function calculate() {
  return divide(10, 0);
}

calculate();
```

در کنسول مرورگر یا نود:
```
Error: تقسیم بر صفر مجاز نیست
    at divide (app.js:2:15)
    at calculate (app.js:6:10)
    at Object.<anonymous> (app.js:9:1)
```

---

## کاربرد Stack Trace در دیباگ

- سریعاً نقطه‌ی دقیق خطا را پیدا می‌کنید.  
- مسیر فراخوانی توابع را مرور می‌کنید تا ببینید کدام تابع، داده‌ی نادرست ارسال کرده.  
- در پروژه‌های بزرگ با چندین لایه، مشخص می‌کند کدام ماژول باعث بروز خطا شده.  

---

## نکات حرفه‌ای

- در کدهای تبدیل‌شده (transpiled) یا minified از Source Map استفاده کنید تا مسیر اصلی فایل‌ها و خطوط را ببینید.  
- برای تولید Stack Trace دلخواه درون کد، می‌توانید `new Error().stack` را بخوانید.  
- در زبان‌های دیگر مثل Java، پایتون یا C# هم گزارش مشابهی دریافت می‌کنید و ساختارش بسته به محیط اجرا کمی متفاوت است.  

---

با درک عمیق Stack Trace، می‌توانید خطاها را سریع‌تر ردیابی و رفع کنید و کیفیت کدتان را بالا ببرید.
