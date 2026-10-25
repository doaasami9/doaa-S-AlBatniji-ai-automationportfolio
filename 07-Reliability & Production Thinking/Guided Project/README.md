# 🧠 RAG with n8n
### Build a Knowledge Pipeline & RAG AI Agent

<p align="center">
  <strong>From Documents → Embeddings → Vector Database → Retrieval → AI Agent → Grounded Answers</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/AI-RAG-6C63FF?style=for-the-badge" alt="RAG">
  <img src="https://img.shields.io/badge/Workflow-n8n-EA4B71?style=for-the-badge" alt="n8n">
  <img src="https://img.shields.io/badge/Database-Supabase-3ECF8E?style=for-the-badge" alt="Supabase">
  <img src="https://img.shields.io/badge/Architecture-Knowledge%20Retrieval-243B53?style=for-the-badge" alt="Knowledge Retrieval">
</p>

---

## 🎯 01. Learning Objectives

يهدف هذا المشروع إلى بناء نظام **Retrieval-Augmented Generation (RAG)** باستخدام `n8n` و`Supabase Vector Store`، وفهم المراحل الداخلية التي تحول المستندات إلى معرفة قابلة للاسترجاع، ثم تستخدم هذه المعرفة للإجابة عن أسئلة المستخدم.

سبق أن تعاملنا مع مفهوم RAG باستخدام Dify، حيث تتولى المنصة إدارة أجزاء من العملية خلف واجهة جاهزة. في هذه التجربة، نعيد بناء المكونات الأساسية داخل n8n لفهم كيفية عمل النظام والتحكم في تدفق البيانات بين مراحله.

**By the end of this project, the key concepts are:**

- معالجة المستندات واستخراج النصوص.
- تقسيم المحتوى إلى أجزاء صغيرة (Chunking).
- إنشاء التمثيلات الرقمية للنصوص (Embeddings).
- تخزين المعرفة واسترجاعها باستخدام Vector Database.
- ربط أدوات استرجاع المعرفة بـ AI Agent.
- التمييز بين Knowledge وMemory، وبين Retrieval وGeneration.
- اختبار سلوك النظام وتحليل الأخطاء على مستوى المكونات.

---

## 🧩 02. Understanding RAG

**RAG = Retrieval-Augmented Generation**

RAG هو أسلوب يتيح لنموذج الذكاء الاصطناعي الاستفادة من مصادر معرفة خارجية عند الإجابة، بدل الاعتماد على المعرفة المتاحة داخل النموذج وحدها.

```text
USER QUESTION
      │
      ▼
RETRIEVE RELEVANT KNOWLEDGE
      │
      ▼
AUGMENT THE CONTEXT
      │
      ▼
GENERATE AN ANSWER
```

تتمثل الفكرة في البحث عن المعلومات ذات الصلة أولًا، ثم إمداد النموذج بالسياق المسترجع حتى يصوغ إجابة مرتبطة بالمصدر المتاح.

> **Core Principle:** RAG is not a single node. It is a system architecture that connects knowledge ingestion, retrieval, and answer generation.

---

## 🏗️ 03. System Architecture

يتكون النظام من مسارين مترابطين، لكل منهما مسؤولية محددة.

### Part A — Knowledge Ingestion Pipeline

مسار تجهيز المعرفة وتحويل المستندات إلى بيانات قابلة للبحث الدلالي.

```text
Google Drive
     │
     ▼
Download File
     │
     ▼
Extract Text
     │
     ▼
Document Loader
     │
     ▼
Text Splitter
     │
     ▼
OpenAI Embeddings
     │
     ▼
Supabase Vector Store
```

### Part B — RAG AI Agent

مسار استقبال أسئلة المستخدم، واسترجاع المعرفة ذات الصلة، وتوليد الإجابات.

```text
User Question
     │
     ▼
Chat Trigger
     │
     ▼
AI Agent ◄──── Gemini Chat Model
     │
     ├──────── Simple Memory
     │
     ▼
Vector Store Tool
     │
     ▼
Question Embedding
     │
     ▼
Similarity Search
     │
     ▼
Relevant Document Chunks
     │
     ▼
AI Agent + Retrieved Context
     │
     ▼
Grounded Answer
```

