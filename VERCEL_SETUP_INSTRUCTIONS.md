# دليل إعداد Vercel Blob Storage
# Vercel Blob Storage Setup Guide

## 🎯 **الهدف**
إعداد رفع الصور على Vercel باستخدام Blob Storage لحل مشكلة عدم إمكانية كتابة الملفات في نظام الملفات على Vercel.

## ✅ **ما تم إنجازه**

### **1. تثبيت الحزمة المطلوبة:**
```bash
npm install @vercel/blob ✅
```

### **2. إنشاء API للرفع إلى Vercel Blob:**
- ✅ `src/app/api/upload-vercel/route.ts`
- ✅ دعم جميع أنواع الصور (JPG, PNG, WebP, GIF)
- ✅ حد أقصى 10MB
- ✅ أسماء ملفات فريدة

### **3. تحديث مكون الرفع:**
- ✅ `src/components/ui/image-upload-folder.tsx`
- ✅ اختيار تلقائي للطريقة حسب البيئة
- ✅ حلول احتياطية متعددة

### **4. إعداد متغيرات البيئة:**
- ✅ `.env.local` للتطوير
- ✅ `.env.production` للإنتاج
- ✅ `BLOB_READ_WRITE_TOKEN` جاهز

## 🚀 **خطوات النشر على Vercel**

### **الخطوة 1: رفع الكود**
```bash
git add .
git commit -m "Add Vercel Blob Storage support"
git push origin main
```

### **الخطوة 2: إعداد متغيرات البيئة في Vercel**

