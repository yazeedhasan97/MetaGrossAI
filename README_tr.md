<div align="center">
  <img src="web/public/logo-light.png" width="200" alt="MetaGrossAI logo">
  <h1>MetaGrossAI</h1>
</div>

## 💡 MetaGrossAI Nedir?
**MetaGrossAI**, LLM'ler için üstün bir bağlam katmanı oluşturmak amacıyla en yeni RAG'i Ajan (Agent) yetenekleriyle birleştiren lider bir Geri Getirme Artırılmış Üretim (RAG) motorudur. Her ölçekteki kuruluşa uyarlanabilen modern bir RAG iş akışı sunar. Birleştirilmiş bir bağlam motoru ve önceden oluşturulmuş ajan şablonlarıyla desteklenen MetaGrossAI, geliştiricilerin karmaşık verileri olağanüstü verimlilik ve hassasiyetle üretime hazır yapay zeka sistemlerine dönüştürmesine olanak tanır.

## 🌟 Temel Özellikler
### 🍭 **"Kaliteli girdi, kaliteli çıktı"**
- Karmaşık formatlara sahip yapılandırılmamış verilerden derin belge anlayışına dayalı bilgi çıkarımı.
- Sınırsız token içerisinden "veri samanlığındaki iğneyi" bulur.

### 🍱 **Şablon tabanlı parçalama (chunking)**
- Akıllı ve açıklanabilir.
- Aralarından seçim yapabileceğiniz çok sayıda şablon seçeneği.

### 🌱 **Azaltılmış halüsinasyonlar ile temellendirilmiş alıntılar**
- İnsan müdahalesine izin vermek için metin parçalamanın görselleştirilmesi.
- Temellendirilmiş cevapları desteklemek için temel referansların ve izlenebilir alıntıların hızlı görünümü.

### 🍔 **Heterojen veri kaynakları ile uyumluluk**
- Word, slaytlar, excel, txt, resimler, taranmış kopyalar, yapılandırılmış veriler, web sayfaları ve daha fazlasını destekler.

## 🎬 Kendi Sunucunda Barındırma (Self-Hosting)
### 📝 Ön Koşullar
- CPU >= 4 cores
- RAM >= 16 GB
- Disk >= 50 GB
- Docker >= 24.0.0 & Docker Compose >= v2.26.1
- Python >= 3.13

### 🚀 Sunucuyu Başlatma
1. `vm.max_map_count` >= 262144 olduğundan emin olun:
   ```bash
<<<<<<< HEAD
   $ sudo sysctl -w vm.max_map_count=262144
=======
   $ git clone https://github.com/infiniflow/ragflow.git
   ```
3. Önceden oluşturulmuş Docker imajlarını kullanarak sunucuyu başlatın:

