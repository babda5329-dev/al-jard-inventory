# مشروع تطبيق الجرد المخزني (Al-Jard Al-Makhzani) - Native Android

## نوع المشروع وحزم التقنيات (Technology Stack)
هذا المشروع هو **تطبيق أندرويد أصيل بالكامل (100% Native Android Application)** ومبني وفق أحدث معايير ومكتبات أندرويد الرسمية:

- **لغة البرمجة الأساسية**: Kotlin (100%)
- **واجهة المستخدم**: Jetpack Compose مع دعم كامل للغة العربية والاتجاه من اليمين لليسار (Full Arabic RTL Support) وتصميم Material Design 3 (M3).
- **قاعدة البيانات والتخزين المحلي**: Android Room Database (SQLite) تعمل 100% بدون إنترنت (Offline-First) مع استمرار وحفظ تلقائي للبيانات.
- **تعدد جلسات الجرد المستقلة**: دعم إنشاء وحفظ والتبديل بين جلسات جرد متعددة بشكل منفصل.
- **تصدير واستيراد قاعدة البيانات (Backup & Restore)**: تصدير واستيراد كامل لقاعدة بيانات Room بتنسيق JSON مع دعم مشاركة الملف أو اختياره عبر Android Storage Access Framework.
- **تصدير التقارير**: إنشاء تقارير PDF احترافية باللغة العربية عبر `android.graphics.pdf.PdfDocument` ومشاركتها مباشرة مع التطبيقات الأخرى عبر `FileProvider`.
- **نظام البناء**: Gradle (Kotlin DSL - `build.gradle.kts`) مع `gradle/libs.versions.toml` Version Catalog.

---

## كيفية تصدير المشروع من منصة AI Studio
1. يمكنك تحميل المشروع كاملاً كملف مضغوط (`ZIP`) عبر قائمة الإعدادات (Settings / Project Options) في واجهة Google AI Studio ثم اختيار **Export as ZIP** أو المزامنة مع **GitHub**.
2. كما تتيح المنصة توليد ملف APK مباشرة من خيارات الإعدادات (Generate APK).

---

## متطلبات التشغيل والبناء المحلي
- **نظام التشغيل**: Windows أو macOS أو Linux
- **حزمة تطوير جافا**: JDK 17 أو أحدث
- **بيئة التطوير**: Android Studio (الإصدارات الحديثة مثل Jellyfish, Koala, Ladybug أو أحدث)
- **Android SDK**: Compile SDK 35، Target SDK 35، Min SDK 24 (يدعم أندرويد 7.0 وما فوق حتى أحدث إصدارات أندرويد).

---

## خطوات فتح المشروع وبناء ملف APK في Android Studio

### 1. فتح المشروع في Android Studio
1. فك ضغط مجلد المشروع على جهاز الكمبيوتر.
2. افتح برنامج **Android Studio**.
3. اختر **Open** ثم حدد المجلد الرئيسي للمشروع (المجلد الذي يحتوي على `settings.gradle.kts` و `build.gradle.kts`).
4. سيبدأ Android Studio تلقائياً بعملية مزامنة الـ Gradle (`Gradle Sync`). انتظر حتى تكتمل العملية بنجاح.

### 2. بناء ملف APK للتجربة والتثبيت (Debug APK)
- **من خلال واجهة Android Studio**:
  1. من القائمة العلوية اختر **Build** > **Build Bundle(s) / APK(s)** > **Build APK(s)**.
  2. بعد اكتمال البناء، ستظهر رسالة في الزاوية السفلية تتيح لك الضغط على **locate** لفتح مجلد ملف الـ APK.
  3. ستجد ملف الـ APK في المسار:
     `app/build/outputs/apk/debug/app-debug.apk`
  4. يمكنك نقله مباشرة إلى هاتفك المحمول عبر كابل USB أو تطبيق WhatsApp/Telegram وتثبيته.

- **أو من خلال سطر الأوامر (Terminal)**:
  - على نظام Windows:
    ```cmd
    gradlew.bat assembleDebug
    ```
  - على نظام Mac / Linux:
    ```bash
    ./gradlew assembleDebug
    ```

