---
spec: "1.0"
atualizado: 2026-05-30
expira: 2026-11-30
status: conforme

controlador:
  nome: "Loja Virtual Ltda"
  cnpj: "12.345.678/0001-90"
  contato: "privacidade@lojavirtual.com.br"

encarregado:
  nome: "Carlos Oliveira"
  contato: "dpo@lojavirtual.com.br"

direitos:
  canal: "https://lojavirtual.com.br/meus-dados"
  prazo: 15

politica:
  url: "https://lojavirtual.com.br/politica-de-privacidade"
  idioma: "pt-br"
  atualizada: 2026-04-15

cookies:
  banner: true
  opt_in_previo: true
  rejeitar_todos: true
  gerenciar_preferencias: true

bases_legais:
  - finalidade: "Processamento de pedidos"
    base: "execução de contrato"
    artigo: "Art. 7, V"
    dados: ["nome", "cpf", "endereço", "telefone", "email"]
  - finalidade: "Emissão de nota fiscal"
    base: "obrigação legal"
    artigo: "Art. 7, II"
    dados: ["nome", "cpf", "endereço"]
  - finalidade: "Marketing por email"
    base: "consentimento"
    artigo: "Art. 7, I"
    dados: ["email", "nome"]
  - finalidade: "Analytics"
    base: "legítimo interesse"
    artigo: "Art. 7, IX"
    dados: ["ip", "navegador", "páginas visitadas"]
  - finalidade: "Prevenção a fraudes"
    base: "legítimo interesse"
    artigo: "Art. 7, IX"
    dados: ["ip", "device fingerprint", "histórico de compras"]

transferencia_internacional:
  transfere: true
  destinos: ["EUA"]
  mecanismo: "SCCs (Resolução 19/2024)"

scripts_terceiros:
  - nome: "Google Analytics"
    finalidade: "analytics"
    consentimento_necessario: true
  - nome: "Meta Pixel"
    finalidade: "marketing"
    consentimento_necessario: true
  - nome: "Cloudflare"
    finalidade: "cdn/segurança"
    consentimento_necessario: false
  - nome: "PagSeguro"
    finalidade: "pagamento"
    consentimento_necessario: false
---

# lgpd.md

> Declaração de conformidade LGPD da Loja Virtual Ltda.

## Notas

- Dados de pagamento são processados diretamente pelo PagSeguro; não armazenamos dados de cartão.
- Retenção de dados fiscais: 5 anos conforme legislação tributária.
- Dados de marketing são eliminados em até 30 dias após revogação do consentimento.

## Histórico de Alterações

| Data | Mudança |
|------|---------|
| 2026-05-30 | Adicionado Meta Pixel com consentimento |
| 2026-04-15 | Atualizada política de privacidade |
| 2026-01-10 | Versão inicial |
