# ADR 002 — Usar DBaaS PostgreSQL da Magalu Cloud



**Status:** Aceito

**Data:** 2026-08-09



## Contexto



A aplicação precisa persistir pedidos e itens de forma confiável.



Como a API executa em múltiplos pods e esses pods podem ser recriados a qualquer momento, os dados não podem depender do ciclo de vida dos containers ou do armazenamento local da VM.



O banco também precisa aceitar conexões simultâneas das duas réplicas da aplicação.



## Alternativas consideradas



### DBaaS PostgreSQL gerenciado



**Vantagens:**

\- Persistência independente dos pods

\- Administração simplificada

\- Backup e manutenção gerenciados pelo provedor

\- Permite acesso simultâneo por múltiplas réplicas

\- Maior disponibilidade do que um banco executando dentro do cluster single-node



**Desvantagens:**

\- Custo adicional por tempo de uso

\- Menor controle sobre algumas configurações avançadas

\- Dependência do provedor para upgrades e manutenção



### PostgreSQL dentro do cluster Kubernetes



**Vantagens:**

\- Menor quantidade de serviços externos

\- Possibilidade de reduzir custo em ambientes pequenos



**Desvantagens:**

\- Exigiria volume persistente

\- Backup e recuperação ficariam sob responsabilidade da equipe

\- Maior risco de perda de dados em caso de falha ou recriação da VM

\- Banco e aplicação compartilhariam o mesmo ponto único de falha



## Decisão



Foi escolhido o **DBaaS PostgreSQL 17 da Magalu Cloud**.



O banco foi mantido fora do cluster K3s e acessado pela aplicação por meio da variável `DATABASE\_URL`, armazenada como Secret no Kubernetes.



A decisão priorizou persistência, disponibilidade e redução do trabalho operacional associado à administração do banco.



## Consequências



### Positivas



\- Dados sobrevivem à recriação dos pods da aplicação

\- As duas réplicas podem acessar o mesmo banco simultaneamente

\- Menor esforço de administração do PostgreSQL

\- Maior separação entre estado da aplicação e infraestrutura de execução

\- Menor risco de perda de dados em caso de falha de pod



### Negativas



\- Existe custo por hora de uso do serviço

\- Há dependência do provedor para manutenção e upgrades

\- Algumas configurações avançadas podem ter menor flexibilidade

\- A aplicação depende da conectividade entre o cluster K3s e o DBaaS