> [!CAUTION]
> Tüm Docker imajları x86 platformları için oluşturulmuştur. Şu anda ARM64 için Docker imajı sunmuyoruz.
> ARM64 platformundaysanız, sisteminizle uyumlu bir Docker imajı oluşturmak için [bu kılavuzu](https://ragflow.io/docs/dev/build_docker_image) takip edin.

> Aşağıdaki komut RAGFlow Docker imajının `v0.26.1` sürümünü indirir. Farklı RAGFlow sürümleri için aşağıdaki tabloya bakın. `v0.26.1` dışında bir sürüm indirmek için, `docker compose` ile sunucuyu başlatmadan önce **docker/.env** dosyasındaki `RAGFLOW_IMAGE` değişkenini güncelleyin.

```bash
   $ cd ragflow/docker

   # git checkout v0.26.1
   # İsteğe bağlı: Kararlı bir etiket kullanın (sürümler: https://github.com/infiniflow/ragflow/releases)
   # Bu adım, koddaki **entrypoint.sh** dosyasının Docker imaj sürümüyle eşleşmesini sağlar.

   # DeepDoc görevleri için CPU kullanımı:
   $ docker compose -f docker-compose.yml up -d

   # DeepDoc görevlerini hızlandırmak için GPU kullanımı:
   # sed -i '1i DEVICE=gpu' .env
   # docker compose -f docker-compose.yml up -d
```

> Not: `v0.22.0` öncesinde hem gömme modelleri içeren imajlar hem de gömme modelleri içermeyen ince (slim) imajlar sunuyorduk. Detaylar aşağıdadır:

| RAGFlow imaj etiketi | İmaj boyutu (GB) | Gömme modelleri var mı? | Kararlı mı?    |
|-----------------------|-------------------|-------------------------|-----------------|
| v0.21.1               | &approx;9        | ✔️                      | Kararlı sürüm   |
| v0.21.1-slim          | &approx;2        | ❌                       | Kararlı sürüm   |

> `v0.22.0`'dan itibaren yalnızca ince (slim) sürümü sunuyoruz ve imaj etiketine artık **-slim** son eki eklemiyoruz.

4. Sunucu çalışır duruma geldikten sonra sunucu durumunu kontrol edin:

   ```bash
   $ docker logs -f docker-ragflow-cpu-1
   ```

   _Aşağıdaki çıktı, sistemin başarıyla başlatıldığını onaylar:_

   ```bash

         ____   ___    ______ ______ __
        / __ \ /   |  / ____// ____// /____  _      __
       / /_/ // /| | / / __ / /_   / // __ \| | /| / /
      / _, _// ___ |/ /_/ // __/  / // /_/ /| |/ |/ /
     /_/ |_|/_/  |_|\____//_/    /_/ \____/ |__/|__/

    * Running on all addresses (0.0.0.0)
   ```

   > Bu onay adımını atlayıp doğrudan RAGFlow'a giriş yaparsanız, o anda RAGFlow tam olarak başlatılmamış olabileceğinden
   > tarayıcınız `ağ hatası` uyarısı verebilir.
   >
5. Web tarayıcınıza sunucunuzun IP adresini girin ve RAGFlow'a giriş yapın.

   > Varsayılan ayarlarla, yalnızca `http://MAKİNENİZİN_IP_ADRESİ` girmeniz yeterlidir (port numarası **gerekmez**),
   > çünkü varsayılan HTTP sunucu portu `80` varsayılan yapılandırmalar kullanıldığında ihmal edilebilir.
   >
6. [service_conf.yaml.template](./docker/service_conf.yaml.template) dosyasında, `user_default_llm` içinde istediğiniz LLM sağlayıcısını seçin ve
   `API_KEY` alanını ilgili API anahtarıyla güncelleyin.

   > Daha fazla bilgi için [llm_api_key_setup](https://ragflow.io/docs/dev/llm_api_key_setup) sayfasına bakın.
   >

   _Gösteri başlasın!_

## 🔧 Yapılandırmalar

Sistem yapılandırmaları söz konusu olduğunda, aşağıdaki dosyaları yönetmeniz gerekecektir:

- [.env](./docker/.env): `SVR_HTTP_PORT`, `MYSQL_PASSWORD` ve `MINIO_PASSWORD` gibi temel sistem ayarlarını içerir.
- [service_conf.yaml.template](./docker/service_conf.yaml.template): Arka uç hizmetlerini yapılandırır. Bu dosyadaki ortam değişkenleri, Docker konteyneri başladığında otomatik olarak doldurulacaktır. Docker konteyneri içinde ayarlanan tüm ortam değişkenleri kullanıma hazır olacak ve hizmet davranışını dağıtım ortamına göre özelleştirmenize olanak tanıyacaktır.
- [docker-compose.yml](./docker/docker-compose.yml): Sistem, başlatılmak için [docker-compose.yml](./docker/docker-compose.yml) dosyasına dayanır.

> [./docker/README](./docker/README.md) dosyası, [service_conf.yaml.template](./docker/service_conf.yaml.template) dosyasında `${ENV_VARS}` olarak kullanılabilen ortam ayarları ve hizmet yapılandırmalarının ayrıntılı bir açıklamasını sağlar.

Varsayılan HTTP sunucu portunu (80) değiştirmek için [docker-compose.yml](./docker/docker-compose.yml) dosyasında `80:80` ifadesini `<SUNUCU_PORTUNUZ>:80` olarak değiştirin.

Yukarıdaki yapılandırma değişikliklerinin etkili olması için tüm konteynerlerin yeniden başlatılması gerekir:

> ```bash
> $ docker compose -f docker-compose.yml up -d
> ```

### Doküman Motorunu Elasticsearch'ten Infinity'ye Geçirme

RAGFlow varsayılan olarak tam metin ve vektörlerin depolanması için Elasticsearch kullanır. [Infinity](https://github.com/infiniflow/infinity/)'ye geçmek için şu adımları izleyin:

1. Çalışan tüm konteynerleri durdurun:

   ```bash
   $ docker compose -f docker/docker-compose.yml down -v
   ```

> [!WARNING]
> `-v` seçeneği Docker konteyner birimlerini silecek ve mevcut veriler temizlenecektir.

2. **docker/.env** dosyasında `DOC_ENGINE` değerini `infinity` olarak ayarlayın.
3. Konteynerleri başlatın:

   ```bash
   $ docker compose -f docker-compose.yml up -d
   ```

> [!WARNING]
> Linux/arm64 makinesinde Infinity'ye geçiş henüz resmi olarak desteklenmemektedir.

## 🔧 Docker İmajı Oluşturma

Bu imaj yaklaşık 2 GB boyutundadır ve harici LLM ile gömme hizmetlerine bağlıdır.

```bash
git clone https://github.com/infiniflow/ragflow.git
cd ragflow/
docker build --platform linux/amd64 -f Dockerfile -t infiniflow/ragflow:nightly .
```

Veya bir proxy arkasındaysanız, proxy parametrelerini iletebilirsiniz:

```bash
docker build --platform linux/amd64 \
  --build-arg http_proxy=http://PROXY_ADRESINIZ:PORT \
  --build-arg https_proxy=http://PROXY_ADRESINIZ:PORT \
  -f Dockerfile -t infiniflow/ragflow:nightly .
```

## 🔨 Geliştirme İçin Kaynaktan Hizmet Başlatma

1. `uv` ve `pre-commit` yükleyin veya zaten yüklüyse bu adımı atlayın:

   ```bash
   pipx install uv pre-commit
   ```
2. Kaynak kodunu klonlayın ve Python bağımlılıklarını yükleyin:

   ```bash
   git clone https://github.com/infiniflow/ragflow.git
   cd ragflow/
   uv sync --python 3.13 # RAGFlow'un bağımlı Python modüllerini yükler
   uv run python3 download_deps.py
   pre-commit install
   ```
3. Bağımlı hizmetleri (MinIO, Elasticsearch, Redis ve MySQL) Docker Compose kullanarak başlatın:

   ```bash
   docker compose -f docker/docker-compose-base.yml up -d
   ```

   **docker/.env** dosyasında belirtilen tüm ana bilgisayar adlarını `127.0.0.1`'e çözümlemek için `/etc/hosts` dosyasına aşağıdaki satırı ekleyin:

   ```
   127.0.0.1       es01 infinity mysql minio redis sandbox-executor-manager
   ```
4. HuggingFace'e erişemiyorsanız, bir ayna site kullanmak için `HF_ENDPOINT` ortam değişkenini ayarlayın:

   ```bash
   export HF_ENDPOINT=https://hf-mirror.com
   ```
5. İşletim sisteminizde jemalloc yoksa, aşağıdaki şekilde yükleyin:

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
6. Arka uç hizmetini başlatın:

   ```bash
   source .venv/bin/activate
   export PYTHONPATH=$(pwd)
   bash docker/launch_backend_service.sh
   ```
7. Ön yüz bağımlılıklarını yükleyin:

   ```bash
   cd web
   npm install
   ```
8. Ön yüz hizmetini başlatın:

   ```bash
   npm run dev
   ```

   _Aşağıdaki çıktı, sistemin başarıyla başlatıldığını onaylar:_

   ![](https://github.com/user-attachments/assets/0daf462c-a24d-4496-a66f-92533534e187)
9. Geliştirme tamamlandıktan sonra RAGFlow ön yüz ve arka uç hizmetini durdurun:

   ```bash
   pkill -f "ragflow_server.py|task_executor.py"
   ```

## 📚 Dokümantasyon

- [Hızlı Başlangıç](https://ragflow.io/docs/dev/)
- [Yapılandırma](https://ragflow.io/docs/dev/configurations)
- [Sürüm Notları](https://ragflow.io/docs/dev/release_notes)
- [Kullanıcı Kılavuzları](https://ragflow.io/docs/category/user-guides)
- [Geliştirici Kılavuzları](https://ragflow.io/docs/category/developer-guides)
- [Referanslar](https://ragflow.io/docs/dev/category/references)
- [SSS](https://ragflow.io/docs/dev/faq)

## 📜 Yol Haritası

[RAGFlow Yol Haritası 2026](https://github.com/infiniflow/ragflow/issues/12241) sayfasına bakın.

## 🏄 Topluluk

- [Discord](https://discord.gg/NjYzJD3GM3)
- [X](https://x.com/infiniflowai)
- [GitHub Tartışmalar](https://github.com/orgs/infiniflow/discussions)

## 🙌 Katkıda Bulunma

RAGFlow, açık kaynak iş birliği sayesinde gelişmektedir. Bu anlayışla, topluluktan gelen çeşitli katkıları benimsiyoruz.
Bir parçası olmak istiyorsanız, önce [Katkıda Bulunma Kılavuzumuzu](https://ragflow.io/docs/dev/contributing) inceleyin.
>>>>>>> upstream/main
