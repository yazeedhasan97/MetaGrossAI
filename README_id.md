<div align="center">
  <img src="web/public/logo-light.png" width="200" alt="MetaGrossAI logo">
  <h1>MetaGrossAI</h1>
</div>

## 💡 Apa itu MetaGrossAI?
**MetaGrossAI** adalah mesin Retrieval-Augmented Generation (RAG) terkemuka yang memadukan RAG mutakhir dengan kemampuan Agen untuk menciptakan lapisan konteks yang unggul untuk LLM. Ini menawarkan alur kerja RAG yang disederhanakan yang dapat diadaptasi untuk perusahaan skala apa pun. Didayai oleh mesin konteks yang terkonvergensi dan templat agen bawaan, MetaGrossAI memungkinkan pengembang untuk mengubah data kompleks menjadi sistem AI yang siap produksi dan presisi tinggi dengan efisiensi luar biasa.

## 🌟 Fitur Utama
### 🍭 **"Kualitas masuk, kualitas keluar"**
- Ekstraksi pengetahuan berbasis pemahaman dokumen mendalam dari data tidak terstruktur dengan format rumit.
- Menemukan "jarum di tumpukan jerami data" dari token yang jumlahnya tidak terbatas.

### 🍱 **Pemotongan (chunking) berbasis templat**
- Cerdas dan dapat dijelaskan.
- Banyak pilihan templat untuk dipilih.

### 🌱 **Kutipan beralasan dengan pengurangan halusinasi**
- Visualisasi pemotongan teks untuk memungkinkan intervensi manusia.
- Tampilan cepat referensi utama dan kutipan yang dapat dilacak untuk mendukung jawaban yang beralasan.

### 🍔 **Kompatibilitas dengan sumber data heterogen**
- Mendukung Word, slide, excel, txt, gambar, salinan pindaian, data terstruktur, halaman web, dan banyak lagi.

## 🎬 Self-Hosting
### 📝 Prasyarat
- CPU >= 4 cores
- RAM >= 16 GB
- Disk >= 50 GB
- Docker >= 24.0.0 & Docker Compose >= v2.26.1
- Python >= 3.13

### 🚀 Memulai Server
1. Pastikan `vm.max_map_count` >= 262144:
   ```bash
<<<<<<< HEAD
   $ sudo sysctl -w vm.max_map_count=262144
=======
   $ git clone https://github.com/infiniflow/ragflow.git
   ```
3. Bangun image Docker pre-built dan jalankan server:

