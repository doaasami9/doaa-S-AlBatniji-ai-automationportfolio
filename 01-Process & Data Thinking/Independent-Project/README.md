# AI Client Intake & Request Routing System

**نظام أتمتة مدعوم بالذكاء الاصطناعي يستخرج المعلومات المنظمة من طلبات العملاء، ويوجّهها وفق قواعد قرار محددة، ويسجّل النتائج، مع تحويل الطلبات غير المكتملة إلى المراجعة البشرية.**

<p align="center">

`AI Understands` · `Logic Controls` · `Humans Judge`

يفهم الذكاء الاصطناعي · يتحكم المنطق · يحكم الإنسان

</p>

---

## 📑 Table of Contents

- [Problem](#-problem)
- [Solution](#-solution)
- [Users](#-users)
- [System Design](#-how-i-designed-the-solution)
- [How It Works](#️-how-it-works)
- [Role of AI](#-role-of-ai)
- [Automation Workflow](#-automation)
- [Data Design](#️-data)
- [Tools & Technologies](#️-tools--technologies)
- [Key Design Decisions](#-key-design-decisions)
- [Testing & Validation](#-testing--validation)
- [Evidence](#-evidence)
- [Challenges & Failures](#️-challenges--failures)
- [Lessons Learned](#-lessons-learned)
- [Future Improvements](#-future-improvements)
- [Repository Structure](#-repository-structure)
- [Project Links](#-project-links)
- [Author](#-author)

---

## 🎯 Problem

### The Context

تستقبل الشركات التي تقدم الخدمات طلبات من عملاء محتملين يبحثون عن خدمات مثل تصميم المواقع الإلكترونية، والتسويق الرقمي، وتطوير التطبيقات.

يصف كل عميل احتياجاته بطريقة مختلفة؛ فبعض العملاء يقدمون متطلبات مفصلة، بينما يرسل آخرون طلبات قصيرة أو مبهمة أو غير مكتملة.

### The Problem

يضطر الموظفون إلى قراءة كل طلب يدويًا، وفهم احتياجات العميل، واستخراج المعلومات ذات الصلة، والتحقق من اكتمالها، ثم تحديد الإجراء المناسب.

ومع زيادة عدد الطلبات، تستغرق هذه العملية المتكررة وقتًا طويلًا، وقد تؤدي إلى تأخر المتابعة، أو إغفال بعض التفاصيل، أو عدم اتساق القرارات.

### Why It Matters

تساعد عملية استقبال الطلبات المنظمة على ترتيب الطلبات الواردة، وتحديد المعلومات الناقصة، وتقليل العمل اليدوي المتكرر، وتوجيه كل طلب إلى الخطوة التالية المناسبة.

الهدف ليس مجرد جمع الطلبات، بل معالجتها بطريقة متسقة والتعامل مع الحالات غير الواضحة بمسؤولية.

---

## 💡 Solution

يقوم **AI Client Intake & Request Routing System** بأتمتة المعالجة الأولية لطلبات العملاء باستخدام الذكاء الاصطناعي وأتمتة سير العمل.

يجمع النظام بين استخراج المعلومات باستخدام الذكاء الاصطناعي، وقواعد التوجيه المحددة مسبقًا، ومسار للمراجعة البشرية عند وجود طلبات غير مكتملة أو غير واضحة.

### What Does the System Do?

- يستقبل طلبات العملاء من خلال Google Forms.
- يستخرج المعلومات ذات الصلة من نص الطلب باستخدام AI by Zapier.
- ينظم المعلومات المستخرجة ضمن حقول محددة مسبقًا.
- يحدد المعلومات الناقصة أو غير الواضحة.
- يصنّف الطلبات إلى `VALID` أو `NEEDS_REVIEW`.
- يوجّه الطلبات وفق قواعد القرار المُعدّة في النظام.
- يسجّل تفاصيل الطلبات ونتائج المعالجة في Google Sheets.
- يرسل إشعارًا مناسبًا وفق مسار المعالجة المحدد.

---

## 👤 Users

### Primary Users

- الموظفون المسؤولون عن استقبال طلبات العملاء ومراجعتها.
- فرق المبيعات ودعم العملاء.
- الشركات الخدمية التي تحتاج إلى آلية منظمة لاستقبال الطلبات.
- الفرق المسؤولة عن متابعة الطلبات المؤهلة.

### User Need

يحتاج المستخدمون إلى طريقة متسقة لفهم الطلبات الواردة، والتحقق من توفر المعلومات الأساسية، وتحديد الإجراء التالي، والاحتفاظ بسجل منظم لكل طلب.

---

## 🧠 How I Designed the Solution

يعتمد المشروع على منهجية هندسية تبدأ بفهم العملية قبل تنفيذ الأتمتة:

**Understand → Decompose → Design → Build → Test**

فبدلًا من البدء بأدوات الأتمتة مباشرة، يتم أولًا تحليل العملية وتقسيمها إلى خطوات أصغر، ثم تحديد البيانات المطلوبة، وقواعد القرار، والحالات الاستثنائية قبل التنفيذ.

يفصل التصميم بين ثلاث مسؤوليات:

| Component | Responsibility |
|---|---|
| AI | استخراج المعلومات من نص الطلب وتنظيمها ضمن حقول محددة. |
| Rule-based Logic | تحديد مسار الطلب باستخدام حالته وفق قواعد واضحة. |
| Human Review | مراجعة الطلبات التي تحتاج إلى توضيح أو تقدير بشري إضافي. |

يعتمد قرار التوجيه على قيمة حالة محددة، بدلًا من توزيع شروط مستقلة متعددة داخل خطوة التوجيه نفسها. ويساعد ذلك على جعل سير العمل أوضح وأسهل في الاختبار والصيانة.

---

## ⚙️ How It Works

### Workflow Overview

```text
Client Submission
       │
       ▼
Google Forms
       │
       ▼
Zapier Trigger
New Form Response
       │
       ▼
AI by Zapier
استخراج المعلومات وتنظيمها
       │
       ▼
Paths by Zapier
فحص request_status
       │
       ├── VALID
       │     │
       │     ▼
       │   Web Team Path
       │     │
       │     ▼
       │   Google Sheets
       │     │
       │     ▼
       │   Valid Request Email
       │
       └── NEEDS_REVIEW
             │
             ▼
          Human Review Path
             │
             ▼
          Google Sheets
             │
             ▼
          Review Notification
```

### Step 1 — Input

يُرسل العميل طلبه من خلال Google Forms.

يجمع النموذج بيانات العميل ووصفًا نصيًا حرًا للخدمة المطلوبة.

### Step 2 — Trigger

يبدأ Zapier تنفيذ سير العمل عند استقبال استجابة جديدة من النموذج.

### Step 3 — AI Processing

يحلل AI by Zapier نص الطلب ويعيد مجموعة من الحقول المنظمة، تشمل نوع الخدمة، والجدول الزمني، والميزانية، وملخص الطلب، والمعلومات الناقصة، وحالة الطلب، وقيمة التوجيه.

### Step 4 — Decision & Routing

تقيّم Paths by Zapier حالة الطلب وفق القواعد المحددة:

- `VALID` → توجيه الطلب إلى مسار **Web Team**.
- أي طلب لا يحقق شرط `VALID` → توجيهه إلى المسار البديل **Human Review**.

### Step 5 — Output

يسجّل كل مسار الطلب ونتائج معالجته في Google Sheets، ثم يرسل إشعار البريد الإلكتروني المرتبط بذلك المسار.

وتظل الطلبات التي تحتاج إلى توضيح خاضعة للمراجعة البشرية.

---

## 🤖 Role of AI

### What Does AI Do?

- يقرأ الوصف النصي الذي يكتبه العميل.
- يستخرج نوع الخدمة والجدول الزمني والميزانية عندما تكون مذكورة صراحةً.
- ينشئ ملخصًا منظمًا للطلب.
- يحدد المعلومات الناقصة.
- يعيد حالة الطلب وتوصية التوجيه وفق التعليمات المحددة.

### What AI Should NOT Do

- اختلاق معلومات لم يقدمها العميل أو تقديرها أو افتراضها.
- تحويل العبارات الزمنية المبهمة إلى مواعيد نهائية محددة.
- التعامل مع المعلومات الناقصة باعتبارها حقائق مؤكدة.
- استبدال قواعد التوجيه الصريحة بقرارات غير مدعومة.
- حسم الطلبات غير الواضحة دون المراجعة البشرية المناسبة.

**Design Principle:** يساعد الذكاء الاصطناعي على تفسير الطلب، لكنه لا يحل محل التحقق، أو قواعد القرار الواضحة، أو الحكم البشري.

---

## 🔄 Automation Workflow

### What Is Automated?

- اكتشاف استجابات النماذج الجديدة.
- تمرير نص الطلب إلى خطوة المعالجة بالذكاء الاصطناعي.
- استخراج المعلومات ذات الصلة وتنظيمها.
- تحديد حالة الطلب.
- توجيه الطلبات عبر المسار المناسب.
- تسجيل نتائج المعالجة في Google Sheets.
- إرسال إشعار البريد الإلكتروني المرتبط بكل مسار.

### What Remains Human-Controlled?

- مراجعة الطلبات غير المكتملة أو غير الواضحة.
- التواصل مع العملاء عند الحاجة إلى توضيحات.
- اتخاذ قرارات العمل اللاحقة وتحديد إجراءات متابعة الخدمة فعليًا.

---

## 🗂️ Data Design

### Input

يستقبل سير العمل البيانات من Google Forms.

| Field | Purpose |
|---|---|
| `Client Name` | تحديد اسم العميل. |
| `Email` | توفير عنوان البريد الإلكتروني للتواصل مع العميل. |
| `Company` | تسجيل اسم الشركة إذا تم تقديمه في النموذج. |
| `Request` | يحتوي على وصف الخدمة المطلوبة، وهو النص الذي يُرسل إلى الذكاء الاصطناعي للتحليل. |

**Important:** تستقبل خطوة المعالجة بالذكاء الاصطناعي نص الطلب فقط. أما بيانات العميل الأخرى، فيتعامل معها سير العمل ويربطها بحقول المخرجات المناسبة.

### AI Output

| Field | Purpose |
|---|---|
| `service_type` | نوع الخدمة المستخرج من الطلب. |
| `timeline` | الإطار الزمني المذكور صراحةً. |
| `budget` | الميزانية المذكورة في الطلب، إن وُجدت. |
| `request_summary` | ملخص منظم للطلب. |
| `missing_information` | المعلومات المطلوبة الناقصة، أو `NONE` عند عدم وجودها. |
| `request_status` | حالة الطلب: `VALID` أو `NEEDS_REVIEW`. |
| `routing` | مسار التوجيه: `web_team` أو `human_review`. |

### Stored Results

يخزن Google Sheets تفاصيل الطلب، والمعلومات المستخرجة، وحالة المعالجة، ونتيجة التوجيه، ومؤشر المراجعة.

وتعتمد الحقول المخزنة فعليًا على إعدادات ربط الأعمدة في جدول البيانات.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Google Forms | استقبال طلبات العملاء. |
| Zapier | ربط التطبيقات وتنفيذ سير العمل الآلي. |
| AI by Zapier | استخراج المعلومات من النصوص الحرة وتنظيمها. |
| Paths by Zapier | توجيه الطلبات وفق شروط محددة مسبقًا. |
| Google Sheets | تخزين سجلات الطلبات ونتائج المعالجة. |
| Email | إرسال الإشعارات الخاصة بمسارات المعالجة. |
| GitHub | توثيق المشروع وقرارات التصميم الهندسي. |

---

## 🔑 Key Design Decisions

### Decision 1 — Analyze Before Automating

**Decision:** فهم عملية العمل وتقسيمها إلى خطوات قبل بناء سير العمل الآلي.

**Why:** يجب أن تعالج الأتمتة مشكلة محددة بوضوح، بدلًا من إعادة تنفيذ عملية يدوية غير فعّالة كما هي.

### Decision 2 — Separate AI from Routing Logic

**Decision:** استخدام الذكاء الاصطناعي لتفسير الطلب، واستخدام قواعد صريحة للتحكم في مسار التوجيه.

**Why:** يساعد هذا الفصل على جعل التوجيه أكثر قابلية للتوقع والشرح والاختبار.

### Decision 3 — Use a Human-Review Fallback

**Decision:** توجيه الطلبات التي لا تحقق شرط `VALID` إلى المراجعة البشرية.

**Why:** لا ينبغي اعتبار الطلبات غير المكتملة أو غير المؤكدة جاهزة للمتابعة دون مراجعة مناسبة.

### Decision 4 — Pass Only the Required AI Input

**Decision:** إرسال نص الطلب إلى خطوة المعالجة بالذكاء الاصطناعي بدلًا من تمرير استجابة النموذج كاملة.

**Why:** يحافظ ذلك على تركيز مهمة الذكاء الاصطناعي على تفسير النص غير المنظم، ويحد من المعلومات التي تُمرر إلى هذه الخطوة.

### Decision 5 — Distinguish Business Exceptions from Technical Failures

**Decision:** معالجة الطلبات غير المكتملة من خلال مسار المراجعة، مع التعامل مع تعطل خطوات سير العمل باعتباره مشكلة تقنية تحتاج إلى التحقيق.

**Why:** قد يكون الطلب غير مكتمل دون أن تكون الأتمتة نفسها قد فشلت؛ ولكل حالة طريقة مختلفة في المعالجة.

---

## 🧪 Testing & Validation

تم تنفيذ أربعة اختبارات على سير العمل. ووفقًا لنتائج التنفيذ، اكتملت الاختبارات الأربعة بنجاح، بدءًا من إرسال النموذج وحتى التوجيه وتسجيل البيانات في Google Sheets وإرسال إشعارات البريد الإلكتروني.

### Test 1 — Complete Request

**Input**

```text
We need a website for our company.
Our budget is $5000.
We need it next month.
```

**Expected Result**

- `service_type`: `website`
- `timeline`: `next month`
- `budget`: `$5000`
- `missing_information`: `NONE`
- `request_status`: `VALID`
- `routing`: `web_team`

يسلك الطلب مسار `VALID`، ويُسجّل مع مؤشر المراجعة المناسب، ثم يُرسل إشعار البريد الإلكتروني الخاص بالطلبات الصالحة.

**Actual Result:** اجتاز الاختبار الشامل لسير العمل بنجاح.

**Status:** ✅ Passed

### Test 2 — Missing Budget

**Input**

```text
We need a website for our company.
We need it next month.
```

**Expected Result**

- `service_type`: `website`
- `timeline`: `next month`
- `budget`: غير متوفرة.
- `missing_information`: `budget`
- `request_status`: `NEEDS_REVIEW`
- `routing`: `human_review`

يسلك الطلب مسار المراجعة البشرية بسبب غياب الميزانية.

**Actual Result:** اجتاز الاختبار الشامل لسير العمل بنجاح.

**Status:** ✅ Passed

### Test 3 — Ambiguous Timeline

**Input**

```text
We need a website soon.
Our budget is $5000.
```

**Expected Result**

- `service_type`: `website`
- `timeline`: غير محدد أو يحتاج إلى توضيح.
- `budget`: `$5000`
- `missing_information`: `timeline`
- `request_status`: `NEEDS_REVIEW`
- `routing`: `human_review`

لا يتم تحويل التعبير المبهم `soon` إلى موعد نهائي محدد، بل يُترك لتوضيحه من خلال المراجعة البشرية.

**Actual Result:** اجتاز الاختبار الشامل لسير العمل بنجاح.

**Status:** ✅ Passed

### Test 4 — Multiple Missing Fields

**Input**

```text
We need a website for our company.
```

**Expected Result**

- `service_type`: `website`
- `timeline`: غير متوفرة.
- `budget`: غير متوفرة.
- `missing_information`: `timeline, budget`
- `request_status`: `NEEDS_REVIEW`
- `routing`: `human_review`

يسلك الطلب مسار المراجعة البشرية بسبب غياب معلومات الجدول الزمني والميزانية.

**Actual Result:** اجتاز الاختبار الشامل لسير العمل بنجاح.

**Status:** ✅ Passed

### Test Summary

| Test | Scenario | Expected Status | Expected Path | Result |
|---|---|---|---|---|
| 01 | طلب مكتمل | `VALID` | Web Team | ✅ Passed |
| 02 | غياب الميزانية | `NEEDS_REVIEW` | Human Review | ✅ Passed |
| 03 | جدول زمني مبهم | `NEEDS_REVIEW` | Human Review | ✅ Passed |
| 04 | غياب الجدول الزمني والميزانية | `NEEDS_REVIEW` | Human Review | ✅ Passed |

---

## 📸 Evidence

تساعد الأدلة المرئية المراجعين على التحقق من التنفيذ دون الحاجة إلى إعادة بناء سير العمل بأنفسهم.

### 1. Complete Automation Workflow

**Evidence to Add:** لقطة شاشة من Zap المنشور في Zapier، توضح المشغّل، وخطوة الذكاء الاصطناعي، ومساري التوجيه، وإجراءات Google Sheets والبريد الإلكتروني.

`(Add screenshot: screenshots/workflow-overview.png)`

### 2. Processed Request Records

**Evidence to Add:** لقطة شاشة من Google Sheets تعرض أمثلة على نتائج طلب مكتمل وطلب يحتاج إلى مراجعة بشرية.

`(Add screenshot: screenshots/processed-requests.png)`

### 3. Email Notifications

**Evidence to Add:** لقطة شاشة توضح إشعار الطلب الصالح وإشعار المراجعة البشرية بعد إرسالهما بنجاح.

`(Add screenshot: screenshots/email-notifications.png)`

*استخدمي بيانات اختبار، واحجبي المعلومات الشخصية أو السرية قبل رفع لقطات الشاشة.*

---

## ⚠️ Challenges & Failures

**Challenges Encountered During Implementation**

يُضاف هنا وصف موجز للتحديات الفعلية التي واجهت التنفيذ وكيفية معالجتها.

إذا لم توجد تحديات جوهرية غير محلولة، فيجب توضيح ذلك بدلًا من اختلاق مشكلات أو حلول لم تحدث.

---

## 📚 Lessons Learned

### Technical Lesson

اكتسبت خبرة عملية في ربط استجابات النماذج، ومعالجة الذكاء الاصطناعي، والتوجيه الشرطي، وسجلات جداول البيانات، وإشعارات البريد الإلكتروني ضمن سير عمل آلي واحد.

### Problem-Solving Lesson

تعلّمت كيفية تقسيم عملية العمل إلى خطوات أصغر، وتحديد النتائج المتوقعة، والتحقق من كل مرحلة من خلال الاختبارات الشاملة لسير العمل.

### AI Lesson

تعلّمت التمييز بين استخراج المعلومات واتخاذ القرار. يستطيع الذكاء الاصطناعي تنظيم النصوص غير المنظمة، لكن القواعد الصريحة والمراجعة البشرية تظلان ضروريتين عندما تكون المعلومات ناقصة أو غير واضحة.

---

## 🔮 Future Improvements

يمكن تطوير المشروع مستقبلًا من خلال التحسينات التالية:

- توسيع مجموعة الاختبارات لتشمل المعلومات المتناقضة والطلبات التي تتضمن خدمات متعددة.
- تحسين مراقبة سير العمل ومعالجة الأخطاء التقنية.
- إضافة آليات لتتبع الطلبات ومتابعتها.
- قياس دقة استخراج المعلومات، ودقة التوجيه، ووقت المعالجة.
- التحقق من الحاجة إلى قواعد إضافية للتعامل مع مخرجات الذكاء الاصطناعي غير المتوقعة أو غير المعتادة.

---

## 📁 Repository Structure

```text
ai-client-intake-request-routing/
├── README.md
├── 01-business-analysis.md
├── 02-workflow-decomposition.md
├── 03-data-design.md
├── 04-decision-and-exception-design.md
├── 05-automation-design.md
├── 06-zapier-implementation.md
└── screenshots/
    ├── workflow-overview.png
    ├── processed-requests.png
    └── email-notifications.png
```

*البنية الموضحة أعلاه مقترحة. احتفظي فقط بالملفات ولقطات الشاشة الموجودة فعليًا في المستودع.*

---

## 🔗 Project Links

- **Request Form:** [Client Intake Form](https://docs.google.com/forms/d/e/1FAIpQLScz3w8sWc5KHbgJeUR3C7tu2xmZm2nipk5W8OCJMdUVRUeyRQ/viewform?usp=header)
- **Zapier Template:** [AI Client Intake & Request Routing System](https://zapier.com/templates/details/ai-client-intake-request-routing-e485e6)
- **Live Demo:** يُضاف رابط العرض التجريبي العام إن كان متاحًا.
- **Workflow:** يُضاف رابط مشاركة Zap المُعدّ إن كان متاحًا.
- **Repository:** يُضاف رابط مستودع GitHub الفعلي.

---

## 👩‍💻 Author

**Doaa Sami Al-Batniji**

Computer Engineer | Data, AI & Automation

[LinkedIn](https://www.linkedin.com/in/doaa-sami-984426271/) · [GitHub](https://github.com/doaasami9)

