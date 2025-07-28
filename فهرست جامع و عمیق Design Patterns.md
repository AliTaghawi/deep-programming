# فهرست جامع و عمیق Design Patterns

در این مرجع، مهم‌ترین الگوهای طراحی (Design Patterns) را در چهار دستهٔ اصلی دسته‌بندی کرده‌ایم. برای هر الگو، هدف (Intent) و موارد کاربرد (Use Case) آورده شده است.

---

## ۱. Creational Patterns  
الگوهایی که نحوهٔ ایجاد اشیاء را انتزاع می‌کنند.

- Singleton  
  - Intent: تضمین یک نمونهٔ یکتا در برنامه  
  - Use Case: logger سراسری، config manager، connection pool  
 
- Factory Method  
  - Intent: تعریف یک متد واسط برای ساخت اشیاء، بدون وابستگی به کلاس‌های واقعی  
  - Use Case: تولید اشیاء وابسته به پارامتر ورودی، plugin architecture  

- Abstract Factory  
  - Intent: ارائهٔ رابطی برای ساخت خانواده‌ای از اشیاء مرتبط  
  - Use Case: UI themes (light/dark)، تولید محصولات هم‌خوان  

- Builder  
  - Intent: تفکیک ساخت‌وساز یک شیء پیچیده از نمایش آن  
  - Use Case: ساخت سند HTML/JSON پیچیده، تنظیم پارامترهای زیاد مرحله‌به‌مرحله  

- Prototype  
  - Intent: ایجاد اشیاء جدید با clone کردن نمونهٔ موجود  
  - Use Case: کپی کردن تنظیمات پیش‌فرض، cloning شیء با state اولیه  

---

## ۲. Structural Patterns  
الگوهایی که ارتباط و ترکیب اشیاء را مدیریت می‌کنند.

- Adapter  
  - Intent: تطبیق رابط یک کلاس با رابط مورد انتظار  
  - Use Case: یکپارچه‌سازی APIهای متفاوت، legacy code integration  

- Facade  
  - Intent: ارائهٔ یک رابط ساده به یک کتابخانه یا زیرسیستم پیچیده  
  - Use Case: wrapper برای چندین ماژول internal، مخفی‌سازی جزئیات پیاده‌سازی  

- Proxy  
  - Intent: جایگزینی یا واسطه‌سازی دسترسی به یک شیء دیگر  
  - Use Case: lazy loading، کنترل دسترسی، logging فراخوانی متدها  

- Decorator  
  - Intent: افزودن رفتار دینامیک به یک شیء بدون تغییر کلاس آن  
  - Use Case: تزریق ویژگی‌های اضافی مثل logging یا caching  

- Composite  
  - Intent: ترکیب اشیاء در ساختاری درختی و کار با آن‌ها به‌صورت یکنواخت  
  - Use Case: مدیریت منوها، ساختارهای nested UI  

- Flyweight  
  - Intent: اشتراک‌گذاری بیشینه دادهٔ مشترک بین اشیاء برای کاهش حافظه  
  - Use Case: rendering تعداد زیاد اشیاء تکراری (مثلاً nodes در گراف)  

---

## ۳. Behavioral Patterns  
الگوهایی که نحوهٔ تعامل و تبادل پیام بین اشیاء را مشخص می‌کنند.

- Observer  
  - Intent: تعریف رابطهٔ یک‌به‌چند؛ وقتی یک شیء تغییر می‌کند، متصلین آگاه می‌شوند  
  - Use Case: event system، state management (Redux subscribe)  

- Strategy  
  - Intent: کپسوله‌سازی خانواده‌ای از الگوریتم‌ها و انتخاب آن‌ها در زمان اجرا  
  - Use Case: انتخاب روش sort، فشرده‌سازی یا rendering در runtime  

- Command  
  - Intent: قرار دادن درخواست‌ها در قالب اشیاء برای queue/undo/redo  
  - Use Case: undo/redo در ویرایشگرها، اجرای batch از دستورات  

- Iterator  
  - Intent: پیمایش عناصر مجموعه بدون افشای ساختار داخلی  
  - Use Case: custom iterable برای داده‌های پیچیده  

- Template Method  
  - Intent: تعریف اسکلت یک الگوریتم در کلاس پایه و اجازهٔ override مراحل  
  - Use Case: توابع پردازش داده با مراحل مشترک و قابل سفارشی‌سازی  

- Chain of Responsibility  
  - Intent: عبور درخواست‌ها از زنجیره‌ای از handlerها تا رسیدن به پاسخ  
  - Use Case: middleware در Express/Koa، event bubbling  

- Mediator  
  - Intent: کپسوله‌سازی تعامل بین چند شیء و کاهش Coupling  
  - Use Case: کنترلر فرم‌های پیچیده، مدیریت state کامپوننت‌های مستقل  

- Memento  
  - Intent: ذخیره و بازیابی وضعیت یک شیء بدون زیرپا گذاشتن encapsulation  
  - Use Case: snapshot state و undo در ویرایشگرها  

- Visitor  
  - Intent: جداکردن الگوریتم از ساختار داده؛ افزودن عملیات جدید بدون تغییر کلاس‌ها  
  - Use Case: traversing AST، export/import چندفرمت  

- State  
  - Intent: تغییر رفتار شیء با تغییر وضعیت داخلی  
  - Use Case: finite state machines در UI (loading, success, error)  

---

## ۴. Concurrency & Architectural Patterns  
الگوهایی برای مدیریت هم‌زمانی و معماری کلی.

- Pub/Sub (Publish–Subscribe)  
  - Intent: انتشار پیام به چند subscriber به‌صورت ناهم‌زمان  
  - Use Case: Event Bus در SPA، real-time notifications  

- MVC / MVP / MVVM  
  - Intent: جداسازی concerns بین منطق، نمایش و داده  
  - Use Case: معماری کلی اپلیکیشن‌های تک‌صفحه‌ای یا دسکتاپ  

- Reactor / Proactor  
  - Intent: الگوهای مدیریت I/O ناهم‌زمان  
  - Use Case: event loop در Node.js (Reactor)، completion port در ویندوز (Proactor)  

---

با تسلط بر این الگوها و درک موارد کاربرد هریک، می‌توانی ساختار کد را بهبود داده، قابلیت نگهداری را افزایش دهی و از پیچیدگی‌های ناخواسته جلوگیری کنی.


