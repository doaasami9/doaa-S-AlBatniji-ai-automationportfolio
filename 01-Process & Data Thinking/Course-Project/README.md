رحيق | RAHEEQ
AI-Powered Client Request Management
نظام لاستقبال طلبات الخدمات وتحليلها وتوجيهها تلقائيًا باستخدام Google Forms وZapier والذكاء الاصطناعي، مع حفظ النتائج في Google Sheets وإرسال إشعار مناسب عبر Gmail.

رحيق يحوّل وصف العميل إلى بيانات منظمة، ثم يوجّه الطلب إلى أحد مسارين: طلب مكتمل أو طلب يحتاج إلى مراجعة بشرية. يعتمد التحليل على المعلومات المذكورة صراحةً، ولا يفترض ميزانية أو موعدًا لم يذكرهما العميل.
🎯 Problem
The Context
تستقبل فرق الخدمات طلبات من عملاء محتملين عبر النماذج الإلكترونية. وقد يختلف مستوى التفصيل من طلب إلى آخر؛ فبعض العملاء يذكرون الخدمة والموعد والميزانية بوضوح، بينما يتركون معلومات أساسية ناقصة أو يصفون الموعد بعبارات عامة.
The Problem
تتطلب معالجة الطلبات يدويًا قراءة وصف كل طلب، واستخراج المعلومات، والتحقق من اكتمالها، ثم تحديد الخطوة التالية. ويؤدي تكرار هذه الخطوات إلى استهلاك الوقت، ويجعل متابعة الطلبات أقل تنظيمًا.
Why It Matters
يساعد الفرز المنظم على تمييز الطلبات الجاهزة للمتابعة عن الطلبات التي تحتاج إلى استكمال، مع تسجيل النتائج وإشعار الفريق المعني بدل الاعتماد على قراءة كل طلب وتصنيفه يدويًا.
💡 Solution
رحيق هو سير عمل مؤتمت يستقبل طلبات الخدمات من Google Forms، ويحلل نص وصف الطلب باستخدام AI by Zapier، ثم يحدد حالة الطلب ومسار توجيهه. بعد ذلك، يسجّل البيانات والنتائج في Google Sheets ويرسل إشعارًا عبر Gmail.
What does the system do?
- يستقبل استجابة جديدة من Google Forms.
- يحلل نص وصف الطلب ويستخرج منه المعلومات المذكورة بوضوح.
- يصنّف الطلب إلى VALID أو NEEDS_REVIEW.
- يوجّه الطلب المكتمل إلى web_team، والطلب الذي يحتاج إلى مراجعة إلى human_review.
- يحفظ بيانات الطلب ونتيجة التحليل في Google Sheets.
- يرسل بريدًا إلكترونيًا يتناسب مع مسار الطلب.
👤 Users
Primary Users
- موظفو استقبال الطلبات ومراجعتها.
- فرق الخدمات التي تحتاج إلى فرز الطلبات قبل المتابعة.
- فريق الويب المسؤول عن متابعة الطلبات المكتملة.
User Need
معرفة الطلبات الجاهزة للمتابعة، وتحديد المعلومات الناقصة في الطلبات الأخرى، والاحتفاظ بسجل منظم يسهل الرجوع إليه.
🧠 How I Designed the Solution
يقسم التصميم العملية إلى مراحل واضحة: استقبال الطلب، وتحليل وصفه، والتحقق من المعلومات المطلوبة، وتحديد الحالة، ثم التوجيه والحفظ وإرسال الإشعار.

