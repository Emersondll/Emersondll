<h1 align="center">Olá, eu sou o Emerson Lima 👋</h1>

<p align="center">
  <b>Tech Lead e Engenheiro de Software Java</b> · Pós-graduado em Desenvolvimento de Software<br>
  Sistemas escaláveis, arquitetura de backend, segurança e automação com IA<br>
  📍 Araraquara-SP · 💼 <a href="https://www.linkedin.com/in/emersondll">LinkedIn</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Java-26-ED8B00?logo=openjdk&logoColor=white" alt="Java">
  <img src="https://img.shields.io/badge/Spring%20Boot-4-6DB33F?logo=springboot&logoColor=white" alt="Spring Boot">
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white" alt="PostgreSQL">
  <img src="https://img.shields.io/badge/MongoDB-47A248?logo=mongodb&logoColor=white" alt="MongoDB">
  <img src="https://img.shields.io/badge/RabbitMQ-FF6600?logo=rabbitmq&logoColor=white" alt="RabbitMQ">
  <img src="https://img.shields.io/badge/Kafka-231F20?logo=apachekafka&logoColor=white" alt="Kafka">
  <img src="https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white" alt="Docker">
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white" alt="Kubernetes">
  <img src="https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black" alt="React">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white" alt="TypeScript">
  <img src="https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white" alt="Python">
</p>

---

## 🚀 Projeto em destaque: Domus

