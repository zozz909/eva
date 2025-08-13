# دليل دعم متعدد اللغات
# Multilingual Support Guide

## 🎯 **نظرة عامة / Overview**

تم إضافة دعم كامل متعدد اللغات للموقع، حيث يمكن الآن عرض المحتوى بالعربية والإنجليزية مع ترجمة أسماء المنتجات والأصناف والبنرات.
Full multilingual support has been added to the website, allowing content to be displayed in both Arabic and English with translation of product names, categories, and banners.

## ✨ **الميزات الجديدة / New Features**

### 🌍 **دعم اللغتين**
- **العربية**: اللغة الافتراضية مع دعم RTL
- **الإنجليزية**: ترجمة كاملة مع دعم LTR

### 🗄️ **قاعدة البيانات متعددة اللغات**
- حقول ترجمة إنجليزية لجميع المحتويات
- ترجمات تلقائية ذكية للمحتوى الموجود
- فهارس محسنة للأداء

### 🎨 **واجهة إدارة محسنة**
- حقول منفصلة للعربية والإنجليزية
- إدخال سهل للترجمات
- معاينة فورية للمحتوى

## 🗄️ **تحديثات قاعدة البيانات / Database Updates**

### **الجداول المحدثة / Updated Tables**

#### **جدول الأصناف / Categories Table**
```sql
ALTER TABLE categories 
ADD COLUMN name_en VARCHAR(255) NULL,
ADD COLUMN description_en TEXT NULL;
```

#### **جدول المنتجات / Products Table**
```sql
ALTER TABLE products 
ADD COLUMN name_en VARCHAR(255) NULL,
ADD COLUMN description_en TEXT NULL;
```

#### **جدول البنرات / Promotions Table**
```sql
ALTER TABLE promotions 
ADD COLUMN title_en VARCHAR(255) NULL,
ADD COLUMN description_en TEXT NULL;
```

### **الفهارس الجديدة / New Indexes**
```sql
CREATE INDEX idx_categories_name_en ON categories(name_en);
CREATE INDEX idx_products_name_en ON products(name_en);
CREATE INDEX idx_promotions_title_en ON promotions(title_en);
```

## 🔄 **آلية العمل / How It Works**

### **منطق اختيار اللغة / Language Selection Logic**
```javascript
// للمنتجات
name: locale === 'en' && product.name_en ? product.name_en : product.name

// للأصناف  
name: locale === 'en' && category.name_en ? category.name_en : category.name

// للبنرات
title: locale === 'en' && promotion.title_en ? promotion.title_en : promotion.title
```

### **الأولوية / Priority**
1. **إذا كانت اللغة إنجليزية** والترجمة موجودة → عرض الترجمة
2. **إذا لم تكن الترجمة موجودة** → عرض النص العربي
3. **إذا كانت اللغة عربية** → عرض النص العربي دائماً

## 🛠️ **كيفية الاستخدام / How to Use**

### 1. **إضافة منتج جديد / Adding New Product**

1. اذهب إلى **لوحة التحكم → المنتجات**
2. اضغط **"إضافة منتج"**
3. املأ الحقول:
   - **الاسم (عربي)**: مطلوب
   - **الاسم (إنجليزي)**: اختياري
   - **الوصف (عربي)**: اختياري
   - **الوصف (إنجليزي)**: اختياري
4. احفظ المنتج

### 2. **تعديل منتج موجود / Editing Existing Product**

1. اضغط **"تعديل"** على المنتج
2. أضف أو عدّل الترجمات الإنجليزية
3. احفظ التغييرات

### 3. **عرض المحتوى / Viewing Content**

- **العربية**: `http://localhost:9002/ar`
- **الإنجليزية**: `http://localhost:9002/en`
- **تبديل اللغة**: استخدم مبدل اللغة في الزاوية

## 📊 **أمثلة الترجمات التلقائية / Auto-Translation Examples**

### **الأصناف / Categories**
| العربية | الإنجليزية |
|---------|------------|
| مقبلات | Appetizers |
| الأطباق الرئيسية | Main Dishes |
| حلويات | Desserts |
| مشروبات | Beverages |
| قهوة | Coffee |
| عصائر | Juices |

