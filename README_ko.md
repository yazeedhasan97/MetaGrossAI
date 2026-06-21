<div align="center">
  <img src="web/public/logo-light.png" width="200" alt="MetaGrossAI logo">
  <h1>MetaGrossAI</h1>
</div>

## 💡 MetaGrossAI란 무엇인가요?
**MetaGrossAI**는 최첨단 RAG와 에이전트 기능을 결합하여 LLM을 위한 우수한 컨텍스트 계층을 생성하는 선도적인 검색 증강 생성(RAG) 엔진입니다. 모든 규모의 기업에 적응할 수 있는 간소화된 RAG 워크플로우를 제공합니다. 융합된 컨텍스트 엔진과 사전 구축된 에이전트 템플릿으로 구동되는 MetaGrossAI를 통해 개발자는 복잡한 데이터를 뛰어난 효율성과 정밀도를 갖춘 프로덕션 지원 AI 시스템으로 변환할 수 있습니다.

## 🌟 주요 기능
### 🍭 **"좋은 품질이 들어가야 좋은 품질이 나옵니다"**
- 복잡한 형식을 가진 비정형 데이터에서 심층적인 문서 이해를 기반으로 한 지식 추출.
- 말 그대로 무제한 토큰의 "데이터 건초 더미에서 바늘"을 찾습니다.

### 🍱 **템플릿 기반 청킹**
- 지능적이고 설명 가능합니다.
- 선택할 수 있는 다양한 템플릿 옵션.

### 🌱 **환각을 줄인 근거 있는 인용**
- 인간의 개입을 허용하는 텍스트 청킹의 시각화.
- 근거 있는 답변을 뒷받침하는 주요 참조 및 추적 가능한 인용의 빠른 보기.

### 🍔 **이기종 데이터 소스와의 호환성**
- Word, 슬라이드, Excel, txt, 이미지, 스캔본, 구조화된 데이터, 웹 페이지 등을 지원합니다.

### 🛀 **자동화되고 손쉬운 RAG 워크플로우**
- 구성 가능한 LLM 및 임베딩 모델.
- 비즈니스와 원활한 통합을 위한 직관적인 API.

## 🎬 셀프 호스팅
### 📝 전제 조건
- CPU >= 4 cores
- RAM >= 16 GB
- Disk >= 50 GB
- Docker >= 24.0.0 & Docker Compose >= v2.26.1
- Python >= 3.13

### 🚀 서버 시작
1. `vm.max_map_count` >= 262144 확인:
   ```bash
<<<<<<< HEAD
   $ sudo sysctl -w vm.max_map_count=262144
=======
   $ git clone https://github.com/infiniflow/ragflow.git
   ```

3. 미리 빌드된 Docker 이미지를 생성하고 서버를 시작하세요:

> [!CAUTION]
> 모든 Docker 이미지는 x86 플랫폼을 위해 빌드되었습니다. 우리는 현재 ARM64 플랫폼을 위한 Docker 이미지를 제공하지 않습니다.
> ARM64 플랫폼을 사용 중이라면, [시스템과 호환되는 Docker 이미지를 빌드하려면 이 가이드를 사용해 주세요](https://ragflow.io/docs/dev/build_docker_image).

   > 아래 명령어는 RAGFlow Docker 이미지의 v0.26.1 버전을 다운로드합니다. 다양한 RAGFlow 버전에 대한 설명은 다음 표를 참조하십시오. v0.26.1와 다른 RAGFlow 버전을 다운로드하려면, docker/.env 파일에서 RAGFLOW_IMAGE 변수를 적절히 업데이트한 후 docker compose를 사용하여 서버를 시작하십시오.

   ```bash
   $ cd ragflow/docker

   # git checkout v0.26.1
   # Optional: use a stable tag (see releases: https://github.com/infiniflow/ragflow/releases)
   # 이 단계는 코드의 entrypoint.sh 파일이 Docker 이미지 버전과 일치하도록 보장합니다.

   # Use CPU for DeepDoc tasks:
   $ docker compose -f docker-compose.yml up -d

   # To use GPU to accelerate DeepDoc tasks:
   # sed -i '1i DEVICE=gpu' .env
   # docker compose -f docker-compose.yml up -d
```

> 참고: `v0.22.0` 이전 버전에서는 embedding 모델이 포함된 이미지와 embedding 모델이 포함되지 않은 slim 이미지를 모두 제공했습니다. 자세한 내용은 다음과 같습니다:

| RAGFlow image tag | Image size (GB) | Has embedding models? | Stable?        |
|-------------------|-----------------|-----------------------|----------------|
| v0.21.1           | &approx;9       | ✔️                    | Stable release |
| v0.21.1-slim      | &approx;2       | ❌                     | Stable release |

> `v0.22.0`부터는 slim 에디션만 배포하며 이미지 태그에 **-slim** 접미사를 더 이상 붙이지 않습니다.

1. 서버가 시작된 후 서버 상태를 확인하세요:

   ```bash
   $ docker logs -f docker-ragflow-cpu-1
   ```

   _다음 출력 결과로 시스템이 성공적으로 시작되었음을 확인합니다:_

   ```bash
        ____   ___    ______ ______ __
       / __ \ /   |  / ____// ____// /____  _      __
      / /_/ // /| | / / __ / /_   / // __ \| | /| / /
     / _, _// ___ |/ /_/ // __/  / // /_/ /| |/ |/ /
    /_/ |_|/_/  |_|\____//_/    /_/ \____/ |__/|__/

    * Running on all addresses (0.0.0.0)
   ```

   > 만약 확인 단계를 건너뛰고 바로 RAGFlow에 로그인하면, RAGFlow가 완전히 초기화되지 않았기 때문에 브라우저에서 `network abnormal` 오류가 발생할 수 있습니다.

2. 웹 브라우저에 서버의 IP 주소를 입력하고 RAGFlow에 로그인하세요.
   > 기본 설정을 사용할 경우, `http://IP_OF_YOUR_MACHINE`만 입력하면 됩니다 (포트 번호는 제외). 기본 HTTP 서비스 포트 `80`은 기본 구성으로 사용할 때 생략할 수 있습니다.
3. [service_conf.yaml.template](./docker/service_conf.yaml.template) 파일에서 원하는 LLM 팩토리를 `user_default_llm`에 선택하고, `API_KEY` 필드를 해당 API 키로 업데이트하세요.

   > 자세한 내용은 [llm_api_key_setup](https://ragflow.io/docs/dev/llm_api_key_setup)를 참조하세요.

   _이제 쇼가 시작됩니다!_

## 🔧 설정

시스템 설정과 관련하여 다음 파일들을 관리해야 합니다:

- [.env](./docker/.env): `SVR_HTTP_PORT`, `MYSQL_PASSWORD`, `MINIO_PASSWORD`와 같은 시스템의 기본 설정을 포함합니다.
- [service_conf.yaml.template](./docker/service_conf.yaml.template): 백엔드 서비스를 구성합니다.
- [docker-compose.yml](./docker/docker-compose.yml): 시스템은 [docker-compose.yml](./docker/docker-compose.yml)을 사용하여 시작됩니다.

[.env](./docker/.env) 파일의 변경 사항이 [service_conf.yaml.template](./docker/service_conf.yaml.template) 파일의 내용과 일치하도록 해야 합니다.

> [./docker/README](./docker/README.md) 파일 ./docker/README은 service_conf.yaml.template 파일에서 ${ENV_VARS}로 사용할 수 있는 환경 설정과 서비스 구성에 대한 자세한 설명을 제공합니다.

기본 HTTP 서비스 포트(80)를 업데이트하려면 [docker-compose.yml](./docker/docker-compose.yml) 파일에서 `80:80`을 `<YOUR_SERVING_PORT>:80`으로 변경하세요.

> 모든 시스템 구성 업데이트는 적용되기 위해 시스템 재부팅이 필요합니다.
>
> ```bash
> $ docker compose -f docker-compose.yml up -d
> ```

### Elasticsearch 에서 Infinity 로 문서 엔진 전환

RAGFlow 는 기본적으로 Elasticsearch 를 사용하여 전체 텍스트 및 벡터를 저장합니다. [Infinity]로 전환(https://github.com/infiniflow/infinity/), 다음 절차를 따르십시오.

1. 실행 중인 모든 컨테이너를 중지합니다.
   ```bash
   $docker compose-f docker/docker-compose.yml down -v
   ```
   Note: `-v` 는 docker 컨테이너의 볼륨을 삭제하고 기존 데이터를 지우며, 이 작업은 컨테이너를 중지하는 것과 동일합니다.
2. **docker/.env**의 "DOC_ENGINE" 을 "infinity" 로 설정합니다.
3. 컨테이너 부팅:
   ```bash
   $docker compose-f docker/docker-compose.yml up -d
   ```
   > [!WARNING]
   > Linux/arm64 시스템에서 Infinity로 전환하는 것은 공식적으로 지원되지 않습니다.

## 🔧 소스 코드로 Docker 이미지를 컴파일합니다

이 Docker 이미지의 크기는 약 1GB이며, 외부 대형 모델과 임베딩 서비스에 의존합니다.

```bash
git clone https://github.com/infiniflow/ragflow.git
cd ragflow/
docker build --platform linux/amd64 -f Dockerfile -t infiniflow/ragflow:nightly .
```

프록시 환경인 경우, 프록시 인수를 전달할 수 있습니다：

```bash
docker build --platform linux/amd64 \
  --build-arg http_proxy=http://YOUR_PROXY:PORT \
  --build-arg https_proxy=http://YOUR_PROXY:PORT \
  -f Dockerfile -t infiniflow/ragflow:nightly .
```

## 🔨 소스 코드로 서비스를 시작합니다.

1. `uv` 와 `pre-commit` 을 설치하거나, 이미 설치된 경우 이 단계를 건너뜁니다:

   ```bash
   pipx install uv pre-commit
   ```

2. 소스 코드를 클론하고 Python 의존성을 설치합니다:

   ```bash
   git clone https://github.com/infiniflow/ragflow.git
   cd ragflow/
   uv sync --python 3.13 # install RAGFlow dependent python modules
   uv run python3 download_deps.py
   pre-commit install
   ```

3. Docker Compose를 사용하여 의존 서비스(MinIO, Elasticsearch, Redis 및 MySQL)를 시작합니다:

   ```bash
   docker compose -f docker/docker-compose-base.yml up -d
   ```

   `/etc/hosts` 에 다음 줄을 추가하여 **conf/service_conf.yaml** 에 지정된 모든 호스트를 `127.0.0.1` 로 해결합니다:

   ```
   127.0.0.1       es01 infinity mysql minio redis sandbox-executor-manager
   ```

4. HuggingFace에 접근할 수 없는 경우, `HF_ENDPOINT` 환경 변수를 설정하여 미러 사이트를 사용하세요:

   ```bash
   export HF_ENDPOINT=https://hf-mirror.com
   ```

5. 만약 운영 체제에 jemalloc이 없으면 다음 방식으로 설치하세요:

   ```bash
   # ubuntu
   sudo apt-get install libjemalloc-dev
   # centos
   sudo yum install jemalloc
   # mac
   sudo brew install jemalloc
   ```

6. 백엔드 서비스를 시작합니다:

   ```bash
   source .venv/bin/activate
   export PYTHONPATH=$(pwd)
   bash docker/launch_backend_service.sh
   ```

7. 프론트엔드 의존성을 설치합니다:

   ```bash
   cd web
   npm install
   ```

8. 프론트엔드 서비스를 시작합니다:

   ```bash
   npm run dev
   ```

   _다음 인터페이스는 시스템이 성공적으로 시작되었음을 나타냅니다:_

   ![](https://github.com/user-attachments/assets/0daf462c-a24d-4496-a66f-92533534e187)


9. 개발이 완료된 후 RAGFlow 프론트엔드 및 백엔드 서비스를 중지합니다.

   ```bash
   pkill -f "ragflow_server.py|task_executor.py"
   ```


## 📚 문서

- [Quickstart](https://ragflow.io/docs/dev/)
- [Configuration](https://ragflow.io/docs/dev/configurations)
- [Release notes](https://ragflow.io/docs/dev/release_notes)
- [User guides](https://ragflow.io/docs/category/user-guides)
- [Developer guides](https://ragflow.io/docs/category/developer-guides)
- [References](https://ragflow.io/docs/dev/category/references)
- [FAQs](https://ragflow.io/docs/dev/faq)

## 📜 로드맵

[RAGFlow 로드맵 2026](https://github.com/infiniflow/ragflow/issues/12241)을 확인하세요.

## 🏄 커뮤니티

- [Discord](https://discord.gg/NjYzJD3GM3)
- [X](https://x.com/infiniflowai)
- [GitHub Discussions](https://github.com/orgs/infiniflow/discussions)

## 🙌 컨트리뷰션

RAGFlow는 오픈소스 협업을 통해 발전합니다. 이러한 정신을 바탕으로, 우리는 커뮤니티의 다양한 기여를 환영합니다. 참여하고 싶으시다면, 먼저 [가이드라인](https://ragflow.io/docs/dev/contributing)을 검토해 주세요.
>>>>>>> upstream/main
