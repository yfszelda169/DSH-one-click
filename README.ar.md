<div dir="rtl" align="right">

# مثبّت DeepSeek Harness لنظام Windows بنقرة واحدة

[中文](README.zh.md) | [English](README.en.md) | [Français](README.fr.md) | [Español](README.es.md) | [Русский](README.ru.md) | العربية

انقر نقراً مزدوجاً على **install-deepseek-harness.cmd** للتشغيل. هذا **مثبّت CMD مستقل في ملف واحد** ولا يحتاج إلى ملف .ps1 منفصل. يستخرج ملف CMD منطقه المضمّن إلى مجلد Windows المؤقت، ثم يشغّله باستخدام PowerShell المدمج، ويحذف الملف المؤقت بعد الانتهاء. يكتشف المثبّت لغة نظام Windows تلقائياً: الصينية أو الإنجليزية أو الفرنسية أو الإسبانية أو الروسية أو العربية. ويمكن فرض اللغة عبر المتغير **DSH_INSTALLER_LANG**، مثلاً:

    set DSH_INSTALLER_LANG=ar
    install-deepseek-harness.cmd

</div>

## بدء سريع

1. انقر نقراً مزدوجاً على **install-deepseek-harness.cmd**.
2. انتظر انتهاء تثبيت Node.js وGit وDeepSeek Harness.
3. سيشغّل المثبّت DeepSeek Harness مرة واحدة لفترة قصيرة لتهيئة ملف تعريف Desktop ثم يغلقه تلقائياً.
4. اختر الإضافات حسب الأرقام ثم اختر المهارات. أمثلة: 1 3 5 أو 1,3,5؛ all لتحديد الكل؛ 0 لعدم التثبيت.
5. تُجلب كل إضافة أو مهارة من GitHub أولاً. وعند انتهاء المهلة أو حدوث خطأ يُستخدم مرآة متاحة في الصين.
6. سيبدأ DeepSeek Harness تلقائياً بعد الانتهاء.
7. أدخل مفتاح DeepSeek API في DeepSeek Harness ضمن **Settings → Models**.

فحص دون تثبيت:

    install-deepseek-harness.cmd -CheckOnly -NoLaunch

## الخيارات

| الخيار | الوصف |
|---|---|
| -CheckOnly | فحص البيئة والشبكة فقط؛ دون تثبيت |
| -SkipPlugins | تخطي اختيار وتثبيت الإضافات/المهارات |
| -NoLaunch | عدم تشغيل DeepSeek Harness بعد التثبيت |
| -Force | فرض إعادة تثبيت تطبيق سطح المكتب |

## ما الذي يتم تثبيته

| المكوّن | المصدر / السلوك |
|---|---|
| تطبيق DeepSeek Harness الرسمي لنظام Windows | https://download.deepseek.com/desktop/dsh-latest-windows-x64.exe |
| Node.js 24+ | يتطلب >=24.9.0؛ WinGet LTS أولاً ثم حزمة ZIP الرسمية. قد يطلب WinGet صلاحيات UAC |
| Git for Windows | WinGet أولاً؛ وفشله لا يوقف العملية الأساسية. قد يطلب WinGet صلاحيات UAC |
| WinGet | يُستخدم إن وُجد؛ وإلا يستخدم Node.js حزمة ZIP الرسمية |

يُستخدم Node.js من قبل find-skills وبيئة DSH CLI. لا يلزم Python للتثبيت الأساسي، والإضافات الاختيارية لا تتطلب Python نظام أيضاً.

## المسارات الافتراضية

| المحتوى | المسار |
|---|---|
| DeepSeek Harness | %LOCALAPPDATA%\Programs\DeepSeek Harness |
| DSH_HOME | %USERPROFILE%\.dsh |
| المهارات | %USERPROFILE%\.dsh\skills |
| سجل المثبّت | %LOCALAPPDATA%\DeepSeekHarnessInstaller |

إذا تم تعيين DSH_HOME فسيحترمه المثبّت.

## إضافات اختيارية (10 إضافات شائعة، تحديد متعدد)

تُثبّت الإضافات في ملف تعريف Desktop عبر الأمر المضمّن **dsh plugin --profile desktop add**. يعرض المثبّت الإضافات العشر التالية ولا يثبّتها كلها افتراضياً. تم التحقق من التركيبة مع DSH **0.2.2-rc.1** في DSH_HOME مؤقت وباستخدام --dump-config.

| # | الإضافة | الإصدار | النجوم | الغرض | GitHub |
|---:|---|---:|---:|---|---|
| 1 | dshmarket | 1.73.15 | 10.3k | سوق الإضافات المرئي | https://github.com/dsh-market/dsh-market |
| 2 | dsh-mnemon | 0.5.42 | 912 | ذاكرة متعددة الطبقات وإدارة السياق | https://github.com/omdsh-dev/dsh-mnemon |
| 3 | superpowers-dsh | 0.4.0 | 135 | مهارات TDD والتصحيح والتخطيط والتعاون | https://github.com/LayneChai/superpowers-dsh |
| 4 | @liustack/modlens | 3.27.3 | 4.7k | الرؤية وOCR وإعادة بناء الواجهة | https://github.com/liustack/modlens |
| 5 | @next-ai-drawio/dsh | 0.1.0 | 36.7k | تحرير مخططات draw.io مباشرة | https://github.com/DayuanJiang/next-ai-draw-io |
| 6 | @linxin666/dsh-client-ui-task-board | 0.4.16 | 10.2k | لوحة مهام ومهام فرعية وتشغيل مجدول | https://github.com/zhu1090093659/dsh-web |
| 7 | dsh-whale-widget | 0.4.2 | 7.0k | أداة رصيد واستخدام DeepSeek | https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget |
| 8 | dsh-better-sidebar | 0.30.1 | 5.0k | شريط جانبي محسّن: طرفية وملفات وGit ووكلاء فرعيون | https://github.com/omdsh-dev/DSH-better-sidebar |
| 9 | dsh-context | 0.82.2 | 4.4k | رؤى السياق والرموز واستدعاءات الأدوات | https://github.com/bowenliang123/dsh-context |
| 10 | @michengai/dsh-skills-manager | 1.2.35 | 444 | إدارة مهارات Agent المحلية | https://github.com/MichengAI/dsh-skills-manager |

