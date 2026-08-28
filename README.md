# FastAPI CI/CD Pipeline

Uma API de exemplo em Python com FastAPI usada como base para uma pipeline CI/CD com GitHub Actions. A aplicação, que cria 2 endpoints (`/` e `/health`), roda em um container Docker e é publicada automaticamente no Docker Hub sempre que há um merge na branch `main`.

## Tecnologias usadas

- Linguagem Python
- Framework FastAPI
- Testes com pytest
- Servidor uvicorn
- Containerização em Docker
- CI/CD no GitHub Actions
- Verificação de segurança (SAST) com Semgrep
- Registro da imagem no Docker Hub

## Como rodar localmente

1. Clonar o repositório
```bash
git clone https://github.com/hal2329/fastapi-cicd-pipeline
cd fastapi-cicd-pipeline
```

2. Criar e ativar ambiente virtual
```bash
python -m venv venv
source venv/Scripts/activate
```

3. Instalar dependências
```bash
pip install -r requirements.txt
```

4. Executar uvicorn
```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

## Como rodar os testes

```bash
python -m pytest tests/ -v
```

O `python -m` é necessário para adicionar a pasta raiz do projeto ao caminho de busca de módulos do Python, evitando o erro `ModuleNotFoundError: No module named 'app'`.

## Estrutura do pipeline CI/CD

### CI (`ci.yml`)

- Acionado sempre que é feito um Pull Request nas branches `main` e `develop`.
- Roda testes com pytest.
- Roda análise de segurança (SAST) com Semgrep.

### CD (`cd.yml`)

- Acionado sempre que há um push na branch `main`.
- Faz login no Docker Hub.
- Cria e publica a imagem no Docker Hub usando as tags `latest` e o SHA do commit.

## Estratégia de branching

- `main`: Onde fica o código de produção e é disparado o `cd.yml`
- `develop`: Onde as `feature/*` são integradas (mescladas) antes de irem pra `main`
- `feature/*`: Estrutura pra criar uma branch para cada feature a ser implementada

## Endpoints da API

- `GET /`: Retorna um JSON com as chaves: mensagem, status e versão
- `GET /health`: Retorna um JSON apenas com status