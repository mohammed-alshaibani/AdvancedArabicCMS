# نظام إدارة المحتوى المتقدم (Advanced Arabic CMS)

## 📋 نظرة عامة

**نظام إدارة المحتوى المتقدم** هو نظام شامل لإدارة المحتوى مبني على Laravel مع ميزات متقدمة للملفات والإشعارات والتحقق من النماذج. النظام مصمم لتوفير حل متكامل لإدارة المواقع والتطبيقات الويب باللغة العربية.

### ✨ المميزات الرئيسية

- 📁 **نظام إدارة ملفات متقدم** مع السحب والإفلات
- 🔔 **نظام إشعارات شامل** مع دعم البريد الإلكتروني
- ✅ **نظام تحقق متقدم** للنماذج
- 📝 **محرر نصوص** مع استكشاف الملفات
- 🖼️ **معرض صور متقدم** باستخدام Fancybox
- 🎨 **واجهة مستخدم حديثة** ومتجاوبة
- 🔐 **نظام مصادقة آمن**
- 📊 **لوحة تحكم شاملة**

## 🛠️ التقنيات المستخدمة

| التقنية | الإصدار | الوصف |
|---------|---------|-------|
| **Laravel** | 9.x | إطار العمل PHP |
| **jQuery** | Latest | مكتبة JavaScript |
| **Bootstrap** | 5.x | إطار العمل CSS |
| **TinyMCE** | Latest | محرر النصوص |
| **DataTables** | Latest | جداول البيانات |
| **Flatpickr** | Latest | منتقي التواريخ |
| **Toastr** | Latest | الإشعارات |

## 🚀 البدء السريع

### المتطلبات الأساسية

- PHP 8.0+
- Composer
- MySQL/MariaDB
- Web Server (Apache/Nginx)

### خطوات التثبيت

1. **استنساخ المستودع**
   ```bash
   git clone <repository-url>
   cd dashboard-master
   ```

2. **تثبيت الاعتماديات**
   ```bash
   composer install
   ```

3. **إعداد ملف البيئة**
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```

4. **تكوين قاعدة البيانات**
   ```env
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=cms_database
   DB_USERNAME=your_username
   DB_PASSWORD=your_password
   ```

5. **تشغيل الهجرات**
   ```bash
   php artisan migrate
   php artisan db:seed
   ```

6. **تشغيل الخادم**
   ```bash
   php artisan serve
   ```

## 📁 هيكل المشروع

```
dashboard-master/
├── app/
│   ├── Http/
│   │   ├── Controllers/
│   │   └── Middleware/
│   ├── Models/
│   └── Providers/
├── database/
│   ├── migrations/
│   └── seeders/
├── resources/
│   ├── views/
│   │   ├── admin/
│   │   └── templates/
│   ├── js/
│   └── css/
├── public/
│   ├── uploads/
│   └── js/
├── routes/
│   ├── web.php
│   └── api.php
└── .env.example
```

## 🔧 المميزات المتقدمة

### 📁 نظام إدارة الملفات

#### رفع الملفات
```php
# رفع ملف واحد
$this->store_file([
    'source' => $request->file,
    'validation' => "image",
    'path_to_save' => '/uploads/users/',
    'type' => 'AVATAR',
    'user_id' => \Auth::user()->id,
    'resize' => [500, 3000],
    'small_path' => 'small/',
    'visibility' => 'PUBLIC',
    'file_system_type' => env('FILESYSTEM_DRIVER'),
    'watermark' => true,
    'optimize' => true,
])['filename'];
```

#### استخدام الملفات
```php
# استخدام ملف محدد
$this->use_hub_file('file_name', 'type_id', 'user_id');

