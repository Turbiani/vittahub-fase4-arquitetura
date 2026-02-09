# ADR-001: Adoção do Modelo Monólito Modular

## Status
**Aceita** — Fevereiro 2026

## Contexto

O VittaHub é um MVP desenvolvido por uma equipe de 5 pessoas, integrado a um hospital parceiro, com foco exclusivo em comunicação de boletins médicos. Na Fase 3, a arquitetura foi descrita como "baseada em microsserviços", porém a implementação real consiste em um backend único em Dart, com banco de dados Oracle compartilhado e ACL embutida no mesmo processo.

A Fase 4 exige uma avaliação crítica da arquitetura e a definição do modelo mais adequado ao estágio do produto.

## Decisão

Adotamos o modelo de **Monólito Modular** — uma aplicação única organizada internamente em módulos com fronteiras claras baseadas nos Bounded Contexts do DDD.

## Alternativas Consideradas

### Microsserviços
- **Rejeitado.** A equipe (5 devs) não teria capacidade de gerenciar a complexidade operacional (service discovery, distributed tracing, circuit breakers, múltiplos deploys). O domínio atual (boletins médicos) não justifica a fragmentação.

### Monólito Tradicional (sem separação interna)
- **Rejeitado.** Não preserva o caminho de evolução. Um monólito sem fronteiras internas se torna progressivamente mais difícil de decompor, gerando acoplamento e dificultando testes.

### Monólito Modular ✅
- **Aceito.** Combina simplicidade operacional com organização interna que preserva o caminho de migração futura.

## Consequências

### Positivas
- Simplicidade de deploy e operação (um artefato, um banco)
- Organização interna por módulos com interfaces explícitas
- Caminho de extração para microsserviços preservado via Strangler Fig
- Alinhamento com a maturidade da equipe e o estágio do produto

### Negativas
- Todos os módulos compartilham o mesmo ciclo de deploy
- Escala é vertical (toda a aplicação), não granular por módulo
- Risco de violação de fronteiras se a disciplina de code review relaxar

### Riscos Mitigados
- Violação de fronteiras → Análise estática + revisão de código obrigatória
- Acoplamento futuro → Comunicação entre módulos exclusivamente via interfaces

## Gatilhos de Revisão

Esta decisão deve ser revisada quando:
- Equipe ultrapassar 12 desenvolvedores
- Mais de 3 hospitais com APIs diferentes estiverem integrados
- Um módulo específico apresentar necessidade de escala independente

## Referências
- [Análise da Arquitetura Atual](../01-analise-arquitetura-atual.md)
- [Decisão do Modelo Arquitetural](../02-decisao-modelo-arquitetural.md)
- Material didático: Aula 4 — Propostas Arquiteturais com RFCs e ADRs