**Architecture Note:** المسار الأول هو Knowledge Ingestion Pipeline وليس AI Agent مستقلًا؛ أما المسار الثاني فهو RAG AI Agent الذي يستخدم قاعدة المعرفة أداةً لاسترجاع المعلومات.

---

## 🔄 04. End-to-End Data Flow

### Knowledge Preparation

تبدأ العملية بمستند محفوظ في Google Drive، ثم يُحمّل ويُستخرج نصه، ويُقسّم إلى أجزاء، وتُنشأ له Embeddings قبل تخزينه في قاعدة البيانات المتجهية.

```text
PDF → Text → Chunks → Embeddings → Vector Database
```

### Knowledge Retrieval

عند طرح سؤال، يُحوّل السؤال إلى تمثيل رقمي، ثم يُستخدم البحث بالتشابه لاسترجاع أجزاء المعرفة المناسبة. يستفيد النموذج من هذه النتائج لصياغة الإجابة.

```text
Question → Query Embedding → Similarity Search
         → Relevant Chunks → LLM → Answer
```

---

# 🟢 PART I — KNOWLEDGE INGESTION PIPELINE

## 🗄️ 05. Supabase Vector Database

تُستخدم Supabase لتخزين المستندات وأجزائها والتمثيلات الرقمية المرتبطة بها، بالاعتماد على PostgreSQL وامتداد `pgvector`.

### Database Preparation

1. إنشاء مشروع Supabase.
2. تفعيل امتداد `vector` من إعدادات قاعدة البيانات.
3. تجهيز جدول المستندات ودالة البحث المتجهي وفق بنية Supabase المناسبة.
4. التحقق من توافق أبعاد عمود `embedding` مع نموذج Embeddings المستخدم.

### Data Structure

| Field | Purpose |
|---|---|
| `id` | معرّف السجل |
| `content` | محتوى الجزء النصي |
| `metadata` | معلومات مصدر المستند |
| `embedding` | التمثيل الرقمي للمحتوى |

يمكن أن تتضمن `metadata` معلومات مثل اسم الملف ومعرّفه ومصدره، مما يساعد على تتبع مصدر المعلومات المسترجعة.

### Similarity Search

تُستخدم دالة مثل `match_documents` للبحث عن الأجزاء الأقرب دلاليًا إلى السؤال، بشرط أن يتوافق اسم الدالة وتوقيعها مع إعدادات قاعدة البيانات المستخدمة في n8n.

**Engineering Consideration:** يجب أن تتوافق أبعاد المتجهات المخزنة مع النموذج المستخدم لإنشائها؛ فاختلاف الأبعاد قد يؤدي إلى أخطاء أثناء الإدخال أو البحث.

---

## 🔵 06. Google Drive Trigger

تبدأ عملية الأتمتة بمراقبة مجلد محدد في Google Drive.

**Example Folder Structure**

```text
AI Automation Knowledge/
├── RAG.pdf
├── AI Agents.pdf
└── Automation.pdf
```

عند اكتشاف ملف جديد أو تغيير يطابق إعدادات المشغّل، يبدأ سير العمل وفق آلية التشغيل والجدولة المتاحة في إصدار n8n المستخدم.

توضح هذه الخطوة مفهوم **Event-Driven Automation**، حيث يبدأ التنفيذ استجابةً لحدث بدل الحاجة إلى تشغيل العملية يدويًا في كل مرة.

---

## 🟣 07. File Identification & Download

### Edit Fields

يُستخرج معرّف الملف `file_id` من بيانات Google Drive، لاستخدامه في تحميل الملف وربط أجزاء المعرفة بمصدرها.

### Google Drive — Download File

يُستخدم `file_id` لتحديد المستند المطلوب وتحميل محتواه إلى سير العمل.

```text
Google Drive Trigger
        │
        ▼
Extract file_id
        │
        ▼
Download File
        │
        ▼
PDF Binary Data
```

**Why It Matters:** بيانات الملف الوصفية لا تعادل محتوى المستند نفسه؛ لذلك يجب تحميل الملف قبل تنفيذ عملية استخراج النص.

---

## 🟡 08. Extract From File

