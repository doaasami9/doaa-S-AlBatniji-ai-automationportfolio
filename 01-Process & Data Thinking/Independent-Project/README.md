

# 🤖 AI Client Intake & Request Routing System

### AI-Powered Client Request Processing & Routing

**نظام ذكي لاستقبال طلبات العملاء، واستخراج المعلومات الأساسية، وتوجيه الطلبات تلقائيًا وفق قواعد واضحة.**

<p>
  <img src="https://img.shields.io/badge/Workflow-Automation-6C63FF?style=for-the-badge" alt="Workflow Automation" />
  <img src="https://img.shields.io/badge/AI-Request%20Analysis-0F9D58?style=for-the-badge" alt="AI Request Analysis" />
  <img src="https://img.shields.io/badge/Google%20Forms-Input-4285F4?style=for-the-badge&logo=googleforms&logoColor=white" alt="Google Forms" />
  <img src="https://img.shields.io/badge/Zapier-Automation-FF4A00?style=for-the-badge&logo=zapier&logoColor=white" alt="Zapier" />
  <img src="https://img.shields.io/badge/Google%20Sheets-Tracking-34A853?style=for-the-badge&logo=googlesheets&logoColor=white" alt="Google Sheets" />
</p>

**From an unstructured client message to a structured, traceable request.**  
من رسالة غير مكتملة إلى طلب منظّم قابل للمتابعة، مع إحالة الحالات التي تحتاج إلى توضيح للمراجعة البشرية بدلًا من افتراض المعلومات الناقصة.

</div>

---

## 📑 Table of Contents

