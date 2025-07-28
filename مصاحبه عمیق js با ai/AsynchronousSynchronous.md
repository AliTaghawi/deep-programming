Asynchronous/Synchronous

سینکرون (Synchronous) و آسنکرون (Asynchronous) دو مدل اصلی اجرای کد هستند که تفاوت اصلی‌شان در بلوکه‌کردن یا عدم بلوکه‌کردن جریانِ اجرای برنامه است.

---

## 1. اجرای سینکرون (Synchronous)

  
در مدل سینکرون، دستورات یک‌به‌یک و پشت‌سرهم اجرا می‌شوند و هیچ دستور بعدی تا تمام شدن دستور جاری آغاز نمی‌شود. اگر یک عملیات زمان‌بر باشد (مثلاً یک حلقه طولانی یا خواندن از دیسک)، کل برنامه تا پایان آن صبر می‌کند.

مثال ساده:
```javascript
console.log('شروع');
for (let i = 0; i < 1e9; i++);  // عملیات سنگین و بلوکه‌کننده
console.log('اتمام');
```

- جریان اجرا:  
  1. «شروع»  
  2. اجرای کامل حلقه  
  3. «اتمام»  
- اگر حلقه بسیار سنگین باشد، مرورگر یا Node.js تا پایان آن هیچ کار دیگری انجام نمی‌دهد.

---

## 2. اجرای آسنکرون (Asynchronous)

  
در مدل آسنکرون، می‌توان عملیات زمان‌بر (I/O، تایمرها، درخواست‌های شبکه و…) را ثبت کرد و ادامه اجرای کد هم‌زمان را بدون صبر کردن برای اتمامِ آن‌ها ادامه داد. وقتی عملیات آسنکرون به پایان برسد، callback یا Promise آن در صف مخصوص قرار می‌گیرد و در فرصتی مناسب اجرا می‌شود.

مثال ساده:
```javascript
console.log('شروع');

setTimeout(() => {
  console.log('کار آسنکرون');
}, 1000);

console.log('ادامه اجرا');
```

- جریان اجرا:  
  1. «شروع»  
  2. ثبت `setTimeout` (بدون صبر)  
  3. «ادامه اجرا»  
  4. پس از ~1 ثانیه: «کار آسنکرون»

---

## 3. چطور جاوااسکریپت آسنکرون را مدیریت می‌کند؟

  
۱. Call Stack  
   - پشته‌ای که توابع در آن قرار می‌گیرند و LIFO اجرا می‌شوند.  

۲. Event Loop  
   - به‌صورت مداوم چک می‌کند Call Stack خالی باشد و بعد از آن تسک‌ک‍یوها را بررسی کند.  

۳. Task Queue (Macro-task)  
   - صف callbackهای `setTimeout`، I/O و…  

۴. Microtask Queue  
   - صف callbackهای Promise (`.then`/`.catch`) و `queueMicrotask`  

چرخه ساده:
1. اجرای کد سینکرون تا خالی شدن Call Stack  
2. اجرای همه‌ی میکروتسک‌ها  
3. اجرای یک تسک ماکرو  
4. تکرار تا پایان برنامه  

---

## 4. روش‌های نوشتن آسنکرون

- Callback  
  ```javascript
  fs.readFile('file.txt', (err, data) => {
    if (err) throw err;
    console.log(data);
  });
  ```

- Promise  
  ```javascript
  fetch('/api/data')
    .then(res => res.json())
    .then(data => console.log(data))
    .catch(err => console.error(err));
  ```

- async/await  
  ```javascript
  async function loadData() {
    try {
      const res = await fetch('/api/data');
      const data = await res.json();
      console.log(data);
    } catch (err) {
      console.error(err);
    }
  }
  loadData();
  ```

---

## 5. مقایسه سینکرون و آسنکرون

| ویژگی                    | سینکرون                         | آسنکرون                           |
|---------------------------|---------------------------------|-----------------------------------|
| بلوکه‌کردن (Blocking)    | بله                             | خیر                                |
| جریان اجرای کد           | پشت‌سرهم و ترتیبی               | غیرترتیبی؛ ادامه بدون انتظار    |
| پیچیدگی مدیریت            | ساده‌تر                         | نیازمند callback/Promise/await   |
| استفاده در I/O            | مناسب نیست                     | بسیار مناسب                       |
| مثال                      | محاسبات سنگین در حلقه           | انواع I/O، `setTimeout`, `fetch` |

---

## 6. مزایا و محدودیت‌ها

