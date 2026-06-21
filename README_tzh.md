<div align="center">
  <img src="web/public/logo-light.png" width="200" alt="MetaGrossAI logo">
  <h1>MetaGrossAI</h1>
</div>

## 💡 什麼是 MetaGrossAI？
**MetaGrossAI** 是一款領先的檢索增強生成 (RAG) 引擎，它將尖端的 RAG 與 Agent 功能相融合，為 LLM 打造了卓越的上下文層。它提供了一種簡化的 RAG 工作流程，適用於任何規模的企業。借助融合的上下文引擎和預置的 Agent 模板，MetaGrossAI 使開發者能夠極其高效和精準地將複雜數據轉化為高保真、生產級的 AI 系統。

## 🌟 主要功能
### 🍭 **“高質量輸入，高質量輸出”**
- 基於深度文檔理解，從格式複雜的非結構化數據中提取知識。
- 能夠在無限的 Token 中尋找“數據大海中的針”。

### 🍱 **基於模板的文本分塊**
- 智能且具備可解釋性。
- 提供多種模板選項供選擇。

### 🌱 **有理有據的引用，減少幻覺**
- 文本分塊可視化，支持人工干預。
- 快速查看關鍵參考資料和可追溯的引用，以支持有理有據的回答。

### 🍔 **兼容異構數據源**
- 支持 Word、PPT、Excel、TXT、圖片、掃描件、結構化數據、網頁等。

### 🛀 **自動化且輕鬆的 RAG 工作流**
- 專為個人和大型企業量身定制的精簡 RAG 編排。
- 可配置的 LLM 和嵌入模型。
- 多路召回與融合重排。
- 直觀的 API，與業務無縫集成。

## 🎬 本地部署
### 📝 前置依賴
- CPU >= 4 核
- RAM >= 16 GB
- Disk >= 50 GB
- Docker >= 24.0.0 & Docker Compose >= v2.26.1
- Python >= 3.13

### 🚀 啟動伺服器
1. 確保 `vm.max_map_count` >= 262144:
   ```bash
<<<<<<< HEAD
   $ sudo sysctl -w vm.max_map_count=262144
=======
   $ git clone https://github.com/infiniflow/ragflow.git
   ```
3. 進入 **docker** 資料夾，利用事先編譯好的 Docker 映像啟動伺服器：

