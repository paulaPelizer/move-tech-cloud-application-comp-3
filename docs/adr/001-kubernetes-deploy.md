# ADR 001 — Usar K3s para deploy da aplicação



**Status:** Aceito

**Data:** 2026-08-09



## Contexto



A aplicação precisava ser implantada na Magalu Cloud de forma acessível publicamente, com múltiplas réplicas, recuperação automática em caso de falha e possibilidade de evolução futura.



Também era necessário utilizar Kubernetes para praticar conceitos de orquestração, probes, Services, Secrets, observabilidade e deploy automatizado.



## Alternativas consideradas



### K3s em VM



**Vantagens:**

- Kubernetes leve e adequado para ambientes pequenos

- Menor consumo de recursos

- Menor custo de infraestrutura

- Provisionamento relativamente simples

- Compatibilidade com manifests Kubernetes padrão

- Permite self-healing, múltiplas réplicas e integração com Helm



**Desvantagens:**

- Cluster single-node

- A própria VM representa um ponto único de falha

- Não há alta disponibilidade nativa do control plane

- Capacidade limitada aos recursos da VM



### Kubernetes gerenciado



**Vantagens:**

- Control plane administrado pelo provedor

- Melhor suporte a alta disponibilidade

- Facilita evolução para múltiplos nós



**Desvantagens:**

- Maior complexidade e custo para o escopo do projeto

- Mais recursos do que o necessário para o laboratório



### Docker Compose em VM



**Vantagens:**

- Implantação mais simples

- Menor curva de aprendizado



**Desvantagens:**

- Não oferece orquestração Kubernetes

- Não possui self-healing declarativo equivalente

- Escalabilidade e gerenciamento de réplicas mais limitados

- Não atenderia aos objetivos da competência



## Decisão



Foi escolhido o **K3s executado em uma VM BV1-4-10 com Ubuntu 24.04 na Magalu Cloud**.



A aplicação é implantada como um Deployment Kubernetes com duas réplicas e exposta externamente pelo Klipper ServiceLB.



A decisão priorizou simplicidade, menor custo e compatibilidade com os recursos do Kubernetes utilizados no projeto.



## Consequências



### Positivas



- Menor custo em comparação com uma solução Kubernetes gerenciada

- Uso de manifests Kubernetes padrão

- Recuperação automática de pods

- Duas réplicas da aplicação

- Uso de livenessProbe e readinessProbe

- Integração com Helm

- Integração com Prometheus e Grafana

- Facilidade para aumentar o número de réplicas



### Negativas



- A VM é um single point of failure

- Não existe alta disponibilidade real do cluster

- A capacidade é limitada aos recursos da VM

- Uma falha completa da VM interrompe todos os workloads

- Para um ambiente de produção mais crítico, seria recomendável migrar para Kubernetes gerenciado ou uma arquitetura com múltiplos nós
