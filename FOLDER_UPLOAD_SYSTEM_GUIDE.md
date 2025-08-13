# دليل نظام رفع الصور إلى المجلدات
# Folder Upload System Guide

## 🎯 **نظرة عامة / Overview**

تم إنشاء نظام رفع صور متطور يحفظ الصور في مجلدات منظمة داخل المشروع ويوفر روابط مباشرة للوصول إليها.

A sophisticated image upload system has been created that saves images in organized folders within the project and provides direct links to access them.

## 📁 **هيكل المجلدات / Folder Structure**

```
public/
└── uploads/
    ├── products/          # صور المنتجات
    ├── categories/        # صور الأصناف  
    └── banners/          # صور البنرات
```

### **مسارات الوصول / Access Paths**
- **المنتجات**: `/uploads/products/filename.jpg`
- **الأصناف**: `/uploads/categories/filename.jpg`
- **البنرات**: `/uploads/banners/filename.jpg`

## ✨ **الميزات الجديدة / New Features**

### 🔧 **API محسن**
- ✅ **أسماء ملفات فريدة**: `timestamp_randomhash.extension`
- ✅ **تنظيم تلقائي**: حفظ في المجلد المناسب حسب النوع
- ✅ **دعم أنواع متعددة**: JPG, PNG, WebP, GIF
- ✅ **حد أقصى 10MB**: للملفات الكبيرة
- ✅ **تسجيل مفصل**: لتتبع العمليات

### 🎨 **مكون رفع متطور**
- ✅ **واجهة جميلة**: مع معاينة فورية
- ✅ **سحب وإفلات**: Drag & Drop
- ✅ **معلومات الملف**: الحجم، النوع، المسار
- ✅ **مؤشر المجلد**: يظهر مجلد الحفظ
- ✅ **معالجة أخطاء**: رسائل واضحة

### 📊 **صفحة إدارة الملفات**
- ✅ **عرض منظم**: بطاقات للملفات
- ✅ **فلترة**: حسب النوع (منتجات، أصناف، بنرات)
- ✅ **عمليات**: عرض، تحميل، حذف
- ✅ **معلومات مفصلة**: الحجم، التاريخ، النوع

## 🛠️ **كيفية الاستخدام / How to Use**

### **1. رفع صورة منتج / Upload Product Image**

```tsx
import { ImageUploadFolder } from '@/components/ui/image-upload-folder';

<ImageUploadFolder
  value={imageUrl}
  onChange={setImageUrl}
  type="products"
  label="صورة المنتج"
/>
```

**النتيجة**:
- الصورة تُحفظ في: `/public/uploads/products/`
- الرابط: `/uploads/products/1703123456789_abc123def.jpg`

### **2. رفع صورة صنف / Upload Category Image**

```tsx
<ImageUploadFolder
  value={imageUrl}
  onChange={setImageUrl}
  type="categories"
  label="صورة الصنف"
/>
```

**النتيجة**:
- الصورة تُحفظ في: `/public/uploads/categories/`
- الرابط: `/uploads/categories/1703123456790_def456ghi.png`

### **3. رفع صورة بنر / Upload Banner Image**

```tsx
<ImageUploadFolder
  value={imageUrl}
  onChange={setImageUrl}
  type="banners"
  label="صورة البنر"
/>
```

**النتيجة**:
- الصورة تُحفظ في: `/public/uploads/banners/`
- الرابط: `/uploads/banners/1703123456791_ghi789jkl.webp`

## 🔧 **التفاصيل التقنية / Technical Details**

### **API Endpoint المحسن**
```
POST /api/upload
```

**المعاملات**:
- `file`: الملف المراد رفعه
- `type`: نوع المجلد (`products`, `categories`, `banners`)

**الاستجابة**:
```json
{
  "success": true,
  "imageUrl": "/uploads/products/1703123456789_abc123def.jpg",
  "fileName": "1703123456789_abc123def.jpg",
  "fileSize": 245760,
  "fileType": "image/jpeg",
  "uploadPath": "/full/path/to/file",
  "message": "تم رفع الصورة بنجاح"
}
```

### **دالة إنشاء اسم فريد**
```javascript
function generateUniqueFileName(originalName) {
  const timestamp = Date.now();
  const randomString = crypto.randomBytes(8).toString('hex');
  const extension = originalName.split('.').pop() || 'jpg';
  return `${timestamp}_${randomString}.${extension}`;
}
```

### **التحقق من الملفات**
```javascript
// الأنواع المدعومة
const allowedTypes = ['image/jpeg', 'image/jpg', 'image/png', 'image/webp', 'image/gif'];

// الحد الأقصى للحجم
const maxSize = 10 * 1024 * 1024; // 10MB
```

## 📊 **أمثلة عملية / Practical Examples**

### **مثال 1: رفع صورة منتج**
```javascript
// الملف الأصلي: "coffee-image.jpg"
// اسم الملف الجديد: "1703123456789_a1b2c3d4.jpg"
// المسار الكامل: "/public/uploads/products/1703123456789_a1b2c3d4.jpg"
// الرابط: "/uploads/products/1703123456789_a1b2c3d4.jpg"
```