- [🎯 Problem](#-problem)
- [💡 Solution](#-solution)
- [👥 Target Users](#-target-users)
- [⚙️ How It Works](#️-how-it-works)
- [🧠 Role of AI](#-role-of-ai)
- [🗂️ Data Design](#️-data-design)
- [🛠️ Tools & Technologies](#️-tools--technologies)
- [🧩 Key Design Decisions](#-key-design-decisions)
- [🧪 Testing & Validation](#-testing--validation)
- [📸 Evidence](#-evidence)
- [⚠️ Challenges & Limitations](#️-challenges--limitations)
- [📚 What I Learned](#-what-i-learned)
- [🚀 Future Improvements](#-future-improvements)
- [📁 Repository Structure](#-repository-structure)
- [🔗 Project Links](#-project-links)
- [👩‍💻 Author](#-author)

---

## 🎯 Problem

### The Context

تستقبل الشركات ومقدّمو الخدمات طلبات العملاء عبر نماذج أو رسائل قد تكون غير واضحة أو تفتقر إلى معلومات أساسية، مثل الميزانية أو الإطار الزمني أو تفاصيل الخدمة المطلوبة.

### The Problem

عندما تُراجع جميع الطلبات يدويًا، قد يستغرق فرزها وتحديد أولويتها وقتًا إضافيًا، وقد يصعب اكتشاف المعلومات الناقصة أو توجيه كل طلب إلى الفريق المناسب بصورة متّسقة.

### Why It Matters

وجود مسار موحّد لمعالجة الطلبات يساعد على تحسين تنظيم البيانات، وتسريع الفرز الأولي، وإبقاء الحالات غير المكتملة واضحة بدلًا من التعامل معها على أنها طلبات جاهزة للتنفيذ.

---

## 💡 Solution

بنيت سير عمل آليًا يربط **Google Forms** و**Zapier** و**AI by Zapier** و**Google Sheets** والإشعارات عبر البريد الإلكتروني.

يقوم النظام بـ:

- استقبال تفاصيل الطلب من النموذج.
- تحليل وصف العميل واستخراج المعلومات المتاحة.
- تحديد المعلومات الناقصة أو غير الواضحة.
- تصنيف الطلب إلى حالة مناسبة.
- توجيه الطلب المكتمل إلى المسار المحدد، وإحالة الطلب الذي يحتاج إلى توضيح للمراجعة البشرية.
- تسجيل نتيجة المعالجة وإرسال إشعار عبر البريد الإلكتروني وفق إعدادات سير العمل.

> **مبدأ التصميم:** يساعد الذكاء الاصطناعي في فهم الطلب وتنظيمه، بينما تتحكم قواعد سير العمل في التوجيه. لا يتم اختلاق معلومات غير مذكورة في طلب العميل.

---

## 👥 Target Users

| User Group | How the System Helps |
|---|---|
| **Service-Based Businesses** | تنظيم الطلبات الواردة وفرزها بطريقة أكثر اتساقًا. |
| **Sales & Support Teams** | إظهار الطلبات التي تحتاج إلى معلومات إضافية قبل المتابعة. |
| **Operations Teams** | الاحتفاظ بسجل منظم للطلبات وحالاتها. |
| **Small Teams & Freelancers** | تقليل الفرز اليدوي الأولي للطلبات المتكررة. |

---

## ⚙️ How It Works

### Workflow Overview

```text
┌─────────────────────┐
│   Google Forms      │
│ Client submits form │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│  Zapier Trigger     │
│ Detect new response │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│   AI by Zapier      │
│ Analyze request     │
│ Extract key details │
└──────────┬──────────┘
           ↓
┌─────────────────────┐
│ Routing / Paths     │
│ Check request state │
└───────┬───────┬─────┘
        ↓       ↓
   ┌────────┐ ┌────────────────┐
   │ VALID  │ │ NEEDS_REVIEW   │
   │web_team│ │ human_review   │
   └────┬───┘ └───────┬────────┘
        └───────┬─────┘
                ↓
      ┌───────────────────┐
      │ Google Sheets     │
      │ Record the result │
      └─────────┬─────────┘
                ↓
      ┌───────────────────┐
      │ Email Notification│
      └───────────────────┘
```

### Process Breakdown

| Step | Component | Responsibility |
|---:|---|---|
| 01 | **Google Forms** | استقبال بيانات العميل ووصف الطلب. |
| 02 | **Zapier Trigger** | بدء سير العمل عند وصول استجابة جديدة. |
| 03 | **AI by Zapier** | تحليل النص واستخراج الحقول المطلوبة وتحديد النواقص. |
| 04 | **Paths / Routing Rules** | توجيه الطلب بناءً على الحالة الناتجة عن التحليل. |
| 05 | **Google Sheets** | حفظ البيانات المنظمة ونتيجة المعالجة. |
| 06 | **Email** | إرسال الإشعارات المهيأة ضمن سير العمل. |

### Routing Logic

| Request Status | Routing Destination | Meaning |
|---|---|---|
| `VALID` | `web_team` | الطلب يحتوي على المعلومات المطلوبة وفق قواعد سير العمل. |
| `NEEDS_REVIEW` | `human_review` | توجد معلومات ناقصة أو غموض يستدعي مراجعة بشرية. |

**Routing rule:** إذا كانت `request_status = VALID` يتم توجيه الطلب إلى `web_team`، وإلا يُوجّه إلى `human_review` وفق إعدادات المسار.

---

## 🧠 Role of AI

يُستخدم الذكاء الاصطناعي لتحويل وصف العميل المكتوب بلغة طبيعية إلى معلومات منظمة يمكن لسير العمل التعامل معها.

### Expected Structured Output

| Field | Purpose |
|---|---|
| `service_type` | تحديد نوع الخدمة المطلوبة إذا كان واضحًا من النص. |
| `timeline` | استخراج الإطار الزمني المذكور. |
| `budget` | استخراج الميزانية عندما يذكرها العميل. |
| `request_summary` | تلخيص الطلب باختصار. |
| `missing_information` | توضيح المعلومات الأساسية غير المتوفرة. |
| `request_status` | تحديد حالة الطلب وفق التعليمات المحددة. |
| `routing` | اقتراح وجهة التوجيه المتوافقة مع الحالة. |

### AI Guardrails

- لا يفترض ميزانية أو موعدًا لم يذكرهما العميل.
- لا يحوّل العبارات الغامضة تلقائيًا إلى معلومات مؤكدة.
- يوضح المعلومات الناقصة بدلًا من إخفائها.
- يوجّه الحالات غير المكتملة إلى المراجعة البشرية وفق القواعد المحددة.
- يقتصر دوره على التحليل والتنظيم الأولي، ولا يستبدل الحكم البشري في الحالات التي تحتاج إلى توضيح.

---

## 🗂️ Data Design

### Input Data

تُجمع بيانات الطلب من نموذج **Google Forms**. وتشمل الحقول المستخدمة في تصميم سير العمل، بحسب إعداد النموذج:

- `Client Name`
- `Email`
- `Company`
- `Request`

ويُستخدم نص `Request` لتحليل وصف العميل واستخراج التفاصيل ذات الصلة.

### Processing & Output

تُحوّل نتيجة التحليل إلى حقول منظمة يمكن تسجيلها في **Google Sheets** وربطها بمسارات التوجيه والإشعارات.

| Data Group | Example Fields |
|---|---|
| Client details | `Client Name`, `Email`, `Company` |
| Request analysis | `service_type`, `timeline`, `budget`, `request_summary` |
| Quality checks | `missing_information`, `request_status` |
| Workflow routing | `routing` |

> **Note:** يجب أن تتطابق أسماء الأعمدة والحقول النهائية مع الإعداد الفعلي في Google Forms وZapier وGoogle Sheets. لا تُعدّ أسماء الحقول أعلاه تأكيدًا لوجود أعمدة إضافية لم تُنشأ في المشروع.

---

## 🛠️ Tools & Technologies

| Tool | Role in the Project |
|---|---|
| **Google Forms** | استقبال طلبات العملاء. |
| **Zapier** | ربط الخطوات وتشغيل الأتمتة وتطبيق قواعد التوجيه. |
| **AI by Zapier** | تحليل وصف الطلب واستخراج المعلومات. |
| **Google Sheets** | حفظ بيانات الطلبات ونتائج المعالجة. |
| **Email Notifications** | تنبيه الجهات المعنية وفق إعدادات سير العمل. |
| **GitHub** | توثيق المشروع وعرض التصميم والاختبارات. |

---

## 🧩 Key Design Decisions

### 1. Analyze Before Routing

لا يُوجّه الطلب اعتمادًا على وجود استجابة جديدة فقط؛ بل يُحلل محتواه أولًا حتى يكون التوجيه مرتبطًا بالمعلومات المتاحة.

### 2. Separate AI Analysis from Workflow Rules

يحلل الذكاء الاصطناعي الطلب، بينما تنفذ قواعد **Zapier Paths** قرار التوجيه المحدد. هذا الفصل يجعل منطق النظام أوضح وأسهل في الاختبار.

### 3. Missing Information Must Stay Visible

عند غياب الميزانية أو الإطار الزمني أو وجود وصف غير واضح، تُسجل النواقص ويُستخدم مسار المراجعة البشرية بدلًا من اختلاق إجابات.

### 4. Use a Human-Review Fallback

وجود مسار `human_review` يوفّر طريقة واضحة للتعامل مع الحالات التي لا تستوفي شروط الانتقال إلى المسار المعتاد.

### 5. Distinguish Business Exceptions from Technical Failures

الطلب الذي يحتاج إلى مراجعة ليس بالضرورة فشلًا تقنيًا. يجب التمييز بين نتيجة تحليل تتطلب متابعة بشرية وبين تعطل أحد مكونات الأتمتة.

---

## 🧪 Testing & Validation

تم اختبار سيناريوهات مختلفة للتحقق من تحليل الطلبات والتوجيه. ووفق نتائج الاختبار التي أكّدتها صاحبة المشروع، **اجتازت السيناريوهات الأربعة الاختبارات**.

| Test Case | Sample Scenario | Expected Handling | Result |
|---|---|---|---|
| **01 — Complete Request** | طلب موقع إلكتروني، بميزانية `$5000`، وموعد `next month`. | `VALID` → `web_team` | ✅ Passed |
| **02 — Missing Budget** | الطلب لا يتضمن ميزانية. | `NEEDS_REVIEW` → `human_review` | ✅ Passed |
| **03 — Vague Timeline** | العميل يستخدم وصفًا غامضًا مثل `soon`. | `NEEDS_REVIEW` → `human_review` | ✅ Passed |
| **04 — Missing Key Details** | الطلب لا يذكر الميزانية ولا الإطار الزمني. | `NEEDS_REVIEW` → `human_review` | ✅ Passed |

### Validation Focus

- التحقق من أن الطلب المكتمل يسلك مسار التوجيه المحدد.
- التحقق من ظهور المعلومات الناقصة في الحالات غير المكتملة.
- التحقق من إحالة الحالات الغامضة إلى المراجعة البشرية.
- التأكد من أن الاختبار لا يعتمد على تخمين معلومات غير موجودة في الطلب.

---

## 📸 Evidence

أضيفي لقطات شاشة فعلية من مشروعك في المسارات المقترحة أدناه، ثم حدّثي الروابط لتطابق أسماء الملفات التي رفعتِها إلى المستودع.

| Evidence | Suggested File | What It Should Show |
|---|---|---|
| Workflow Overview | `screenshots/workflow-overview.png` | خطوات Zapier والروابط بين مكونات سير العمل. |
| Processed Requests | `screenshots/processed-requests.png` | سجلات الطلبات ونتائج التحليل في Google Sheets. |
| Email Notifications | `screenshots/email-notifications.png` | مثال على إشعار ناتج عن سير العمل. |

> **Before publishing:** هذه أسماء مقترحة وليست ملفات مرفقة حاليًا. لا تضيفي روابط الصور إلى README إلا بعد رفع الصور الفعلية إلى المستودع.

---

## ⚠️ Challenges & Limitations

### Current Considerations

- تعتمد جودة النتائج على وضوح وصف العميل وعلى التعليمات المهيأة لخطوة الذكاء الاصطناعي.
- قد تحتاج الحالات التي تحتوي على معلومات غامضة إلى مراجعة بشرية، حتى لو نجحت الأتمتة تقنيًا.
- يجب أن تتوافق أسماء الحقول ومخرجات الذكاء الاصطناعي مع خرائط البيانات الفعلية في خطوات Zapier وGoogle Sheets.

### Documentation Note

لم تُحدّد في المعلومات المتاحة تفاصيل مؤكدة عن أعطال فعلية أو إخفاقات تقنية واجهت هذا المشروع؛ لذلك لم تُضف أمثلة غير موثقة. يمكن تحديث هذا القسم لاحقًا إذا توفرت حالات واقعية من التنفيذ.

---

## 📚 What I Learned

### Technical Learning

- تصميم سير عمل يربط استقبال البيانات بالتحليل والمعالجة والتوجيه.
- استخدام الذكاء الاصطناعي لاستخراج بيانات منظمة من نصوص مكتوبة بلغة طبيعية.
- ربط مخرجات التحليل بقواعد توجيه واضحة داخل أداة أتمتة.
- توثيق سيناريوهات الاختبار والتحقق من سلوك المسارات.

### Problem-Solving Learning

- تقسيم المشكلة إلى مراحل مستقلة يسهل فهمها واختبارها.
- التعامل مع نقص المعلومات كحالة يجب إظهارها، لا كفراغ ينبغي ملؤه بالتخمين.
- إدراج المراجعة البشرية كجزء مقصود من تصميم النظام.

### AI Workflow Learning

- تحديد دور الذكاء الاصطناعي وحدوده داخل سير العمل.
- أهمية فصل التحليل عن قواعد اتخاذ قرار التوجيه.
- أن نجاح الأتمتة لا يعني إزالة الحاجة إلى المراجعة البشرية في جميع الحالات.

---

## 🚀 Future Improvements

- إضافة سجل أوضح لأسباب إحالة الطلب إلى `NEEDS_REVIEW`.
- تحسين التحقق من تنسيق البريد الإلكتروني والحقول المطلوبة.
- توسيع الاختبارات لتغطية اختلافات أكثر في صياغة طلبات العملاء.
- إضافة مؤشرات متابعة مثل عدد الطلبات المكتملة والطلبات المحالة للمراجعة.
- تحسين معالجة الأخطاء التقنية وإظهار حالة الفشل بوضوح.
- إضافة لقطات شاشة توثيقية وشرح مرئي لكل خطوة في سير العمل.

> تمثل هذه النقاط تحسينات مستقبلية مقترحة، وليست ميزات مؤكدة التنفيذ حاليًا.

---

## 📁 Repository Structure

يمكن تنظيم المستودع بهذا الشكل، مع الاحتفاظ بالملفات التي أنشأتِها فعلًا فقط:

```text
ai-client-intake-request-routing/
├── README.md
├── screenshots/
│   ├── workflow-overview.png
│   ├── processed-requests.png
│   └── email-notifications.png
└── docs/
    └── workflow-notes.md
```

**Important:** لا تنشئي ملفات فارغة لمجرد مطابقة الهيكل المقترح. أضيفي `screenshots/` و`docs/` فقط إذا كان لديك محتوى فعلي لرفعهما.

---

## 🔗 Project Links

| Resource | Link |
|---|---|
| **Google Form** | [Open the Client Intake Form](https://docs.google.com/forms/d/e/1FAIpQLScz3w8sWc5KHbgJeUR3C7tu2xmZm2nipk5W8OCJMdUVRUeyRQ/viewform?usp=header) |
| **Zapier Template** | [AI Client Intake & Request Routing](https://zapier.com/templates/details/ai-client-intake-request-routing-e485e6) |
| **GitHub Profile** | [doaasami9](https://github.com/doaasami9) |
| **LinkedIn Profile** | [Doaa Sami Al-Batniji](https://www.linkedin.com/in/doaa-sami-984426271/) |
| **Live Demo / Repository** | يُضاف الرابط بعد نشر النسخة النهائية إن وُجد. |

---

<div align="center">

## 👩‍💻 Author

### **DOAA SAMI AL-BATNIJI**

**Computer Engineer | Data, AI & Automation**

[![GitHub](https://img.shields.io/badge/GitHub-doaasami9-181717?style=for-the-badge&logo=github)](https://github.com/doaasami9)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/doaa-sami-984426271/)

*AI Understands. Logic Controls. Humans Judge.*

**Built with a focus on clarity, responsible AI, and practical workflow automation.**

</div>
