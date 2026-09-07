# عرّاب — Mac On-prem Delivery Kit

هذه الحزمة مخصصة لتشغيل النسخة التجارية المملوكة من عرّاب على `macOS` عبر `Docker Desktop`.

## المسار الأبسط للمشتري

1. فك الحزمة في أي مجلد.
2. افتح `0-setup-arrab.command`.
3. إذا ظهر تنبيه أمان من macOS، اضغط `تم` ثم من Finder اضغط بزر الفأرة الأيمن على `0-setup-arrab.command` واختر `Open` ثم `Open` مرة ثانية.
4. انتظر حتى تظهر العبارة `ARRAB_MAC_BOOTSTRAP_OK`.
5. سيفتح عرّاب على `http://127.0.0.1:8080` افتراضيًا.

## ما الذي تحتاجه الحزمة

- `Docker Desktop for Mac` مثبت ويعمل.
- اتصال إنترنت عند أول build لصورة `arrab-web` إذا لم تكن موجودة محليًا.
- مساحة قرص كافية للحزمة وبيانات MariaDB.
- يمكن تغيير المنفذ الافتراضي `8080` إذا كان مستخدمًا مسبقًا.

## المسار الفني

### فحص الجاهزية

```bash
sh ./deploy/macos/preflight-arrab.sh
```

أو مع فحص الحزمة ووجهة التركيب:

```bash
sh ./deploy/macos/preflight-arrab.sh \
  --bundle-zip ./arrab-release.zip \
  --install-dir "$HOME/Applications/Arrab"
```

### التركيب

```bash
sh ./deploy/macos/install-arrab.sh \
  --bundle-zip ./arrab-release.zip \
  --install-dir "$HOME/Applications/Arrab"
```

## ملاحظات

- هذا المسار يجعل النسخة بعد التثبيت مستقلة عن موقع عرّاب وCursor.
- التشغيل محلي على جهاز الماك عبر `Docker Desktop`.
- الفروع تعمل على نفس النسخة المركزية داخل الجهاز/الخادم. الأوفلاين الكامل للفروع ليس ضمن هذه الحزمة.
