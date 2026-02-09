# 5. Roadmap de Evolução Técnica

## 5.1 Objetivo

Conectar as decisões arquiteturais da Fase 4 ao futuro do produto, demonstrando que as escolhas feitas (Monólito Modular + Clean Architecture) não são estáticas, mas sim a base de um plano de evolução incremental orientado por evidências.

---

## 5.2 Princípio do Roadmap

> **Evolução por evidência, não por especulação.**
>
> Cada transição de fase é ativada por **gatilhos concretos** — indicadores mensuráveis que sinalizam que a arquitetura atual não atende mais às necessidades do produto. Nenhuma mudança é feita "porque pode ser que precise".

---

## 5.3 Visão Geral das Fases de Evolução

```mermaid
graph LR
    subgraph F1["📍 FASE ATUAL"]
        A["Monólito Modular<br/>Clean Architecture<br/>1 hospital • 5 devs<br/>MVP focado em boletins"]
    end

    subgraph F2["🎯 FASE 2"]
        B["Monólito Modular<br/>Amadurecido<br/>3-5 hospitais • 8-12 devs<br/>Múltiplas integrações MV"]
    end

    subgraph F3["🔮 FASE 3"]
        C["Híbrido<br/>Monólito + Serviços<br/>10+ hospitais • 15+ devs<br/>Novos domínios (exames, IA)"]
    end

    subgraph F4["⭐ FASE 4"]
        D["Microsserviços<br/>Plataforma completa<br/>50+ hospitais • múltiplas equipes<br/>Ecossistema de saúde digital"]
    end

    F1 -->|"Gatilhos<br/>de escala"| F2
    F2 -->|"Gatilhos de<br/>complexidade"| F3
    F3 -->|"Gatilhos<br/>organizacionais"| F4

    style F1 fill:#27ae60,stroke:#1e8449,color:#fff
    style F2 fill:#2980b9,stroke:#1a5276,color:#fff
    style F3 fill:#8e44ad,stroke:#6c3483,color:#fff
    style F4 fill:#d35400,stroke:#a04000,color:#fff
```

---

## 5.4 Detalhamento de Cada Fase

### 📍 Fase 1 — Monólito Modular (ATUAL)

**Contexto:** MVP em operação com 1 hospital parceiro, equipe de 5 desenvolvedores, domínio focado exclusivamente em boletins médicos.

**Decisões arquiteturais ativas:**

| Decisão | Justificativa |
|---|---|
| Monólito Modular | Proporcional à equipe e escopo atual |
| Clean Architecture | Organização interna com regra de dependência |
| ACL para MV | Desacoplamento do sistema legado |
| Oracle compartilhado | Simplicidade operacional para MVP |
| DDD com Bounded Contexts | Fronteiras conceituais preparando evolução futura |

**Ações técnicas desta fase:**

1. **Formalizar a Clean Architecture** no código existente — reorganizar pastas conforme estrutura proposta na Seção 3
2. **Definir interfaces (ports)** para todos os pontos de integração — Repository, Gateway, Logger
3. **Implementar testes unitários** nos Use Cases usando mocks das interfaces
4. **Definir observabilidade** com Elasticsearch para logs de auditoria (endereçando o gap da Fase 3)
5. **Documentar ADRs** para cada decisão significativa

**Métricas de monitoramento:**

| Métrica | Ferramenta | Objetivo |
|---|---|---|
| Cobertura de testes nos Use Cases | SonarQube | > 80% |
| Tempo de build + deploy | GitHub Actions | < 10 minutos |
| Violações de dependência entre camadas | Análise estática / Code Review | Zero |
| Tempo de resposta da API | Prometheus/Grafana | p95 < 500ms |
| Disponibilidade | Health checks | > 99.5% |

---

### 🎯 Fase 2 — Monólito Modular Amadurecido

**Gatilhos de transição (quando sair da Fase 1):**

| Gatilho | Indicador Mensurável |
|---|---|
| Novos hospitais | 3+ hospitais parceiros, cada um com versão diferente da API do MV |
| Crescimento da equipe | 8+ desenvolvedores com conflitos frequentes de merge (> 5/semana) |
| Carga de dados | Volume de boletins importados impactando performance das queries |

**O que muda:**