بعد تحميل ملف PDF، تُستخدم عقدة `Extract From File` لاستخراج النص الذي ستتم معالجته في المراحل التالية.

```text
PDF Binary Data
       │
       ▼
Extract From File
       │
       ▼
Raw Text
```

لا يمكن الانتقال مباشرةً من ملف PDF ثنائي إلى Embeddings دون توفير المحتوى النصي المناسب؛ إذ يحتاج النظام إلى نص يمكن تقسيمه ومعالجته.

---

## 🧩 09. Supabase Vector Store — Document Ingestion

تُستخدم عقدة `Supabase Vector Store` لإدخال المستندات وأجزائها وتمثيلاتها الرقمية إلى قاعدة البيانات.

### Core Configuration

| Setting | Purpose |
|---|---|
| Credential | إعداد اتصال Supabase بأمان |
| Table Name | تحديد جدول المستندات |
| Query Name | تحديد دالة البحث المتجهي عند الحاجة |
| Operation | اختيار عملية إدخال المستندات |

عند اتباع بنية Supabase Quick Start الافتراضية، قد يكون اسم الجدول `documents` واسم الدالة `match_documents`. يجب التحقق من أن البنية الفعلية تتطابق مع إعدادات n8n.

**Security:** تحفظ مفاتيح API وبيانات الاتصال الحساسة داخل نظام Credentials المناسب. لا تُضمّن مفاتيح Supabase السرية أو مفاتيح Service Role في ملفات GitHub أو صور الشاشة المنشورة.

---

## 🟠 10. Document Loader & Text Splitter

### Default Data Loader

يتولى تحويل المحتوى إلى Document objects تحتوي على النص وبيانات المصدر اللازمة للمعالجة والتخزين.

### Recursive Character Text Splitter

يقسّم النص إلى أجزاء أصغر تسمى `Chunks`، مما يتيح استرجاع المقاطع الأكثر صلة بالسؤال بدل التعامل مع المستند كاملًا كوحدة واحدة.

```text
Extracted Document
        │
        ▼
Document Loader
        │
        ▼
Recursive Character Text Splitter
        │
        ├── Chunk 1
        ├── Chunk 2
        ├── Chunk 3
        └── ...
```

### Chunk Size & Overlap

يمكن البدء بقيم تجريبية مثل:

| Parameter | Example | Purpose |
|---|---:|---|
| Chunk Size | 500 | تحديد الحجم الابتدائي للجزء |
| Chunk Overlap | 50 | الاحتفاظ بتداخل محدود بين الأجزاء |

هذه القيم أمثلة أولية وليست إعدادات مثالية لكل المستندات. يعتمد الاختيار على طبيعة المحتوى وطول الفقرات والأسئلة وجودة نتائج الاسترجاع.

> **Engineering Insight:** Chunking is an engineering decision, not a magic number.

---

## 🔢 11. OpenAI Embeddings

تُستخدم Embeddings لتحويل النص إلى تمثيل رقمي يمكن استخدامه في البحث عن التشابه الدلالي.

**Example Model:** `text-embedding-3-small`

```text
"AI Agent uses external tools"
               │
               ▼
       Embedding Model
               │
               ▼
       Numerical Vector
```

لا يعتمد البحث الدلالي على تطابق الكلمات حرفيًا فقط، بل يقارن التمثيلات الرقمية للعثور على محتوى ذي صلة بالمعنى.

يجب استخدام إعدادات متوافقة بين نموذج Embeddings المستخدم عند تخزين المستندات والنموذج المستخدم عند البحث عنها.

---

## 🧪 12. Ingestion Verification

بعد إعداد العقد، تُشغّل عملية الإدخال وفق إعدادات الاختبار أو المشغّل المتاحة.

### Verification Checklist

- [ ] استقبال بيانات الملف من Google Drive.
- [ ] استخراج `file_id` الصحيح.
- [ ] تحميل ملف PDF.
- [ ] استخراج النص بنجاح.
- [ ] تحويل المحتوى إلى Documents وChunks.
- [ ] إنشاء Embeddings.
- [ ] إدخال السجلات إلى Supabase.
- [ ] التحقق من ظهور السجلات في جدول المستندات.