> [!CAUTION]
> Semua gambar Docker dibangun untuk platform x86. Saat ini, kami tidak menawarkan gambar Docker untuk ARM64.
> Jika Anda menggunakan platform ARM64, [silakan gunakan panduan ini untuk membangun gambar Docker yang kompatibel dengan sistem Anda](https://ragflow.io/docs/dev/build_docker_image).

> Perintah di bawah ini mengunduh edisi v0.26.1 dari gambar Docker RAGFlow. Silakan merujuk ke tabel berikut untuk deskripsi berbagai edisi RAGFlow. Untuk mengunduh edisi RAGFlow yang berbeda dari v0.26.1, perbarui variabel RAGFLOW_IMAGE di docker/.env sebelum menggunakan docker compose untuk memulai server.

```bash
   $ cd ragflow/docker

   # git checkout v0.26.1
   # Opsional: gunakan tag stabil (lihat releases: https://github.com/infiniflow/ragflow/releases)
   # This steps ensures the **entrypoint.sh** file in the code matches the Docker image version.

   # Use CPU for DeepDoc tasks:
   $ docker compose -f docker-compose.yml up -d

   # To use GPU to accelerate DeepDoc tasks:
   # sed -i '1i DEVICE=gpu' .env
   # docker compose -f docker-compose.yml up -d
```

> Catatan: Sebelum `v0.22.0`, kami menyediakan image dengan model embedding dan image slim tanpa model embedding. Detailnya sebagai berikut:

| RAGFlow image tag | Image size (GB) | Has embedding models? | Stable?        |
|-------------------|-----------------|-----------------------|----------------|
| v0.21.1           | &approx;9       | ✔️                    | Stable release |
| v0.21.1-slim      | &approx;2       | ❌                     | Stable release |

> Mulai dari `v0.22.0`, kami hanya menyediakan edisi slim dan tidak lagi menambahkan akhiran **-slim** pada tag image.

1. Periksa status server setelah server aktif dan berjalan:

   ```bash
   $ docker logs -f docker-ragflow-cpu-1
   ```

   _Output berikut menandakan bahwa sistem berhasil diluncurkan:_

   ```bash

         ____   ___    ______ ______ __
        / __ \ /   |  / ____// ____// /____  _      __
       / /_/ // /| | / / __ / /_   / // __ \| | /| / /
      / _, _// ___ |/ /_/ // __/  / // /_/ /| |/ |/ /
     /_/ |_|/_/  |_|\____//_/    /_/ \____/ |__/|__/

    * Running on all addresses (0.0.0.0)
   ```

   > Jika Anda melewatkan langkah ini dan langsung login ke RAGFlow, browser Anda mungkin menampilkan error `network abnormal`
   > karena RAGFlow mungkin belum sepenuhnya siap.
   >
2. Buka browser web Anda, masukkan alamat IP server Anda, dan login ke RAGFlow.

   > Dengan pengaturan default, Anda hanya perlu memasukkan `http://IP_DEVICE_ANDA` (**tanpa** nomor port) karena
   > port HTTP default `80` bisa dihilangkan saat menggunakan konfigurasi default.
   >
3. Dalam [service_conf.yaml.template](./docker/service_conf.yaml.template), pilih LLM factory yang diinginkan di `user_default_llm` dan perbarui
   bidang `API_KEY` dengan kunci API yang sesuai.

   > Lihat [llm_api_key_setup](https://ragflow.io/docs/dev/llm_api_key_setup) untuk informasi lebih lanjut.
   >

   _Sistem telah siap digunakan!_

## 🔧 Konfigurasi

Untuk konfigurasi sistem, Anda perlu mengelola file-file berikut:

- [.env](./docker/.env): Menyimpan pengaturan dasar sistem, seperti `SVR_HTTP_PORT`, `MYSQL_PASSWORD`, dan
  `MINIO_PASSWORD`.
- [service_conf.yaml.template](./docker/service_conf.yaml.template): Mengonfigurasi aplikasi backend.
- [docker-compose.yml](./docker/docker-compose.yml): Sistem ini bergantung pada [docker-compose.yml](./docker/docker-compose.yml) untuk memulai.

Untuk memperbarui port HTTP default (80), buka [docker-compose.yml](./docker/docker-compose.yml) dan ubah `80:80`
menjadi `<YOUR_SERVING_PORT>:80`.

Pembaruan konfigurasi ini memerlukan reboot semua kontainer agar efektif:

> ```bash
> $ docker compose -f docker-compose.yml up -d
> ```

## 🔧 Membangun Docker Image

Image ini berukuran sekitar 2 GB dan bergantung pada aplikasi LLM eksternal dan embedding.

```bash
git clone https://github.com/infiniflow/ragflow.git
cd ragflow/
docker build --platform linux/amd64 -f Dockerfile -t infiniflow/ragflow:nightly .
```

Jika berada di belakang proxy, Anda dapat melewatkan argumen proxy:

```bash
docker build --platform linux/amd64 \
  --build-arg http_proxy=http://YOUR_PROXY:PORT \
  --build-arg https_proxy=http://YOUR_PROXY:PORT \
  -f Dockerfile -t infiniflow/ragflow:nightly .
```

## 🔨 Menjalankan Aplikasi dari untuk Pengembangan

1. Instal `uv` dan `pre-commit`, atau lewati langkah ini jika sudah terinstal:

   ```bash
   pipx install uv pre-commit
   ```
2. Clone kode sumber dan instal dependensi Python:

   ```bash
   git clone https://github.com/infiniflow/ragflow.git
   cd ragflow/
   uv sync --python 3.13 # install RAGFlow dependent python modules
   uv run python3 download_deps.py
   pre-commit install
   ```
3. Jalankan aplikasi yang diperlukan (MinIO, Elasticsearch, Redis, dan MySQL) menggunakan Docker Compose:

   ```bash
   docker compose -f docker/docker-compose-base.yml up -d
   ```

   Tambahkan baris berikut ke `/etc/hosts` untuk memetakan semua host yang ditentukan di **conf/service_conf.yaml** ke `127.0.0.1`:

   ```
   127.0.0.1       es01 infinity mysql minio redis sandbox-executor-manager
   ```
4. Jika Anda tidak dapat mengakses HuggingFace, atur variabel lingkungan `HF_ENDPOINT` untuk menggunakan situs mirror:

   ```bash
   export HF_ENDPOINT=https://hf-mirror.com
   ```
5. Jika sistem operasi Anda tidak memiliki jemalloc, instal sebagai berikut:

   ```bash
   # ubuntu
   sudo apt-get install libjemalloc-dev
   # centos
   sudo yum install jemalloc
   # mac
   sudo brew install jemalloc
   ```
6. Jalankan aplikasi backend:

   ```bash
   source .venv/bin/activate
   export PYTHONPATH=$(pwd)
   bash docker/launch_backend_service.sh
   ```
7. Instal dependensi frontend:

   ```bash
   cd web
   npm install
   ```
8. Jalankan aplikasi frontend:

   ```bash
   npm run dev
   ```

   _Output berikut menandakan bahwa sistem berhasil diluncurkan:_

   ![](https://github.com/user-attachments/assets/0daf462c-a24d-4496-a66f-92533534e187)
9. Hentikan layanan front-end dan back-end RAGFlow setelah pengembangan selesai:

   ```bash
   pkill -f "ragflow_server.py|task_executor.py"
   ```

## 📚 Dokumentasi

- [Quickstart](https://ragflow.io/docs/dev/)
- [Configuration](https://ragflow.io/docs/dev/configurations)
- [Release notes](https://ragflow.io/docs/dev/release_notes)
- [User guides](https://ragflow.io/docs/category/user-guides)
- [Developer guides](https://ragflow.io/docs/category/developer-guides)
- [References](https://ragflow.io/docs/dev/category/references)
- [FAQs](https://ragflow.io/docs/dev/faq)

## 📜 Roadmap

Lihat [Roadmap RAGFlow 2026](https://github.com/infiniflow/ragflow/issues/12241)

## 🏄 Komunitas

- [Discord](https://discord.gg/NjYzJD3GM3)
- [X](https://x.com/infiniflowai)
- [GitHub Discussions](https://github.com/orgs/infiniflow/discussions)

## 🙌 Kontribusi

RAGFlow berkembang melalui kolaborasi open-source. Dalam semangat ini, kami menerima kontribusi dari komunitas.
Jika Anda ingin berpartisipasi, tinjau terlebih dahulu [Panduan Kontribusi](https://ragflow.io/docs/dev/contributing).
>>>>>>> upstream/main
