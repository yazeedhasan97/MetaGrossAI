<div align="center" dir="rtl">
  <img src="web/public/logo-light.png" width="200" alt="MetaGrossAI logo">
  <h1>MetaGrossAI</h1>
</div>

<div dir="rtl">

## 💡 ما هو MetaGrossAI؟
**MetaGrossAI** هو محرك رائد للتوليد المعزز بالاسترجاع (RAG) يدمج تقنية RAG المتطورة مع قدرات الوكلاء (Agents) لإنشاء طبقة سياق فائقة لنماذج اللغة الكبيرة (LLMs). إنه يوفر سير عمل RAG مبسط وقابل للتكيف مع الشركات من أي حجم. بفضل محرك السياق المدمج وقوالب الوكلاء الجاهزة، يُمكّن MetaGrossAI المطورين من تحويل البيانات المعقدة إلى أنظمة ذكاء اصطناعي عالية الدقة وجاهزة للإنتاج بكفاءة ودقة استثنائيتين.

## 🌟 الميزات الرئيسية
### 🍭 **"جودة المدخلات تعني جودة المخرجات"**
- استخراج المعرفة بناءً على الفهم العميق للمستندات من البيانات غير المهيكلة ذات التنسيقات المعقدة.
- العثور على "إبرة في كومة قش البيانات" من عدد غير محدود من الرموز (tokens).

### 🍱 **تقسيم النصوص المستند إلى القوالب**
- ذكي وقابل للتفسير.
- الكثير من خيارات القوالب للاختيار من بينها.

### 🌱 **اقتباسات موثقة للحد من الهلوسة**
- تصور وتقسيم النص للسماح بالتدخل البشري.
- عرض سريع للمراجع الرئيسية والاقتباسات القابلة للتتبع لدعم الإجابات الموثقة.

### 🍔 **التوافق مع مصادر البيانات غير المتجانسة**
- يدعم Word وشرائح العرض و Excel و txt والصور والنسخ الممسوحة ضوئيًا والبيانات المهيكلة وصفحات الويب والمزيد.

### 🛀 **سير عمل RAG آلي وسهل**
- نماذج لغة (LLMs) ونماذج تضمين قابلة للتكوين.
- واجهات برمجة تطبيقات (APIs) بديهية للتكامل السلس مع الأعمال.

## 🎬 الاستضافة الذاتية
### 📝 المتطلبات الأساسية
- وحدة المعالجة المركزية (CPU) >= 4 نوى
- ذاكرة الوصول العشوائي (RAM) >= 16 جيجابايت
- القرص الصلب >= 50 جيجابايت
- Docker >= 24.0.0 و Docker Compose >= v2.26.1
- Python >= 3.13

### 🚀 تشغيل الخادم
1. تأكد من أن `vm.max_map_count` >= 262144:
   ```bash
<<<<<<< HEAD
   $ sudo sysctl -w vm.max_map_count=262144
=======
   $ git clone https://github.com/infiniflow/ragflow.git
   ```
3. ابدأ تشغيل الخادم باستخدام صور Docker المعدة مسبقًا:

> [!CAUTION]
> جميع الصور Docker مصممة لمنصات x86. لا نعرض حاليًا صور Docker لـ ARM64.
> إذا كنت تستخدم نظامًا أساسيًا ARM64، فاتبع [هذا الدليل](https://ragflow.io/docs/dev/build_docker_image) لإنشاء صورة Docker متوافقة مع نظامك.

> يقوم الأمر أدناه بتنزيل إصدار `v0.26.1` من الصورة RAGFlow Docker. راجع الجدول التالي للحصول على أوصاف لإصدارات RAGFlow المختلفة. لتنزيل إصدار RAGFlow مختلف عن `v0.26.1`، قم بتحديث المتغير `RAGFLOW_IMAGE` وفقًا لذلك في **docker/.env** قبل استخدام `docker compose` لبدء تشغيل الخادم.

```bash
   $ cd ragflow/docker

   # git checkout v0.26.1
   # Optional: use a stable tag (see releases: https://github.com/infiniflow/ragflow/releases)
   # This step ensures the **entrypoint.sh** file in the code matches the Docker image version.

   # Use CPU for DeepDoc tasks:
   $ docker compose -f docker-compose.yml up -d

   # To use GPU to accelerate DeepDoc tasks:
   # sed -i '1i DEVICE=gpu' .env
   # docker compose -f docker-compose.yml up -d
```

> ملاحظة: قبل `v0.22.0`، قدمنا ​​كلتا الصورتين بنماذج embedding وصورًا رفيعة بدون نماذج embedding. التفاصيل على النحو التالي:

| RAGFlow علامة الصورة | حجم الصورة (جيجابايت) | هل لديه نماذج embedding؟ | مستقر؟        |
|-------------------|-----------------|-----------------------|----------------|
| v0.21.1 | &approx;9 | ✔️ | إصدار مستقر |
| v0.21.1-slim | &approx;2 | ❌ | إصدار مستقر |

> بدءًا من `v0.22.0`، نقوم بشحن الإصدار النحيف فقط ولم نعد نلحق اللاحقة **-slim** بعلامة الصورة.

4. التحقق من حالة الخادم بعد تشغيل الخادم:

   ```bash
   $ docker logs -f docker-ragflow-cpu-1
   ```

   _النتيجة التالية تؤكد الإطلاق الناجح للنظام:_

   ```bash

         ____   ___    ______ ______ __
        / __ \ /   |  / ____// ____// /____  _      __
       / /_/ // /| | / / __ / /_   / // __ \| | /| / /
      / _, _// ___ |/ /_/ // __/  / // /_/ /| |/ |/ /
     /_/ |_|/_/  |_|\____//_/    /_/ \____/ |__/|__/

    * Running on all addresses (0.0.0.0)
   ```

   > إذا تخطيت خطوة التأكيد هذه وقمت بتسجيل الدخول مباشرة إلى RAGFlow، فقد يعرض متصفحك تنبيه `network abnormal`
   > خطأ لأنه في تلك اللحظة، قد لا تتم تهيئة RAGFlow بشكل كامل.
   >
5. في متصفح الويب الخاص بك، أدخل عنوان IP الخاص بالخادم الخاص بك وقم بتسجيل الدخول إلى RAGFlow.

   > باستخدام الإعدادات الافتراضية، ما عليك سوى إدخال `http://IP_OF_YOUR_MACHINE` (**من دون** رقم المنفذ) كإعداد افتراضي
   > HTTP يمكن حذف منفذ العرض `80` عند استخدام التكوينات الافتراضية.
   >
6. في [service_conf.yaml.template](./docker/service_conf.yaml.template)، حدد المصنع LLM المطلوب في `user_default_llm` وقم بالتحديث
   الحقل `API_KEY` مع مفتاح API المقابل.

   > راجع [llm_api_key_setup](https://ragflow.io/docs/dev/llm_api_key_setup) لمزيد من المعلومات.
   >

   _العرض بدأ!_

## 🔧 التكوينات

عندما يتعلق الأمر بتكوينات النظام، ستحتاج إلى إدارة الملفات التالية:

- [.env](./docker/.env): يحتفظ بالإعدادات الأساسية للنظام، مثل `SVR_HTTP_PORT`، `MYSQL_PASSWORD`، و
  `MINIO_PASSWORD`.
- [service_conf.yaml.template](./docker/service_conf.yaml.template): تكوين الخدمات الخلفية. سيتم ملء متغيرات البيئة في هذا الملف تلقائيًا عند بدء تشغيل الحاوية Docker. ستكون أي متغيرات بيئة تم تعيينها داخل حاوية Docker متاحة للاستخدام، مما يسمح لك بتخصيص سلوك الخدمة استنادًا إلى بيئة النشر.
- [docker-compose.yml](./docker/docker-compose.yml): يعتمد النظام على [docker-compose.yml](./docker/docker-compose.yml) لبدء التشغيل.

> يوفر الملف [./docker/README](./docker/README.md) وصفًا تفصيليًا لإعدادات البيئة والخدمة
> التكوينات التي يمكن استخدامها كـ `${ENV_VARS}` في ملف [service_conf.yaml.template](./docker/service_conf.yaml.template).

لتحديث منفذ العرض الافتراضي HTTP (80)، انتقل إلى [docker-compose.yml](./docker/docker-compose.yml) وقم بتغيير `80:80`
إلى `<YOUR_SERVING_PORT>:80`.

تتطلب تحديثات التكوينات المذكورة أعلاه إعادة تشغيل جميع الحاويات لتصبح سارية المفعول:

> ```bash
> $ docker compose -f docker-compose.yml up -d
> ```

### تبديل محرك المستندات من Elasticsearch إلى Infinity

RAGFlow يستخدم Elasticsearch بشكل افتراضي لتخزين النص الكامل والمتجهات. للتبديل إلى [Infinity](https://github.com/infiniflow/infinity/)، اتبع الخطوات التالية:

1. إيقاف كافة الحاويات قيد التشغيل:

   ```bash
   $ docker compose -f docker/docker-compose.yml down -v
   ```

> [!WARNING]
> `-v` سوف يحذف docker وحدات تخزين الحاوية، وسيتم مسح البيانات الموجودة.

2. اضبط `DOC_ENGINE` في **docker/.env** على `infinity`.
3. ابدأ الحاويات:

   ```bash
   $ docker compose -f docker-compose.yml up -d
   ```

> [!WARNING]
> التبديل إلى Infinity على جهاز Linux/arm64 غير مدعوم رسميًا بعد.

## 🔧 أنشئ صورة Docker

يبلغ حجم هذه الصورة حوالي 2 غيغابايت وتعتمد على خدمات LLM وembedding الخارجية.

```bash
git clone https://github.com/infiniflow/ragflow.git
cd ragflow/
docker build --platform linux/amd64 -f Dockerfile -t infiniflow/ragflow:nightly .
```

أو إذا كنت خلف وكيل، فيمكنك تمرير وسيطات الوكيل:

```bash
docker build --platform linux/amd64 \
  --build-arg http_proxy=http://YOUR_PROXY:PORT \
  --build-arg https_proxy=http://YOUR_PROXY:PORT \
  -f Dockerfile -t infiniflow/ragflow:nightly .
```

## 🔨 إطلاق الخدمة من المصدر للتطوير

1. قم بتثبيت `uv` و`pre-commit`، أو قم بتخطي هذه الخطوة إذا كانا مثبتين بالفعل:

   ```bash
   pipx install uv pre-commit
   ```
2. استنساخ الكود المصدري وتثبيت تبعيات بايثون:

   ```bash
   git clone https://github.com/infiniflow/ragflow.git
   cd ragflow/
   uv sync --python 3.13 # install RAGFlow dependent python modules
   uv run python3 download_deps.py
   pre-commit install
   ```
3. قم بتشغيل الخدمات التابعة (MinIO وElasticsearch وRedis وMySQL) باستخدام Docker Compose:

   ```bash
   docker compose -f docker/docker-compose-base.yml up -d
   ```

   أضف السطر التالي إلى `/etc/hosts` لحل كافة المضيفين المحددين في **docker/.env** إلى `127.0.0.1`:

   ```
   127.0.0.1       es01 infinity mysql minio redis sandbox-executor-manager
   ```
4. إذا لم تتمكن من الوصول إلى HuggingFace، فقم بتعيين متغير البيئة `HF_ENDPOINT` لاستخدام موقع مرآة:

   ```bash
   export HF_ENDPOINT=https://hf-mirror.com
   ```
5. إذا كان نظام التشغيل لديك لا يحتوي على jemalloc، فيرجى تثبيته على النحو التالي:

   ```bash
   # Ubuntu
   sudo apt-get install libjemalloc-dev
   # CentOS
   sudo yum install jemalloc
   # OpenSUSE
   sudo zypper install jemalloc
   # macOS
   sudo brew install jemalloc
   ```
6. إطلاق الخدمة الخلفية:

   ```bash
   source .venv/bin/activate
   export PYTHONPATH=$(pwd)
   bash docker/launch_backend_service.sh
   ```
7. تثبيت تبعيات الواجهة الأمامية:

   ```bash
   cd web
   npm install
   ```
8. إطلاق خدمة الواجهة الأمامية:

   ```bash
   npm run dev
   ```

   _النتيجة التالية تؤكد الإطلاق الناجح للنظام:_

   ![](https://github.com/user-attachments/assets/0daf462c-a24d-4496-a66f-92533534e187)
9. أوقف خدمة الواجهة الأمامية والخلفية RAGFlow بعد اكتمال التطوير:

   ```bash
   pkill -f "ragflow_server.py|task_executor.py"
   ```

## 📚 التوثيق

- [البدء السريع](https://ragflow.io/docs/dev/)
- [التكوين](https://ragflow.io/docs/dev/configurations)
- [ملاحظات الإصدار](https://ragflow.io/docs/dev/release_notes)
- [أدلة المستخدم](https://ragflow.io/docs/category/user-guides)
- [أدلة المطورين](https://ragflow.io/docs/category/developer-guides)
- [المراجع](https://ragflow.io/docs/dev/category/references)
- [الأسئلة الشائعة](https://ragflow.io/docs/dev/faq)

## 📜 Roadmap

راجع [RAGFlow Roadmap 2026](https://github.com/infiniflow/ragflow/issues/12241)

## 🏄 المجتمع

- [Discord](https://discord.gg/NjYzJD3GM3)
- [X](https://x.com/infiniflowai)
- [مناقشات جيثب](https://github.com/orgs/infiniflow/discussions)

## 🙌 المساهمة

RAGFlow يزدهر من خلال التعاون مفتوح المصدر. وبهذه الروح، فإننا نحتضن المساهمات المتنوعة من المجتمع.
إذا كنت ترغب في أن تكون جزءًا، فراجع [إرشادات المساهمة](https://ragflow.io/docs/dev/contributing) أولاً.
>>>>>>> upstream/main