**Expected Evidence:** ظهور سجلات تتضمن المحتوى والبيانات الوصفية والـ Embeddings المتوافقة مع إعدادات قاعدة البيانات.

نجاح هذه المرحلة يعني أن المعرفة أصبحت مخزنة وقابلة للاستخدام في مرحلة الاسترجاع، لكنه لا يثبت وحده أن إجابات الوكيل دقيقة؛ فهذا يتطلب اختبارات مستقلة.

---

# 🤖 PART II — RAG AI AGENT

## 🎯 13. Agent Objective

الهدف هو بناء وكيل يستقبل أسئلة المستخدم، ويسترجع المعرفة ذات الصلة من قاعدة البيانات، ثم يستخدم النموذج اللغوي لصياغة إجابة تستند إلى المحتوى المسترجع.

```text
Understand Question
        │
        ▼
Select Knowledge Tool
        │
        ▼
Retrieve Relevant Chunks
        │
        ▼
Use Retrieved Context
        │
        ▼
Generate Answer
```

---

## 🟢 14. Chat Trigger

تُستخدم عقدة `When Chat Message Received` أو التسمية المقابلة في إصدار n8n لاستقبال رسالة المستخدم وبدء مسار الإجابة.

**Example Input**

`What is RAG?`

يمر السؤال إلى AI Agent ليقرر كيفية التعامل معه، بما في ذلك استخدام أداة المعرفة عند الحاجة.

---

## 🧠 15. AI Agent & Gemini Chat Model

يمثل AI Agent مركز اتخاذ القرار داخل هذا المسار، بينما يتولى نموذج المحادثة فهم السؤال وصياغة الإجابة باستخدام السياق المتاح.

### Agent Components

| Component | Responsibility |
|---|---|
| AI Agent | تنسيق خطوات الاستجابة واستخدام الأدوات |
| Gemini Chat Model | فهم السؤال وتوليد الإجابة |
| Simple Memory | الحفاظ على سياق المحادثة |
| Supabase Vector Store Tool | استرجاع المعلومات من قاعدة المعرفة |
| OpenAI Embeddings | تمثيل السؤال رقميًا لأغراض البحث |

**Important Distinction:** Gemini هو نموذج لغوي، وليس قاعدة المعرفة نفسها. مصدر المعرفة في هذا المشروع هو المستندات المخزنة في Supabase.

---

## 🧠 16. Simple Memory vs Knowledge Base

تساعد `Simple Memory` على الاحتفاظ بسياق المحادثة، بما يتيح فهم الأسئلة اللاحقة التي تعتمد على رسائل سابقة.

**Example Conversation**

```text
User: What is RAG?
Agent: RAG retrieves relevant knowledge to support an answer.

User: Why is it useful?
Agent: The follow-up question is interpreted in context.
```

| Memory | Vector Store |
|---|---|
| يحتفظ بسياق المحادثة | يخزن المعرفة الخارجية |
| يدعم فهم الأسئلة اللاحقة | يسترجع أجزاء من المستندات |
| يرتبط بالتفاعل مع المستخدم | يرتبط بقاعدة المعرفة |
| لا يحل محل المستندات | لا يحل محل ذاكرة المحادثة |

---

## 🔵 17. Supabase Vector Store as an AI Tool

في هذا المسار، تُستخدم عقدة `Supabase Vector Store` أداةً للوكيل بدل استخدامها لإدخال المستندات.

### Tool Configuration

| Setting | Example |
|---|---|
| Operation | Retrieve Documents (As Tool for AI Agent) |
| Tool Name | `search_course_knowledge_base` |
| Tool Description | `Search the course knowledge base to retrieve relevant information for answering questions about the course.` |
| Result Limit | 4 — قيمة ابتدائية للاختبار |

يساعد اسم الأداة ووصفها الواضحان النموذج على فهم وظيفتها وتحديد الحالات التي تحتاج إلى استخدامها.

يمكن تجربة حدود استرجاع مختلفة، مثل 3 أو 4 أو 5 نتائج، ومقارنة جودة المقاطع المسترجعة قبل اعتماد الإعداد المناسب.

---

## 🔢 18. Query Embeddings & Retrieval

