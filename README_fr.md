<div align="center">
  <img src="web/public/logo-light.png" width="200" alt="MetaGrossAI logo">
  <h1>MetaGrossAI</h1>
</div>

## 💡 Qu'est-ce que MetaGrossAI ?
**MetaGrossAI** est un moteur de génération augmentée par la recherche (RAG) de premier plan qui fusionne une technologie RAG de pointe avec des capacités d'Agent pour créer une couche de contexte supérieure pour les LLM. Il offre un flux de travail RAG rationalisé et adaptable aux entreprises de toutes tailles. Propulsé par un moteur de contexte convergent et des modèles d'agents prédéfinis, MetaGrossAI permet aux développeurs de transformer des données complexes en systèmes d'IA haute fidélité, prêts pour la production, avec une efficacité et une précision exceptionnelles.

## 🌟 Fonctionnalités clés
### 🍭 **"La qualité en entrée détermine la qualité en sortie"**
- Extraction de connaissances basée sur la compréhension profonde de documents à partir de données non structurées.
- Trouve "l'aiguille dans une botte de foin de données" parmi un nombre illimité de tokens.

### 🍱 **Découpage (chunking) basé sur des modèles**
- Intelligent et explicable.
- Nombreuses options de modèles au choix.

### 🌱 **Citations fondées réduisant les hallucinations**
- Visualisation du découpage du texte pour permettre l'intervention humaine.
- Vue rapide des références clés et citations traçables pour étayer les réponses.

### 🍔 **Compatibilité avec des sources de données hétérogènes**
- Prend en charge Word, diapositives, Excel, txt, images, copies numérisées, données structurées, pages Web, etc.

### 🛀 **Flux de travail RAG automatisé et sans effort**
- LLM et modèles d'intégration configurables.
- API intuitives pour une intégration transparente avec l'entreprise.

## 🎬 Auto-hébergement
### 📝 Prérequis
- CPU >= 4 cores
- RAM >= 16 GB
- Disk >= 50 GB
- Docker >= 24.0.0 & Docker Compose >= v2.26.1
- Python >= 3.13

### 🚀 Démarrer le serveur
1. Assurez-vous que `vm.max_map_count` >= 262144 :
   ```bash
<<<<<<< HEAD
   $ sudo sysctl -w vm.max_map_count=262144
=======
   $ git clone https://github.com/infiniflow/ragflow.git
   ```
3. Démarrez le serveur en utilisant les images Docker préconstruites :