يُستخدم الذكاء الاصطناعي لاستخراج المعلومات من النص، بينما ينفّذ Paths by Zapier قاعدة التوجيه. إذا كانت قيمة request_status تساوي VALID، ينتقل الطلب إلى مسار web_team؛ وفي غير ذلك ينتقل إلى مسار human_review. ويساعد هذا الفصل على جعل قرار التوجيه واضحًا وقابلًا للاختبار.
⚙️ How It Works
Input
يُدخل العميل بياناته في Google Forms، بما يشمل الاسم والبريد الإلكتروني والخدمة المختارة والموعد المطلوب ووصف الطلب والنتيجة التي يريد الوصول إليها.
Workflow
1. Google Forms — Input: يستقبل النموذج طلب الخدمة.
2. Zapier Trigger — New Form Response: يبدأ سير العمل عند وصول استجابة جديدة.
3. AI by Zapier — Extraction: يحلل نص وصف الطلب وينشئ مخرجات منظمة.
4. Paths by Zapier — Routing: يتحقق من حالة الطلب ويوجهه إلى المسار المناسب.
5. Google Sheets — Record: يحفظ بيانات الطلب ونتيجة التحليل.
6. Gmail — Notification: يرسل إشعارًا يتناسب مع المسار.
Routing Rules
- VALID → web_team → حفظ السجل مع needs_human_review = false وإرسال إشعار الطلب المكتمل.
- NEEDS_REVIEW → human_review → حفظ السجل مع needs_human_review = true وإرسال إشعار المراجعة.
🤖 Role of AI
What does AI do in this solution?
يستقبل AI by Zapier نص وصف الطلب (request_text) فقط، ويُنتج الحقول التالية:
- service_type: نوع الخدمة المستخرج من وصف الطلب.
- timeline: الموعد أو الإطار الزمني المذكور بوضوح.
- budget: الميزانية إذا ذكرها العميل صراحةً في وصف الطلب.
- request_summary: ملخص الطلب.
- missing_information: المعلومات المطلوبة التي لم تُذكر.
- request_status: حالة الطلب، VALID أو NEEDS_REVIEW.
- routing: مسار التوجيه، web_team أو human_review.
What AI Should NOT Do
- اختراع أو تقدير ميزانية لم يذكرها العميل.
- تحويل موعد غامض مثل «قريبًا» أو soon إلى موعد محدد.
- استخدام اسم العميل أو بريده الإلكتروني مدخلًا لتحليل وصف الطلب.
- اتخاذ قرار نيابةً عن الفريق البشري بشأن الطلبات الناقصة.
🔄 Automation
What parts of the process are automated?
- التقاط استجابات النموذج الجديدة.
- تحليل وصف الطلب واستخراج الحقول.
- تصنيف الطلب وتحديد مساره.
- تسجيل البيانات والنتائج في Google Sheets.
- إرسال إشعارات البريد الإلكتروني بحسب المسار.
تظل مراجعة الطلبات الناقصة، وطلب المعلومات الإضافية عند الحاجة، واتخاذ قرار المتابعة النهائي من مسؤولية الفريق البشري.
🗂️ Data
Input
يتضمن نموذج Google Forms الحقول التالية:
- Client Name
- Email
- What service do you need
- When do you need the service completed
- Please describe your request and the result you want to achieve
ملاحظة مهمة: لا يوجد حقل مستقل للميزانية في النموذج. يمكن للعميل ذكر الميزانية ضمن وصف الطلب، ويستخرجها الذكاء الاصطناعي إذا وردت بوضوح. كما تُحفظ الخدمة والمدة اللتان اختارهما العميل في النموذج بصورة منفصلة عن الحقول المستخرجة من نص الوصف.

لا يُرسل إلى AI by Zapier سوى نص وصف الطلب (request_text) للتحليل، وليس اسم العميل أو بريده الإلكتروني.
Output
Field	Purpose
service_type	نوع الخدمة المستخرج من وصف الطلب
timeline	الموعد المذكور بوضوح في وصف الطلب
budget	الميزانية إذا وردت صراحةً في الوصف
request_summary	ملخص الطلب
missing_information	المعلومات الناقصة، أو NONE
request_status	VALID أو NEEDS_REVIEW
routing	web_team أو human_review


يسجّل Google Sheets البيانات في 15 عمودًا:

submitted_at, client_name, email, service_selected, deadline_selected, request_text, service_type, timeline, budget, request_summary, missing_information, request_status, routing, needs_human_review, processed_at.
ملاحظة تنفيذية: حقل processed_at مربوط حاليًا بوقت إرسال النموذج (Last Submitted Time)، لذلك لا يمثل وقت المعالجة الفعلي حتى الآن.

🛠️ Tools & Technologies
Tool	Purpose
Google Forms	استقبال طلبات العملاء
Zapier	تشغيل خطوات سير العمل وربطها
AI by Zapier	استخراج المعلومات المنظمة من وصف الطلب
Paths by Zapier	توجيه الطلب حسب حالته
Google Sheets	حفظ الطلبات ونتائج التحليل
Gmail	إرسال إشعارات البريد الإلكتروني
GitHub	توثيق المشروع ومشاركة ملفاته