عندما يطرح المستخدم سؤالًا، يُحوّل السؤال إلى Embedding باستخدام نموذج متوافق مع النموذج المستخدم في تجهيز المستندات.

```text
USER QUESTION
      │
      ▼
OpenAI Embeddings
      │
      ▼
QUERY VECTOR
      │
      ▼
SUPABASE SIMILARITY SEARCH
      │
      ▼
TOP-K RELEVANT CHUNKS
      │
      ▼
AI AGENT
```

### Two Embedding Operations

**During Ingestion:** تُنشأ Embeddings لأجزاء المستندات وتُخزّن في قاعدة البيانات.

**During Retrieval:** يُنشأ Embedding للسؤال، ثم يُقارن بالتمثيلات المخزنة للعثور على المقاطع ذات الصلة.

هذا الفصل أساسي لفهم كيفية انتقال المعرفة من المستندات إلى الإجابة.

---

## 🛡️ 19. Grounded Answering & System Instructions

يحتاج الوكيل إلى تعليمات تحدد مصدر المعلومات المقبول، ومتى يستخدم أداة المعرفة، وكيف يتصرف عندما لا يجد معلومات كافية.

### Instruction Design

```text
You are a helpful course assistant.

Use the search_course_knowledge_base tool whenever
the user's question requires information from the course
knowledge base.

Answer using the retrieved knowledge.

Do not invent information that is not supported
by the retrieved content.

If the required information cannot be found in the
knowledge base, clearly say that you do not have
enough information to answer.
```

### Grounding Principles

- استخدام قاعدة المعرفة عند الحاجة إلى معلومات منها.
- الاستناد إلى المحتوى المسترجع عند الإجابة عن الأسئلة المعرفية.
- عدم تقديم معلومات غير مدعومة على أنها حقائق مؤكدة.
- التصريح بعدم كفاية المعلومات عند غياب الدليل المناسب.
- عدم افتراض أن كل رسالة تحتاج إلى بحث؛ فالتحيات والرسائل العامة لا تتطلب بالضرورة استرجاع المعرفة.

**Reliability Principle:** وجود RAG في النظام لا يضمن صحة كل إجابة تلقائيًا؛ إذ تعتمد الجودة على المحتوى المسترجع، وإعدادات البحث، وتعليمات الوكيل، وطريقة تقييم النتائج.

---

## 🧪 20. RAG Agent Testing Strategy

يجب تقييم النظام عبر حالات مختلفة، وليس الاكتفاء باختبار سؤال واحد أو التأكد من أن الوكيل يعيد إجابة.

### Test 1 — Known Information

**Input**

`What is RAG?`

**Expected Behavior**

- العثور على محتوى ذي صلة في قاعدة المعرفة.
- تمرير المقاطع المسترجعة إلى الوكيل.
- صياغة إجابة مدعومة بالمحتوى المتاح.

**What It Validates:** استرجاع المعلومات المعروفة واستخدامها في الإجابة.

### Test 2 — Semantic Search

**Source Statement**

`AI Agents can use external tools.`

**Input**

`Can an AI Agent interact with external systems?`

**Expected Behavior**

العثور على المحتوى ذي الصلة رغم اختلاف صياغة السؤال عن صياغة النص الأصلي.

**What It Validates:** قدرة البحث الدلالي على استرجاع محتوى ذي معنى مرتبط بالسؤال، وليس مجرد تطابق حرفي للكلمات.

### Test 3 — Unknown Information

**Input**

`What is the admission policy of Harvard University?`

**Expected Behavior**

إذا لم تتضمن قاعدة المعرفة معلومات كافية عن سياسة القبول، ينبغي للوكيل توضيح عدم توفر المعلومات اللازمة بدل اختلاق إجابة.

**What It Validates:** التعامل مع المعلومات غير الموجودة والحد من الإجابات غير المدعومة.

### Test 4 — Conversation Memory

**First Message**

`What is RAG?`

**Follow-up**

`Why is it useful?`

**Expected Behavior**

فهم السؤال اللاحق في سياق المحادثة، مع استخدام قاعدة المعرفة عند الحاجة إلى معلومات إضافية.

**What It Validates:** التفاعل بين ذاكرة المحادثة واسترجاع المعرفة.

