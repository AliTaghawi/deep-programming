# فهرست جامع و عمیق مباحث Next.js

در ادامه فهرستی چندسطحی از مهم‌ترین مفاهیم، قابلیت‌ها و الگوهای پیشرفتهٔ Next.js ارائه شده است. با مرور هر بخش می‌توانی بر تمام جوانب این فریمورک تسلط پیدا کنی.

---

## ۱. مقدمه و معماری کلی

- مزایا نسبت به React خالص:  
  - رندر سمت سرور (SSR) و تولید استاتیک (SSG)  
  - File-system routing خودکار  
- ساختار پروژه:  
  - دایرکتوری‌های `pages`، `public`، `styles`  
  - پیکربندی در `next.config.js`

---

## ۲. File-System Routing

- صفحات استاتیک:  
  - `pages/index.js` → `/`  
- مسیرهای داینامیک:  
  - `[slug].js`، catch-all `[...params].js`  
- Nested Routes: ساختار پوشه‌ای برای URLهای عمیق  
- API Routes در `pages/api/*`

---

## ۳. Data Fetching و پیش‌پردازش

- getStaticProps (SSG)  
- getStaticPaths برای صفحات داینامیک استاتیک  
- getServerSideProps (SSR)  
- ISR (Incremental Static Regeneration)  
- Client-side Fetching:  
  - SWR، React Query  
  - Hooks اختصاصی

---

## ۴. API Routes و Middleware

- ساخت روتر در `pages/api`  
- پارامترهای داینامیک در API  
- Middleware سفارشی (`middleware.js`)  
- Edge Functions: اجرا در لبهٔ شبکه  
- کنترل authentication و rate-limiting در مسیرها

---

## ۵. دایرکتوری جدید App (Next.js 13+)

- Server Components vs Client Components  
- فایل‌های ویژه:  
  - `app/layout.js` (Nested Layouts)  
  - `app/page.js`  
  - `app/loading.js` و `app/error.js`  
- Data Fetching در Server Components (`fetch`, `cache`)  
- Streaming و Suspense برای رندر تدریجی

---

## ۶. بهینه‌سازی استایل و Assetها

- استایل‌دهی:  
  - Global CSS، CSS Modules  
  - `styled-jsx`، Tailwind CSS  
- فونت:  
  - `next/font` برای بارگذاری بهینه  
- تصاویر:  
  - مؤلفهٔ `<Image>` و بهینه‌سازی خودکار  
- اسکریپت‌ها:  
  - `<Script>` با استراتژی‌ بارگذاری (`beforeInteractive`, `lazyOnload`)

---

## ۷. بین‌المللی‌سازی (i18n)

- پیکربندی `i18n` در `next.config.js`  
- مسیرهای چندزبانه خودکار  
- ترجمهٔ محتوا در getStaticProps / Server Components  
- سوئیچ زبان با لینک‌های پیش‌فرض

---

## ۸. احراز هویت و مجوزها

- NextAuth.js:  
  - Providers (OAuth، Credentials)  
  - Session Management و JWT  
- Middleware-based Auth: کنترل دسترسی در `middleware.js`  
- پیاده‌سازی رول‌ها (RBAC/ABAC) در API Routes

---

## ۹. عملکرد و بهینه‌سازی

- Prefetch خودکار لینک‌ها (`<Link prefetch>`)  
- Code Splitting با `next/dynamic`  
- اندازهٔ باندل (Bundle Analyzer)  
- جلوگیری از رندر غیرضروری: React.memo، useMemo، useCallback  
- CDN و Cache Headers در Vercel

---

## ۱۰. SEO و متادیتا

- `<Head>` برای تگ‌های متا، Open Graph، Twitter Cards  
- پشتیبانی از AMP  
- sitemap.xml و robots.txt خودکار یا سفارشی  
- next-seo برای مدیریت ساده‌تر

---

## ۱۱. خطا و صفحات پیش‌فرض

- صفحهٔ 404 سفارشی: `pages/404.js` یا `app/not-found.js`  
- صفحهٔ خطای سرور: `pages/_error.js`  
- Error Boundaries در Client Components  
- گزارش خطا: Sentry، LogRocket

---

## ۱۲. محیط و کانفیگ

- فایل‌های محیطی: `.env.local`, `.env.production`  
- متغیرهای عمومی (`NEXT_PUBLIC_`) و سرور  
- پیکربندی Image Domains، rewrites, redirects در `next.config.js`  
- Experimental Features و Flags

---

## ۱۳. تست و تضمین کیفیت

- Unit Test: Jest + Testing Library (React)  
- Integration/E2E: Cypress، Playwright  
- Mock API Routes با msw  
- Snapshot Testing برای صفحات استاتیک

---

## ۱۴. استقرار و مقیاس‌پذیری

- Deploy در Vercel (پیش‌فرض)  
- Deploy به Netlify، AWS, GCP  
- تنظیم Region، Environment Variables  
- Auto Scaling و Edge Caching

---

## ۱۵. معماری و Patterns

- Folder Structure پیشنهادی (Feature-based)  
- Atomic Design در Next.js  
- Shared Layouts و Template Patterns  
- Domain-Driven Design (DDD) در صفحات و API Routes

---

## ۱۶. Monorepo و ابزارهای مرتبط

- Turborepo: تنظیم کارپوشه‌های همزمان  
- pnpm Workspace، Yarn Workspaces  
- Code Sharing بین وب و موبایل (Expo)

---

## ۱۷. رندر سمت سرور پیشرفته

- Middleware برای اصلاح Request/Response  
- Streaming SSR (React 18+)  
- Edge Rendering: اجرای SSR در لبهٔ شبکه  
- Preview Mode برای پیش‌نمایش محتوای CMS

---

## ۱۸. امنیت و Hardening

- Content Security Policy (CSP)  
- Helmet.js یا تنظیمات HTTP headers  
- XSS/CSRF Protection در API Routes  
- Rate Limiting و CORS

---

## ۱۹. مانیتورینگ و آنالیز

- Vercel Analytics و Lighthouse  
- Log Aggregation: Datadog، New Relic  
- Performance Tracing: OpenTelemetry

---

## ۲۰. مباحث نوظهور و آینده

- Server Actions (Next.js 14+)  
- React Server Components تکمیلی  
- Edge Runtime پیشرفته  
- ترکیب WebAssembly با Next.js  
- Plug-in Architecture (Next.js Plugin System)

---

با تسلط گام‌به‌گام و عمیق بر این سرفصل‌ها، می‌توانی به خوبی از امکانات Next.js بهره ببری و در پروژه‌های بزرگ یک الگوی Senior-level ایجاد کنی. موفق باشی!