# استخدام ملفات متعددة
$uploaded_files = json_decode($request["fileuploader-list-attachment"]);
$attachments = [];
foreach($uploaded_files as $uploaded_file) {
    array_push($attachments, $uploaded_file->file);
}
foreach($attachments as $attachment) {
    $this->use_hub_file($attachment, $item->id, auth()->user()->id);
}
```

#### حذف الملفات
```php
# حذف ملف
$this->remove_hub_file('file_name');
```

### 🎯 خاصية السحب والإفلات (Drag and Drop)

#### للرفع الفردي
```blade
@include('admin.templates.dropzone',[
    'selector' => '#file-uploader-single',
    'url' => route('admin.upload.file'),
    'method' => 'POST',
    'remove_url' => route('admin.upload.remove-file'),
    'remove_method' => 'POST',
    'enable_selector_after_upload' => '#submitForm',
    'max_files' => 1,
    'max_file_size' => '50',
    'accepted_files' => "['image/*']"
])
```

#### للرفع المتعدد
```blade
@include('admin.templates.dropzone',[
    'selector' => '#file-uploader-multiple',
    'url' => route('admin.upload.file'),
    'method' => 'POST',
    'remove_url' => route('admin.upload.remove-file'),
    'remove_method' => 'POST',
    'enable_selector_after_upload' => '#submitForm',
    'max_files' => 100,
    'max_file_size' => '50',
    'accepted_files' => "['image/*', 'application/pdf']"
])
```

### 📝 محرر النصوص المتقدم

#### محرر مع استكشاف الملفات
```blade
<textarea name="description" class="form-control editor with-file-explorer" 
          required minlength="3" maxlength="10000"></textarea>
```

#### محرر عادي
```blade
<textarea name="description" class="form-control editor" 
          required minlength="3" maxlength="10000"></textarea>
```

### 🖼️ معرض الصور (Fancybox)

#### استخدام الصور المنفردة
```html
<img src="image.jpg" data-fancybox />
```

#### استخدام المعرض
```html
<div class="fancybox">
    <img src="image1.jpg" />
    <img src="image2.jpg" />
    <img src="image3.jpg" />
</div>
```

### ✅ التحقق من النماذج

#### تفعيل التحقق التلقائي
```html
<form id="validate-form">
    <!-- حقول النموذج هنا -->
</form>
```

#### أو استخدام مخصص
```html
<form id="custom-validation"></form>

@section('scripts')
<script type="text/javascript">
    $("#custom-validation").validate();
</script>
@endsection
```

## 🔔 نظام الإشعارات

### الإشعارات في الخادم
```php
// الوثائق: https://github.com/mckenziearts/laravel-notify

notify()->info('المحتوى', 'العنوان');
notify()->success('تم الحفظ بنجاح', 'نجاح');
notify()->error('حدث خطأ', 'خطأ');
```

### الإشعارات في الواجهة الأمامية
```javascript
// الوثائق: https://github.com/CodeSeven/toastr

// عرض إشعار تحذير
toastr.warning('رسالة تحذير');

// عرض إشعار نجاح مع عنوان
toastr.success('تمت العملية بنجاح!', 'نجاح');

// عرض إشعار خطأ
toastr.error('حدث خطأ ما', 'خطأ');

// إزالة الإشعارات الحالية
toastr.remove();

// مسح جميع الإشعارات
toastr.clear();

// إشعار مخصص
toastr.success('رسالة مخصصة', 'عنوان مخصص', {
    timeOut: 5000,
    closeButton: true,
    progressBar: true
});
```

### الإشعارات المتعددة القنوات
```php
(new \MainHelper)->notify_user([
    'user_id' => 2,
    'message' => "محتوى الإشعار",
    'url' => "http://example.com",
    'methods' => ['database', 'mail']
]);
```

## ⚙️ إعدادات النظام

### إعدادات .env
```env
# نظام الملفات
FILESYSTEM_DRIVER=local
STORAGE_BASE=/storage
STORAGE_URL="${STORAGE_BASE}"

