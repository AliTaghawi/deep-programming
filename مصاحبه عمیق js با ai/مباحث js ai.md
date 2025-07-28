# خلاصه‌ی مباحث مصاحبه (سوالات ۱ تا ۲۰)

برای مرور سریع، هر سوال به همراه کلیدواژه‌ها و توضیح کوتاهی آورده شده است.  

---

## سوال 1: انواع داده‌ها و تخصیص مقدار (Value vs Reference)

- داده‌های اولیه (Primitive): number, string, boolean, null, undefined, symbol, BigInt  
- اشیاء (Object) و آرایه‌ها: ارجاع (reference) به حافظهٔ Heap  
- تفاوت در نحوهٔ کپی: اختصاص مقدار برای Primitive و کپی آدرس برای Object  
- مثال:  
  ```js
  let a = 5;      // copy by value  
  let b = a;      
  b = 7;          // a تغییر نمی‌کند  
   
  let x = {v:10}; 
  let y = x;      
  y.v = 20;       // x.v هم 20 می‌شود  
  ```

---

## سوال 2: عملگرهای مقایسه (== vs ===) و Coercion

- `==` با تبدیل نوع (type coercion) مقادیر را مقایسه می‌کند  
- `===` بدون تبدیل نوع، فقط اگر نوع و مقدار یکسان باشند `true` می‌دهد  
- قوانین پرکاربرد coercion:  
  - `"" == false` → true  
  - `null == undefined` → true  
  - عدد و رشته: `"5" == 5` → true  
- توصیه: تا حد امکان از `===` استفاده کنید.

---

## سوال 3: توابع Declaration vs Expression و Arrow Functions

- Function Declaration:  
  ```js
  function foo() {…}
  ```
  – hoisted کامل (قابل فراخوانی قبل از تعریف)  
- Function Expression:  
  ```js
  const bar = function() {…};
  ```
  – در TDZ برای `let`/`const` قرار می‌گیرد  
- Arrow Function:  
  - نگارش کوتاه‌تر  
  - `this` لکسیکال از کانتکست والد می‌گیرد  
  - فاقد `arguments` و `new`  

---

## سوال 4: توابع Higher-Order و Callback‌ها

- تابع higher-order: تابعی که تابع دیگر را می‌پذیرد یا برمی‌گرداند  
- Callback: تابعی که به عنوان آرگومان به تابع دیگر داده می‌شود  
- مسائل رایج: callback hell و pyramid of doom  
- راهکار: Promises، async/await، و کتابخانه‌هایی مثل async.js  

---

## سوال 5: کلیدواژه this و قواعد تعیین آن

- چهار حالت بایندینگ:  
  1. Global / Default (strict mode: undefined)  
  2. Implicit (شیء قبل از نقطه)  
  3. Explicit (`call`/`apply`/`bind`)  
  4. New Binding (`new` operator)  
- Arrow Function: this لکسیکال  
- مثال:
  ```js
  const obj = {x:1, foo(){ console.log(this.x); }};
  obj.foo();      // this → obj  
  const f = obj.foo;
  f();            // this → undefined (strict)  
  ```

---

## سوال 6: Scope و Execution Context

- انواع اسکوپ:  
  - Global Scope  
  - Function Scope  
  - Block Scope (ES6)  
- هر فراخوانی تابع یک Execution Context می‌سازد  
- Lexical Environment: نگهدارندهٔ متغیرهای تعریف‌شده  
- Scope Chain برای دسترسی به متغیرهای محیط‌های بیرونی  

---

## سوال 7: Hoisting و Temporal Dead Zone

- Hoisting:  
  - معرف‌سازی (declaration) متغیرها و توابع در فاز کامپایل  
  - `var` → مقدار اولیه `undefined`  
- Temporal Dead Zone (TDZ):  
  - `let`/`const` قبل از تعریف قابل دسترسی نیستند → ReferenceError  
- مثال:
  ```js
  console.log(aVar);  // undefined
  console.log(aLet);  // ReferenceError
  var aVar; let aLet;
  ```

---

## سوال 8: Promise و مدیریت asynchronous

- ایجاد Promise: `new Promise((resolve, reject) => {…})`  
- متدها: `.then()`, `.catch()`, `.finally()`  
- Promise chaining و error propagation  
- مثال:  
  ```js
  fetch(url)
    .then(r => r.json())
    .then(data => console.log(data))
    .catch(err => console.error(err));
  ```

---

## سوال 9: async/await و بهبود خوانایی

- `async function` → تابع همیشه یک Promise برمی‌گرداند  
- `await` داخل async برای منتظر ماندن روی Promise  
- try/catch برای مدیریت خطا  
- مثال:
  ```js
  async function getData() {
    try {
      const res = await fetch(url);
      const data = await res.json();
      return data;
    } catch (e) {
      console.error(e);
    }
  }
  ```

---