### Test Evidence

تُوثق لكل حالة المدخلات، والنتيجة المتوقعة، والنتيجة الفعلية، وأي ملاحظات أو أخطاء ظهرت أثناء التنفيذ.

> **Testing Note:** هذه حالات الاختبار ومعايير نجاحها المتوقعة. تُسجّل الحالة على أنها Passed فقط بعد تنفيذها والتحقق من النتيجة الفعلية.

---

## 🔍 21. Debugging & Failure Analysis

يساعد تقسيم النظام إلى طبقات مستقلة على تحديد موقع المشكلة بدل اعتبار سير العمل كله وحدة واحدة.

| Symptom | What to Inspect |
|---|---|
| عدم ظهور سجلات في Supabase | Trigger، تحميل الملف، استخراج النص، Loader، Splitter، Embeddings |
| فشل البحث في قاعدة البيانات | اسم الجدول، دالة البحث، إعدادات الاتصال، بنية قاعدة البيانات |
| خطأ في أبعاد المتجهات | توافق نموذج Embeddings مع تعريف عمود `vector` |
| إجابة غير مدعومة | المقاطع المسترجعة، وصف الأداة، تعليمات الوكيل |
| نتائج استرجاع ضعيفة | Chunk Size، Chunk Overlap، Result Limit، جودة المستندات |
| عدم فهم السؤال اللاحق | إعدادات الذاكرة ومعرّف الجلسة وسياق المحادثة |

### Debugging Method

1. تحديد الطبقة التي يظهر عندها الخطأ.
2. مراجعة مدخلات العقدة ومخرجاتها.
3. التحقق من إعدادات الاتصال وأسماء الجداول والدوال.
4. تعديل متغير واحد في كل مرة.
5. إعادة الاختبار وتوثيق النتيجة.

---

## 🧠 22. RAG Mental Model

يلخص المخطط التالي العلاقة بين المعرفة المخزنة وسؤال المستخدم وعملية توليد الإجابة.

```text
                KNOWLEDGE
                    │
                Documents
                    │
                    ▼
               Extract Text
                    │
                    ▼
                 Chunking
                    │
                    ▼
                Embeddings
                    │
                    ▼
              Vector Database
                    ▲
                    │
USER ──────► Question Embedding
                    │
                    ▼
             Similarity Search
                    │
                    ▼
             Relevant Chunks
                    │
                    ▼
             Context + Question
                    │
                    ▼
                   LLM
                    │
                    ▼
                 Answer
```

الهدف هو فهم أن الإجابة تنتج عن تفاعل عدة مكونات؛ فقاعدة البيانات تسترجع المعرفة، بينما يتولى النموذج اللغوي صياغة الإجابة باستخدام السياق المتاح.

---

## 🔄 23. Retrieval vs Generation

| Retrieval | Generation |
|---|---|
| يبحث عن المعلومات ذات الصلة | يصوغ الإجابة |
| يعتمد على قاعدة المعرفة وآلية البحث | يعتمد على النموذج والسياق المتاح |
| يعيد المقاطع المناسبة | يحول السؤال والسياق إلى استجابة |
| تتأثر جودته بالمستندات والتقسيم والبحث | تتأثر جودته بالسياق والتعليمات والنموذج |

**Retrieval**

```text
Question → Embedding → Vector Search → Relevant Chunks
```

**Generation**

```text
Question + Retrieved Context → LLM → Answer
```

الفصل بين المرحلتين يسهل تشخيص المشكلات: فقد يكون الخلل في العثور على المعلومة، أو في استخدام المعلومة المسترجعة لصياغة الإجابة.

---

## 🔄 24. Dify RAG vs n8n RAG

تعرض المقارنة التالية اختلاف طريقة البناء، وليس حكمًا بأن إحدى المنصتين أفضل في جميع الحالات.

