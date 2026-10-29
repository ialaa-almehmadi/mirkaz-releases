أبي أخلّي تطبيق «مِركاز» (sa.mirkaz.tv) الشاشة الرئيسية لتلفزيوني (Google TV / Android TV) عن طريق adb من هذا الكمبيوتر.
الكمبيوتر والتلفزيون على نفس الشبكة، وفعّلت «تصحيح الأخطاء اللاسلكي» في التلفزيون.

من صفحة «تصحيح الأخطاء اللاسلكي» في التلفزيون:
- عنوان الاتصال (IP:المنفذ): [اكتبه هنا]
- من «إقران الجهاز باستخدام رمز إقران»: عنوان الإقران [اكتبه هنا] والرمز [اكتبه هنا]

نفّذ بالترتيب، وكل خطوة تأكد من نتيجتها قبل اللي بعدها. إذا صار شي غير متوقع وقف واسألني:

1. إذا adb مو موجود، ثبّت Android SDK Platform-Tools الرسمية من جوجل.
2. `adb pair <عنوان الإقران> <الرمز>` ثم `adb connect <عنوان الاتصال>`.
3. إذا `sa.mirkaz.tv` مو مثبّت: نزّل
   https://github.com/ialaa-almehmadi/mirkaz-releases/releases/latest/download/Mirkaz.apk
   وثبّته بـ `adb install`.
4. قبل أي تغيير، احفظ في ملف على الكمبيوتر: نتيجة
   `cmd package resolve-activity --brief -a android.intent.action.MAIN -c android.intent.category.HOME`
   (هذي الواجهة الأصلية)، و`settings get secure screensaver_components`، و`pm list packages -d`.
5. نفّذ:
   - `cmd package set-home-activity sa.mirkaz.tv/.HomeActivity`
   - `settings put secure screensaver_components sa.mirkaz.tv/.Screensaver`
   - `appops set sa.mirkaz.tv SYSTEM_ALERT_WINDOW allow`
   - `appops set sa.mirkaz.tv SCHEDULE_EXACT_ALARM allow`
6. افحص مين حافظ شبكة الواي فاي: `dumpsys wifi | grep -o 'cname=[^ ]*' | sort -u`.
   إذا ظهر `com.google.android.tungsten.setupwraith` **لا تكمل**، وقل لي أنسى الشبكة من إعدادات التلفزيون وأرجع أتصل فيها،
   ثم أعد الفحص. تعطيله وهو حافظ الشبكة يمسح الواي فاي.
7. عطّل الواجهة الأصلية اللي حفظتها في الخطوة 4 بـ `pm disable-user --user 0 <الحزمة>`،
   وإذا موجود عطّل كمان `com.google.android.tungsten.setupwraith` (يفتح شاشة إعداد بدل الواجهة).
8. `input keyevent 3` ثم تأكد إن الظاهر `sa.mirkaz.tv/.HomeActivity`
   (`dumpsys activity activities | grep topResumedActivity`)، وإن الواي فاي متصل.
9. إذا فشلت أي خطوة: رجّع كل شي من الملف (`pm enable` للحزم، و`settings put` لشاشة التوقف الأصلية أو `settings delete`،
   و`cmd package set-home-activity` للواجهة الأصلية).

لا تحذف أي تطبيق، ولا تغيّر أي إعداد غير المذكور. وفي النهاية أعطني ملخص قصير وأوامر الرجوع.