1. **اذهب إلى Vercel Dashboard:**
   - [https://vercel.com/dashboard](https://vercel.com/dashboard)
   - اختر مشروع `evamun`

2. **اذهب إلى Settings → Environment Variables:**
   - أضف المتغيرات التالية:

```env
# قاعدة البيانات
DB_HOST=193.203.168.5
DB_PORT=3306
DB_USER=u283511061_mun
DB_PASSWORD=E123123123ee@90
DB_NAME=u283511061_eeva

# Next.js
NEXTAUTH_URL=https://evamun.vercel.app
NEXTAUTH_SECRET=menu-eva-super-secret-key-2024-production

# المصادقة
JWT_SECRET=menu-eva-super-secret-key-2024-admin-panel
ADMIN_USERNAME=admin
ADMIN_PASSWORD=admin123

# Vercel Blob Storage
BLOB_READ_WRITE_TOKEN=vercel_blob_rw_7pHQ2HFckWpllaSy_CRZ9rfdXPErsZYbam9JuaRvtkMaOIR

# إعدادات الرفع
UPLOAD_MAX_SIZE=10485760
UPLOAD_ALLOWED_TYPES=image/jpeg,image/jpg,image/png,image/webp,image/gif
NODE_ENV=production
```

### **الخطوة 3: إعادة النشر**
```bash
vercel --prod
```

## 🧪 **اختبار النظام**

### **1. اختبار محلي:**
- افتح: `http://localhost:9002/test-upload.html`
- جرب رفع صورة
- يجب أن يعمل مع Vercel Blob أو الطريقة الاحتياطية

### **2. اختبار الإنتاج:**
- افتح: `https://evamun.vercel.app/ar/admin`
- سجل دخول بـ: `admin` / `admin123`
- جرب إضافة منتج جديد مع صورة
- يجب أن تُرفع الصورة إلى Vercel Blob

## 🔍 **كيفية التحقق من النجاح**

### **في المتصفح:**
1. افتح Developer Tools (F12)
2. اذهب إلى Console
3. ابحث عن رسائل مثل:
   ```
   ✅ Upload successful with /api/upload-vercel
   ```

### **في Vercel Dashboard:**
1. اذهب إلى **Storage → Blob**
2. يجب أن ترى الملفات المرفوعة
3. تحقق من استخدام المساحة

### **في قاعدة البيانات:**
- روابط الصور يجب أن تبدأ بـ: `https://`
- مثال: `https://7phq2hfckwpllasy.public.blob.vercel-storage.com/...`

## 🛠️ **استكشاف الأخطاء**

### **خطأ: "حدث خطأ أثناء رفع الملف إلى التخزين السحابي"**

**الأسباب المحتملة:**
1. `BLOB_READ_WRITE_TOKEN` غير صحيح
2. حزمة `@vercel/blob` غير مثبتة
3. مشكلة في الشبكة

**الحلول:**
```bash
# تحقق من التثبيت
npm list @vercel/blob

# أعد التثبيت إذا لزم الأمر
npm install @vercel/blob

# تحقق من متغيرات البيئة
vercel env ls
```

### **خطأ: "الصور لا تظهر"**

**الأسباب المحتملة:**
1. رابط الصورة غير صحيح في قاعدة البيانات
2. مشكلة في CORS
3. الصورة لم تُرفع بنجاح

**الحلول:**
1. تحقق من رابط الصورة في قاعدة البيانات
2. تأكد من أن الرابط يبدأ بـ `https://`
3. جرب فتح الرابط مباشرة في المتصفح

### **خطأ: "بطء في الرفع"**

**الحلول:**
1. قلل حجم الصورة قبل الرفع
2. استخدم تنسيق WebP للحجم الأصغر
3. تحقق من سرعة الإنترنت

## 📊 **مراقبة الأداء**

### **في Vercel Dashboard:**
1. **Functions**: راقب `/api/upload-vercel`
2. **Blob Storage**: راقب المساحة المستخدمة
3. **Analytics**: راقب أوقات الاستجابة

### **حدود Vercel Blob:**
- **مجاني**: 1GB شهرياً
- **Pro**: $0.15/GB شهرياً
- **حد أقصى لحجم الملف**: 500MB

## 🔐 **الأمان**

### **نصائح مهمة:**
1. **لا تشارك** `BLOB_READ_WRITE_TOKEN` مع أحد
2. **استخدم HTTPS** دائماً في الإنتاج
3. **راجع الصور** المرفوعة دورياً
4. **احذف الصور** غير المستخدمة لتوفير المساحة

### **إعدادات الأمان المطبقة:**
- ✅ التحقق من نوع الملف
- ✅ حد أقصى لحجم الملف (10MB)
- ✅ أسماء ملفات فريدة لمنع التعارض
- ✅ رفع آمن إلى Blob Storage

## 🎊 **النتيجة المتوقعة**

بعد إكمال هذه الخطوات، ستحصل على:

### **✅ نظام رفع صور متكامل:**
- 🏠 **التطوير**: مجلدات محلية سريعة
- ☁️ **الإنتاج**: Vercel Blob Storage موثوق
- 🔄 **حلول احتياطية**: Base64 للطوارئ
- ⚡ **أداء ممتاز**: اختيار تلقائي للطريقة الأفضل

### **✅ إدارة محتوى كاملة:**
- 📦 **المنتجات**: إضافة وتعديل مع الصور
- 📁 **الأصناف**: تنظيم المحتوى
- 🌍 **دعم متعدد اللغات**: العربية والإنجليزية
- 🏷️ **ميزات متقدمة**: منتجات جديدة، أكثر مبيعاً

## 📞 **الدعم**

إذا واجهت مشاكل:

1. **تحقق من Vercel Function Logs:**
   - [https://vercel.com/dashboard](https://vercel.com/dashboard)
   - اختر المشروع → Functions → View Logs

2. **تحقق من Blob Storage:**
   - Dashboard → Storage → Blob

3. **اختبر محلياً أولاً:**
   - `http://localhost:9002/test-upload.html`

4. **راجع متغيرات البيئة:**
   ```bash
   vercel env ls
   ```

## 🎯 **الخطوة التالية**

**قم بتطبيق الخطوات أعلاه، وسيكون لديك نظام إدارة محتوى متكامل يعمل بشكل مثالي على Vercel!** 🚀

---

**ملاحظة:** جميع الإعدادات جاهزة، تحتاج فقط إلى إضافة متغيرات البيئة في Vercel Dashboard وإعادة النشر.