### 3. بناء ملف APK نهائي موقع (Signed Release APK)
1. من القائمة العلوية في Android Studio اختر:
   **Build** > **Generate Signed Bundle / APK...**
2. اختر **APK** ثم اضغط **Next**.
3. حدد مفتاح التوقيع (Keystore) الخاص بك، أو أنشئ مفتاحاً جديداً (Create new...).
4. اختر نوع البناء **release** وحدد توقيع **V1 (Jar Signature)** و **V2 (Full APK Signature)**.
5. اضغط **Finish** وسيتم إنشاء ملف الـ APK الجاهز للنشر في مجلد `app/release/`.

---

## الهيكل العام للمشروع (Project Structure)
```
├── app/
│   ├── build.gradle.kts              # إعدادات واجهات ومكتبات التطبيق
│   ├── proguard-rules.pro            # قواعد حماية وتقليص الكود
│   └── src/
│       ├── main/
│       │   ├── AndroidManifest.xml   # ملف بيان التطبيق مع الأذونات وإعدادات RTL
│       │   ├── java/com/example/
│       │   │   ├── MainActivity.kt   # نقطة انطلاق التطبيق مع إعدادات واجهة المستخدم
│       │   │   ├── data/
│       │   │   │   ├── local/        # قاعدة بيانات Room والـ DAO والـ Database
│       │   │   │   ├── model/        # نماذج بيانات الجلسات والبنود والنسخ الاحتياطي
│       │   │   │   └── repository/   # مستودع البيانات للتعامل مع العمليات المحلية
│       │   │   ├── ui/
│       │   │   │   ├── InventoryScreen.kt      # شاشة تفاصيل الجرد وإدخال البنود
│       │   │   │   ├── SessionsListScreen.kt   # شاشة إدارة الجلسات وتصدير/استيراد البيانات
│       │   │   │   ├── InventoryViewModel.kt   # إدارة حالة التطبيق (StateFlow/ViewModel)
│       │   │   │   ├── components/             # البطاقات والجداول ونوافذ الحوار
│       │   │   │   └── theme/                  # ألوان وخطوط وثيمات التطبيق
│       │   │   └── util/                       # مولد الـ PDF والنسخ الاحتياطي وتنسيق الأرقام
│       │   └── res/
│       │       ├── values/strings.xml          # اسم التطبيق "الجرد المخزني"
│       │       ├── font/cairo.ttf              # خط القاهرة العربي Cairo
│       │       ├── mipmap/                     # أيقونات التطبيق التكيفية بجميع الأحجام
│       │       └── xml/file_paths.xml          # إعدادات مشاركة الملفات الآمنة
├── gradle/
│   ├── libs.versions.toml            # كتالوج إصدارات المكتبات والمكونات
│   └── wrapper/                      # ملفات الـ Gradle Wrapper
├── build.gradle.kts                  # إعدادات المشروع العامة
├── settings.gradle.kts               # ربط الوحدات واسم المشروع
├── metadata.json                     # بيانات التطبيق على المنصة
└── README.md                         # دليل المشروع وتعليمات البناء والتثبيت
```


## بناء ملف APK عبر GitHub Actions

هذا المشروع تطبيق Android أصلي باستخدام Kotlin وJetpack Compose، وليس مشروع موقع ويب.
بعد رفع ملفات المشروع إلى مستودع GitHub، افتح تبويب **Actions** وشغّل سير العمل **Build Android APK** (أو انتظر تشغيله بعد رفع الملفات).
عند نجاح البناء، افتح نتيجة التشغيل ثم قسم **Artifacts** ونزّل الملف `al-jard-al-makhzani-debug-apk`. فك الضغط عن ملف Artifact للحصول على APK وتثبيته على هاتف Android.

ملاحظات:
- هذه الوصفة تبني نسخة **Debug APK** للتجربة والتثبيت الشخصي، وليست نسخة متجر Google Play الموقّعة بمفتاح إصدار خاص.
- يجب أن تكون بيئة البناء قادرة على تنزيل Gradle وAndroid SDK وحزم Maven.
- إذا فشل البناء، افتح سجل خطوة **Build debug APK** وانسخ رسالة الخطأ الأخيرة لتشخيصها.