**[Domus Platform](https://github.com/Emersondll/Domus-platform)** é uma plataforma de **portaria virtual multicondomínio**. Ela foi pensada para empresas que operam a portaria remota de vários condomínios ao mesmo tempo: atendimento, controle de acesso, visitantes, encomendas, reservas e comunicação com os moradores.

> **Estado atual:** fases 0 a 8 executadas. O projeto ainda não está pronto para produção, porque nada foi testado com hardware real. O [`STATUS.md`](https://github.com/Emersondll/Domus-platform/blob/main/docs/STATUS.md) do repositório lista com transparência o que está concluído, simulado e pendente.

### O que tem dentro

| Peça | O que faz | Tecnologias |
|---|---|---|
| **Backend (API)** | Monólito modular, com um pacote por domínio (identidade, comercial, moradores, veículos, encomendas, reservas, acesso, notificações, dispositivos, privacidade) | Java 26, Spring Boot 4, JPA, Flyway, PostgreSQL 17 |
| **Painel da central** | Tela do operador: atendimento em andamento, abertura de porta com justificativa obrigatória, transferência e histórico | React, TypeScript |
| **App do morador (PWA)** | Chamada à central e convite com QR Code de uso limitado | React, TypeScript, PWA |
| **Agente local de contingência** | Mantém o condomínio operando quando a internet cai | Java, SQLite, systemd, pacote `.deb` |
| **Identidade e acesso** | Login com MFA para operadores e administradores, em tema próprio em português | Keycloak, OIDC, OAuth2 Resource Server (JWT), PKCE |
| **Observabilidade** | Métricas, painéis e traces | Micrometer, Prometheus, Grafana, OpenTelemetry |
| **Infraestrutura** | Docker Compose de produção com TLS, backup e restauração testados | Docker, Caddy, scripts de restore-drill |

### Decisões de arquitetura que me orgulho de ter documentado

Cada decisão está registrada em ADRs no próprio repositório:

- **Multi-tenancy no banco:** uma coluna `condominium_id` em toda tabela, com **chaves estrangeiras compostas** para o banco recusar vínculos entre condomínios. Recurso fora do escopo do usuário responde 404, para não revelar que ele existe.
- **Autorização por associações, não por token:** os papéis vêm de `membership` e não do JWT, então revogar um acesso vale na hora.
- **Agente de contingência com modelo *pull*:** não abre nenhuma porta de entrada no condomínio. Usa política assinada e versionada, SQLite com outbox e IDs idempotentes, e **falha fechada** quando o uso não pode ser registrado. Na reconciliação, conflitos ficam explícitos em vez de "a última escrita vence".
- **Atualização assinada do agente:** o manifesto é verificado por assinatura e o launcher faz rollback automático depois de 3 falhas.
- **Privacidade (LGPD) desde o início:** há documentação própria de LGPD, segurança e operação, além de auditoria e criptografia na camada compartilhada.

### Qualidade comprovada, não prometida

- **Mais de 500 testes automatizados no backend**, a maioria de integração com PostgreSQL real via Testcontainers. Na última rodada: 108 unitários e 441 de integração, com 0 falhas.
- **Testes do agente** (43), do painel e do app do morador (mais de 170 de UI, OIDC e renovação de token).
- **Provas ponta a ponta em scripts:** queda real da central com o agente local assumindo, navegador real com Playwright, login com Keycloak e MFA, atualização assinada do agente, restauração de backup e teste de carga.
- **CI no GitHub Actions**, Dependabot e verificação de segredos com gitleaks.

<p align="center">
  <a href="https://github.com/Emersondll/Domus-platform"><b>➡️ Ver o repositório do Domus</b></a>
</p>

---

## 🧩 Outros projetos

### 🤖 [AI Refactor Agent](https://github.com/Emersondll/ai-refactor-agent)
Agente autônomo de refatoração Java que roda **100% local** com Ollama, sem enviar código para a nuvem. Ele combina ferramentas determinísticas (OpenRewrite, Google Java Format) com LLMs que trabalham método a método, em um pipeline de **21 etapas**. Gera testes até chegar a 90% de cobertura e **só aceita uma mudança se o `mvn compile` continuar verde**. Tem dashboard ao vivo e relatório narrativo por classe.
`Python` · `Ollama` · `OpenRewrite` · `JaCoCo` · `Maven`

### 📐 [java-spec](https://github.com/Emersondll/java-spec)
Conjunto de **27 especificações** para padronizar microsserviços Java 26 e Spring Boot: arquitetura hexagonal, qualidade de código, estratégia de testes, observabilidade com OpenTelemetry, circuit breaker, SAGA com RabbitMQ, idempotência HTTP, segurança, CI/CD e GitOps. Serve de base para o agente de refatoração acima.

### 💳 Autorizadores de cartão
Desafios técnicos resolvidos com qualidade de produção: Java com records, injeção por construtor, tratamento centralizado de erros, JavaDoc completo e testes.
- [**card-transaction-authorizer**](https://github.com/Emersondll/card-transaction-authorizer): regras por MCC com fallback, MongoDB e cobertura de testes acima de 99%.
- [**smart-card-authorizer**](https://github.com/Emersondll/smart-card-authorizer): compra à vista, parcelada, saque e pagamento via API REST.
- [**vr-card-authorizer**](https://github.com/Emersondll/vr-card-authorizer): mini autorizador de Vale Refeição e Vale Alimentação.

### 🛠️ Ferramentas
- [**spotify-audio-recorder-web**](https://github.com/Emersondll/spotify-audio-recorder-web): grava o áudio da aba e gera um MP3 a cada mudança de faixa.
- [**java-academy**](https://github.com/Emersondll/java-academy): plataforma de cursos e conteúdos educacionais.

---

## 💼 Trajetória

Tenho mais de 7 anos construindo backends Java em empresas de grande porte, em crédito, antifraude e varejo/indústria.

| Período | Empresa | Papel |
|---|---|---|
| 2025 – hoje | **Invillia** | Tech Lead, equipe de Crédito. Liderei o Pipeline Padrão (k8shelper com Jenkins) e a revisão de políticas IAM no Vault |
| 2024 – 2025 | **Livelo** | Tech Lead, segurança antifraude no onboarding (LGPD e GDPR) |
| 2023 – 2024 | **BEES / Ambev** | Dev Java Sênior, APIs de precificação e gestão de deals na América do Sul |
| 2021 – 2023 | **Solutis** | Dev Java Sênior, antifraude, Smart Router para substituir o Apigee, WebFlux |
| 2019 – 2021 | **Exactaworks** | Dev Java Sênior, APIs B2B, WebFlux e Flyway |
| 2018 – 2019 | **DXC Technology** | Dev Java Pleno, Full Stack no Banco Votorantim |

## 🛠️ Stack

**Backend:** Java · Spring Boot · Spring Cloud · WebFlux (reativo) · JPA/Hibernate · Apache Camel
**Dados:** PostgreSQL · MongoDB · Oracle · MySQL · PL/SQL · SQLite · Flyway
**Mensageria:** RabbitMQ · Kafka
**Cloud e DevOps:** Docker · Kubernetes · AWS (EKS) · Azure · OpenShift · Jenkins · GitHub Actions · ArgoCD (GitOps) · Vault
**Observabilidade:** OpenTelemetry · Prometheus · Grafana · New Relic · Kibana
**Front-end:** React · TypeScript · Angular
**Qualidade:** JUnit 5 · Mockito · Testcontainers · JaCoCo · Playwright
**IA:** LLMs locais com Ollama · Claude Code · automação de engenharia com agentes

## 📈 Como eu trabalho

- Decisões de arquitetura registradas em **ADRs**, com as alternativas descartadas e o motivo.
- **Segurança e privacidade** como requisito de projeto, não como remendo.
- Comprovo o que entrego com **testes e provas ponta a ponta**, e digo com clareza o que ainda está pendente.
- Mentoria técnica, code review e definição de boas práticas nas equipes que lidero.

## 📫 Contato

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/emersondll)
[![E-mail](https://img.shields.io/badge/E--mail-emersondll%40outlook.com-D14836?logo=gmail&logoColor=white)](mailto:emersondll@outlook.com)
[![Portfólio](https://img.shields.io/badge/Portf%C3%B3lio-emersondll.github.io-181717?logo=github&logoColor=white)](https://emersondll.github.io)

<p align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=Emersondll&show_icons=true&theme=tokyonight&hide_border=true" alt="Estatísticas do GitHub" height="160">
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=Emersondll&layout=compact&theme=tokyonight&hide_border=true" alt="Linguagens mais usadas" height="160">
</p>