🔑 Key Design Decisions
Decision 1 — Limit the AI Input
Decision: إرسال نص وصف الطلب فقط إلى الذكاء الاصطناعي للتحليل.

Why: إبقاء التحليل مركّزًا على النص غير المنظم، وعدم الحاجة إلى إرسال بيانات التواصل إلى خطوة استخراج المعلومات.
Decision 2 — Use Explicit Routing Rules
Decision: استخدام شرط request_status = VALID لمسار الطلب المكتمل، وجعل مسار المراجعة هو المسار البديل.

Why: جعل قرار التوجيه واضحًا وقابلًا للاختبار، وتجنّب إرسال الطلبات غير المكتملة إلى المسار المخصص للطلبات الجاهزة.
Decision 3 — Do Not Assume Missing Information
Decision: لا تُستخرج الميزانية إلا إذا ذكرها العميل بوضوح، ولا يُعد الموعد الغامض موعدًا محددًا.

Why: تجنّب إدخال معلومات غير موجودة في طلب العميل، وإظهار ما يحتاج إلى استكمال قبل المتابعة.
Decision 4 — Separate Extraction from Routing
Decision: يتولى الذكاء الاصطناعي تحليل النص وإنتاج الحقول، بينما يتولى Paths by Zapier تطبيق قاعدة التوجيه.

Why: فصل استخراج المعلومات عن قرار المسار يجعل من الأسهل اختبار كل جزء والتحقق من سلوكه.
🧪 Testing
تم تنفيذ أربعة اختبارات من إرسال النموذج حتى حفظ السجل وإرسال البريد الإلكتروني. ووفقًا لنتائج الاختبار التي تم التحقق منها، نجحت الحالات الأربع في إكمال سير العمل.
Test 1 — Complete Request
Input

We need a website for our company. Our budget is $5000. We need it next month.

Expected Result
- service_type = website
- timeline = next month
- budget = $5000
- missing_information = NONE
- request_status = VALID
- routing = web_team
- needs_human_review = false
Actual Result

أعاد التحليل القيم المتوقعة، وكان ملخص الطلب أطول من المثال المختصر. اكتمل الاختبار حتى حفظ السجل وإرسال البريد الإلكتروني.

Status: Passed
Test 2 — Budget Not Mentioned
Input

We need a website for our company. We need it next month.

Expected Result

عدم افتراض ميزانية، وتحديد budget ضمن المعلومات الناقصة، مع request_status = NEEDS_REVIEW وrouting = human_review.

Actual Result

كانت الحالة NEEDS_REVIEW، والتوجيه human_review، والمعلومة الناقصة budget. اكتمل الاختبار حتى حفظ السجل وإرسال البريد الإلكتروني.

Status: Passed
Test 3 — Vague Timeline
Input

We need a website soon. Our budget is $5000.

Expected Result

عدم تحويل soon إلى موعد محدد، وتحديد timeline ضمن المعلومات الناقصة، مع request_status = NEEDS_REVIEW وrouting = human_review.

Actual Result

لم يُحوَّل soon إلى موعد محدد؛ جاء حقل الموعد فارغًا، وكانت المعلومة الناقصة timeline، والحالة NEEDS_REVIEW، والتوجيه human_review. اكتمل الاختبار حتى حفظ السجل وإرسال البريد الإلكتروني.

Status: Passed
Test 4 — Timeline and Budget Not Mentioned
Input

We need a website for our company.

Expected Result

عدم افتراض موعد أو ميزانية، وتحديد timeline وbudget ضمن المعلومات الناقصة، مع request_status = NEEDS_REVIEW وrouting = human_review.

Actual Result

كانت المعلومات الناقصة timeline, budget، والحالة NEEDS_REVIEW، والتوجيه human_review. اكتمل الاختبار حتى حفظ السجل وإرسال البريد الإلكتروني.