```mermaid
graph TB
    subgraph "Monólito Modular Amadurecido"
        subgraph "Módulo Auth"
            AUTH_UC[Use Cases Auth]
            AUTH_DB[(Schema Auth)]
        end
        
        subgraph "Módulo Boletim"
            BOL_UC[Use Cases Boletim]
            BOL_DB[(Schema Boletim)]
        end
        
        subgraph "Módulo Auditoria"
            AUD_UC[Use Cases Auditoria]
            AUD_DB[(Schema Auditoria)]
        end
        
        subgraph "Módulo ACL"
            ACL_UC[Adapters ACL]
            ACL_H1[Adapter Hospital 1<br/>MV REST v2]
            ACL_H2[Adapter Hospital 2<br/>MV REST v3]
            ACL_H3[Adapter Hospital 3<br/>FHIR R4]
        end
    end

    AUTH_UC -.->|"via interface"| BOL_UC
    BOL_UC -.->|"via interface"| AUD_UC
    BOL_UC -.->|"via interface"| ACL_UC

    style AUTH_DB fill:#d35400,stroke:#a04000,color:#fff
    style BOL_DB fill:#d35400,stroke:#a04000,color:#fff
    style AUD_DB fill:#d35400,stroke:#a04000,color:#fff
```

**Ações técnicas desta fase:**

1. **Separar schemas no Oracle** — cada módulo passa a ter seu próprio schema, preparando a separação futura de bancos de dados
2. **Implementar múltiplos adapters de ACL** — um por hospital/versão de MV, todos implementando a mesma interface `MVGateway`
3. **Comunicação entre módulos via interfaces** — módulos não importam classes uns dos outros, apenas contratos
4. **Implementar cache de boletins** — reduzir chamadas ao MV para boletins já importados
5. **Feature flags** — habilitar funcionalidades por hospital sem deploys separados

**Por que ainda é monólito:** Mesmo com 3-5 hospitais, o deploy único simplifica operações. A separação de schemas já prepara o terreno, mas o custo de orquestrar microsserviços ainda não se justifica.

---

### 🔮 Fase 3 — Arquitetura Híbrida (Monólito + Serviços)

**Gatilhos de transição (quando sair da Fase 2):**

| Gatilho | Indicador Mensurável |
|---|---|
| Domínios divergentes | Novos módulos (exames, IA de simplificação) com requisitos técnicos muito diferentes dos boletins |
| Escala assimétrica | Módulo ACL precisa de 10x mais recursos que Auditoria |
| Resiliência | Falha na integração MV derruba funcionalidades que não dependem dela |
| Equipe | 15+ devs, necessidade de ownership por domínio |

**O que muda (Strangler Fig em ação):**

```mermaid
graph TB
    subgraph "API Gateway"
        GW[🔀 API Gateway<br/>Roteamento + Rate Limiting]
    end

    subgraph "Microsserviço Extraído"
        ACL_SVC["🔄 ACL Service<br/>[Deploy independente]<br/>Adapters por hospital<br/>Circuit breaker<br/>Retry policies"]
        ACL_DB[(ACL Database)]
        ACL_SVC --> ACL_DB
    end

    subgraph "Monólito Reduzido"
        AUTH[Módulo Auth]
        BOL[Módulo Boletim]
        AUD[Módulo Auditoria]
        CORE_DB[(Core Database)]
        AUTH --> CORE_DB
        BOL --> CORE_DB
        AUD --> CORE_DB
    end

    subgraph "Novo Domínio"
        IA_SVC["🤖 IA Service<br/>[Deploy independente]<br/>Simplificação de linguagem<br/>médica para leigos"]
        IA_DB[(IA Database)]
        IA_SVC --> IA_DB
    end

    GW --> ACL_SVC
    GW --> AUTH
    GW --> BOL
    GW --> IA_SVC
    BOL -->|"Async / Mensageria"| ACL_SVC
    BOL -->|"Async / Mensageria"| IA_SVC
    ACL_SVC -->|"Eventos"| AUD

    style ACL_SVC fill:#e74c3c,stroke:#c0392b,color:#fff
    style IA_SVC fill:#e74c3c,stroke:#c0392b,color:#fff
    style GW fill:#f39c12,stroke:#d68910,color:#fff
```

**Ações técnicas desta fase:**

1. **Extrair ACL como microsserviço** (Strangler Fig) — primeiro módulo a ser extraído porque:
   - Tem requisitos de escala diferentes (múltiplos hospitais = múltiplas chamadas externas)
   - Beneficia-se de circuit breaker e retry independentes
   - Falha na integração MV não deve derrubar consulta de boletins já importados
2. **Introduzir API Gateway** — roteamento centralizado, rate limiting, autenticação
3. **Comunicação assíncrona** — eventos entre serviços via mensageria (ex: RabbitMQ ou similar)
4. **Novo serviço de IA** — se o produto evoluir para simplificação de linguagem médica, nasce como serviço independente desde o início (não entra no monólito)

**Por que a ACL é o primeiro candidato a extração:**

