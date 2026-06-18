# متتبع الأسعار

## رفع المشروع على GitHub
```
git init
git add .
git commit -m "أول نسخة"
git branch -M main
git remote add origin <رابط الريبو بتاعك>
git push -u origin main
```

## النشر على Railway
1. اعمل New Project → Deploy from GitHub repo، واختار الريبو.
2. من تبويب Variables ضيف:
   - `TELEGRAM_BOT_TOKEN` = توكن البوت من BotFather
   - `TELEGRAM_BOT_USERNAME` = يوزر البوت بدون @
   - `SECRET_KEY` = أي نص عشوائي طويل
   - `PUBLIC_BASE_URL` = رابط دومين Railway اللي هيظهر بعد أول deploy (مثلاً `https://xxx.up.railway.app`)
3. **مهم:** ضيف Volume من تبويب Settings واربطه بمسار `/app/instance` عشان قاعدة البيانات SQLite تفضل محفوظة لو الخدمة اعادت التشغيل.
4. بعد أول نشر ناجح، افتح في المتصفح:
   `https://رابط-مشروعك/telegram/setup?action=register`
   ده هيسجل webhook تليجرام تلقائيًا على رابط مشروعك.
5. تأكد إن الـ webhook اتسجل صحيح بفتح:
   `https://رابط-مشروعك/telegram/setup`

## ملاحظة مهمة
الـ Procfile مضبوط على `--workers 1` عمدًا. لو زودت الـ workers، الـ scheduler هيشتغل أكتر من مرة وهترسل تنبيهات مكررة لكل تغيير سعر.

## هيكل المشروع
- `app/scraper/` — استخراج السعر من EasyOrders وWooCommerce وShopify
- `app/telegram_bot/` — معالجة رسائل تليجرام وربط الحساب
- `app/scheduler/` — الفحص الدوري وإرسال التنبيهات
- `app/routes/` — صفحات الموقع (تسجيل، دخول، لوحة تحكم، webhook)
