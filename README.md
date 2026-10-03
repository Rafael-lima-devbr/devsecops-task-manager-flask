# DevSecOps Task Manager Flask

Projeto acadêmico que aplica práticas de **DevOps e DevSecOps** a uma aplicação web em Python/Flask, cobrindo containerização, CI/CD, testes, análise de segurança, deploy de stage simulado e monitoramento.

**Status:** Projeto acadêmico concluído

## Visão geral

A aplicação base é um gerenciador de tarefas em Flask. O foco deste repositório está nas adaptações realizadas para exercitar um ciclo DevSecOps mais completo:

- containerização com Docker e Docker Compose;
- pipeline CI/CD com GitHub Actions;
- testes automatizados com `pytest`;
- SAST com Bandit;
- análise de dependências com `pip-audit`;
- DAST com OWASP ZAP;
- stage simulado;
- métricas com Prometheus;
- visualização e monitoramento com Grafana.

## Pipeline

```text
Code
  |
  v
Tests
  |
  v
SAST + dependency audit
  |
  v
Docker build
  |
  v
Stage
  |
  v
DAST
  |
  v
Monitoring
```

O workflow principal está em `.github/workflows/ci.yml`.

## Tecnologias

- Python
- Flask
- Flask-SQLAlchemy
- Docker
- Docker Compose
- GitHub Actions
- pytest
- Bandit
- pip-audit
- OWASP ZAP
- Prometheus
- Grafana

## Estrutura

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
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── README.md
```

## Execução local

```bash
pip install -r requirements.txt
cd todo_project
python run.py
```

Aplicação:

```text
http://127.0.0.1:5000
```

## Execução com Docker

```bash
docker compose up --build
```

Serviços principais:

```text
Aplicação:  http://localhost:5000
Métricas:   http://localhost:5000/metrics
Prometheus: http://localhost:9090
Grafana:    http://localhost:3000
```

## Segurança

### SAST

Bandit é utilizado para analisar o código Python:

```bash
python3 -m bandit -r todo_project -f txt
```

### Dependências

```bash
python3 -m pip_audit -r requirements.txt
```

### DAST

O OWASP ZAP é executado contra a aplicação em funcionamento no ambiente controlado de stage.

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

A aplicação expõe métricas em `/metrics`, coletadas pelo Prometheus e visualizadas no Grafana.

O projeto também inclui regras de alerta para situações como:

- aplicação indisponível;
- possível tentativa de força bruta na rota de login;
- respostas HTTP 5xx.

## Fluxo de branches utilizado

```text
main         -> branch principal
development  -> desenvolvimento
stage        -> homologação / stage
production   -> produção
```

## Escopo acadêmico

Este repositório simula um ciclo DevSecOps em ambiente controlado. Algumas etapas, especialmente stage e monitoramento, foram construídas com Docker e GitHub Actions para fins de estudo e demonstração.

## Créditos

A aplicação Flask utilizada como base veio do projeto público:

https://github.com/AdityaBagad/Task-Manager-using-Flask

As adaptações deste repositório concentram-se em containerização, CI/CD, testes, segurança e monitoramento.

## Autor

Adaptado e documentado por Rafael Lima como projeto acadêmico de DevSecOps.