| Concept | Dify | n8n Implementation |
|---|---|---|
| Knowledge Base | إدارة المعرفة داخل Dify | Supabase Vector Store |
| Document Processing | إمكانات مدمجة حسب الإعداد | Workflow Nodes |
| Chunking | إعدادات تقسيم المعرفة | Text Splitter |
| Embeddings | مزود النموذج وإعداداته | OpenAI Embeddings |
| Vector Storage | إدارة المنصة أو إعداداتها | Supabase |
| Retrieval | إمكانات الاسترجاع في Dify | Vector Store |
| LLM | النموذج المهيأ | Gemini Chat Model |
| Memory | حسب التطبيق والإعداد | Simple Memory |
| Agent | تطبيق أو وكيل Dify | AI Agent |
| Tool Integration | إمكانات المنصة | Vector Store Tool |

**Learning Insight:** استخدام Dify يساعد على بناء التطبيقات، بينما يتيح تنفيذ المسار داخل n8n دراسة تدفق البيانات والتكاملات والمكونات التي تعمل خلف عملية RAG.

---

## 🏗️ 25. Final Architecture

### Knowledge Ingestion

```text
Google Drive Trigger
        │
        ▼
Edit Fields — File ID
        │
        ▼
Google Drive — Download
        │
        ▼
Extract From File
        │
        ▼
Default Data Loader
        │
        ├── Recursive Character Text Splitter
        │
        └── OpenAI Embeddings
                    │
                    ▼
          Supabase Vector Store
```

### RAG AI Agent

```text
Chat Trigger
     │
     ▼
AI Agent ◄──── Gemini Chat Model
     │
     ├──────── Simple Memory
     │
     ▼
Supabase Vector Store Tool
     │
     ▼
OpenAI Embeddings
     │
     ▼
Similarity Search
     │
     ▼
Relevant Documents
     │
     ▼
AI Agent + Retrieved Context
     │
     ▼
Final Answer
```

**Implementation Note:** المخططات توضح العلاقات المنطقية بين المكونات. قد تختلف أسماء العقد والتوصيلات التفصيلية بحسب إصدار n8n والإعدادات المستخدمة.

---

## 📚 26. Key Engineering Takeaways

| Question | Key Understanding |
|---|---|
| لماذا نحتاج Vector Database؟ | لتخزين المتجهات وإجراء البحث الدلالي. |
| لماذا نستخدم Chunking؟ | لاسترجاع أجزاء محددة بدل تحميل المستند كاملًا إلى سياق الإجابة. |
| ما وظيفة Embeddings؟ | تحويل النص إلى تمثيل رقمي قابل للمقارنة الدلالية. |
| لماذا نستخدم Embeddings مرتين؟ | لتمثيل المستندات أثناء الإدخال والأسئلة أثناء الاسترجاع. |
| ما دور Supabase؟ | تخزين المستندات والمتجهات وإتاحة البحث فيها وفق الإعدادات. |
| ما دور AI Agent؟ | تنسيق الاستجابة واختيار الأدوات واستخدام نتائجها. |
| ما دور Memory؟ | الحفاظ على سياق المحادثة. |
| ما دور Vector Store Tool؟ | تمكين الوكيل من البحث في قاعدة المعرفة. |
| ما الفرق بين Retrieval وGeneration؟ | الاسترجاع يعثر على المعلومات، والتوليد يصوغ الإجابة. |
| ما مفهوم RAG؟ | استرجاع المعرفة، وإضافتها إلى السياق، ثم توليد الإجابة. |

---

## ⚠️ 27. Security & Reliability Considerations

يتطلب نقل التجربة من بيئة التعلم إلى استخدام تشغيلي الاهتمام بأكثر من نجاح تنفيذ العقد.

- **Credential Security:** تخزين المفاتيح والبيانات الحساسة في Credentials وعدم نشرها داخل المستودع.
- **Knowledge Quality:** التأكد من صلاحية المستندات ومحتواها قبل إدخالها إلى قاعدة المعرفة.
- **Grounding:** اختبار مدى ارتباط الإجابات بالمقاطع المسترجعة.
- **Failure Handling:** تسجيل الأخطاء وتحديد كيفية التعامل مع فشل التحميل أو الإدخال أو البحث.
- **Observability:** الاحتفاظ بمعلومات تشغيل كافية لتشخيص المشكلات دون كشف الأسرار.
- **Evaluation:** اختبار المعلومات المعروفة، والأسئلة المعاد صياغتها، والمعلومات غير الموجودة، والأسئلة المتتابعة.
- **Configuration Consistency:** الحفاظ على توافق نماذج Embeddings وأبعاد المتجهات وإعدادات قاعدة البيانات بين الإدخال والاسترجاع.

