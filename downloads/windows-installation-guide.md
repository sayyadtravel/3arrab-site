# عرّاب — Windows On-prem Deployment Kit

هذه الحزمة أصبحت تحمل مسارين:

- `0-setup-arrab.cmd`: المسار الأبسط للعميل النهائي. يشغّل bootstrap واحدًا يحاول تجهيز `XAMPP` تلقائيًا ثم ينسخ عرّاب ويهيئ قاعدة البيانات وملفات الإعداد.
- `preflight-arrab.ps1` + `install-arrab.ps1`: مسار فني أدق للمزوّد أو الدعم.

## الهدف

- فحص جاهزية جهاز العميل قبل التثبيت
- توحيد مسار التسليم
- تقليل أخطاء البيئة قبل أول تشغيل

## الاستخدام

### للعميل النهائي

1. فك الحزمة في أي مجلد.
2. شغّل `0-setup-arrab.cmd`.
3. انتظر حتى تظهر عبارة `ARRAB_BOOTSTRAP_OK`.
4. سيوضع اختصار `Arrab` على سطح المكتب ويفتح البرنامج على `http://localhost/arrab`.

### للمزوّد/الدعم

1. افتح PowerShell بصلاحية مناسبة.
2. شغّل:

```powershell
powershell -ExecutionPolicy Bypass -File .\preflight-arrab.ps1
```

ويمكن تمرير الحزمة ووجهة التركيب للفحص الواقعي:

```powershell
powershell -ExecutionPolicy Bypass -File .\preflight-arrab.ps1 -BundleZip .\arrab-release.zip -InstallDir C:\Arrab
```

3. عالج أي بند يظهر بحالة `FAIL`.
4. بعد اكتمال الجاهزية:
   - أنشئ release bundle من جهة المزود:

```bash
php scripts/arrab_make_release_bundle.php 2026.09-commercial ./arrab-release.zip
```

   - انسخ `arrab-release.zip` إلى جهاز العميل
   - فك/ركّب الحزمة:

```powershell
powershell -ExecutionPolicy Bypass -File .\install-arrab.ps1 -BundleZip .\arrab-release.zip -InstallDir C:\Arrab
```

   - يمكن تمرير `-BundleMetaJson` إذا كان ملف الـ sidecar في مسار مختلف
   - إذا وُجد `arrab-release.zip.meta.json` بجانب الحزمة فسيُستخدم تلقائيًا للتحقق من `SHA256`
   - سينشئ المثبّت `install receipt` داخل `company/{id}/support/`

   - أو استخدم `bootstrap-arrab.ps1 -AutoInstallStack` لتجهيز `XAMPP` تلقائيًا عند غيابه
   - فعّل `ARRAB_PRODUCTION=1`
   - ارفع الترخيص من داخل `الإعدادات → الترخيص التجاري`
   - عند وجود ترخيص جاهز يمكن نسخه أثناء التثبيت عبر `-LicenseJson` و`-LicenseSig`

## ما تتحقق منه أداة preflight

- إصدار PowerShell
- مساحة التخزين المتاحة
- وجود OpenSSL
- وجود PHP
- وجود MariaDB/MySQL client
- وجود Apache service أو بديلها
- وجود الحزمة إن مُرّرت
- `SHA256` عند وجود ملف sidecar
- `arrab-release.json` وحقول العد الأساسية داخل الحزمة
- ملاءمة مساحة القرص لحجم الحزمة
- حالة `InstallDir` إن كان فارغًا أو تثبيت عرّاب قائمًا

## ملاحظات

- bootstrap الحالي يفترض ويندوز حديثًا يدعم `winget` إذا احتاج الجهاز تنزيل `XAMPP` تلقائيًا.
- الهدف في هذه المرحلة: **تسليم تجاري منضبط** مع مسار أقرب للعميل النهائي بدل الاعتماد على تجهيز البيئة يدويًا.
- الفروع تعمل على نفس الخادم المركزي. الأوفلاين الكامل للفروع ليس ضمن هذه الحزمة.
