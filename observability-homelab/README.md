# 🔭 observability-homelab

> Stack completa de monitoramento e observabilidade rodando localmente com Docker Compose.  
> Projeto de estudo e portfólio — construído para demonstrar práticas reais de SRE e Observabilidade.

![Stack](https://img.shields.io/badge/Stack-Prometheus%20%7C%20Grafana%20%7C%20Zabbix%20%7C%20Alertmanager-blue?style=flat-square)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat-square&logo=docker&logoColor=white)
![Status](https://img.shields.io/badge/Status-Em%20desenvolvimento-yellow?style=flat-square)

---

## 🧱 Arquitetura

```
┌─────────────────────────────────────────────────┐
│                   Docker Host                   │
│                                                 │
│  ┌─────────────┐     ┌─────────────────────┐   │
│  │  Prometheus │────▶│    Alertmanager      │   │
│  │  :9090      │     │    :9093             │   │
│  └──────┬──────┘     └──────────┬──────────┘   │
│         │scrape                 │notify         │
│  ┌──────▼──────┐         ┌──────▼──────┐        │
│  │Node Exporter│         │    Slack    │        │
│  │  :9100      │         │  #alertas  │        │
│  └─────────────┘         └─────────────┘        │
│                                                 │
│  ┌─────────────┐     ┌─────────────────────┐   │
│  │   Grafana   │────▶│  Prometheus (source)│   │
│  │   :3000     │     └─────────────────────┘   │
│  └─────────────┘                               │
│                                                 │
│  ┌─────────────┐  ┌──────────┐  ┌──────────┐  │
│  │Zabbix Server│─▶│ MySQL DB │  │Zabbix Web│  │
│  │  :10051     │  │          │  │  :8080   │  │
│  └─────────────┘  └──────────┘  └──────────┘  │
└─────────────────────────────────────────────────┘
```

## 🛠️ Stack

| Ferramenta | Função | Porta |
|---|---|---|
| Prometheus | Coleta e armazenamento de métricas | 9090 |
| Node Exporter | Exporta métricas do host (CPU, memória, disco) | 9100 |
| Grafana | Visualização e dashboards | 3000 |
| Alertmanager | Gerenciamento e roteamento de alertas | 9093 |
| Zabbix Server | Monitoramento de infraestrutura | 10051 |
| Zabbix Web | Interface web do Zabbix | 8080 |

---

## 🚀 Como rodar

### Pré-requisitos
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) instalado
- Git instalado

### 1. Clone o repositório
```bash
git clone https://github.com/SEU-USUARIO/observability-homelab.git
cd observability-homelab
```

### 2. Configure o Alertmanager (opcional)
Edite `alertmanager/alertmanager.yml` e insira seu webhook do Slack:
```yaml
api_url: "https://hooks.slack.com/services/SEU/WEBHOOK/AQUI"
```

### 3. Suba o ambiente
```bash
docker compose up -d
```

### 4. Acesse os serviços

| Serviço | URL | Login |
|---|---|---|
| Grafana | http://localhost:3000 | admin / admin123 |
| Prometheus | http://localhost:9090 | — |
| Alertmanager | http://localhost:9093 | — |
| Zabbix Web | http://localhost:8080 | Admin / zabbix |

---

## 📊 Alertas configurados

| Alerta | Condição | Severidade |
|---|---|---|
| HostDown | Target sem resposta por 1min | 🔴 Critical |
| HighCPUUsage | CPU > 85% por 5min | 🟡 Warning |
| HighMemoryUsage | Memória > 90% por 5min | 🟡 Warning |
| DiskSpaceLow | Disco < 15% disponível | 🔴 Critical |

---

## 📁 Estrutura do projeto

```
observability-homelab/
├── docker-compose.yml
├── prometheus/
│   ├── prometheus.yml       # Configuração de scrape e regras
│   └── alerts.yml           # Regras de alerta
├── grafana/
│   ├── provisioning/
│   │   ├── datasources/     # Prometheus como datasource padrão
│   │   └── dashboards/      # Provisionamento automático de dashboards
│   └── dashboards/          # Arquivos JSON dos dashboards
├── alertmanager/
│   └── alertmanager.yml     # Roteamento de alertas (Slack)
└── zabbix/
```

---

## 📌 Próximos passos

- [ ] Adicionar Loki para coleta de logs
- [ ] Criar dashboard de SLO/SLI no Grafana
- [ ] Adicionar script Python para relatório de alertas
- [ ] Integrar Blackbox Exporter para monitoramento de endpoints HTTP

---

## 👩‍💻 Autora

**Bianca Gasparino de Campos**  
Observability & SRE Engineer  
[LinkedIn](https://www.linkedin.com/in/bianca-gasparino/) · [GitHub](https://github.com/SEU-USUARIO)
