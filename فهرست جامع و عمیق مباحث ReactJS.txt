# فهرست جامع و عمیق مباحث ReactJS

برای تسلط بر ReactJS و تبدیل شدن به یک توسعه‌دهنده‌ی Senior در این حوزه، نیاز است همهٔ لایه‌ها از مفاهیم پایه تا مباحث پیشرفته و اکوسیستم پیرامون آن را بشناسی. در ادامه ساختاری گام‌به‌گام و چندسطحی از این مباحث ارائه شده است.

---

## ۱. مبانی و فلسفهٔ React

- **Virtual DOM**  
  مقایسهٔ درخت‌های DOM برای به‌روزرسانی بهینه  
- **یک‌طرفه بودن جریان داده (One-way Data Binding)**  
  از والد به فرزند با props  
- **Component-Based Architecture**  
  تفکیک رابط کاربری به قطعات کوچک و قابل استفاده مجدد  
- **Declarative Programming**  
  تمرکز روی «چه چیزی» نه «چگونه»

---

## ۲. JSX و نحوهٔ کارکرد آن

- **ترکیب HTML و JavaScript**  
  تبدیل به فراخوانی‌های `React.createElement`  
- **Expressions در JSX**  
  دسترسی ایمن به مقادیر با `{}`  
- **Conditional Rendering**  
  استفاده از عملگر شرطی، `&&`، توابع کمکی  
- **Lists & Keys**  
  اهمیت کلید یکتا برای بهینه‌سازی رندر  
- **Fragments**  
  حذف المان‌های اضافی با `<>…</>`

---

## ۳. کامپوننت‌ها

- **Functional Components**  
  سبک تابعی و ساده‌ترین شکل  
- **Class Components**  
  قبل از هوک‌ها و برای lifecycle مفصل  
- **Component Composition**  
  ترکیب چند کامپوننت برای ساخت رفتارهای پیچیده  
- **Presentational vs Container**  
  جداسازی رابط و منطق

---

## ۴. Props و State

- **Props**  
  مقداردهی از والد، immutable در کامپوننت فرزند  
- **State**  
  داده‌های داخلی قابل تغییر برای هر کامپوننت  
- **Lifting State Up**  
  اشتراک‌گذاری state بین چند فرزند با بالا بردن آن تا نزدیک‌ترین والد مشترک  
- **Derived State**  
  محاسبهٔ داده‌ها از props یا state دیگر

---

## ۵. چرخهٔ حیات (Lifecycle) در کلاس

- **Mounting**  
  `constructor` → `render` → `componentDidMount`  
- **Updating**  
  `shouldComponentUpdate` → `render` → `componentDidUpdate`  
- **Unmounting**  
  `componentWillUnmount`  
- **Error Boundaries**  
  `componentDidCatch` برای مدیریت خطاهای داخل UI  

---

## ۶. هوک‌ها (Hooks)

- **useState**  
  مدیریت state محلی در تابع  
- **useEffect**  
  شبیه lifecycleها: mount، update، unmount  
- **useRef**  
  نگهداری ارجاع به DOM یا مقدار mutable بدون رندر مجدد  
- **useMemo & useCallback**  
  بهینه‌سازی محاسبات و جلوگیری از ایجاد توابع جدید در هر رندر  
- **useContext**  
  استفاده از Context API بدون HOC  
- **Custom Hooks**  
  استخراج منطق مشترک بین چند کامپوننت  
- **Additional Built-in Hooks**  
  `useReducer`, `useLayoutEffect`, `useImperativeHandle`, `useDebugValue`

---

## ۷. Context API

- **ایجاد Context**  
  `React.createContext(defaultValue)`  
- **Provider / Consumer**  
  ارسال و دریافت داده بدون props drilling  
- **ترکیب با useContext**  
  خواندن مقدار در کامپوننت تابعی  
- **موارد کاربرد**  
  ترجمه (i18n)، theme، احراز هویت  

---

## ۸. مدیریت استیت سراسری

- **Redux**  
  Store، Actions، Reducers، Middleware (Thunk, Saga)  
- **MobX**  
  observable state و reactive programming  
- **React Query / SWR**  
  کش و اعتبارسنجی داده‌های fetch شده  
- **Recoil / Zustand / Jotai**  
  کتابخانه‌های سبک برای atom-based state  
- **Context + useReducer**  
  پیاده‌سازی سبک Redux بدون وابستگی خارجی  

---

## ۹. مسیریابی (Routing)

- **react-router**  
  BrowserRouter, HashRouter, Routes, Route  
- **پارامترها و کوئری‌ها**  
  `useParams`, `useLocation`, `useNavigate`  
- **Lazy Loading و Code Splitting**  
  `React.lazy` + `Suspense`  
- **Nested Routes**  
  ساختار RESTful در UI

---

## ۱۰. دریافت و کش داده (Data Fetching)

- **Fetch API / Axios**  
  درخواست‌های HTTP ساده  
- **React Query / SWR**  
  کش خودکار، polling، refetch on focus  
