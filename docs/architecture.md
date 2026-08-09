\# Arquitetura da Solução



\## Visão geral



A solução consiste em uma API de pedidos desenvolvida com FastAPI e implantada na Magalu Cloud sobre um cluster K3s single-node.



A aplicação é executada com duas réplicas, utiliza PostgreSQL gerenciado para persistência, Container Registry para armazenamento das imagens Docker, GitHub Actions para CI/CD e kube-prometheus-stack para observabilidade com Prometheus e Grafana.



\## Diagrama de arquitetura


```mermaid
flowchart LR

    USER["Cliente / Browser"]
    GH["GitHub Actions"]

    subgraph MGC["Magalu Cloud"]
        direction LR

        CR["Container Registry"]
        DB[("DBaaS PostgreSQL 17")]

        subgraph VM["VM BV1-4-10 / Ubuntu 24.04 / K3s"]
            direction TB

            LB["Klipper ServiceLB<br/>Porta 80"]

            subgraph APP["Aplicação"]
                direction LR
                API1["cloud-application<br/>Pod 1"]
                API2["cloud-application<br/>Pod 2"]
            end

            subgraph OBS["Observabilidade"]
                direction LR
                PROM["Prometheus"]
                GRAF["Grafana"]
            end

            LB -->|HTTP :8000| API1
            LB -->|HTTP :8000| API2

            PROM -->|GET /metrics| API1
            PROM -->|GET /metrics| API2

            GRAF -->|PromQL / HTTP| PROM
        end
    end

    USER -->|HTTP :80| LB

    API1 -->|PostgreSQL TCP :5432| DB
    API2 -->|PostgreSQL TCP :5432| DB

    GH -->|HTTPS / Docker push| CR
    CR -->|HTTPS / Docker pull| API1
    CR -->|HTTPS / Docker pull| API2

    GH -->|kubectl / Kubernetes API| VM

    USER -->|HTTP :3000| GRAF
    USER -->|HTTP :9090| PROM
