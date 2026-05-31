---
spec: "1.0"
atualizado: 2026-05-30
expira: 2026-11-30
status: parcial

controlador:
  nome: "SaaS Corp Tecnologia S.A."
  cnpj: "98.765.432/0001-10"
  contato: "privacy@saascorp.com.br"

encarregado:
  nome: "Ana Ferreira"
  contato: "dpo@saascorp.com.br"

direitos:
  canal: "https://saascorp.com.br/privacidade/direitos"
  prazo: 15

politica:
  url: "https://saascorp.com.br/privacidade"
  idioma: "pt-br"
  atualizada: 2026-03-01

cookies:
  banner: true
  opt_in_previo: true
  rejeitar_todos: true
  gerenciar_preferencias: true

bases_legais:
  - finalidade: "Prestação do serviço"
    base: "execução de contrato"
    artigo: "Art. 7, V"
    dados: ["nome", "email corporativo", "cargo"]
  - finalidade: "Cobrança e faturamento"
    base: "execução de contrato"
    artigo: "Art. 7, V"
    dados: ["razão social", "cnpj", "endereço"]
  - finalidade: "Suporte ao cliente"
    base: "execução de contrato"
    artigo: "Art. 7, V"
    dados: ["nome", "email", "histórico de tickets"]
  - finalidade: "Melhoria do produto"
    base: "legítimo interesse"
    artigo: "Art. 7, IX"
    dados: ["dados de uso anonimizados"]
  - finalidade: "Comunicações de produto"
    base: "legítimo interesse"
    artigo: "Art. 7, IX"
    dados: ["email corporativo"]

transferencia_internacional:
  transfere: true
  destinos: ["EUA", "UE"]
  mecanismo: "SCCs (Resolução 19/2024) + Adequação Brasil-UE (Resolução 32/2026)"

scripts_terceiros:
  - nome: "Plausible Analytics"
    finalidade: "analytics"
    consentimento_necessario: false
  - nome: "Intercom"
    finalidade: "suporte/chat"
    consentimento_necessario: true
  - nome: "Stripe"
    finalidade: "pagamento"
    consentimento_necessario: false
---

# lgpd.md

> Declaração de conformidade LGPD da SaaS Corp Tecnologia S.A.

## Notas

- Status "parcial": canal de portabilidade de dados em implementação (previsão: julho 2026).
- Plausible Analytics não usa cookies e não requer consentimento.
- Dados de clientes enterprise são processados conforme DPA específico.

## Histórico de Alterações

| Data | Mudança |
|------|---------|
| 2026-05-30 | Status alterado para "parcial" (portabilidade pendente) |
| 2026-03-01 | Adicionada adequação Brasil-UE |
| 2025-12-01 | Versão inicial |