- **GraphQL**  
  Apollo Client یا Relay  
- **WebSocket / Server-Sent Events**  
  بروزرسانی‌های Real-time  

---

## ۱۱. الگوهای طراحی کامپوننت

- **Higher-Order Components (HOC)**  
  تابعی که کامپوننت می‌گیرد و آن را تغییر می‌دهد  
- **Render Props**  
  ارسال تابع به‌عنوان prop برای کنترل rendering  
- **Compound Components**  
  اشتراک internal state بین زیرکامپوننت‌ها  
- **Function as Child**  
  مشابه Render Props با استفاده از children  
- **Controlled vs Uncontrolled Components**  
  فرم‌ها با مدیریت React vs ref مستقیم  

---

## ۱۲. بهینه‌سازی عملکرد

- **Virtual DOM Diffing**  
  درک الگوریتم reconciliation  
- **PureComponent / React.memo**  
  جلوگیری از رندر مجدد غیرضروری  
- **useMemo و useCallback**  
  کش محاسبات و توابع  
- **Windowing / List Virtualization**  
  react-window یا react-virtualized  
- **Lazy Loading تصاویر و کامپوننت‌ها**  
  Intersection Observer API  
- **Profiling**  
  DevTools Profiler و Lighthouse  

---

## ۱۳. SSR، SSG و فریم‌ورک‌ها

- **Next.js**  
  `getStaticProps`, `getServerSideProps`, Incremental Static Regeneration  
- **Gatsby**  
  GraphQL data layer، پلاگین‌ها  
- **Remix**  
  روتر و data-fetch با رندر سمت سرور  
- **React Server Components**  
  خروجی ترکیب شده از سرور و کلاینت  

---

## ۱۴. تست و تضمین کیفیت

- **Unit Testing**  
  Jest, react-testing-library  
- **Integration Testing**  
  ترکیب چند کامپوننت و mock API  
- **E2E Testing**  
  Cypress, Playwright  
- **Snapshot Testing**  
  جلوگیری از تغییرات غیرمنتظره در UI  
- **Mocking & Stubbing**  
  msw، jest.mock  

---

## ۱۵. TypeScript با React

- **typing کامپوننت‌ها**  
  FC<Props>, PropsWithChildren, Generic Components  
- **هوک‌ها و Context**  
  تعریف نوع برای state و dispatch  
- **HOCs و Render Props**  
  نگه‌داشتن تایپ‌ها در آبجکت‌های higher-order  
- **Utility Types**  
  Partial, Pick, Omit, ReturnType  

---

## ۱۶. استایل‌دهی و طراحی

- **CSS Modules**  
  scoping خودکار  
- **CSS-in-JS**  
  Styled Components, Emotion  
- **Tailwind CSS**  
  utility-first framework  
- **Theme Management**  
  ThemeProvider، custom hooks  
- **Responsive Design**  
  Media Queries، CSS Grid، Flexbox  

---

## ۱۷. دسترسی‌پذیری (Accessibility)

- **WAI-ARIA**  
  نقش‌ها (roles) و صفت‌ها (aria-*)  
- **Keyboard Navigation**  
  مدیریت focus، تَب ایندکس  
- **Screen Reader Testing**  
  axe-core، Lighthouse accessibility  
- **Color Contrast**  
  WCAG guidelines  

---

## ۱۸. معماری و Best Practices

- **Folder Structure**  
  Feature-based vs Layer-based  
- **Atomic Design**  
  Atoms, Molecules, Organisms  
- **Clean Architecture**  
  Separation of Concerns، Dependency Inversion  
- **Code Splitting**  
  dynamic import و bundle analysis  
- **Monitoring & Logging**  
  Sentry، LogRocket  

---

## ۱۹. مباحث پیشرفته

- **Concurrent Mode** (بخشی از React 18)  
  Suspense for Data Fetching، Transitions  
- **Fiber Architecture**  
  درک نحوهٔ کار رندر در React  
- **Portals**  
  رندر فرزندها خارج از درخت والد  
- **Error Boundaries پیشرفته**  
  fallback UI و گزارش خطا  
- **Profiler API**  
  ضبط و آنالیز رفتار کامپوننت‌ها  

---

## ۲۰. اکوسیستم و ابزارها

- **DevTools**  
  React DevTools Profiler، Extensionهای مرورگر  
- **CLI و Create React App**  
  customization با CRACO یا eject  
- **Bundlers**  
  Webpack, Vite, Rollup  
- **Linting & Formatting**  
  ESLint (eslint-plugin-react), Prettier  
- **Storybook**  
  مستندسازی و تست کامپوننت‌ها  
- **Package Management**  
  npm, Yarn, pnpm  

---

با یادگیری عمیق و تمرین روی هر یک از این سرفصل‌ها و زیرموضوع‌ها، می‌توانی در سطح Senior ReactJS Developer قرار بگیری و به‌خوبی در پروژه‌های پیچیدهٔ واقعی عمل کنی. موفق باشی!
