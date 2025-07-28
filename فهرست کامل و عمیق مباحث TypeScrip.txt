# فهرست کامل و عمیق مباحث TypeScript

در ادامه ساختاری طبقه‌بندی‌شده از مباحث مهم و پیشرفتهٔ TypeScript ارائه شده است تا بتوانی گام‌به‌گام از مبانی تا پیچیده‌ترین ویژگی‌ها را دریابی.

---

## ۱. مقدمه و پیکربندی پروژه

- نصب TypeScript با npm/yarn و راه‌اندازی اولیه  
- فایل `tsconfig.json`  
  - گزینه‌های کلیدی: `strict`, `noImplicitAny`, `strictNullChecks`, `target`, `module`, `moduleResolution`  
  - مسیرهای جایگزین (Path Aliases) و ریشهٔ ماژول (`baseUrl`/`paths`)  
  - تنظیم `include`/`exclude` برای کنترل پردازش فایل‌ها  
- ابزارها و ادغام با Babel، Webpack، Rollup و ts-node  

---

## ۲. انواع پایه (Primitive Types)

- `string`, `number`, `boolean`  
- `null`, `undefined` و تفاوت در strict null checks  
- `object`, `any`, `unknown`, `never`, `void`, `bigint`, `symbol`  
- Literal Types (رشته/عدد/بولین ثابت)  
- تفاوت `any` و `unknown` و توصیه‌های امن‌سازی  

---

## ۳. آرایه‌ها، تاپل و Enum

- آرایه‌های ساده: `number[]` و `Array<string>`  
- `readonly` arrays  
- Tuples: تعریف طول و انواع مختلط  
- Enumها: `const enum` در مقابل `enum` معمولی  
- String Enums و Numeric Enums  

---

## ۴. Type Alias vs Interface

- تعریف نوع با `type` و `interface`  
- الحاق (Declaration Merging) در اینترفیس‌ها  
- توارث (Extends) و پیاده‌سازی (Implements)  
- کاربرد هر کدام و بهترین شیوه‌ها  

---

## ۵. توابع و امضای تابع (Function Types)

- تایپ آرگومان‌ها و مقدار بازگشتی  
- پارامترهای اختیاری (`?`)، پیش‌فرض و بقیه پارامترها (`...args`)  
- Function Overloads  
- اینترفیس یا Type Alias برای امضای توابع  
- Typing Callbacks و Higher-Order Functions  

---

## ۶. Genericها

- توابع Generic با `<T>`  
- Generic در کلاس‌ها و رابط‌ها  
- محدودیت Generic (Constraints) با `extends`  
- پارامترهای Generic پیش‌فرض  
- چند Generic و رابطهٔ بین آن‌ها  

---

## ۷. Utility Types داخلی

- Partial, Required, Readonly  
- Pick, Omit, Exclude, Extract  
- Record, NonNullable, ReturnType, InstanceType  
- Parameters, ConstructorParameters, Awaited  
-Mapped Types و Key Remapping  

---

## ۸. انواع پیشرفته (Advanced Types)

- Union Types و Intersection Types  
- Conditional Types (`T extends U ? X : Y`)  
- توزیع Union در Conditional Types  
- `infer` برای استنباط نوع در Conditional Types  
- Template Literal Types برای رشته‌های ترکیبی  
- Recursive Types (مثلاً لیست‌های پیوندی)  
- Branded Types (برای تعریف نوع‌های ایمن)  

---

## ۹. کنترل جریان نوع (Type Guards & Narrowing)

- `typeof`, `instanceof`, `in`  
- user-defined type guards با `value is Type`  
- افزونه‌های assertion functions (`asserts value is Type`)  
- Discriminated Unions با property مشترک  

---

## ۱۰. مدول‌ها و فضای نام (Modules & Namespaces)

- ES Module (`import`/`export`) و CommonJS (`require`/`module.exports`)  
- تعریف ambient modules و ماژول‌های ثالث  
- Namespaceها و جلوگیری از آلودگی فضای نام  
- Declaration Merging و Module Augmentation  

---

## ۱۱. فایل‌های اعلان (`.d.ts`) و DefinitelyTyped

- نوشتن و سازماندهی declaration files  
- روش‌های تهیه خودکار (`--declaration`)  
- پکیج‌های @types و مدیریت نسخه  
- Ambient Declarations برای متغیرهای سراسری و کتابخانه‌های بدون تایپ  

---

## ۱۲. کلاس‌ها و دکوراتورها

- مفاهیم کلاس در TS: modifiers (`public`/`private`/`protected`)، `readonly`, `static`  
- وراثت کلاس‌ها و پیاده‌سازی اینترفیس  
- دکوراتورهای experimental برای کلاس، متد، پراپرتی و پارامتر  
- کاربرد در فریم‌ورک‌هایی مثل Angular و NestJS  

---

## ۱۳. کانفیگ کامپایل و بهینه‌سازی

- Source Maps، Declaration Maps  
- Incremental Compilation و Project References  
- Composite Projects و چند زیرپروژه‌ای (Monorepo)  
- حذف کد غیرقابل‌دسترس (Dead Code Elimination)  

---

## ۱۴. یکپارچه‌سازی با فریم‌ورک‌ها

- React + TS (tsx): تایپ کامپوننت‌ها و هوک‌ها  
- Next.js + TS: `GetStaticProps`, `GetServerSideProps`, API Routes  
- Node.js + TS: تایپ Express, Koa, NestJS  
- Electron/React Native + TS  

---

## ۱۵. تست و تضمین کیفیت

- Jest و ts-jest  
- Testing Library با TypeScript  
- Mock کردن تایپ‌شده (msw, jest.mock)  
- Typing در Cypress/Playwright  

---

## ۱۶. linting و فرمتینگ

- ESLint + @typescript-eslint/parser & plugin  
- قواعد پیشنهادی: `noImplicitAny`, `strictNullChecks`, `noUnusedLocals`  
- Prettier و هماهنگی با ESLint  

---

## ۱۷. الگوها و معماری با TypeScript

- Domain-Driven Design: Entity, Value Object, Aggregate  
- Dependency Injection با Typedi, InversifyJS  
- Repository/Service Pattern  
- Typed Redux (Redux Toolkit)، RTK Query  

---

## ۱۸. مهاجرت گام‌به‌گام (Gradual Adoption)

- استفاده از `allowJs` و `checkJs`  
- JSDoc برای تایپ کمی پیش‌فرض  
- تبدیل فایل به `.ts/.tsx` به‌صورت incremental  
- ابزارهای Migrate: TS-Migrate, codemods  

---

## ۱۹. کارایی و محدودیت‌ها

- تأثیر strict mode بر سرعت کامپایل  
- کد تولیدشده و اندازه باندل  
- زمان پیشنهادات IDE و تنظیمات حفظ عملکرد  

---

## ۲۰. جامعه و منابع

- DefinitelyTyped، TypeSearch  
- پروژه‌های open-source با codebase TS  
- TypeScript Deep Dive (کتاب)  
- Type Challenges (سایت تمرین تایپ)  
- کنفرانس‌ها و مقالات رسمی (TSConf, blog مایکروسافت)  

---

با این نقشهٔ راه، می‌توانی از مبانی TypeScript تا عمیق‌ترین ویژگی‌های آن را یاد بگیری و در هر مرحله کدهایی با اطمینان‌خاطر و نوع‌نگاری دقیق تولید کنی. موفق باشی!
