# 🏥 VittaHub — Evolução da Arquitetura de Software

## Tech Challenge Fase 4 | Entregável 1

**Projeto:** VittaHub — Sistema de Comunicação Digital de Boletins Médicos  
**Curso:** MBA Tech Management — FIAP/PosTech  
**Grupo 18:** Gleiciele Correia • Henrique Silva Corrêa • Igor Cavalcante • Leonardo Turbiani • Regina Lima Garrido  
**Responsável por este entregável:** Leonardo Turbiani (RM366050)  
**Data:** Fevereiro 2026

---

## 📋 Sobre este Entregável

Este repositório documenta a **evolução da arquitetura de software** do VittaHub, e corresponde ao Entregável 1 da Fase 4 do Tech Challenge. O trabalho nosso trabalho contém:

- ✅ Avaliação crítica da arquitetura implementada na Fase 3
- ✅ Definição e justificativa do modelo arquitetural (Monólito Modular)
- ✅ Aplicação da Clean Architecture como padrão de organização interna
- ✅ Diagramas C4 atualizados nos 3 primeiros níveis
- ✅ Roadmap de evolução técnica com gatilhos concretos de transição
- ✅ ADRs (Architecture Decision Records) para cada decisão significativa

---

## 🗂️ Estrutura do Repositório

```
├── README.md                              ← Você está aqui
├── docs/
│   ├── 01-analise-arquitetura-atual.md    # Diagnóstico crítico da Fase 3
│   ├── 02-decisao-modelo-arquitetural.md  # Monólito Modular: por quê
│   ├── 03-clean-architecture-aplicada.md  # Clean Architecture no VittaHub
│   ├── 04-diagramas-c4.md                # Diagramas C4 (Context, Container, Component)
│   ├── 05-roadmap-evolucao.md            # Roadmap técnico com gatilhos
│   └── adrs/
│       ├── ADR-001-modelo-monolito-modular.md
│       ├── ADR-002-clean-architecture.md
│       └── ADR-003-estrategia-migracao-futura.md
```

---

## 🚀 Leitura Recomendada

Os documentos foram escritos em ordem lógica. Para melhor compreensão, recomenda-se a leitura sequencial:

### 1. [Análise Crítica da Arquitetura Atual](docs/01-analise-arquitetura-atual.md)
Avaliação honesta da arquitetura da Fase 3. Identificando pontos fortes (DDD, ACL, Privacy by Design) e gaps (classificação imprecisa como "microsserviços", ausência de camadas formais, observabilidade indefinida) que tivémos na fase anterior. No final, propondo a reclassificação para **Monólito Modular**.

### 2. [Decisão do Modelo Arquitetural](docs/02-decisao-modelo-arquitetural.md)
Análise comparativa entre Monólito Tradicional, Microsserviços e Monólito Modular. Justifica a escolha do Monólito Modular com base em critérios concretos e realistas: tamanho da equipe (5 devs), estágio do produto (MVP), escopo funcional e custo operacional. Define gatilhos para migração futura.

### 3. [Clean Architecture Aplicada](docs/03-clean-architecture-aplicada.md)
Aplicação prática da Clean Architecture ao projeto do VittaHub. Mapeia as 4 camadas (Entities, Use Cases, Interface Adapters, Frameworks & Drivers) para componentes reais do sistema. Incluímos exemplos de código em Dart(utilizado no projeto), proposta de estruturação de pastas e explicação detalhada da regra de dependência.

### 4. [Diagramas C4 Atualizados](docs/04-diagramas-c4.md)
Diagramas nos 3 primeiros níveis do modelo C4, atualizados para refletir a arquitetura evoluída. **Destaque:** Diagrama de Componentes (Nível 3), ausente na Fase 3, agora documenta a organização interna do backend com Clean Architecture.

### 5. [Roadmap de Evolução Técnica](docs/05-roadmap-evolucao.md)
Plano de evolução em 4 fases (Monólito Modular → Amadurecido → Híbrido → Microsserviços), cada uma ativada por gatilhos mensuráveis. Inclui timeline estimada, conexão com marcos de negócio e gestão de débito técnico.

### 📝 ADRs (Architecture Decision Records)
- [ADR-001: Monólito Modular](docs/adrs/ADR-001-modelo-monolito-modular.md) — Por que este modelo e não microsserviços
- [ADR-002: Clean Architecture](docs/adrs/ADR-002-clean-architecture.md) — Por que este padrão interno
- [ADR-003: Strangler Fig](docs/adrs/ADR-003-estrategia-migracao-futura.md) — Como migrar no futuro

---

## 🏗️ Resumo das Decisões Arquiteturais

| Decisão | Escolha | Justificativa |
|---|---|---|
| Modelo arquitetural | **Monólito Modular** | Proporcional ao estágio do produto (MVP, 5 devs, 1 hospital) |
| Padrão interno | **Clean Architecture** | Regra de dependência protege domínio e compliance LGPD |
| Estratégia de evolução | **Strangler Fig Pattern** | Migração incremental e reversível quando gatilhos forem ativados |
| Primeiro candidato a extração | **ACL (Integração MV)** | Maior variabilidade, risco externo e benefício de isolamento |
| Observabilidade | **Elasticsearch** | Endereça gap da Fase 3 para logs de auditoria |

---

## 📊 Diagramas Principais

- **Arquitetura AS-IS** → [Seção 1.2](docs/01-analise-arquitetura-atual.md#12-inventário-da-arquitetura-real-as-is)
- **Comparativo de Modelos** → [Seção 2.2](docs/02-decisao-modelo-arquitetural.md#22-modelos-considerados)
- **Camadas Clean Architecture** → [Seção 3.2](docs/03-clean-architecture-aplicada.md#32-as-quatro-camadas-e-a-regra-de-dependência)
- **Diagramas C4 (3 níveis)** → [Seção 4](docs/04-diagramas-c4.md)
- **Roadmap visual (Gantt)** → [Seção 5.5](docs/05-roadmap-evolucao.md#55-conexão-com-o-roadmap-de-produto)
- **Strangler Fig** → [Seção 2.5](docs/02-decisao-modelo-arquitetural.md#25-critérios-de-extração-futura-gatilhos-para-microsserviços)

---

## 🔗 Contexto do Projeto

- **Fase 3 (concluída):** Arquitetura Empresarial com TOGAF/ADM, Governança Digital, Segurança da Informação, Gestão de Desenvolvimento
- **Vídeo da Fase 3:** [YouTube](https://youtu.be/LfIiEZa_lwQ)
- **Fase 4 (atual):** Evolução de Arquitetura, Governança de TI, Dados & IA, DevSecOps, Ecossistemas Digitais


---

## 📚 Referências

- Martin, Robert C. *Clean Architecture: A Craftsman's Guide to Software Structure and Design*. Prentice Hall, 2017.
- Fowler, Martin. *StranglerFigApplication*. martinfowler.com, 2004.
- Newman, Sam. *Monolith to Microservices*. O'Reilly, 2019.
- Brown, Simon. *The C4 Model for Visualising Software Architecture*. c4model.com.
- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Material didático PosTech Fase 4: Aulas 1-5 de Arquitetura de Software.