Status: Passed
📸 Evidence
Workflow Diagram
```mermaid
flowchart TD
    A["Google Forms<br/>استقبال الطلب"] --> B["Zapier Trigger<br/>New Form Response"]
    B --> C["AI by Zapier<br/>استخراج المعلومات"]
    C --> D{"Paths by Zapier<br/>request_status = VALID?"}
    D -->|نعم| E["web_team"]
    D -->|لا| F["human_review"]
    E --> G["Google Sheets<br/>حفظ السجل"]
    F --> H["Google Sheets<br/>حفظ السجل"]
    G --> I["Gmail<br/>إشعار الطلب المكتمل"]
    H --> J["Gmail<br/>إشعار المراجعة البشرية"]
```
End-to-End Test Coverage
Test Case	Expected Route	Verified Outcome
طلب مكتمل	web_team	اكتمل سير العمل
الميزانية غير مذكورة	human_review	اكتمل سير العمل
الموعد غامض	human_review	اكتمل سير العمل
الموعد والميزانية غير مذكورين	human_review	اكتمل سير العمل


(يمكن إضافة صور فعلية من Zapier وGoogle Sheets والبريد الإلكتروني هنا إذا كانت متاحة ومناسبة للمشاركة، مع إخفاء أي بيانات شخصية أو مفاتيح وصول.)
⚠️ Challenges & Failures
What didn't work as expected?
خلال مرحلة البناء والاختبار، احتاجت بعض الخطوات إلى ضبط قبل نجاح سير العمل كاملًا:
- كان إعداد Prompt في خطوة الذكاء الاصطناعي بحاجة إلى تحديث وربط نص وصف الطلب بمدخل التحليل.
- لم تظهر بعض الحقول في قوائم الربط داخل Zapier عند استخدام سجل اختبار لا يحتوي على قيم لها.
- ظهرت القيم المدخلة يدويًا على شكل نص حرفي في رسالة بريد خلال إحدى مراحل البناء.
- حُذف أحد المسارات بالخطأ أثناء الإعداد، ثم أُعيد بناؤه.
- ظهر في أحد الاختبارات اختلاف بين الموعد المذكور في وصف الطلب والموعد المختار في حقل النموذج المنفصل؛ وتحتاج أولوية المصدر في هذه الحالة إلى تحديد أوضح.
How did I solve it?
- تحديث Prompt وربط نص وصف الطلب بمدخل الذكاء الاصطناعي.
- استخدام سجل اختبار مكتمل لإظهار الحقول المتاحة أثناء الإعداد.
- اختيار القيم الديناميكية من حقول Zapier بدل إدراجها كنص ثابت.
- إعادة بناء المسار والتحقق من شرط request_status = VALID.
- اختبار الحالات الأربع من النموذج حتى Google Sheets والبريد الإلكتروني.
📚 What I Learned
Technical Lesson
تؤثر بيانات الاختبار في الحقول التي تظهر ضمن قوائم الربط؛ لذلك يفيد استخدام سجل اختبار يحتوي على قيم في التحقق من الحقول وربطها بالخطوات التالية.
Problem-Solving Lesson
اختبار كل مرحلة ومقارنة النتائج المتوقعة بالفعلية يساعد على تحديد موضع الخلل قبل اختبار سير العمل كاملًا.
AI Lesson
يحتاج استخراج المعلومات إلى قواعد واضحة للحالات الناقصة والعبارات الغامضة، مع التأكد من أن النموذج لا يخمّن معلومات لم يذكرها العميل.
🔮 Future Improvements
If I continued developing this project, I would:
- تحديد قاعدة واضحة عند اختلاف الموعد المذكور في وصف الطلب عن الموعد المختار في النموذج.
- تسجيل وقت المعالجة الفعلي في processed_at بدل استخدام وقت إرسال النموذج.
- توجيه إشعارات المراجعة إلى بريد مشترك للفريق عند تهيئة النظام للاستخدام التشغيلي.
- إضافة اختبارات لحالات أخرى، مثل وصف غير واضح أو طلب يحتوي على معلومات متعارضة.
🔗 Project Links
- Request Form: https://docs.google.com/forms/d/e/1FAIpQLSf1A8j58o7fnQ3gECBpCiFoPK5jgw3GHozOWuQgpYb_4uuPbQ/viewform?usp=header
- Zapier Template: https://zapier.com/templates/details/raheeq-ai-powered-client-request-management-c5b92a
- GitHub Repository: (أضيفي رابط مستودع رحيق الفعلي على GitHub هنا.)
👩‍💻 Author
DOAA SAMI AL-BATNIJI
Computer Engineer | Data, AI & Automation

LinkedIn (https://www.linkedin.com/in/doaa-sami-984426271/)
· GitHub
