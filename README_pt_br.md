<div align="center">
  <img src="web/public/logo-light.png" width="200" alt="MetaGrossAI logo">
  <h1>MetaGrossAI</h1>
</div>

## 💡 O que é MetaGrossAI?
**MetaGrossAI** é um mecanismo líder em Geração Aumentada por Recuperação (RAG) que funde a tecnologia RAG de ponta com recursos de Agente para criar uma camada de contexto superior para LLMs. Ele oferece um fluxo de trabalho RAG simplificado, adaptável a empresas de qualquer escala. Alimentado por um mecanismo de contexto convergente e modelos de agentes pré-construídos, o MetaGrossAI permite que os desenvolvedores transformem dados complexos em sistemas de IA de alta fidelidade e prontos para produção, com eficiência e precisão excepcionais.

## 🌟 Principais Recursos
### 🍭 **"Qualidade na entrada, qualidade na saída"**
- Extração de conhecimento baseada na compreensão profunda de documentos a partir de dados não estruturados com formatos complexos.
- Encontra "uma agulha em um palheiro de dados" de tokens literalmente ilimitados.

### 🍱 **Fragmentação (chunking) baseada em modelos**
- Inteligente e explicável.
- Várias opções de modelos para escolher.

### 🌱 **Citações fundamentadas com redução de alucinações**
- Visualização da fragmentação do texto para permitir a intervenção humana.
- Visualização rápida das principais referências e citações rastreáveis para apoiar respostas fundamentadas.

### 🍔 **Compatibilidade com fontes de dados heterogêneas**
- Suporta Word, slides, excel, txt, imagens, cópias digitalizadas, dados estruturados, páginas da web e muito mais.

## 🎬 Auto-hospedagem
### 📝 Pré-requisitos
- CPU >= 4 cores
- RAM >= 16 GB
- Disk >= 50 GB
- Docker >= 24.0.0 & Docker Compose >= v2.26.1
- Python >= 3.13

### 🚀 Iniciando o servidor
1. Certifique-se de que `vm.max_map_count` >= 262144:
   ```bash
<<<<<<< HEAD
   $ sudo sysctl -w vm.max_map_count=262144
=======
   $ git clone https://github.com/infiniflow/ragflow.git
   ```
3. Inicie o servidor usando as imagens Docker pré-compiladas:

> [!CAUTION]
> Todas as imagens Docker são construídas para plataformas x86. Atualmente, não oferecemos imagens Docker para ARM64.
> Se você estiver usando uma plataforma ARM64, por favor, utilize [este guia](https://ragflow.io/docs/dev/build_docker_image) para construir uma imagem Docker compatível com o seu sistema.

    > O comando abaixo baixa a edição`v0.26.1` da imagem Docker do RAGFlow. Consulte a tabela a seguir para descrições de diferentes edições do RAGFlow. Para baixar uma edição do RAGFlow diferente da `v0.26.1`, atualize a variável `RAGFLOW_IMAGE` conforme necessário no **docker/.env** antes de usar `docker compose` para iniciar o servidor.

```bash
   $ cd ragflow/docker

   # git checkout v0.26.1
   # Opcional: use uma tag estável (veja releases: https://github.com/infiniflow/ragflow/releases)
   # Esta etapa garante que o arquivo entrypoint.sh no código corresponda à versão da imagem do Docker.

   # Use CPU for DeepDoc tasks:
   $ docker compose -f docker-compose.yml up -d

   # To use GPU to accelerate DeepDoc tasks:
   # sed -i '1i DEVICE=gpu' .env
   # docker compose -f docker-compose.yml up -d
```

> Nota: Antes da `v0.22.0`, fornecíamos imagens com modelos de embedding e imagens slim sem modelos de embedding. Detalhes a seguir:

| RAGFlow image tag | Image size (GB) | Has embedding models? | Stable?        |
|-------------------|-----------------|-----------------------|----------------|
| v0.21.1           | &approx;9       | ✔️                    | Stable release |
| v0.21.1-slim      | &approx;2       | ❌                     | Stable release |

> A partir da `v0.22.0`, distribuímos apenas a edição slim e não adicionamos mais o sufixo **-slim** às tags das imagens.

4. Verifique o status do servidor após tê-lo iniciado:

   ```bash
   $ docker logs -f docker-ragflow-cpu-1
   ```

   _O seguinte resultado confirma o lançamento bem-sucedido do sistema:_

   ```bash
        ____   ___    ______ ______ __
       / __ \ /   |  / ____// ____// /____  _      __
      / /_/ // /| | / / __ / /_   / // __ \| | /| / /
     / _, _// ___ |/ /_/ // __/  / // /_/ /| |/ |/ /
    /_/ |_|/_/  |_|\____//_/    /_/ \____/ |__/|__/

    * Rodando em todos os endereços (0.0.0.0)
   ```

   > Se você pular essa etapa de confirmação e acessar diretamente o RAGFlow, seu navegador pode exibir um erro `network abnormal`, pois, nesse momento, seu RAGFlow pode não estar totalmente inicializado.
   >
5. No seu navegador, insira o endereço IP do seu servidor e faça login no RAGFlow.

   > Com as configurações padrão, você só precisa digitar `http://IP_DO_SEU_MÁQUINA` (**sem** o número da porta), pois a porta HTTP padrão `80` pode ser omitida ao usar as configurações padrão.
   >
6. Em [service_conf.yaml.template](./docker/service_conf.yaml.template), selecione a fábrica LLM desejada em `user_default_llm` e atualize o campo `API_KEY` com a chave de API correspondente.

   > Consulte [llm_api_key_setup](https://ragflow.io/docs/dev/llm_api_key_setup) para mais informações.
   >

_O show está no ar!_

## 🔧 Configurações

Quando se trata de configurações do sistema, você precisará gerenciar os seguintes arquivos:

- [.env](./docker/.env): Contém as configurações fundamentais para o sistema, como `SVR_HTTP_PORT`, `MYSQL_PASSWORD` e `MINIO_PASSWORD`.
- [service_conf.yaml.template](./docker/service_conf.yaml.template): Configura os serviços de back-end. As variáveis de ambiente neste arquivo serão automaticamente preenchidas quando o contêiner Docker for iniciado. Quaisquer variáveis de ambiente definidas dentro do contêiner Docker estarão disponíveis para uso, permitindo personalizar o comportamento do serviço com base no ambiente de implantação.
- [docker-compose.yml](./docker/docker-compose.yml): O sistema depende do [docker-compose.yml](./docker/docker-compose.yml) para iniciar.

> O arquivo [./docker/README](./docker/README.md) fornece uma descrição detalhada das configurações do ambiente e dos serviços, que podem ser usadas como `${ENV_VARS}` no arquivo [service_conf.yaml.template](./docker/service_conf.yaml.template).

Para atualizar a porta HTTP de serviço padrão (80), vá até [docker-compose.yml](./docker/docker-compose.yml) e altere `80:80` para `<SUA_PORTA_DE_SERVIÇO>:80`.

Atualizações nas configurações acima exigem um reinício de todos os contêineres para que tenham efeito:

> ```bash
> $ docker compose -f docker-compose.yml up -d
> ```

### Mudar o mecanismo de documentos de Elasticsearch para Infinity

O RAGFlow usa o Elasticsearch por padrão para armazenar texto completo e vetores. Para mudar para o [Infinity](https://github.com/infiniflow/infinity/), siga estas etapas:

1. Pare todos os contêineres em execução:

   ```bash
   $ docker compose -f docker/docker-compose.yml down -v
   ```

   Note: `-v` irá deletar os volumes do contêiner, e os dados existentes serão apagados.
2. Defina `DOC_ENGINE` no **docker/.env** para `infinity`.
3. Inicie os contêineres:

   ```bash
   $ docker compose -f docker-compose.yml up -d
   ```

> [!ATENÇÃO]
 > A mudança para o Infinity em uma máquina Linux/arm64 ainda não é oficialmente suportada.

## 🔧 Criar uma imagem Docker

Esta imagem tem cerca de 2 GB de tamanho e depende de serviços externos de LLM e incorporação.

```bash
git clone https://github.com/infiniflow/ragflow.git
cd ragflow/
docker build --platform linux/amd64 -f Dockerfile -t infiniflow/ragflow:nightly .
```

Se você estiver atrás de um proxy, pode passar argumentos de proxy:

```bash
docker build --platform linux/amd64 \
  --build-arg http_proxy=http://YOUR_PROXY:PORT \
  --build-arg https_proxy=http://YOUR_PROXY:PORT \
  -f Dockerfile -t infiniflow/ragflow:nightly .
```

## 🔨 Lançar o serviço a partir do código-fonte para desenvolvimento

1. Instale o `uv` e o `pre-commit`, ou pule esta etapa se eles já estiverem instalados:

   ```bash
   pipx install uv pre-commit
   ```
2. Clone o código-fonte e instale as dependências Python:

   ```bash
   git clone https://github.com/infiniflow/ragflow.git
   cd ragflow/
   uv sync --python 3.13 # instala os módulos Python dependentes do RAGFlow
   uv run python3 download_deps.py
   pre-commit install
   ```
3. Inicie os serviços dependentes (MinIO, Elasticsearch, Redis e MySQL) usando Docker Compose:

   ```bash
   docker compose -f docker/docker-compose-base.yml up -d
   ```

   Adicione a seguinte linha ao arquivo `/etc/hosts` para resolver todos os hosts especificados em **docker/.env** para `127.0.0.1`:

   ```
   127.0.0.1       es01 infinity mysql minio redis sandbox-executor-manager
   ```
4. Se não conseguir acessar o HuggingFace, defina a variável de ambiente `HF_ENDPOINT` para usar um site espelho:

   ```bash
   export HF_ENDPOINT=https://hf-mirror.com
   ```
5. Se o seu sistema operacional não tiver jemalloc, instale-o da seguinte maneira:

   ```bash
   # ubuntu
   sudo apt-get install libjemalloc-dev
   # centos
   sudo yum instalar jemalloc
   # mac
   sudo brew install jemalloc
   ```
6. Lance o serviço de back-end:

   ```bash
   source .venv/bin/activate
   export PYTHONPATH=$(pwd)
   bash docker/launch_backend_service.sh
   ```
7. Instale as dependências do front-end:

   ```bash
   cd web
   npm install
   ```
8. Lance o serviço de front-end:

   ```bash
   npm run dev
   ```

   _O seguinte resultado confirma o lançamento bem-sucedido do sistema:_

   ![](https://github.com/user-attachments/assets/0daf462c-a24d-4496-a66f-92533534e187)
9. Pare os serviços de front-end e back-end do RAGFlow após a conclusão do desenvolvimento:

   ```bash
   pkill -f "ragflow_server.py|task_executor.py"
   ```

## 📚 Documentação

- [Quickstart](https://ragflow.io/docs/dev/)
- [Configuration](https://ragflow.io/docs/dev/configurations)
- [Release notes](https://ragflow.io/docs/dev/release_notes)
- [User guides](https://ragflow.io/docs/category/user-guides)
- [Developer guides](https://ragflow.io/docs/category/developer-guides)
- [References](https://ragflow.io/docs/dev/category/references)
- [FAQs](https://ragflow.io/docs/dev/faq)

## 📜 Roadmap

Veja o [RAGFlow Roadmap 2026](https://github.com/infiniflow/ragflow/issues/12241)

## 🏄 Comunidade

- [Discord](https://discord.gg/NjYzJD3GM3)
- [X](https://x.com/infiniflowai)
- [GitHub Discussions](https://github.com/orgs/infiniflow/discussions)

## 🙌 Contribuindo

O RAGFlow prospera por meio da colaboração de código aberto. Com esse espírito, abraçamos contribuições diversas da comunidade.
Se você deseja fazer parte, primeiro revise nossas [Diretrizes de Contribuição](https://ragflow.io/docs/dev/contributing).
>>>>>>> upstream/main
