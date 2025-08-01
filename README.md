# InNoHassle Search

*Readme for the [Search](https://github.com/one-zero-eight/search) project*

[![GitHub Actions pre-commit](https://img.shields.io/github/actions/workflow/status/one-zero-eight/search/pre-commit.yaml?label=pre-commit)](https://github.com/one-zero-eight/search/actions)

## 📑 Table of Content
1. [About the Project](#-about-the-project)
2. [Functionality](#%EF%B8%8F-functionality)
3. [Technologies](#%EF%B8%8F-technologies)
4. [Development](#-development)
   - [Run with Docker](#run-with-docker)
   - [Run Locally](#run-locally)
   - [PyCharm Integrations](#pycharm-integrations)
5. [Testing](#-testing)
6. [Contributing](#-contributing)


## 📌 About the Project

This API powers the search functionality within the **InNoHassle** ecosystem — a unified digital platform designed to simplify daily life for students of Innopolis University.
- Services code: [GitHub - one-zero-eight](https://github.com/one-zero-eight)  
- Frontend: [GitHub - website](https://github.com/one-zero-eight/website)    
- API endpoints: [api.innohassle.ru](https://api.innohassle.ru/)  
- InNoHassle website: [innohassle.ru](https://innohassle.ru/)

The search service acts as a smart assistant, helping users quickly find information across various Innopolis University services.
## ⚙️ Functionality

### Supported features:
- `search` - search by keywords. Provides answers in the form of cards linking to resource websites with the ability to preview.  
- `ask` - search by queries in free form. Provides answers generated by LLM with links to resource data.  
- `act` (🚧 *Work in Progress*) - dialogue with action-based assistant. Provides the ability to perform actions in other Innopolis services on behalf of the user.

### Resources used:
- [Campus life](http://campuslife.innopolis.ru/) - general knowledge about the university for quick adaptation
- [Eduwiki](https://eduwiki.innopolis.university) - reference book on the educational process
- [Hotel](https://hotel.innopolis.university/) - information about dormitories
- [Residents](https://sez-innopolis.ru/residents/) - list of resident companies in Innopolis
- [InNoHassle](https://innohassle.ru) - an ecosystem that collects the functionality of Innopolis services in one place or links to them
- [My University](https://my.university.innopolis.ru/) - a system for performing administrative requests

## 🛠️ Technologies

- Environment: [Python 3.11](https://www.python.org/downloads/), [Poetry](https://python-poetry.org/docs/)
- API: [FastAPI](https://fastapi.tiangolo.com/), [Pydantic](https://docs.pydantic.dev/latest/)
- Database: [MongoDB](https://www.mongodb.com/), [Beanie](https://beanie-odm.dev/)
- Dev Tools: [Ruff](https://docs.astral.sh/ruff/), [pre-commit](https://pre-commit.com/)
- Deployment: [Docker](https://www.docker.com/), [Docker Compose](https://docs.docker.com/compose/),
  [GitHub Actions](https://github.com/features/actions)


## 🧑‍💻 Development

### Run with Docker

1. Set up project settings file
   ```bash
   cp settings.example.yaml settings.yaml
   ```
   Replace the `db_url` and `mongo_url` fields in `settings.yaml` with:
   ```bash
   "mongodb://mongoadmin:secret@db:27017/db?authSource=admin"
   ```

2. Set up database settings for [docker-compose](https://docs.docker.com/compose/) container
      in `.env` file:
      ```bash
      cp .env.example .env
      ```

3. Start all services:
   ```bash
   docker compose up
   ```

Now you can find API docs on http://localhost:8004/docs.

### Run locally

1. Install [Python 3.11](https://www.python.org/downloads/)
2. Install [Poetry](https://python-poetry.org/docs/)
3. Install project dependencies with [Poetry](https://python-poetry.org/docs/cli/#options-2).
   ```bash
   poetry install
   ```
4. Set up [pre-commit](https://pre-commit.com/) hooks:

   ```bash
   poetry run pre-commit install --install-hooks -t pre-commit -t commit-msg
   ```
5. Set up project settings file
   ```bash
   cp settings.example.yaml settings.yaml
   ```
6. Set up a [MongoDB](https://www.mongodb.com/) and [Minio](https://min.io/) instances.

    - Set up database settings for [docker-compose](https://docs.docker.com/compose/) container
      in `.env` file:
      ```bash
      cp .env.example .env
      ```
    - Run the database instance:
      ```bash
      docker compose up -d db minio
      ```
    - Make sure to set up the actual database connection in `settings.yaml`.
7. Choose a model runner:
   - **Local models:** leave `infinity_url` unset in `settings.yaml`

   - **Infinity engine:** set `infinity_url` to your deployed instance

   To run Infinity locally:
      ```bash
      uv run --no-project --with "infinity_emb[all]" --with "transformers<4.49" infinity_emb v2 --model-id jinaai/jina-embeddings-v3 --model-id jinaai/jina-reranker-v2-base-multilingual
      ```
   - Or use deployed Infinity engine provided by someone else.

8. Start the ML client
   ```bash
   poetry run python -m src.ml_service
   ```
9. Start the ASGI server
   ```bash
   poetry run python -m src.api
   ```
   Access the API docs on http://127.0.0.1:8001/docs


### PyCharm integrations

- Ruff – [Plugin](https://plugins.jetbrains.com/plugin/20574-ruff):
   linting and formatting  
   Enable enable `Use ruff format` in plugin settings.
- Pydantic – [Plugin](https://plugins.jetbrains.com/plugin/12861-pydantic):
   type hinting support
- Conventional commits – [Plugin](https://plugins.jetbrains.com/plugin/13389-conventional-commit):
   following [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/)

## 🧪 Testing

We use ```pytest``` for testing.
* To run tests enter in your terminal:
   ```
   poetry run pytest tests
   ```
* To generate test coverage report:
   ```
   poetry run pytest  --cov-config=.coveragerc --cov=src/ tests/
   ```
   You can change coverage ignored folders/files in `.coveragerc`.

## 🤝 Contributing

We are open to contributions of any kind.
You can help us with code, bugs, design, documentation, media, new ideas, etc.
If you are interested in contributing, please read
our [contribution guide](https://github.com/one-zero-eight/.github/blob/main/CONTRIBUTING.md).
