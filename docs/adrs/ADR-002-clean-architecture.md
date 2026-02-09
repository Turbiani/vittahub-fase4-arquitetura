# ADR-002: Adoção da Clean Architecture como Padrão Interno

## Status
**Aceita** — Fevereiro 2026

## Contexto

Com a definição do Monólito Modular (ADR-001), é necessário definir como o código se organiza *dentro* de cada módulo. O backend do VittaHub utiliza DDD com Bounded Contexts, mas não possui uma separação formal em camadas arquiteturais, o que gera risco de acoplamento indesejado entre regras de negócio e infraestrutura.

## Decisão

Adotamos a **Clean Architecture** (Robert C. Martin) como padrão de organização interna, com quatro camadas concêntricas: Entities, Use Cases, Interface Adapters e Frameworks & Drivers.

A **regra de dependência** é a diretriz central: dependências de código (imports) só podem apontar de fora para dentro — das camadas externas em direção ao domínio.

## Alternativas Consideradas

### Hexagonal Architecture (Ports & Adapters)
- Candidata viável. Conceitos de portas e adapters são compatíveis com a ACL existente.
- **Rejeitada** em favor da Clean Architecture, que já incorpora esses conceitos com nomenclatura mais clara e camadas mais bem definidas.

### Onion Architecture
- Candidata viável. Organização em camadas concêntricas com domínio no centro.
- **Rejeitada** porque a Clean Architecture é uma evolução que unifica os conceitos da Onion e da Hexagonal em um modelo mais completo.

### Clean Architecture ✅
- **Aceita.** Síntese dos padrões anteriores, com nomenclatura clara, regra de dependência explícita e alinhamento natural com o DDD já existente.

## Consequências

### Positivas
- Regras de negócio e compliance LGPD isoladas da infraestrutura
- Use Cases testáveis com mocks, sem dependência de banco ou APIs externas
- ACL existente mapeada naturalmente como Interface Adapter
- Estrutura de pastas intuitiva para onboarding de novos desenvolvedores
- Preparação para extração de módulos em microsserviços futuros

### Negativas
- Overhead inicial de refatoração para organizar o código existente
- Curva de aprendizado para desenvolvedores não familiarizados com o padrão
- Mais arquivos e interfaces (boilerplate) em comparação com abordagem menos formal

### Riscos Mitigados
- Curva de aprendizado → Documentação detalhada + exemplos em Dart + code review
- Overhead de refatoração → Migração incremental (módulo a módulo), não big-bang

## Referências
- [Clean Architecture Aplicada ao VittaHub](../03-clean-architecture-aplicada.md)
- Martin, Robert C. "Clean Architecture: A Craftsman's Guide to Software Structure and Design", 2017
- Material didático: Aula 1 — Arquiteturas da Atualidade