هذه اعتبارات تصميم وتحقق؛ ولا يعني إدراجها أن جميع آليات الحماية والمراقبة والإنتاج قد نُفذت بالفعل.

---

## 🚀 28. Future Improvements

يمكن تطوير المشروع لاحقًا من خلال:

1. مقارنة أحجام Chunks مختلفة وقياس أثرها في جودة الاسترجاع.
2. اختبار قيم مختلفة لـ Top K ومقارنة المقاطع الناتجة.
3. تحسين تتبع مصدر المعلومة باستخدام Metadata.
4. توثيق نتائج الاختبارات بأدلة قابلة للمراجعة.
5. إضافة معالجة واضحة للأخطاء وإعادة المحاولة عند الحاجة.
6. اختبار التكرار عند إعادة إدخال المستند نفسه لتجنب السجلات غير المرغوبة.
7. تطوير آلية تقييم تقيس جودة Retrieval ومدى استناد الإجابات إلى المصادر.
8. مراجعة الصلاحيات وسياسات الاحتفاظ بالبيانات قبل الاستخدام الفعلي.

---

## 🖼️ 29. Project Screenshots

تعرض الصور التالية المكونات الأساسية للمشروع، وتساعد على مراجعة بنية مسارات العمل وقاعدة البيانات وأدلة الاختبار.

### 01 — Knowledge Ingestion Pipeline

![Knowledge Ingestion Pipeline](Screenshots/01%20%E2%80%94%20Knowledge%20Ingestion%20Pipeline.png)

*مسار تجهيز المستندات وتحويلها إلى معرفة قابلة للبحث.*

### 02 — RAG AI Agent Workflow

![RAG AI Agent Workflow](Screenshots/02%20%E2%80%94%20RAG%20AI%20Agent%20Workflow.png)

*بنية الوكيل الذكي واتصاله بنموذج المحادثة والذاكرة وأداة استرجاع المعرفة.*

### 03 — Supabase Vector Database

![Supabase Vector Database](Screenshots/03%20%E2%80%94%20Supabase%20Vector%20Database.png)

*عرض قاعدة البيانات المتجهية والسجلات المستخدمة في تخزين المعرفة.*

### 04 — Test Evidence

![Test 2](Screenshots/test2.png)

*دليل اختبار إضافي — يُحدد وصفه النهائي وفق محتوى الصورة الفعلي.*

### 05 — Knowledge Retrieval Test

![Knowledge Retrieval Test](Screenshots/%D8%A7%D8%AE%D8%AA%D8%A8%D8%A7%D8%B1%20%D9%88%D8%AC%D9%88%D8%AF%20%D8%A7%D9%84%D9%85%D8%B9%D9%84%D9%88%D9%85%D8%A9.png)

*صورة مرتبطة باختبار وجود المعلومة — تُراجع النتيجة الظاهرة فيها قبل توثيقها بوصفها حالة ناجحة.*

[View All Screenshots](Screenshots/)

---

## 🔗 30. Project Resources

- **Workflow Platform:** [n8n](https://n8n.io/)
- **Vector Database:** [Supabase](https://supabase.com/)
- **Embedding Model Documentation:** [OpenAI Embeddings](https://platform.openai.com/docs/guides/embeddings)
- **Project Screenshots:** [View Screenshots](Screenshots/)
- **GitHub Repository:** [AI Automation Portfolio](https://github.com/doaasami9/doaa-S-AlBatniji-ai-automationportfolio)

---

## 👩‍💻 31. Author

**DOAA SAMI AL-BATNIJI**

*Computer Engineer | Data, AI & Automation*

[LinkedIn](#) · [GitHub](https://github.com/doaasami9)

---

<p align="center">
  <strong>RAG is not just about generating answers.</strong>
  <br>
  It is about retrieving the right knowledge, using it responsibly, and evaluating the result.
</p>

<p align="center">
  <sub>Unit 07 — Reliability & Production Thinking · Guided Project</sub>
</p>