## سوال 10: مدل هم‌زمانی (Concurrency Model)

- Single-threaded event loop  
- Call Stack و Web APIs (در مرورگر) یا libuv (در Node.js)  
- Macrotask Queue (setTimeout, I/O) مقابل Microtask Queue (Promise callbacks)  
- ترتیب اجرا:  
  1. اجرای کد هم‌زمان  
  2. اجرای تمام میکروتسک‌ها  
  3. اجرای یک ماکروتسک  
  4. رندر (در مرورگر)  
- کاربرد: جلوگیری از UI Blocking، مدیریت heavy computations با Web Worker  

---

## سوال 11: var, let و const

- Scope  
  - `var` → Function/Global  
  - `let`/`const` → Block  
- Hoisting  
  - `var` → hoisted با value=`undefined`  
  - `let`/`const` → hoisted در TDZ  
- Redeclaration vs Reassignment  
  - `var` قابل تعریف مجدد  
  - `let`/`const` یکبار تعریف  
  - `const` الزام اولیه مقداردهی  
- الگو: استفاده از `const`، سپس `let` و پرهیز از `var`

---

## سوال 12: Event Delegation و Bubbling

- Event Bubbling: انتشار رویداد از فرزند به والد  
- Delegation: نصب یک listener روی والد  
- تشخیص `event.target` یا استفاده از `closest()`  
- مزایا: کارایی بالا، مدیریت المان‌های داینامیک  
- جلوگیری از حباب‌دهی با `stopPropagation()` در صورت نیاز  

---

## سوال 13: Prototypal Inheritance

- `[[Prototype]]` (داخلی) و `__proto__` (دسترسی)  
- جستجوی property در Prototype Chain تا `Object.prototype`  
- Constructor Function + `Constructor.prototype`  
- `Object.create(proto)`  
- ES6 Classes: `class`/`extends`/`super`  
- `instanceof` و `isPrototypeOf`  

---

## سوال 14: Closure (جزئیات)

- تعریف: تابع داخلی که به متغیرهای تابع والد دسترسی دارد  
- Lexical Environment و Scope Chain  
- مثال Factory Function و Module Pattern  
- کاربردها: encapsulation، private APIs، partial application  
- تأثیر روی GC: جلوگیری از جمع‌آوری متغیر والد تا زمانی که closure زنده باشد  

---

## سوال 15: مدیریت هم‌زمانی (Event Loop تفصیلی)

- Call Stack، Macrotask و Microtask Queues  
- ترتیب اجرا در هر tick  
- مثال تفاوت `setTimeout(..., 0)` و Promise  
- چالش‌ها: starvation، UI blocking  
- الگوهای بهینه: functional updates، throttle/debounce، Web Worker  

---

## سوال 16: فرق var، let و const (عمقی‌تر)

- TDZ و خطای دسترسی قبل از تعریف  
- تفاوت خطا در Redeclaration vs Reassignment  
- توصیه‌های پروژه‌ای: strict mode، ESLint rules  
- مثال‌های متداول خطا  

---

## سوال 17: Event Delegation (React & Vanilla)

- Vanilla: listener روی والد، `event.target.closest()`  
- React: مشابه، اما React خودش handlerها را مدیریت می‌کند  
- پاک‌سازی: useEffect cleanup برای listenerهای مستقیم DOM  
- جلوگیری از نشت حافظه و re-subscribe مکرر با `useCallback` یا ref واسط  

---

## سوال 18: Inheritance و Prototype Chain (جزئیات)

- `[[Prototype]]`، `constructor.prototype` و لینک خودکار با `new`  
- الگوریتم lookup پراپرتی  
- پیکربندی prototype با `Object.create`  
- ES6 Classes پشت‌صحنه و super()  
- نکات shared state و override  

---

## سوال 19: call, apply, bind

- تفاوت در ارسال آرگومان:  
  - `call(thisArg, arg1, arg2)`  
  - `apply(thisArg, [arg1, arg2])`  
  - `bind(thisArg, …presetArgs)` → تابع جدید  
- کاربردها: method borrowing, currying, تنظیم this برای callback  

---

## سوال 20: Shallow Copy vs Deep Copy

- Shallow: `{…obj}`, `Object.assign()`, اشتراک مرجع در زیرآیتم‌ها  
- Deep (ساده): `JSON.parse(JSON.stringify(obj))` (محدودیت‌ها: Date, function, circular)  
- Deep (سفارشی): بازگشتی، پشتیبانی از Date, RegExp, Map, Set، جلوگیری از circular با WeakMap  
- بررسی performance و مصرف حافظه  
- انتخاب روش بر اساس ساختار داده  

---

با این مرورِ جامع می‌توانی مباحث کلیدی هر سوال را در یک نگاه ببینی و در صورت نیاز دوباره جزئیات هر مبحث را واکاوایی کنی. موفق باشی!
