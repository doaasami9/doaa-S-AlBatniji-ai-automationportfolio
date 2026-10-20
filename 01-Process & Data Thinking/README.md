# U1 — UNDERSTAND THE WORK
### Process & Data Thinking

> Before automating the work, understand the work.

في هذه الوحدة، أتعلم كيف أحوّل مشكلة واقعية إلى عملية واضحة يمكن تحليلها وتصميم حل مناسب لها باستخدام **AI Automation**.

الهدف ليس البدء بالأداة، بل فهم العمل أولًا، وتحديد المشكلة، وتحليل البيانات، وتصميم القرارات التي يحتاج النظام إلى اتخاذها.

---

## 🎯 Capability

**Process & Data Thinking**

القدرة على تحليل العمل، وتفكيك العمليات، وفهم المدخلات، وتصميم منطق اتخاذ القرار قبل تنفيذ الأتمتة.

---

## 🧠 What I Learned

خلال هذه الوحدة، طوّرت قدرتي على:

- **Understand the Work:** فهم المشكلة وسياق العمل والنتيجة المطلوبة.
- **Decompose the Work:** تفكيك العملية إلى Workflow وTasks وSubtasks.
- **Understand the Input:** تحديد البيانات المطلوبة والتمييز بين البيانات المنظمة وغير المنظمة.
- **Design the Decision:** تحديد متى نستخدم AI ومتى تكفي القواعد المحددة.
- **Handle Exceptions Safely:** التعامل مع الحالات غير المتوقعة وتحديد متى نحتاج إلى Human Review.
- **Design the Automation:** تحويل تصميم العملية إلى Workflow قابل للتنفيذ.

---

## 🛠️ Projects

### 01. Guided Project — AI Client Intake & Request Routing System

مشروع تطبيقي موجّه لتعلّم كيفية تحليل طلبات العملاء وتصميم عملية أتمتة تساعد على تنظيمها وتحديد الإجراء المناسب لكل طلب.

**Problem**

تعتمد عملية معالجة طلبات العملاء على تدخل الموظفين في عدة خطوات متكررة، بدءًا من فهم الطلب وحتى تحديد الإجراء التالي.

**Solution**

تصميم نظام أتمتة يساعد على معالجة طلبات العملاء بطريقة منظمة، وتقليل العمل اليدوي، مع إبقاء الحالات التي تحتاج إلى تدخل بشري تحت المراجعة.

**Tools**

- **Google Forms:** استقبال طلبات العملاء.
- **Google Sheets:** تسجيل الطلبات ونتائج معالجتها.
- **Zapier:** بناء تدفق الأتمتة وربط الخطوات.
- **AI by Zapier:** استخراج المعلومات من الطلبات وتنظيمها.
- **Gmail:** إرسال الإشعارات عبر البريد الإلكتروني.

🔗 **[Explore the Guided Project](https://zapier.com/templates/details/ai-client-intake-request-routing-e485e6)**

---

### 02. Independent Project — Raheeq

**Raheeq | AI-Powered Client Request Management**

مشروع مستقل أطبّق فيه مفاهيم تحليل العمل وتصميم الأتمتة على مشكلة واقعية، مع تصميم الحل وتنفيذه وتوثيق النتائج.

**Problem**

يواجه أصحاب المهارات والمستقلون، والشركات الناشئة والمشاريع الصغيرة تحديات في تنظيم طلبات العملاء، ومراجعة المعلومات المقدمة، وتحديد الإجراء المناسب لكل طلب. وقد تؤدي المعالجة اليدوية إلى استهلاك الوقت، وتكرار المهام، وتأخر معالجة الطلبات التي تحتاج إلى مراجعة إضافية.

**Solution**

رحيق هو نظام أتمتة يستقبل طلبات العملاء عبر Google Forms، ويستخدم الذكاء الاصطناعي لاستخراج المعلومات الأساسية دون تخمين. بعد ذلك، يحدد مسار الطلب وفق قواعد واضحة:

- **Valid:** متابعة الطلب المستوفي للشروط المحددة.
- **Needs Review:** إحالة الطلبات التي تحتاج إلى تحقق أو مراجعة بشرية.

ثم يسجل النظام النتائج في Google Sheets ويرسل إشعارًا مناسبًا عبر Gmail.

**Workflow**

`Google Forms → Zapier Trigger → AI by Zapier → Paths by Zapier → Google Sheets → Gmail`

**Tools**

- **Google Forms:** استقبال طلبات العملاء.
- **Zapier:** ربط خطوات سير العمل وأتمتتها.
- **AI by Zapier:** استخراج المعلومات الأساسية من الطلبات دون تخمين.
- **Paths by Zapier:** توجيه الطلبات وفق قواعد القرار المحددة.
- **Google Sheets:** تسجيل الطلبات ونتائج معالجتها.
- **Gmail:** إرسال الإشعارات المناسبة.

🔗 **[Explore the Raheeq Project](https://zapier.com/templates/details/raheeq-ai-powered-client-request-management-c5b92a)**

---

## 🔍 Engineering Focus

في هذه الوحدة، أركز على الأسئلة التالية:

1. ما المشكلة الحقيقية التي نريد حلها؟
2. كيف تتم العملية حاليًا؟
3. ما المدخلات والمخرجات التي يحتاج إليها النظام؟
4. أين نحتاج إلى AI، وأين تكفي القواعد المحددة؟
5. ماذا يحدث عندما تكون المعلومات ناقصة أو غير واضحة؟
6. كيف نحوّل تصميم العملية إلى Automation Workflow قابل للتنفيذ؟

---

## 📌 Key Takeaway

> **A good automation starts with a well-understood process, not with a tool.**

نجاح الأتمتة يبدأ بفهم العمل وتصميم العملية، وليس بمجرد اختيار الأداة.

---

## 📈 Capability Progress

- [ ] فهم المشكلة والنتيجة المطلوبة.
- [ ] تفكيك العمل إلى خطوات واضحة.
- [ ] تحديد المدخلات والمخرجات.
- [ ] تصميم قرارات AI وRule-Based Logic.
- [ ] تحديد الحالات التي تحتاج إلى Human Review.
- [ ] تنفيذ الحل وتوثيق نتائجه.

---

## 🔗 Related Projects

| Project | Type | Link |
|---|---|---|
| AI Client Intake & Request Routing System | Guided Project | [Explore](https://zapier.com/templates/details/ai-client-intake-request-routing-e485e6) |
| Raheeq — AI-Powered Client Request Management | Independent Project | [Explore](https://zapier.com/templates/details/raheeq-ai-powered-client-request-management-c5b92a) |

---

### Next: U2 — BUILD THE SYSTEM