- مزایای آسنکرون  
  - نگه داشتن UI پاسخگو  
  - افزایش کارایی در عملیات I/O  
  - امکان موازی‌سازی غیرمسدودکننده  

- محدودیت‌های آسنکرون  
  - پیچیدگی بیشتر در مدیریت خطا و جریان کنترل  
  - خطر Callback Hell (با callbackهای تو در تو)  
  - نیاز به درک event loop و صف‌ها  

---

اگر بخش یا جزئیات خاصی مبهم است یا می‌خواهی روی مثال‌های پیشرفته‌تر (مثل Web Workers، Streams، یا concurrency در Node.js) بیشتر کار کنیم، بگو تا با مثال و عمق بیشتری پیش بریم!


----------------------

# مکانیسم `async/await` در جاوااسکریپت

وقتی یک تابع را با `async` علامت می‌زنیم و داخل آن از `await` استفاده می‌کنیم، عملاً داریم به جاوااسکریپت می‌گوییم:

1. این تابع همیشه یک Promise برمی‌گرداند.  
2. هر بار که به `await` می‌رسیم، اجرای بقیهٔ آن تابع را موقتاً معلق می‌کنیم تا Promiseِ سمت راستِ `await` حل شود (resolve یا reject شود).  
3. در همین حین، Event Loop آزاد است که سایر کارها (رندر، دیگر callbacks، میکروتسک‌ها) را انجام دهد.  
4. وقتی Promise تمام شد، ادامهٔ تابع به‌صورت یک میکروتسک (microtask) دوباره در Call Stack قرار می‌گیرد و اجرا می‌شود.

---

## چرا برنامه کلش بلوکه نمی‌شود؟

| پرسش                 | پاسخ                                                               |
|----------------------|---------------------------------------------------------------------|
| آیا کل برنامه متوقف می‌شود؟ | خیر. فقط اجرایی که بعد از `await` است به‌صورت موقت عقب می‌افتد.      |
| بقیهٔ کدها چه می‌شوند؟     | Event Loop و Call Stack آزادند کارهای دیگر را انجام دهند.          |
| آیا انتظار (`await`) بلوک‌کننده است؟ | بله برای همان تابع، اما نه برای تمام برنامه یا دیگر توابع. |

---

## مقایسه ساده: با و بدون `async/await`

```javascript
// حالت عادی با Promise
function fetchData() {
  return fetch('/api').then(res => res.json());
}

console.log('start');
fetchData().then(data => {
  console.log('got data', data);
});
console.log('end');
```
جریان:
1. «start»  
2. fetch ثبت می‌شود و Promise برمی‌گردد  
3. «end»  
4. وقتی پاسخ آمد، callback در میکروتسک اجرا و «got data» چاپ می‌شود  

```javascript
// با async/await
async function load() {
  console.log('start');
  const data = await fetch('/api').then(r => r.json());
  console.log('got data', data);
  console.log('end');
}
load();
```
جریان:
1. `load()` فراخوانی و Promise برمی‌گرداند  
2. «start»  
3. به `await` می‌رسد → موقتاً برمی‌گردد به Event Loop  
4. وقتی fetch تمام شد → ادامهٔ `load` به میکروتسک می‌رود  
5. «got data»  
6. «end»  

در این الگو هیچ‌وقت خط بعد از `await` قبل از آماده‌شدن داده‌ها اجرا نمی‌شود، اما بقیهٔ برنامه (مثل رندر، پردازش دیگر callbacks و میکروتسک‌های موجود) مختل نمی‌شود.

---

## گام‌های داخلی اجرای `await`

1. اجرای سینکرون تا نقطه‌ی `await` (تمام کدهای قبل اجرا می‌شوند).  
2. ایجاد یک میکروتسک که ادامهٔ تابع را پس از نتیجهٔ Promise اجرا کند.  
3. برگرداندن کنترل به Event Loop (برای اجرای دیگر کارها).  
4. وقتی Promise حل شد، میکروتسک ادامهٔ تابع را وارد Call Stack می‌کند.  

```
Call Stack                   Microtask Queue              Macro Task Queue
--------------               ------------------            -----------------
| load()       |             (empty)                     (empty)
| …            |             ↳ resumeLoad()              ↳ setTimeout callbacks
--------------             
         ↓ at await
--------------               ↳ resumeLoad()               …
| (empty)      |             (بعد از resolve شدن Promise)
--------------
```

---

## پاسخ به ابهام‌های شما