# الصور الافتراضية
DEFAULT_IMAGE="${APP_URL}/images/default/image.jpg"
DEFAULT_IMAGE_FAVICON="${APP_URL}/images/default/favicon.png"
DEFAULT_IMAGE_AVATAR="${APP_URL}/images/default/avatar.png"
DEFAULT_IMAGE_LOGO="${APP_URL}/images/default/logo.png"
DEFAULT_IMAGE_WIDELOGO="${APP_URL}/images/default/wide-logo.png"
DEFAULT_IMAGE_COVER="${APP_URL}/images/default/cover.png"
DEFAULT_IMAGE_NOTIFICATION="${APP_URL}/images/default/notification.png"

# البريد الإلكتروني الافتراضي
DEFAULT_EMAIL=admin@admin.com
DEFAULT_PASSWORD=password
```

## 🎨 الأقسام الرئيسية في القوالب

### هيكل الصفحة
```blade
@yield('styles')
@yield('content')
@yield('after-body')
@yield('scripts')
```

## 📊 جداول البيانات (DataTables)

### التكوين الأساسي
```javascript
$('#datatable').DataTable({
    language: {
        url: '//cdn.datatables.net/plug-ins/1.10.24/i18n/Arabic.json'
    },
    responsive: true,
    pageLength: 25,
    order: [[0, 'desc']]
});
```

## 📅 منتقي التواريخ (Flatpickr)

### التكوين الأساسي
```javascript
flatpickr("#date-picker", {
    locale: "ar",
    dateFormat: "Y-m-d",
    minDate: "today",
    maxDate: new Date().fp_incr(365) // سنة من الآن
});
```

## 🔐 التحكم في الوصول للملفات

### إعدادات الأمان
```php
// في ملف config/filesystems.php
'public' => [
    'driver' => 'local',
    'root' => storage_path('app/public'),
    'url' => env('APP_URL').'/storage',
    'visibility' => 'public',
],
```

## 🚀 أوامر Artisan المتاحة

### أوامر التطوير
```bash
# تشغيل الخادم
php artisan serve

# مسح الكاش
php artisan cache:clear
php artisan config:clear
php artisan view:clear

# إنشاء تحكم
php artisan make:controller AdminController

# إنشاء نموذج
php artisan make:model Post -m

# تشغيل الهجرات
php artisan migrate
php artisan migrate:fresh --seed
```

### أوامر قائمة الانتظار
```bash
# تشغيل عامل قائمة الانتظار
php artisan queue:work

# تشغيل المهام المجدولة
php artisan schedule:run
```

## 📱 بيانات الاعتماد الافتراضية

```
صفحة تسجيل الدخول: http://127.0.0.1:8000/login
البريد الإلكتروني: admin@admin.com
كلمة المرور: password
```

## 🧪 الاختبار

### تشغيل الاختبارات
```bash
# جميع الاختبارات
php artisan test

# اختبارات معينة
php artisan test --filter AdminTest

# تغطية الكود
php artisan test --coverage
```

## 📄 الترخيص

هذا المشروع مرخص تحت رخصة MIT.

## 🤝 المساهمة

نرحب بمساهماتكم! يرجى اتباع الخطوات التالية:

1. قم بعمل Fork للمشروع
2. أنشئ فرعًا جديدًا: `git checkout -b feature/amazing-feature`
3. قم بالتغييرات
4. ادفع الفرع: `git push origin feature/amazing-feature`
5. افتح Pull Request

## 🆘 الدعم

لأي استفسارات أو مساعدة:
- 📝 افتح مشكلة (issue) جديدة
- 📧 تواصل مع فريق الدعم
- 📖 راجع الوثائق التقنية

## 🔄 التحديثات المستقبلية

- 🚀 دعم React و Vue.js
- 📱 تطبيق جوال أصلي
- 🔔 نظام إشعارات فوري
- 📊 تقارير متقدمة
- 🌍 دعم لغات إضافية

---

**مبني بـ ❤️ باستخدام [Laravel](https://laravel.com/)**
