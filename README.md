# DevSecOps Task Manager Flask

Projeto acadêmico de DevSecOps aplicado a uma aplicação web de gerenciamento de tarefas pessoais desenvolvida com Python e Flask.

O objetivo deste projeto é demonstrar a aplicação prática de conceitos de DevOps e DevSecOps, incluindo containerização, versionamento, pipeline CI/CD, análise estática de segurança, análise de dependências, análise dinâmica, deploy em ambiente de stage e monitoramento pós-implantação.

## Sobre o projeto

A aplicação base consiste em um sistema simples de gerenciamento de tarefas, desenvolvido com Flask. A partir dessa aplicação, foram realizadas adaptações para permitir sua execução em container Docker e sua integração com práticas modernas de DevSecOps.

As principais melhorias implementadas foram:

* Ajuste de dependências para execução em ambiente Python 3.11;
* Containerização da aplicação com Docker;
* Execução da aplicação com Docker Compose;
* Criação de pipeline CI/CD com GitHub Actions;
* Execução de testes automatizados com pytest;
* Análise estática de segurança com Bandit;
* Análise de dependências com pip-audit;
* Análise dinâmica de segurança com OWASP ZAP;
* Deploy em ambiente de stage simulado;
* Monitoramento pós-implantação com Prometheus e Grafana;
* Exposição de métricas da aplicação no endpoint `/metrics`.

## Tecnologias utilizadas

* Python
* Flask
* Flask-SQLAlchemy
* Docker
* Docker Compose
* GitHub Actions
* pytest
* Bandit
* pip-audit
* OWASP ZAP
* Prometheus
* Grafana

## Estrutura do projeto

```text
.
├── .github/
│   └── workflows/
│       └── ci.yml
├── monitoring/
│   ├── prometheus.yml
│   └── alert.rules.yml
├── tests/
│   └── test_basic.py
├── todo_project/
│   ├── run.py
│   └── todo/
│       ├── models.py
│       ├── forms.py
│       ├── routes.py
│       └── templates/
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── README.md
```

## Execução local

Para executar a aplicação localmente, instale as dependências e inicie o servidor Flask:

```bash
pip install -r requirements.txt
cd todo_project
python run.py
```

A aplicação ficará disponível em:

```text
http://127.0.0.1:5000
```

## Execução com Docker

Para executar a aplicação em container Docker:

```bash
docker compose up --build
```

Em ambientes com a versão antiga do Docker Compose, utilize:

```bash
docker-compose up --build
```

A aplicação ficará disponível em:

```text
http://localhost:5000
```

## Pipeline CI/CD

O pipeline foi implementado com GitHub Actions no arquivo:

```text
.github/workflows/ci.yml
```

O fluxo contempla as seguintes etapas:

* Checkout do código;
* Configuração do ambiente Python;
* Instalação das dependências;
* Validação da aplicação Flask;
* Execução de testes automatizados;
* Build da imagem Docker;
* Etapa de review para Pull Requests;
* Deploy em ambiente de stage;
* Execução de DAST com OWASP ZAP no stage;
* Monitoramento pós-implantação com Prometheus e Grafana.

## Segurança

### SAST com Bandit

A análise estática de segurança foi realizada com Bandit, ferramenta voltada para projetos Python.

Exemplo de execução:

```bash
python3 -m bandit -r todo_project -f txt
```

Também é possível gerar relatório em HTML:

```bash
python3 -m bandit -r todo_project -f html -o bandit-report.html
```

### Análise de dependências com pip-audit

As dependências do projeto foram analisadas com pip-audit, verificando vulnerabilidades conhecidas nos pacotes listados em `requirements.txt`.

```bash
python3 -m pip_audit -r requirements.txt
```

### DAST com OWASP ZAP

A análise dinâmica foi realizada com OWASP ZAP, testando a aplicação em execução.

Exemplo de execução:

```bash
docker run --rm \
  --network host \
  -v "$(pwd)/zap-reports":/zap/wrk \
  ghcr.io/zaproxy/zaproxy:stable \
  zap-baseline.py \
  -t http://127.0.0.1:5000 \
  -r zap-report.html
```

## Monitoramento

A aplicação foi adaptada para expor métricas no endpoint:

```text
/metrics
```

Essas métricas podem ser coletadas pelo Prometheus e visualizadas no Grafana.

O ambiente de monitoramento foi configurado no `docker-compose.yml`, incluindo:

* Aplicação Flask;
* Prometheus;
* Grafana.

Endpoints principais:

```text
Aplicação:   http://localhost:5000
Métricas:    http://localhost:5000/metrics
Prometheus:  http://localhost:9090
Grafana:     http://localhost:3000
```

Credenciais padrão do Grafana:

```text
Usuário: admin
Senha: admin
```

## Regras de alerta

O projeto inclui regras de alerta no Prometheus para situações como:

* Aplicação indisponível;
* Possível tentativa de força bruta na rota de login;
* Erros internos HTTP 5xx.

Essas regras estão localizadas em:

```text
monitoring/alert.rules.yml
```

## Branches utilizadas

O projeto foi estruturado considerando branches para diferentes momentos do fluxo DevSecOps:

```text
main         → branch principal
development  → desenvolvimento
stage        → ambiente de homologação/stage
production   → produção
```

## Objetivo acadêmico

Este projeto foi desenvolvido com finalidade acadêmica, simulando um ciclo DevSecOps completo em uma aplicação Flask. Algumas etapas, como deploy em stage e monitoramento, foram simuladas em ambiente controlado utilizando Docker e GitHub Actions.

## Observação sobre linguagens

O GitHub pode classificar este repositório majoritariamente como JavaScript devido à presença de arquivos estáticos da aplicação base. No entanto, a aplicação é executada com Python/Flask, e o foco deste projeto está nas práticas de DevSecOps aplicadas ao ciclo de desenvolvimento, segurança, entrega e monitoramento.

## Créditos

A aplicação base utilizada neste projeto foi obtida a partir do repositório público:

```text
https://github.com/AdityaBagad/Task-Manager-using-Flask
```

As adaptações realizadas neste repositório têm foco em práticas de DevSecOps, incluindo containerização, pipeline CI/CD, testes, segurança e monitoramento.

## Autor

Projeto adaptado e documentado por Rafael Lima como parte de um estudo acadêmico de DevSecOps.
