# ADR-003: Estratégia de Migração Futura via Strangler Fig Pattern

## Status
**Proposta** — Fevereiro 2026 (será ativada quando gatilhos forem atingidos)

## Contexto

O VittaHub adota atualmente o modelo de Monólito Modular (ADR-001). Embora adequado ao estágio atual, o produto possui ambição de expansão para múltiplos hospitais e novos domínios. É necessário definir *como* a arquitetura evoluirá quando os gatilhos de transição forem ativados, evitando uma migração big-bang de alto risco.

## Decisão

Quando for necessário extrair módulos do monólito para serviços independentes, adotaremos o padrão **Strangler Fig** — uma estratégia de migração incremental onde novos serviços são criados ao lado do monólito existente, assumindo responsabilidades gradualmente até que o módulo original possa ser removido.

## Como Funciona

1. **Identificar** o módulo candidato a extração (baseado nos gatilhos da Seção 5 do roadmap)
2. **Criar** o novo serviço implementando a mesma interface (port) que o módulo interno
3. **Rotear** requisições gradualmente para o novo serviço (canary deployment)
4. **Validar** que o novo serviço opera corretamente em produção
5. **Remover** o módulo interno do monólito quando o serviço externo estiver estável

## Ordem de Extração Prevista

| Prioridade | Módulo | Justificativa |
|---|---|---|
| 1ª | ACL / Integração MV | Maior variabilidade (APIs por hospital), risco de falha externa, benefício de circuit breaker independente |
| 2ª | Auditoria/Logs | Volume de dados cresce desproporcionalmente, pode beneficiar-se de armazenamento especializado |
| 3ª | Autenticação | Se adotado Identity Provider externo (Keycloak, Auth0), módulo interno se torna apenas um adapter fino |
| Última | Boletim (Core) | Domínio central — só se extrai quando há múltiplas equipes e domínios concorrentes |

## Consequências

### Positivas
- Migração de baixo risco (incremental, reversível)
- Cada serviço extraído é validado em produção antes de remover o módulo original
- Equipe ganha experiência com microsserviços gradualmente
- Possibilidade de rollback a qualquer momento

### Negativas
- Durante a transição, o sistema opera em modo "híbrido" com complexidade temporária aumentada
- Requer infraestrutura de roteamento (API Gateway) a partir da primeira extração
- Necessidade de manter compatibilidade entre serviço novo e módulo antigo durante período de transição

## Pré-requisitos (já endereçados)
- ✅ Bounded Contexts bem definidos (DDD da Fase 3)
- ✅ Interfaces/Ports entre módulos (Clean Architecture — ADR-002)
- ✅ Comunicação via contratos, não referências diretas

## Gatilhos de Ativação

Conforme definido no [Roadmap de Evolução Técnica](../05-roadmap-evolucao.md), Seção 5.4 — Fase 3.

## Referências
- [Roadmap de Evolução Técnica](../05-roadmap-evolucao.md)
- Fowler, Martin. "StranglerFigApplication", martinfowler.com
- Newman, Sam. "Monolith to Microservices", O'Reilly, 2019
- Material didático: Aula 1 — Arquiteturas da Atualidade (trade-offs entre modelos)
