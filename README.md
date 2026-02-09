# Tech Challenge Fase 4 - Parte 1: Evolução da Arquitetura VittaHub

> **Produto:** Sistema de Boletins Médicos para Familiares de Pacientes Internados  
> **Equipe:** Grupo 18 - POSTECH FIAP Tech Management  
> **Data:** Fevereiro 2026  
> **Responsável Fase 4 Parte 1:** Leonardo Turbiani (RM366050)

---

## 📋 Índice

### 📊 Análise e Evolução
- [Análise da Arquitetura Atual (AS-IS)](./docs/architecture/fase4-evolution/01-analise-arquitetura-atual.md)
- [Proposta de Evolução Arquitetural (TO-BE)](./docs/architecture/fase4-evolution/02-proposta-evolucao.md)
- [Roadmap Arquitetural](./docs/architecture/fase4-evolution/03-roadmap-arquitetural.md)

### 📐 Diagramas C4
- [Diagrama Container AS-IS (Fase 3)](./docs/architecture/diagramas/diagrama-container-as-is.md)
- [Diagrama Container TO-BE (Evolução Fase 4)](./docs/architecture/diagramas/diagrama-container-to-be.md)
- [Diagrama Component - Backend Clean Architecture](./docs/architecture/diagramas/diagrama-component-backend.md)
- [Diagrama de Evolução por Fases](./docs/architecture/diagramas/diagrama-evolucao-fases.md)

### 📝 Architecture Decision Records (ADRs)
- [ADR-001: Arquitetura Híbrida Estratégica](./docs/architecture/adr/adr-001-arquitetura-hibrida.md)
- [ADR-002: Clean Architecture no Backend Dart](./docs/architecture/adr/adr-002-clean-architecture.md)
- [ADR-003: Estratégia de Cache e Performance](./docs/architecture/adr/adr-003-estrategia-cache.md)
- [ADR-004: Extração de AuthFlowManager](./docs/architecture/adr/adr-004-auth-microsservico.md)
- [ADR-005: Disaster Recovery e Alta Disponibilidade](./docs/architecture/adr/adr-005-drp-disponibilidade.md)

---

## 🎯 Objetivo da Fase 4 Parte 1

Avaliar criticamente a arquitetura atual do VittaHub (documentada na Fase 3) e propor sua evolução estratégica, definindo:

1. **Modelo arquitetural adequado:** Transição de Modular Monolith para Arquitetura Híbrida
2. **Padrão interno:** Clean Architecture para escalabilidade e manutenibilidade
3. **Roadmap técnico:** Evolução faseada alinhada ao crescimento do produto
4. **Decisões documentadas:** ADRs completos com justificativas técnicas

---

## 🔑 Principais Decisões

### ✅ Decisão 1: Arquitetura Híbrida Estratégica
**Status Atual (Fase 3):** Descrito como "microsserviços" mas é um Modular Monolith  
**Decisão:** Manter Modular Monolith no MVP e evoluir gradualmente para Híbrido  
**Justificativa:** Time pequeno (5 pessoas), domínio coeso, evitar complexidade prematura  
📄 [Ver ADR-001](./docs/architecture/adr/adr-001-arquitetura-hibrida.md)

### ✅ Decisão 2: Clean Architecture Internamente
**Problema:** Padrão arquitetural interno não estava explícito na Fase 3  
**Decisão:** Aplicar Clean Architecture (Uncle Bob) em todos os Bounded Contexts  
**Benefício:** Testabilidade, preparação para extração de microsserviços futuros  
📄 [Ver ADR-002](./docs/architecture/adr/adr-002-clean-architecture.md)

### ✅ Decisão 3: Extração Gradual de Serviços
**Quando extrair:**
- **Fase 2 (6-12 meses):** AuthFlowManager + ACL MV
- **Fase 3 (12-18 meses):** Notification Service + CQRS

**Por quê:**
- Auth será reutilizado pelo futuro "VittaHub Pacientes"
- ACL isola acoplamento com sistema legado MV
- Notificações são naturalmente assíncronas

📄 [Ver ADR-004](./docs/architecture/adr/adr-004-auth-microsservico.md)

---

## 📊 Evolução Arquitetural Resumida