### **مثال 2: رفع صورة صنف**
```javascript
// الملف الأصلي: "category-banner.png"
// اسم الملف الجديد: "1703123456790_e5f6g7h8.png"
// المسار الكامل: "/public/uploads/categories/1703123456790_e5f6g7h8.png"
// الرابط: "/uploads/categories/1703123456790_e5f6g7h8.png"
```

### **مثال 3: استخدام الصورة في HTML**
```html
<img src="/uploads/products/1703123456789_a1b2c3d4.jpg" alt="صورة المنتج" />
```

## 🎨 **واجهة المكون / Component Interface**

### **خصائص ImageUploadFolder**
```typescript
interface ImageUploadFolderProps {
  value?: string;                    // رابط الصورة الحالي
  onChange: (imageUrl: string) => void; // دالة التحديث
  type?: 'products' | 'categories' | 'banners'; // نوع المجلد
  label?: string;                    // تسمية المكون
  className?: string;                // فئات CSS إضافية
  accept?: string;                   // أنواع الملفات المقبولة
}
```

### **الميزات التفاعلية**
- 🖱️ **النقر**: لاختيار الملف
- 🖐️ **السحب والإفلات**: Drag & Drop
- 👁️ **المعاينة**: عرض فوري للصورة
- 📊 **المعلومات**: تفاصيل الملف
- 🗑️ **الحذف**: إزالة الصورة

## 📱 **صفحة إدارة الملفات / File Management Page**

### **الوصول**
```
/ar/admin/uploads
```

### **الميزات**
- 📋 **عرض جميع الملفات**: في بطاقات منظمة
- 🔍 **فلترة**: حسب النوع (الكل، منتجات، أصناف، بنرات)
- 👁️ **عرض**: فتح الصورة في نافذة جديدة
- 💾 **تحميل**: تنزيل الملف
- 🗑️ **حذف**: إزالة الملف

### **معلومات الملف**
- 📝 **الاسم**: اسم الملف الفريد
- 📏 **الحجم**: بالبايت، KB، MB
- 🎨 **النوع**: JPEG, PNG, WebP, GIF
- 📅 **التاريخ**: تاريخ ووقت الرفع
- 📁 **الفئة**: منتجات، أصناف، بنرات

## 🚀 **المميزات / Advantages**

### ✅ **الأداء**
- **سرعة عالية**: ملفات محلية
- **لا توجد قيود خارجية**: لا يعتمد على خدمات خارجية
- **وصول مباشر**: روابط مباشرة للصور

### ✅ **التنظيم**
- **مجلدات منفصلة**: لكل نوع مجلد خاص
- **أسماء فريدة**: لا تعارض في الأسماء
- **سهولة الإدارة**: واجهة بديهية

### ✅ **الأمان**
- **تحقق من النوع**: فقط الصور المدعومة
- **حد أقصى للحجم**: منع الملفات الكبيرة
- **أسماء آمنة**: لا تعارض مع ملفات النظام

### ✅ **المرونة**
- **أنواع متعددة**: دعم جميع أنواع الصور الشائعة
- **قابل للتوسع**: سهل إضافة أنواع جديدة
- **قابل للتخصيص**: واجهة قابلة للتعديل

## 🔄 **الصيانة / Maintenance**

### **تنظيف الملفات غير المستخدمة**
```javascript
// يمكن إضافة API لحذف الملفات غير المرتبطة بالمنتجات
POST /api/admin/cleanup-unused-files
```

### **نسخ احتياطي**
```bash
# نسخ احتياطي لمجلد الرفع
cp -r public/uploads/ backup/uploads-$(date +%Y%m%d)/
```

### **مراقبة المساحة**
```bash
# فحص حجم مجلد الرفع
du -sh public/uploads/
```

## 🎊 **الخلاصة / Summary**

تم إنشاء نظام رفع صور متكامل يوفر:

### **✅ للمطور**
- 🔧 **API محسن** مع معالجة شاملة للأخطاء
- 🎨 **مكونات جاهزة** للاستخدام المباشر
- 📊 **واجهة إدارة** متطورة للملفات
- 📁 **تنظيم ممتاز** للملفات والمجلدات

### **✅ للمستخدم**
- 🖱️ **سهولة الاستخدام** مع السحب والإفلات
- 👁️ **معاينة فورية** للصور المرفوعة
- 📊 **معلومات مفصلة** عن كل ملف
- ⚡ **سرعة عالية** في الرفع والعرض

### **✅ للنظام**
- 📁 **تنظيم محكم** للملفات
- 🔒 **أمان عالي** مع التحقق من الأنواع
- 🚀 **أداء ممتاز** مع الملفات المحلية
- 🔧 **سهولة الصيانة** والإدارة

**النظام جاهز للاستخدام الإنتاجي!** 🎉

## 🔗 **الملفات المرتبطة / Related Files**

- `src/app/api/upload/route.ts` - API الرفع المحسن
- `src/components/ui/image-upload-folder.tsx` - مكون الرفع
- `src/app/[locale]/admin/uploads/page.tsx` - صفحة إدارة الملفات
- `public/uploads/` - مجلدات الصور