- جاوااسکریپت **خودکاراً** کارهای زمان‌بر (I/O، fetch، تایمر) را غیرمسدود (non-blocking) می‌سازد ولی شما با `await` درون تابع اجازه نمی‌دهید ادامهٔ آن تابع قبل از پایان آن کار زمان‌بر اجرا شود.  
- وقتی `await` می‌نویسید، **فقط** اجرای تابع `async` بعد از آن معلق می‌شود، نه کل اپلیکیشن یا دیگر توابع.

---

اگر این توضیح کامل نبود یا دوست داری یک دیاگرام دقیق‌تر یا مثال تعاملی ببینی، خبر بده تا بیشتر عمیق شویم!




----------------/////////////////////// 

# مکانیسم `setTimeout` در جریان اجرای جاوااسکریپت

وقتی `setTimeout` را صدا می‌زنید، تابع callback شما بلافاصله اجرا نمی‌شود، بلکه در یکی از «فازهای» Event Loop برنامه زمان‌بندی و به صف Macro-task منتقل می‌شود.  

---

## مراحل داخلی اجرای `setTimeout`

1. صدا زدن Web API  
   وقتی در کد می‌نویسید:
   ```javascript
   setTimeout(() => {
     console.log('وقت تمام شد');
   }, 1000);
   ```
   مرورگر یا محیط Node.js این درخواست را به Web API (یا در Node.js به Thread Pool) می‌سپارد.  

2. شمارشگر (Timer)  
   Web API شروع می‌کند به شمارش ۱۰۰۰ میلی‌ثانیه.  

3. پس از پایان تأخیر  
   Web API یک Macro-task برای اجرای callback شما ثبت می‌کند و آن را به **Timer Queue** (یکی از صف‌های Macro-task) اضافه می‌کند.  

4. Event Loop  
   Event Loop مرتب بررسی می‌کند:
   - آیا Call Stack خالی است؟  
   - اگر خالی بود، **اول** همه Microtasks را اجرا می‌کند.  
   - سپس از Timer Queue (جزو Macro-task) یک callback برمی‌دارد و آن را روی Call Stack اجرا می‌کند.  

---

## فلو در یک مثال ساده

```javascript
console.log('start');          // 1
setTimeout(() => console.log('timer callback'), 0); // 2
Promise.resolve().then(() => console.log('microtask')); // 3
console.log('end');            // 4
```

- خط 1: “start” چاپ می‌شود.  
- خط 2: `setTimeout(..., 0)` یک Macro-task برنامه‌ریزی می‌کند.  
- خط 3: Promise یک Microtask ثبت می‌کند.  
- خط 4: “end” چاپ می‌شود.  
- پس از خالی شدن Call Stack، **ابتدا** Microtask اجرا و “microtask” چاپ می‌شود.  
- سپس Macro-task Timer اجرا و “timer callback” چاپ می‌شود.  

---

## نکات کلیدی

- **حداقل تأخیر (Clamping)**  
  مرورگرها معمولاً برای تاخیر زیر ۴ میلی‌ثانیه، آن را به ۴ میلی‌ثانیه بالا می‌برند.  

- **پاک‌سازی تایمر**  
  با `clearTimeout(id)` می‌توانید callback زمان‌بندی‌شده را حذف کنید.  

- **تأثیر Call Stack پر**  
  اگر Call Stack مشغول باشد یا میکروتسک‌ها طولانی اجرا شوند، اجرای callback شما به بعد موکول می‌شود—یعنی ممکن است کمی دیرتر از زمان دقیق اجرا گردد.  

- **فازهای Event Loop (در مرورگر)**  
  1. **Timers**: اجرای callbackهای `setTimeout` و `setInterval`  
  2. **Pending Callbacks**: callbackهای I/O سبک  
  3. **Poll**: دریافت رویدادهای I/O و اجرای callbackهای شبکه  
  4. **Check**: اجرای callbackهای `setImmediate` (در Node.js)  
  5. **Close Callbacks**: بسته شدن سوکت‌ها و cleanup  

---

## تفاوت در Node.js

- Node.js علاوه بر صف Timers، یک Thread Pool برای I/O سنگین دارد.  
- فازهای Event Loop در Node.js کمی متفاوت‌اند و فاز **check** (برای `setImmediate`) قبل از فاز **close callbacks** اجرا می‌شود.  

---

با این روند، `setTimeout` بدون بلوکه‌کردن Call Stack اجازه می‌دهد کدهای غیرزمان‌بر بلافاصله اجرا شوند و زمانی که نوبت رسید، callback شما را به Queue ارسال و سپس اجرا کند.  