> أعداد النجوم مأخوذة من كتالوج Awesome DSH Plugin بتاريخ 2026-11-07.

ملاحظات: يحتاج ModLens إلى نموذج رؤية أو إعداد محلي موجود؛ ويُنزّل Next AI Draw.io خادم MCP عبر npx عند أول استخدام؛ ويستخدم Task Board النموذج المُعد حالياً في DSH؛ وتستخدم أداة الحوت بيانات اعتماد DeepSeek التي يديرها DSH.

## مهارات اختيارية (تحديد متعدد)

| المهارة | المصدر | التوافق |
|---|---|---|
| ponytail | https://github.com/DietrichGebert/ponytail | تنسيق SKILL.md قياسي ويكتشفه DSH |
| find-skills | https://github.com/vercel-labs/skills | يستخدم npx skills؛ والمثبّت يثبّت Node.js/npm |
| find-plugins | https://github.com/Nagi-ovo/dsh-find-plugins | يبحث عن إضافات DSH على GitHub |

المهارات مثبتة على commits محددة لتجنب تغيّر فرع main.

## GitHub أولاً ثم المرايا الصينية

الإضافات: يحاول المثبّت أولاً commit GitHub المحدد. وإذا انتهت المهلة بعد 60 ثانية أو فشل التثبيت أو لم يتغير ملف التعريف، يثبّت الإصدار نفسه من **registry.npmmirror.com**.

المهارات: تُجرّب GitHub Raw أولاً. وعند الفشل أو انتهاء المهلة تُستخدم مرآة CDN من jsDelivr. جميع مصادر Git مثبتة على commits ولا تستخدم فرع main متغيراً.

## إعداد مفتاح API

لا يطلب المثبّت مفتاح DeepSeek API ولا يتحقق منه ولا يخزنه. بعد التثبيت شغّل DeepSeek Harness وأدخل المفتاح ضمن **Settings → Models**. يُدار المفتاح بواسطة آلية الإعدادات وبيانات الاعتماد الخاصة بـ DSH.

## صلاحيات المسؤول وUAC

- تطبيق DSH و الإضافات والمهارات وتغييرات PATH والملفات ضمن %USERPROFILE%\.dsh كلها عمليات على مستوى المستخدم ولا تتطلب صلاحيات مسؤول.
- تُثبّت Node.js وGit أولاً عبر WinGet باستخدام --scope user. وإذا لم تدعم حزمة WinGet نطاق المستخدم فقد يعود المثبّت إلى التثبيت النظامي وقد تعرض Windows مطالبة UAC.
- لا يقوم -CheckOnly إلا بفحص البيئة والشبكة، ولا يثبّت شيئاً ولا يفعّل UAC.
- للحصول على تثبيت خالٍ من UAC تماماً، ثبّت Node.js >=24.9.0 وGit مسبقاً، أو عدّل المثبّت لاستخدام نسخ محمولة بالكامل من Node.js وGit.
- شغّل المثبّت كمستخدم عادي. ينبّه السكربت عند اكتشاف جلسة مسؤول لأن الصلاحيات المرتفعة لا تكون مطلوبة عادة.

## التحقق والأمان

- يتحقق مثبّت DSH الرسمي من توقيع Authenticode ويتوقف عند فشله.
- يُعرض SHA256 للمثبّت.
- تتحقق حزمة Node.js المحمولة من ملف SHASUMS256.txt الرسمي.
- تستخدم الإضافات أولاً commit GitHub محدداً ثم الإصدار نفسه من npmmirror؛ وتستخدم المهارات commits محددة مع بديل jsDelivr.
- تعمل الإضافات الخارجية داخل عملية DSH بصلاحيات المستخدم. راجع كل مستودع قبل تثبيته.

## ملاحظات

- يتطلب Windows 10/11 بنسخة 64-bit ومساحة حرة لا تقل عن 2 GB واتصالاً بالإنترنت.
- لا يتعامل المثبّت مع مفتاح API. اضبطه ضمن **Settings → Models** بعد أول تشغيل.
- يُثبّت تطبيق DSH الرسمي للمستخدم الحالي ولا يتطلب عادة صلاحيات مسؤول.
- قد يظهر التطبيق لفترة قصيرة أثناء تهيئة ملف تعريف Desktop، ثم يغلقه المثبّت تلقائياً.
- إذا فشلت إضافة، أعد تشغيل المثبّت واخترها مرة أخرى.
- ما زال DeepSeek Harness في مرحلة Developer Preview وقد تتغير واجهات الإضافات. الإصدارات مثبتة في المثبّت.
- قبل إزالة DeepSeek Harness، احذف أمر dsh الذي يسجله التطبيق. يضيف هذا المثبّت فقط مجلد CLI المضمّن إلى PATH الخاص بالمستخدم.

## المراجع

- DeepSeek Harness: https://github.com/deepseek-ai/deepseek-harness
- دليل Web UI الرسمي: https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/guide/index.md
- قواعد المهارات الرسمية: https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/skill/skill-filesystem/README.md
