# درک عمیق Execution Context در جاوااسکریپت

Execution Context محیطی است که در آن کد جاوااسکریپت اجرا می‌شود. هر بار که مفسر وارد بخشی از کد می‌شود—چه در سطح جهانی، چه هنگام فراخوانی تابع، چه هنگام اجرای `eval`—یک Execution Context جدید ساخته و وارد آن می‌شود. این محیط حاوی اطلاعات لازم برای ارزیابی و اجرای کد است.

---

## 1. انواع Execution Context

1. Global Execution Context  
   - همیشه یک بار در شروع برنامه ساخته می‌شود.  
   - شیء `global object` (مثل `window` در مرورگر و `global` در Node) و متغیر `this` در آن به آن شیء اشاره دارد.  

2. Function Execution Context  
   - هر بار تابعی فراخوانی شود، یک Context جدید ایجاد می‌شود.  
   - `this` درون تابع، آرگومان‌ها (`arguments`)، متغیرها و فضای نام (Scope) مخصوص تابع را شامل می‌شود.  

3. Eval Execution Context  
   - هنگام اجرای کد درون `eval()` ایجاد می‌شود.  
   - کمتر در عمل استفاده می‌شود و توصیه نمی‌شود به دلیل مسائل امنیتی و عملکرد.

---

## 2. دو فاز در ساخت و اجرای Context

هر Execution Context دو فاز اصلی دارد:

1. Creation Phase  
   - تخصیص حافظه برای متغیرها (`var`)، توابع (Function Declarations) و پارامترها  
   - ایجاد **Variable Environment** (ثبت متغیرها و مقدار اولیه `undefined`)  
   - ایجاد **Lexical Environment** (برای ساختن scope chain)  
   - تعیین مقدار `this` بر اساس نحوه فراخوانی  

2. Execution Phase  
   - مقداردهی واقعی به متغیرها و پارامترها  
   - اجرای بدنه کد خط به خط  
   - ارزیابی عبارات و اعمال تغییرات در محیط  

---

## 3. ساختار محیط‌های درونی

هر Execution Context شامل دو رکورد کلیدی است:

- Variable Environment  
  *شامل ثبت‌ها (bindings) برای متغیرهای `var`، پارامترها و توابع.*  
- Lexical Environment  
  *شامل همه ثبت‌ها و همچنین ارجاع به محیط‌های بالاتر (scope chain) برای حل نام‌گذاری‌ها.*

رابطه‌ی این دو بدین شکل است:

```
ExecutionContext
├─ VariableEnvironment
└─ LexicalEnvironment → [Local, Outer, …, Global]
```

---

## 4. Scope Chain و نام‌گشایی (Identifier Resolution)

وقتی جاوااسکریپت با متغیری مواجه می‌شود:

1. ابتدا در **Lexical Environment** فعلی به دنبال آن می‌گردد.  
2. اگر پیدا نشد، به محیط والد (outer environment) مراجعه می‌کند.  
3. این روند تا رسیدن به Global ادامه می‌یابد.  
4. اگر متغیر یافت نشد، ارور `ReferenceError` پرتاب می‌شود.

---

## 5. this Binding در Context

مقدار `this` در هر Execution Context بر اساس الگوی فراخوانی تابع تعیین می‌شود:

- فراخوانی به صورت تابع معمولی: `this` اشاره به `global object` (یا `undefined` در strict mode)  
- فراخوانی متدی روی شیء: `this` اشاره به آن شیء  
- فراخوانی با `call`/`apply`/`bind`: `this` با مقدار صریح تعیین می‌شود  
- استفاده از arrow function: `this` از محیط والد به ارث می‌برد  

```js
const obj = {
  x: 10,
  getX() { return this.x; }
};

function foo() {
  console.log(this);
}

obj.getX();            // this → obj
foo();                 // this → global (یا undefined در strict)
foo.call({ y: 20 });   // this → { y: 20 }
```

---

## 6. Call Stack و مدیریت Contextها

- زمانی که برنامه اجرا می‌شود، هر Context درون یک **Call Stack** قرار می‌گیرد.  
- Global Context در پایین‌ترین سطح است.  
- هر فراخوانی تابع، Context جدید روی پشته قرار می‌دهد.  
- پس از پایان اجرای تابع، Context از پشته خارج (pop) می‌شود.  

```
┌───────────────┐
│ Function C    │ ← جاری
├───────────────┤
│ Function B    │
├───────────────┤
│ Function A    │
├───────────────┤
│ Global Context│
└───────────────┘
```

---

## 7. مثال عملی

```js
const a = 5;

function outer(b) {
  const c = 10;
  function inner(d) {
    const e = a + b + c + d;
    return e;
  }
  return inner(20);
}

console.log( outer(15) );  // روند زیر اتفاق می‌افتد:
// 1. Global Context ساخته می‌شود.
// 2. اجرای outer → Context جدید.
// 3. اجرای inner → Context جدید.
// 4. اجرای inner کامل شد → Context inner پاک.
// 5. برگشت به Context outer → پاک.
// 6. اتمام Global.
```

---

## 8. نکات پیشرفته

- متغیرهای `let` و `const` در **Temporal Dead Zone** هستند تا قبل از مقداردهی اولیه نتوانند خوانده شوند.  
- Closureها (تابع‌هایی که به Lexical Environment والد اشاره دارند) به زندگی محیط والد ادامه می‌دهند حتی پس از خروج از Context والد.  
- بهینه‌سازهای موتور جاوااسکریپت—مثل V8—ممکن است برخی مراحل را ادغام یا بهینه کنند اما مدل منطقی Execution Context همیشه پابرجاست.  

---

## 9. گام‌های بعدی (فراتر از Execution Context)

برای درک عمیق‌تر اجرای کد در جاوااسکریپت، می‌توانید مباحث زیر را کاوش کنید:

- Event Loop و نحوه تعامل با Call Stack  
- صف Microtask و Macrotask (Promise، setTimeout و…)  
- Garbage Collection و چرخه حیات متغیرها  
- طراحی موتورهای JIT و بهینه‌سازی Execution Context  
- تفاوت Execution Context در Web Workers یا Node.js Worker Threads  

با تسلط بر Execution Context و مفاهیم پیرامون آن، دید وسیع و دقیقی نسبت به چرخه‌ی حیات و اجرای کد در جاوااسکریپت خواهید داشت.