A ACL é o componente com maior variabilidade (cada hospital pode ter API diferente) e maior risco de falha (depende de sistema externo). Extraí-la primeiro:
- **Isola falhas:** se o MV de um hospital cair, boletins já importados continuam acessíveis
- **Escala independente:** 50 hospitais = 50 conexões simultâneas, sem impactar o core
- **Deploy independente:** novo adapter para hospital novo sem redeployar o sistema inteiro

---

### ⭐ Fase 4 — Microsserviços (Visão de Longo Prazo)

**Gatilhos de transição (quando sair da Fase 3):**

| Gatilho | Indicador Mensurável |
|---|---|
| Múltiplas equipes | 4+ squads independentes com ownership de domínios diferentes |
| Ecossistema completo | VittaHub como plataforma (boletins + exames + IA + telemedicina + prontuário do paciente) |
| Escala regional/nacional | 50+ hospitais com SLAs diferentes |

**Esta fase é uma visão, não um plano imediato.** Sua concretização depende do crescimento real do produto e da organização. O valor de documentá-la é demonstrar que a arquitetura atual foi pensada com esse horizonte em mente.

---

## 5.5 Conexão com o Roadmap de Produto

As fases técnicas se alinham com marcos de negócio do VittaHub:

| Marco de Negócio | Fase Arquitetural | Decisão-chave |
|---|---|---|
| **MVP com hospital piloto** | Fase 1 — Monólito Modular | Validar produto com simplicidade operacional |
| **Expansão para rede hospitalar** | Fase 2 — Monólito Amadurecido | Múltiplos adapters ACL, schemas separados |
| **Novos domínios (exames, IA)** | Fase 3 — Híbrido | Extração de serviços para domínios divergentes |
| **Plataforma de saúde digital** | Fase 4 — Microsserviços | Cada domínio como produto independente |

### Timeline Estimada

```mermaid
gantt
    title Roadmap de Evolução Arquitetural do VittaHub
    dateFormat  YYYY-MM
    axisFormat  %Y-%m

    section Fase 1 - Monólito Modular
    Formalizar Clean Architecture           :active, f1a, 2026-02, 2026-04
    Implementar interfaces/ports            :f1b, 2026-03, 2026-05
    Testes unitários em Use Cases           :f1c, 2026-03, 2026-06
    Definir observabilidade (Elasticsearch) :f1d, 2026-04, 2026-06
    Documentar ADRs                         :f1e, 2026-02, 2026-03

    section Fase 2 - Amadurecimento
    Separar schemas Oracle                  :f2a, 2026-07, 2026-09
    Múltiplos adapters ACL                  :f2b, 2026-08, 2026-11
    Cache de boletins                       :f2c, 2026-09, 2026-10
    Feature flags por hospital              :f2d, 2026-10, 2026-12

    section Fase 3 - Híbrido (sob demanda)
    Extrair ACL como microsserviço          :f3a, 2027-01, 2027-04
    API Gateway                             :f3b, 2027-02, 2027-04
    Comunicação assíncrona                  :f3c, 2027-03, 2027-05
    Novo serviço de IA (se aplicável)       :f3d, 2027-04, 2027-07
```

> **Nota:** As datas da Fase 3 em diante são estimativas condicionais — dependem da ativação dos gatilhos descritos. Se o produto não escalar para múltiplos hospitais, a Fase 1 pode ser suficiente por anos.

---

## 5.6 Gestão de Débito Técnico

Conforme o material da Fase 4 (Aula 5 — Gerenciamento de Débito Técnico), cada decisão de "não fazer agora" gera débito técnico consciente. O roadmap gerencia esse débito de forma explícita:

| Débito Técnico | Fase em que é "pago" | Risco se não for pago |
|---|---|---|
| Observabilidade indefinida | Fase 1 (urgente) | Incidentes invisíveis, compliance LGPD comprometido |
| Banco compartilhado entre módulos | Fase 2 | Acoplamento de dados impede extração de serviços |
| Deploy único para todos os módulos | Fase 3 | Ciclos de release amarrados entre equipes |
| Comunicação síncrona entre módulos | Fase 3 | Cascata de falhas quando MV fica indisponível |

> **Princípio:** Débito técnico não é necessariamente ruim — assim como débito financeiro, pode ser uma ferramenta estratégica. O problema é débito **inconsciente** (quando nem sabemos que devemos). Este roadmap garante que todo débito é documentado, priorizado e tem um plano de pagamento.

---

*Seção anterior: [04 - Diagramas C4 Atualizados](04-diagramas-c4.md)*
*Próxima etapa: [ADRs — Architecture Decision Records](adrs/)*