اگر مثال عملی بیشتر یا توضیح درباره چگونگی تعامل با Promise و async/await در کنار `setTimeout` می‌خواهی، بگو تا بیشتر عمیق شویم!



-----------------////////////////// 

# I/O (Input/Output) در جاوااسکریپت چیست؟

I/O مخفف Input/Output است و به هر تعاملی بین برنامهٔ شما و “دنیای بیرون” اشاره می‌کند. مثل خواندن یا نوشتن فایل، ارسال درخواست شبکه، دریافت ورودی کاربر و …

---

## ۱. انواع اصلی I/O

- فایل‌ها  
  - خواندن (`fs.readFile` در Node.js)  
  - نوشتن (`fs.writeFile`)  
- شبکه  
  - درخواست HTTP/HTTPS (مثلاً `fetch` در مرورگر یا `http.request` در Node)  
  - WebSocket  
- پایگاه داده  
  - پرس‌وجو به MongoDB، MySQL و…  
- ورودی کاربر  
  - رویدادهای صفحه‌کلید و ماوس در مرورگر  
  - خواندن از stdin در کنسول  

---

## ۲. I/O همگام vs ناهمگام

- همگام (Synchronous I/O)  
  - تابع تا پایان کار I/O بلوکه می‌شود  
  - مثال در Node.js  
    ```javascript
    const data = fs.readFileSync('file.txt', 'utf8');
    console.log(data);
    // تا زمان پایان خواندن فایل، هیچ کار دیگری انجام نمی‌شود
    ```
- ناهمگام (Asynchronous I/O)  
  - تابع درخواست I/O را ثبت می‌کند و بلافاصله برمی‌گردد  
  - نتیجهٔ I/O در آینده از طریق Callback، Promise یا `async/await` در دسترس قرار می‌گیرد  
  - مثال با Callback  
    ```javascript
    fs.readFile('file.txt', 'utf8', (err, data) => {
      if (err) throw err;
      console.log(data);
    });
    console.log('این خط قبل از تمام شدن خواندن فایل اجرا می‌شود');
    ```
  - مثال با Promise  
    ```javascript
    import { readFile } from 'fs/promises';
    readFile('file.txt', 'utf8').then(data => console.log(data));
    ```

---

## ۳. پشت صحنهٔ I/O ناهمگام

۱. ثبت درخواست در Web API (مرورگر) یا در **Thread Pool** (Node.js via libuv)  
۲. ادامه اجرای Call Stack بدون انتظار  
3. بعد از پایان I/O:  
   - در مرورگر: callback یا Promise handler به Task Queue اضافه می‌شود  
   - در Node.js: نتیجه روی Event Loop برگشته و سپس اجرا می‌شود  
4. Event Loop بعد از خالی شدن Call Stack ابتدا Microtasks و سپس Macro-tasks (callbackهای I/O جزو Macro-tasks) را اجرا می‌کند

---

## ۴. چرا I/O ناهمگام مهم است؟

- نگه داشتن UI پاسخگو در مرورگر  
- افزایش کارایی سرورهای Node.js با رسیدگی همزمان به صدها اتصال بدون مسدود کردن  
- جلوگیری از بلوکه‌شدن کل برنامه در عملیات سنگین I/O  

---

## ۵. مثال ترکیبی: fetch + فایل

```javascript
// ۱. خواندن فایل پیکربندی
const config = await fetch('/api/config').then(r => r.json());

// ۲. ارسال داده به سرور
fs.writeFile('out.json', JSON.stringify(config), err => {
  if (err) throw err;
  console.log('ذخیره شد');
});

// سایر کدها تا زمان اتمام I/O ادامه پیدا می‌کنند
console.log('در حال انجام کارهای دیگر…');
```

در این مثال هم request شبکه‌ای و هم نوشتن فایل ناهمگام است و هیچ‌کدام جریان اصلی برنامه را بلوکه نمی‌کنند.

---

# ادامه و عمق بیشتر  
اگر دوست داری به مباحث زیر هم بپردازیم:  
- معماری Thread Pool و libuv در Node.js  
- مدیریت backpressure و Streams برای I/O حجیم  
- تفاوت I/O غیرمسدود (non-blocking) و غیرهم‌زمان (asynchronous)  
- نحوه کار WebSocket و SSE برای ارتباطات دائم  

خبر بده که کدام بخش برات جالب‌تره!