### **المنتجات / Products**
| العربية | الإنجليزية |
|---------|------------|
| قهوة عربية | Coffee عربية |
| شاي أخضر | Tea أخضر |
| عصير برتقال | Juice برتقال |
| كيك شوكولاتة | Cake شوكولاتة |
| بيتزا مارجريتا | Pizza مارجريتا |

## 🎨 **واجهة الإدارة الجديدة / New Admin Interface**

### **حقول الترجمة / Translation Fields**
- **الاسم (عربي)**: مطلوب، النص الأساسي
- **الاسم (إنجليزي)**: اختياري، يظهر في النسخة الإنجليزية
- **الوصف (عربي)**: اختياري، الوصف الأساسي
- **الوصف (إنجليزي)**: اختياري، يظهر في النسخة الإنجليزية

### **نصائح للاستخدام / Usage Tips**
1. **املأ النص العربي دائماً** (مطلوب)
2. **أضف الترجمة الإنجليزية** لتحسين تجربة المستخدمين الأجانب
3. **اتركها فارغة** إذا لم تكن الترجمة متاحة (سيظهر النص العربي)

## 🔧 **التفاصيل التقنية / Technical Details**

### **API Endpoints المحدثة / Updated API Endpoints**

#### **إنشاء منتج / Create Product**
```json
POST /api/products
{
  "name": "قهوة عربية",
  "name_en": "Arabic Coffee",
  "description": "قهوة عربية أصيلة",
  "description_en": "Authentic Arabic Coffee"
}
```

#### **تحديث منتج / Update Product**
```json
PUT /api/products/[id]
{
  "name_en": "Updated English Name",
  "description_en": "Updated English Description"
}
```

### **دوال التحويل / Transform Functions**
```javascript
// تحويل المنتج مع اللغة
transformProductForOldFormat(product, locale)

// تحويل الصنف مع اللغة  
transformCategoryForOldFormat(category, locale)

// تحويل البنر مع اللغة
transformPromotionForOldFormat(promotion, locale)
```

## 🚀 **المميزات / Features**

### ✅ **ما يعمل الآن / What Works Now**
- ✅ تبديل اللغة فوري
- ✅ ترجمة أسماء المنتجات
- ✅ ترجمة أسماء الأصناف
- ✅ ترجمة البنرات الترويجية
- ✅ دعم RTL/LTR تلقائي
- ✅ واجهة إدارة محسنة
- ✅ ترجمات تلقائية ذكية

### 🔄 **التوافق مع النظام القديم / Backward Compatibility**
- ✅ المحتوى الموجود يعمل بشكل طبيعي
- ✅ لا حاجة لتعديل البيانات الموجودة
- ✅ الترجمات اختيارية
- ✅ النظام يعمل بدون ترجمات

## 📝 **أمثلة عملية / Practical Examples**

### **مثال 1: منتج بترجمة كاملة**
```json
{
  "name": "قهوة عربية مميزة",
  "name_en": "Premium Arabic Coffee",
  "description": "قهوة عربية أصيلة بنكهة مميزة",
  "description_en": "Authentic Arabic coffee with premium flavor"
}
```
**النتيجة**: 
- العربية: "قهوة عربية مميزة"
- الإنجليزية: "Premium Arabic Coffee"

### **مثال 2: منتج بدون ترجمة**
```json
{
  "name": "قهوة تركية",
  "name_en": null,
  "description": "قهوة تركية أصيلة",
  "description_en": null
}
```
**النتيجة**:
- العربية: "قهوة تركية"
- الإنجليزية: "قهوة تركية" (يظهر النص العربي)

## 🎊 **الخلاصة / Summary**

تم تطبيق دعم متعدد اللغات بنجاح مع:
- ✅ **قاعدة بيانات محسنة** مع حقول الترجمة
- ✅ **واجهة إدارة سهلة** لإدخال الترجمات
- ✅ **عرض ذكي** للمحتوى حسب اللغة
- ✅ **ترجمات تلقائية** للمحتوى الموجود
- ✅ **أداء محسن** مع فهارس جديدة
- ✅ **توافق كامل** مع النظام الموجود

الآن يمكن للموقع خدمة العملاء بالعربية والإنجليزية بسلاسة! 🌟