> [!CAUTION]
> Toutes les images Docker sont construites pour les plateformes x86. Nous ne proposons pas actuellement d'images Docker pour ARM64.
> Si vous êtes sur une plateforme ARM64, suivez [ce guide](https://ragflow.io/docs/dev/build_docker_image) pour construire une image Docker compatible avec votre système.

> La commande ci-dessous télécharge l'édition `v0.26.1` de l'image Docker RAGFlow. Consultez le tableau suivant pour les descriptions des différentes éditions de RAGFlow. Pour télécharger une édition de RAGFlow différente de `v0.26.1`, mettez à jour la variable `RAGFLOW_IMAGE` dans **docker/.env** avant d'utiliser `docker compose` pour démarrer le serveur.

```bash
   $ cd ragflow/docker

   # git checkout v0.26.1
   # Optionnel : utiliser un tag stable (voir les versions : https://github.com/infiniflow/ragflow/releases)
   # Cette étape garantit que le fichier **entrypoint.sh** dans le code correspond à la version de l'image Docker.

   # Use CPU for DeepDoc tasks:
   $ docker compose -f docker-compose.yml up -d

   # To use GPU to accelerate DeepDoc tasks:
   # sed -i '1i DEVICE=gpu' .env
   # docker compose -f docker-compose.yml up -d
```

> Remarque : Avant `v0.22.0`, nous fournissions à la fois des images avec des modèles d'embedding et des images slim sans modèles d'embedding. Détails ci-dessous :

| RAGFlow image tag | Image size (GB) | Has embedding models? | Stable?        |
|-------------------|-----------------|-----------------------|----------------|
| v0.21.1           | &approx;9       | ✔️                    | Stable release |
| v0.21.1-slim      | &approx;2       | ❌                     | Stable release |

> À partir de `v0.22.0`, nous ne distribuons que l'édition slim et ne rajoutons plus le suffixe **-slim** au tag d'image.

4. Vérifiez l'état du serveur après son démarrage :

   ```bash
   $ docker logs -f docker-ragflow-cpu-1
   ```

   _La sortie suivante confirme un lancement réussi du système :_

   ```bash

         ____   ___    ______ ______ __
        / __ \ /   |  / ____// ____// /____  _      __
       / /_/ // /| | / / __ / /_   / // __ \| | /| / /
      / _, _// ___ |/ /_/ // __/  / // /_/ /| |/ |/ /
     /_/ |_|/_/  |_|\____//_/    /_/ \____/ |__/|__/

    * Running on all addresses (0.0.0.0)
   ```

   > Si vous sautez cette étape de confirmation et vous connectez directement à RAGFlow, votre navigateur peut afficher une erreur `network abnormal`, car à ce moment-là, votre RAGFlow peut ne pas être entièrement initialisé.
   >
5. Dans votre navigateur web, entrez l'adresse IP de votre serveur et connectez-vous à RAGFlow.

   > Avec les paramètres par défaut, il vous suffit d'entrer `http://IP_OF_YOUR_MACHINE` (**sans** numéro de port), car le port HTTP par défaut `80` peut être omis lors de l'utilisation des configurations par défaut.
   >
6. Dans [service_conf.yaml.template](./docker/service_conf.yaml.template), sélectionnez la fabrique LLM souhaitée dans `user_default_llm` et mettez à jour le champ `API_KEY` avec la clé API correspondante.

   > Voir [llm_api_key_setup](https://ragflow.io/docs/dev/llm_api_key_setup) pour plus d'informations.
   >

   _Le spectacle commence !_

## 🔧 Configurations

En ce qui concerne les configurations système, vous devrez gérer les fichiers suivants :

- [.env](./docker/.env) : Conserve les paramètres de base du système, tels que `SVR_HTTP_PORT`, `MYSQL_PASSWORD` et `MINIO_PASSWORD`.
- [service_conf.yaml.template](./docker/service_conf.yaml.template) : Configure les services back-end. Les variables d'environnement dans ce fichier seront automatiquement renseignées au démarrage du conteneur Docker. Toutes les variables d'environnement définies dans le conteneur Docker seront disponibles, vous permettant de personnaliser le comportement du service en fonction de l'environnement de déploiement.
- [docker-compose.yml](./docker/docker-compose.yml) : Le système s'appuie sur [docker-compose.yml](./docker/docker-compose.yml) pour démarrer.

> Le fichier [./docker/README](./docker/README.md) fournit une description détaillée des paramètres d'environnement et des configurations de services qui peuvent être utilisés comme `${ENV_VARS}` dans le fichier [service_conf.yaml.template](./docker/service_conf.yaml.template).

Pour mettre à jour le port HTTP de service par défaut (80), accédez à [docker-compose.yml](./docker/docker-compose.yml) et changez `80:80` en `<YOUR_SERVING_PORT>:80`.

Les mises à jour des configurations ci-dessus nécessitent un redémarrage de tous les conteneurs pour prendre effet :

> ```bash
> $ docker compose -f docker-compose.yml up -d
> ```

### Passer du moteur de documents Elasticsearch à Infinity

RAGFlow utilise Elasticsearch par défaut pour stocker le texte intégral et les vecteurs. Pour passer à [Infinity](https://github.com/infiniflow/infinity/), suivez ces étapes :

1. Arrêtez tous les conteneurs en cours d'exécution :

   ```bash
   $ docker compose -f docker/docker-compose.yml down -v
   ```

> [!WARNING]
> `-v` supprimera les volumes des conteneurs Docker, et les données existantes seront effacées.

2. Définissez `DOC_ENGINE` dans **docker/.env** sur `infinity`.
3. Démarrez les conteneurs :

   ```bash
   $ docker compose -f docker-compose.yml up -d
   ```

> [!WARNING]
> Le passage à Infinity sur une machine Linux/arm64 n'est pas encore officiellement pris en charge.

## 🔧 Construire une image Docker

Cette image fait environ 2 Go et dépend de services LLM et d'embedding externes.

```bash
git clone https://github.com/infiniflow/ragflow.git
cd ragflow/
docker build --platform linux/amd64 -f Dockerfile -t infiniflow/ragflow:nightly .
```

Ou si vous êtes derrière un proxy, vous pouvez passer des arguments de proxy :

```bash
docker build --platform linux/amd64 \
  --build-arg http_proxy=http://YOUR_PROXY:PORT \
  --build-arg https_proxy=http://YOUR_PROXY:PORT \
  -f Dockerfile -t infiniflow/ragflow:nightly .
```

## 🔨 Lancer le service depuis les sources pour le développement

1. Installez `uv` et `pre-commit`, ou ignorez cette étape s'ils sont déjà installés :

   ```bash
   pipx install uv pre-commit
   ```
2. Clonez le code source et installez les dépendances Python :

   ```bash
   git clone https://github.com/infiniflow/ragflow.git
   cd ragflow/
   uv sync --python 3.13 # install RAGFlow dependent python modules
   uv run python3 download_deps.py
   pre-commit install
   ```
3. Lancez les services dépendants (MinIO, Elasticsearch, Redis et MySQL) avec Docker Compose :

   ```bash
   docker compose -f docker/docker-compose-base.yml up -d
   ```

   Ajoutez la ligne suivante à `/etc/hosts` pour résoudre tous les hôtes spécifiés dans **docker/.env** vers `127.0.0.1` :

   ```
   127.0.0.1       es01 infinity mysql minio redis sandbox-executor-manager
   ```
4. Si vous ne pouvez pas accéder à HuggingFace, définissez la variable d'environnement `HF_ENDPOINT` pour utiliser un site miroir :

   ```bash
   export HF_ENDPOINT=https://hf-mirror.com
   ```
5. Si votre système d'exploitation n'a pas jemalloc, installez-le comme suit :

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
6. Lancez le service back-end :

   ```bash
   source .venv/bin/activate
   export PYTHONPATH=$(pwd)
   bash docker/launch_backend_service.sh
   ```
7. Installez les dépendances front-end :

   ```bash
   cd web
   npm install
   ```
8. Lancez le service front-end :

   ```bash
   npm run dev
   ```

   _La sortie suivante confirme un lancement réussi du système :_

   ![](https://github.com/user-attachments/assets/0daf462c-a24d-4496-a66f-92533534e187)
9. Arrêtez les services front-end et back-end de RAGFlow une fois le développement terminé :

   ```bash
   pkill -f "ragflow_server.py|task_executor.py"
   ```

## 📚 Documentation

- [Quickstart](https://ragflow.io/docs/dev/)
- [Configuration](https://ragflow.io/docs/dev/configurations)
- [Release notes](https://ragflow.io/docs/dev/release_notes)
- [User guides](https://ragflow.io/docs/category/user-guides)
- [Developer guides](https://ragflow.io/docs/category/developer-guides)
- [References](https://ragflow.io/docs/dev/category/references)
- [FAQs](https://ragflow.io/docs/dev/faq)

## 📜 Roadmap

Voir la [Feuille de route RAGFlow 2026](https://github.com/infiniflow/ragflow/issues/12241)

## 🏄 Communauté

- [Discord](https://discord.gg/NjYzJD3GM3)
- [X](https://x.com/infiniflowai)
- [GitHub Discussions](https://github.com/orgs/infiniflow/discussions)

## 🙌 Contribuer

RAGFlow s'épanouit grâce à la collaboration open-source. Dans cet esprit, nous accueillons des contributions diverses de la communauté.
Si vous souhaitez en faire partie, consultez d'abord nos [Directives de contribution](https://ragflow.io/docs/dev/contributing).
>>>>>>> upstream/main