> [!CAUTION]
> 所有 Docker 映像檔都是為 x86 平台建置的。目前，我們不提供 ARM64 平台的 Docker 映像檔。
> 如果您使用的是 ARM64 平台，請使用 [這份指南](https://ragflow.io/docs/dev/build_docker_image) 來建置適合您系統的 Docker 映像檔。

> 執行以下指令會自動下載 RAGFlow Docker 映像 `v0.26.1`。請參考下表查看不同 Docker 發行版的說明。如需下載不同於 `v0.26.1` 的 Docker 映像，請在執行 `docker compose` 啟動服務之前先更新 **docker/.env** 檔案內的 `RAGFLOW_IMAGE` 變數。

```bash
   $ cd ragflow/docker

   # git checkout v0.26.1
   # 可選：使用穩定版標籤（查看發佈：https://github.com/infiniflow/ragflow/releases）
   # 此步驟確保程式碼中的 entrypoint.sh 檔案與 Docker 映像版本一致。

   # Use CPU for DeepDoc tasks:
   $ docker compose -f docker-compose.yml up -d

   # To use GPU to accelerate DeepDoc tasks:
   # sed -i '1i DEVICE=gpu' .env
   # docker compose -f docker-compose.yml up -d
```

> 注意：在 `v0.22.0` 之前的版本，我們會同時提供包含 embedding 模型的映像和不含 embedding 模型的 slim 映像。具體如下：

| RAGFlow image tag | Image size (GB) | Has embedding models? | Stable?        |
|-------------------|-----------------|-----------------------|----------------|
| v0.21.1           | &approx;9       | ✔️                    | Stable release |
| v0.21.1-slim      | &approx;2       | ❌                     | Stable release |

> 從 `v0.22.0` 開始，我們只發佈 slim 版本，並且不再在映像標籤後附加 **-slim** 後綴。

> [!TIP]
> 如果你遇到 Docker 映像檔拉不下來的問題，可以在 **docker/.env** 檔案內根據變數 `RAGFLOW_IMAGE` 的註解提示選擇華為雲或阿里雲的對應映像。
>
> - 華為雲鏡像名：`swr.cn-north-4.myhuaweicloud.com/infiniflow/ragflow`
> - 阿里雲鏡像名：`registry.cn-hangzhou.aliyuncs.com/infiniflow/ragflow`

4. 伺服器啟動成功後再次確認伺服器狀態：

   ```bash
   $ docker logs -f docker-ragflow-cpu-1
   ```

   _出現以下介面提示說明伺服器啟動成功：_

   ```bash
        ____   ___    ______ ______ __
       / __ \ /   |  / ____// ____// /____  _      __
      / /_/ // /| | / / __ / /_   / // __ \| | /| / /
     / _, _// ___ |/ /_/ // __/  / // /_/ /| |/ |/ /
    /_/ |_|/_/  |_|\____//_/    /_/ \____/ |__/|__/

    * Running on all addresses (0.0.0.0)
   ```

   > 如果您跳過這一步驟系統確認步驟就登入 RAGFlow，你的瀏覽器有可能會提示 `network abnormal` 或 `網路異常`，因為 RAGFlow 可能並未完全啟動成功。
   >
5. 在你的瀏覽器中輸入你的伺服器對應的 IP 位址並登入 RAGFlow。

   > 上面這個範例中，您只需輸入 http://IP_OF_YOUR_MACHINE 即可：未改動過設定則無需輸入連接埠（預設的 HTTP 服務連接埠 80）。
   >
6. 在 [service_conf.yaml.template](./docker/service_conf.yaml.template) 檔案的 `user_default_llm` 欄位設定 LLM factory，並在 `API_KEY` 欄填入和你選擇的大模型相對應的 API key。

   > 詳見 [llm_api_key_setup](https://ragflow.io/docs/dev/llm_api_key_setup)。
   >

   _好戲開始，接著奏樂接著舞！ _

## 🔧 系統配置

系統配置涉及以下三份文件：

- [.env](./docker/.env)：存放一些系統環境變量，例如 `SVR_HTTP_PORT`、`MYSQL_PASSWORD`、`MINIO_PASSWORD` 等。
- [service_conf.yaml.template](./docker/service_conf.yaml.template)：設定各類別後台服務。
- [docker-compose.yml](./docker/docker-compose.yml): 系統依賴該檔案完成啟動。

請務必確保 [.env](./docker/.env) 檔案中的變數設定與 [service_conf.yaml.template](./docker/service_conf.yaml.template) 檔案中的設定保持一致！

如果無法存取映像網站 hub.docker.com 或模型網站 huggingface.co，請依照 [.env](./docker/.env) 註解修改 `RAGFLOW_IMAGE` 和 `HF_ENDPOINT`。

> [./docker/README](./docker/README.md) 解釋了 [service_conf.yaml.template](./docker/service_conf.yaml.template) 用到的環境變數設定和服務配置。

如需更新預設的 HTTP 服務連接埠(80), 可以在[docker-compose.yml](./docker/docker-compose.yml) 檔案中將配置 `80:80` 改為 `<YOUR_SERVING_PORT>:80` 。

> 所有系統配置都需要透過系統重新啟動生效：
>
> ```bash
> $ docker compose -f docker-compose.yml up -d
> ```

###把文檔引擎從 Elasticsearch 切換成為 Infinity

RAGFlow 預設使用 Elasticsearch 儲存文字和向量資料. 如果要切換為 [Infinity](https://github.com/infiniflow/infinity/), 可以按照下面步驟進行:

1. 停止所有容器運作:

   ```bash
   $ docker compose -f docker/docker-compose.yml down -v
   ```

   Note: `-v` 將會刪除 docker 容器的 volumes，已有的資料會被清空。
2. 設定 **docker/.env** 目錄中的 `DOC_ENGINE` 為 `infinity`.
3. 啟動容器:

   ```bash
   $ docker compose -f docker-compose.yml up -d
   ```

> [!WARNING]
> Infinity 目前官方並未正式支援在 Linux/arm64 架構下的機器上運行.

## 🔧 原始碼編譯 Docker 映像

本 Docker 映像大小約 2 GB 左右並且依賴外部的大模型和 embedding 服務。

```bash
git clone https://github.com/infiniflow/ragflow.git
cd ragflow/
docker build --platform linux/amd64 -f Dockerfile -t infiniflow/ragflow:nightly .
```

若您位於代理環境，可傳遞代理參數：

```bash
docker build --platform linux/amd64 \
  --build-arg http_proxy=http://YOUR_PROXY:PORT \
  --build-arg https_proxy=http://YOUR_PROXY:PORT \
  -f Dockerfile -t infiniflow/ragflow:nightly .
```

## 🔨 以原始碼啟動服務

1. 安裝 `uv` 和 `pre-commit`。如已安裝，可跳過此步驟：

   ```bash
   pipx install uv pre-commit
   export UV_INDEX=https://mirrors.aliyun.com/pypi/simple
   ```
2. 下載原始碼並安裝 Python 依賴：

   ```bash
   git clone https://github.com/infiniflow/ragflow.git
   cd ragflow/
   uv sync --python 3.13 # install RAGFlow dependent python modules
   uv run python3 download_deps.py
   pre-commit install
   ```
3. 透過 Docker Compose 啟動依賴的服務（MinIO, Elasticsearch, Redis, and MySQL）：

   ```bash
   docker compose -f docker/docker-compose-base.yml up -d
   ```

   在 `/etc/hosts` 中加入以下程式碼，將 **conf/service_conf.yaml** 檔案中的所有 host 位址都解析為 `127.0.0.1`：

   ```
   127.0.0.1       es01 infinity mysql minio redis sandbox-executor-manager
   ```
4. 如果無法存取 HuggingFace，可以把環境變數 `HF_ENDPOINT` 設為對應的鏡像網站：

   ```bash
   export HF_ENDPOINT=https://hf-mirror.com
   ```
5. 如果你的操作系统没有 jemalloc，请按照如下方式安装：

   ```bash
   # ubuntu
   sudo apt-get install libjemalloc-dev
   # centos
   sudo yum install jemalloc
   # mac
   sudo brew install jemalloc
   ```
6. 啟動後端服務：

   ```bash
   source .venv/bin/activate
   export PYTHONPATH=$(pwd)
   bash docker/launch_backend_service.sh
   ```
7. 安裝前端依賴：

   ```bash
   cd web
   npm install
   ```
8. 啟動前端服務：

   ```bash
   npm run dev
   ```

   以下界面說明系統已成功啟動：_

   ![](https://github.com/user-attachments/assets/0daf462c-a24d-4496-a66f-92533534e187)

   ```

   ```
9. 開發完成後停止 RAGFlow 前端和後端服務：

   ```bash
   pkill -f "ragflow_server.py|task_executor.py"
   ```

## 📚 技術文檔

- [Quickstart](https://ragflow.io/docs/dev/)
- [Configuration](https://ragflow.io/docs/dev/configurations)
- [Release notes](https://ragflow.io/docs/dev/release_notes)
- [User guides](https://ragflow.io/docs/category/user-guides)
- [Developer guides](https://ragflow.io/docs/category/developer-guides)
- [References](https://ragflow.io/docs/dev/category/references)
- [FAQs](https://ragflow.io/docs/dev/faq)

## 📜 路線圖

詳見 [RAGFlow Roadmap 2026](https://github.com/infiniflow/ragflow/issues/12241) 。

## 🏄 開源社群

- [Discord](https://discord.gg/NjYzJD3GM3)
- [X](https://x.com/infiniflowai)
- [GitHub Discussions](https://github.com/orgs/infiniflow/discussions)

## 🙌 貢獻指南

RAGFlow 只有透過開源協作才能蓬勃發展。秉持這項精神,我們歡迎來自社區的各種貢獻。如果您有意參與其中,請查閱我們的 [貢獻者指南](https://ragflow.io/docs/dev/contributing) 。

## 🤝 商務合作

- [預約諮詢](https://aao615odquw.feishu.cn/share/base/form/shrcnjw7QleretCLqh1nuPo1xxh)

## 👥 加入社區

掃二維碼加入 RAGFlow 小助手，進 RAGFlow 交流群。

<p align="center">
  <img src="https://github.com/infiniflow/ragflow/assets/7248/bccf284f-46f2-4445-9809-8f1030fb7585" width=50% height=50%>
</p>
>>>>>>> upstream/main
