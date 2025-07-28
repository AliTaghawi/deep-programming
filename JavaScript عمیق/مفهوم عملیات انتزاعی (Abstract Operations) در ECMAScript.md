# مفهوم عملیات انتزاعی (Abstract Operations) در ECMAScript

عملیات انتزاعی توصیف‌های گام‌به‌گام و عمومی هستند که در مشخصات ECMAScript برای تبدیل و بررسی انواع داده‌ها تعریف شده‌اند. این توصیف‌ها نه متدهای قابل دسترس در کد، بلکه الگوریتم‌های درونی زبان هستند.

---

## انواع عملیات انتزاعی اصلی

- ToPrimitive  
- ToBoolean  
- ToNumber  
- ToString  
- ToObject  

---

## ToPrimitive

عملکرد: تبدیل یک مقدار غیر اولیه (مثلاً شیء یا آرایه) به مقدار اولیه (Primitive)  
الگوریتم کلی:

- اگر ورودی از نوع Primitive باشد، همان را برگردان.  
- اگر hint برابر string باشد، متدهای toString و سپس valueOf را صدا بزن.  
- اگر hint برابر number باشد، متدهای valueOf و سپس toString را صدا بزن.  
- اگر خروجی هنوز شیء بود، خطا پرتاب کن.

---

## ToBoolean

عملکرد: ارزیابی حقیقت‌‌نمای (truthiness) یک مقدار  
قوانین:

- مقادیر false, 0, NaN, '' (رشته خالی), null, undefined به false تبدیل می‌شوند.  
- بقیه مقادیر (از جمله اشیاء، آرایه‌ها و رشته‌های غیرخالی) به true تبدیل می‌شوند.

---

## ToNumber

عملکرد: تبدیل انواع به عدد  
گام‌های اصلی:

- boolean: true → 1, false → 0  
- null → 0, undefined → NaN  
- رشته: مطابق قواعد عددخوانی رشته parse می‌شود  
- شیء: ابتدا ToPrimitive با hint number و سپس به عدد تبدیل می‌شود

---

## ToString

عملکرد: تبدیل انواع به رشته  
قواعد تبدیل:

- undefined → "undefined", null → "null"  
- boolean → "true" یا "false"  
- عدد → نمایش رشته‌ای عدد (شامل نمای علمی برای اعداد بزرگ)  
- شیء → ابتدا ToPrimitive با hint string و سپس به رشته تبدیل می‌شود

---

## ToObject

عملکرد: تولید یک شیء براساس مقدار اولیه  
قواعد:

- null و undefined باعث خطای TypeError می‌شوند  
- رشته، عدد و boolean به اشیاء Wrapper تبدیل می‌شوند  
- اشیاء اصلی (object, array, function) بدون تغییر برگردانده می‌شوند

---

## جدول مروری عملیات

| عملیات        | ورودی                      | خروجی                                      |
|---------------|----------------------------|---------------------------------------------|
| ToPrimitive   | هر نوع                     | Primitive (string یا number بسته به hint)   |
| ToBoolean     | هر نوع                     | Boolean                                     |
| ToNumber      | هر نوع                     | Number                                      |
| ToString      | هر نوع                     | String                                      |
| ToObject      | هر نوع                     | Object (یا خطا برای null/undefined)         |

---

برای درک عمیق‌تر می‌توانید به فصل مربوط به Abstract Operations در مشخصات ECMAScript مراجعه کنید. آیا دوست داری مثال‌های عملی این تبدیل‌ها را در کد ببینی یا به جزئیات بیشتری مثل ToLength و ToPropertyKey بپردازیم؟
