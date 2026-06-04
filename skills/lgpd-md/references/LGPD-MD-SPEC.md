# A Especificação `/lgpd.md`

Uma proposta de padrão aberto para que sites declarem seu estado de conformidade com a Lei Geral de Proteção de Dados (Lei 13.709/2018) em formato legível por humanos, ferramentas automatizadas e agentes de IA.

Autor: Fabricio Telles  
Publicado: 31 de maio de 2026  
Versão: 1.0  
Licença: Apache 2.0  
Repositório: https://github.com/lgpd-app/lgpd-md

---

## Contexto

A LGPD exige que sites brasileiros informem como tratam dados pessoais. Na prática, essa informação está dispersa em políticas de privacidade extensas, banners de cookies e páginas jurídicas — todas escritas para humanos, nenhuma para máquinas.

Enquanto isso, o ecossistema web evoluiu para incluir arquivos padronizados que comunicam informações estruturadas:

- `robots.txt` diz a crawlers o que podem acessar
- `security.txt` (RFC 9116) diz a pesquisadores como reportar vulnerabilidades
- `llms.txt` diz a modelos de linguagem como entender um site
- `ads.txt` diz a ad exchanges quem pode vender inventário
- `auth.md` diz a agentes de IA como se autenticar
- `DESIGN.md` diz a coding agents como seguir um design system

**Não existe nenhum padrão que diga a ferramentas, reguladores e titulares de dados como um site trata dados pessoais sob a LGPD.**

O `/lgpd.md` preenche essa lacuna.

---

## Proposta

Publicar um arquivo `/lgpd.md` na raiz de sites que processam dados pessoais de pessoas no Brasil. O arquivo combina:

1. **Frontmatter YAML** — dados estruturados, validáveis por JSON Schema, parseáveis por ferramentas
2. **Corpo Markdown** — contexto em prosa, legível por humanos e agentes de IA

O arquivo não substitui a política de privacidade. Ele a complementa com uma declaração estruturada e verificável.

---

## Formato

O arquivo `/lgpd.md` usa Markdown com frontmatter YAML, delimitado por `---`. Codificação UTF-8.

```
---
[frontmatter YAML — dados estruturados]
---

[corpo Markdown — contexto humano]
```

O frontmatter é validável contra o JSON Schema publicado em `https://lgpd.md/schema.json`.

---

## Localização

O arquivo DEVE estar acessível em:

```
https://{domínio}/lgpd.md
```

Opcionalmente, o site PODE indicar a existência do arquivo via:

- Tag HTML: `<link rel="lgpd" href="/lgpd.md" type="text/markdown">`
- Header HTTP: `Link: </lgpd.md>; rel="lgpd"; type="text/markdown"`

O arquivo DEVE ser servido sobre HTTPS. O Content-Type DEVE ser `text/markdown; charset=utf-8`.

---

## Campos do Frontmatter

### Terminologia

- **DEVE** (MUST): campo obrigatório
- **DEVE SE** (MUST IF): obrigatório quando a condição se aplica
- **RECOMENDADO** (SHOULD): fortemente sugerido
- **PODE** (MAY): opcional

---

### Meta

| Campo | Tipo | Obrigatoriedade | Descrição |
|-------|------|----------------|-----------|
| `spec` | string | DEVE | Versão da especificação. Valor atual: `"1.0"` |
| `atualizado` | date | DEVE | Data da última atualização do arquivo (YYYY-MM-DD) |
| `expira` | date | DEVE | Data até a qual o arquivo é considerado válido. Após esta data, ferramentas DEVEM alertar que o arquivo precisa de revisão |
| `status` | enum | DEVE | Estado declarado de conformidade: `conforme`, `parcial`, ou `em-adequação` |

**Sobre `expira`:** Inspirado no campo `Expires` do security.txt (RFC 9116). Força revisão periódica. O intervalo máximo recomendado é 365 dias.

**Sobre `status`:**
- `conforme` — o controlador declara atender todos os requisitos aplicáveis da LGPD
- `parcial` — há requisitos conhecidos ainda não atendidos
- `em-adequação` — o controlador está em processo ativo de adequação

---

### Controlador

Identificação do agente de tratamento responsável (Art. 5º, VI; Art. 41, §1º).

| Campo | Tipo | Obrigatoriedade | Descrição |
|-------|------|----------------|-----------|
| `controlador.nome` | string | DEVE | Razão social ou nome completo do controlador |
| `controlador.cnpj` | string | DEVE SE pessoa jurídica | CNPJ no formato `XX.XXX.XXX/XXXX-XX` |
| `controlador.contato` | string | DEVE | Email ou URL para questões de privacidade |

---

### Encarregado (DPO)

Identificação do Encarregado pelo Tratamento de Dados Pessoais (Art. 41).

| Campo | Tipo | Obrigatoriedade | Descrição |
|-------|------|----------------|-----------|
| `encarregado.nome` | string | DEVE | Nome do Encarregado |
| `encarregado.contato` | string | DEVE | Email ou URL de contato público |

**Nota:** O Art. 41, §1º da LGPD determina que a identidade e informações de contato do encarregado devem ser divulgadas publicamente, preferencialmente no site do controlador.

---

### Direitos do Titular

Canal para exercício dos direitos previstos no Art. 18 da LGPD.

| Campo | Tipo | Obrigatoriedade | Descrição |
|-------|------|----------------|-----------|
| `direitos.canal` | string | DEVE | URL ou email para exercício de direitos |
| `direitos.prazo` | integer | DEVE | Prazo máximo de resposta em dias. O Art. 19, II estabelece o limite de 15 dias |

**Nota:** O canal DEVE ser acessível sem necessidade de login. O titular pode não ter conta no serviço.

---

### Política de Privacidade

Referência à política de privacidade completa (Art. 9º).

| Campo | Tipo | Obrigatoriedade | Descrição |
|-------|------|----------------|-----------|
| `politica.url` | URI | DEVE | URL da política de privacidade completa |
| `politica.idioma` | string | RECOMENDADO | Código de idioma BCP 47 (ex: `pt-br`) |
| `politica.atualizada` | date | RECOMENDADO | Data da última atualização da política |

**Relação bidirecional:** A política de privacidade RECOMENDA-SE que contenha um link para o `/lgpd.md`, e o `/lgpd.md` DEVE linkar para a política.

---

### Cookies

Declaração sobre o modelo de consentimento de cookies. DEVE estar presente se o site utiliza cookies ou tecnologias de rastreamento não essenciais.

| Campo | Tipo | Obrigatoriedade | Descrição |
|-------|------|----------------|-----------|
| `cookies.banner` | boolean | DEVE | Exibe banner de consentimento |
| `cookies.opt_in_previo` | boolean | DEVE | Nenhum cookie não essencial é setado antes do aceite explícito (Art. 8, §4º) |
| `cookies.rejeitar_todos` | boolean | DEVE | Opção de rejeitar todos com mesma proeminência visual (Art. 8 — consentimento livre) |
| `cookies.gerenciar_preferencias` | boolean | RECOMENDADO | Modal com controles granulares por categoria |

---

### Bases Legais

Mapeamento entre finalidades de tratamento, bases legais e categorias de dados (Art. 7 + Art. 9º). DEVE conter ao menos uma entrada.

| Campo | Tipo | Obrigatoriedade | Descrição |
|-------|------|----------------|-----------|
| `bases_legais[].finalidade` | string | DEVE | Descrição da finalidade do tratamento |
| `bases_legais[].base` | enum | DEVE | Base legal conforme Art. 7 |
| `bases_legais[].artigo` | string | RECOMENDADO | Referência ao inciso (ex: `Art. 7, I`) |
| `bases_legais[].dados` | array | DEVE | Categorias de dados pessoais tratados |

**Valores permitidos para `base`** (Art. 7, incisos I a X):

| Valor | Inciso | Descrição |
|-------|--------|-----------|
| `consentimento` | I | Consentimento do titular |
| `obrigação legal` | II | Cumprimento de obrigação legal ou regulatória |
| `administração pública` | III | Execução de políticas públicas |
| `pesquisa` | IV | Realização de estudos por órgão de pesquisa |
| `execução de contrato` | V | Execução de contrato ou procedimentos preliminares |
| `exercício regular de direitos` | VI | Exercício regular de direitos em processo |
| `proteção da vida` | VII | Proteção da vida ou incolumidade física |
| `tutela da saúde` | VIII | Tutela da saúde por profissionais da área |
| `legítimo interesse` | IX | Legítimo interesse do controlador ou terceiro |
| `proteção do crédito` | X | Proteção do crédito |

---

### Transferência Internacional

Declaração sobre transferência de dados para fora do Brasil (Art. 33). DEVE estar presente se há transferência internacional.

| Campo | Tipo | Obrigatoriedade | Descrição |
|-------|------|----------------|-----------|
| `transferencia_internacional.transfere` | boolean | DEVE | Indica se há transferência |
| `transferencia_internacional.destinos` | array | DEVE SE `transfere: true` | Países ou regiões de destino |
| `transferencia_internacional.mecanismo` | string | DEVE SE `transfere: true` | Mecanismo legal autorizador |

**Mecanismos comuns (Art. 33):**
- Decisão de adequação (Resolução 32/2026 — Brasil-UE)
- Cláusulas contratuais padrão / SCCs (Resolução 19/2024)
- Normas corporativas globais
- Consentimento específico do titular
- Cooperação jurídica internacional

---

### Scripts de Terceiros

Declaração de serviços de terceiros que processam dados pessoais dos visitantes. RECOMENDADO.

| Campo | Tipo | Obrigatoriedade | Descrição |
|-------|------|----------------|-----------|
| `scripts_terceiros[].nome` | string | DEVE (se seção presente) | Nome do serviço |
| `scripts_terceiros[].finalidade` | string | DEVE (se seção presente) | Finalidade (analytics, marketing, cdn/segurança, etc.) |
| `scripts_terceiros[].consentimento_necessario` | boolean | DEVE (se seção presente) | Se requer consentimento prévio |

---

## Corpo Markdown

Abaixo do frontmatter, o arquivo DEVE conter um corpo Markdown com:

| Seção | Obrigatoriedade | Formato | Descrição |
|-------|----------------|---------|-----------|
| Título | DEVE | `# lgpd.md` | H1 com o nome do arquivo |
| Resumo | RECOMENDADO | `> blockquote` | Uma frase identificando o controlador |
| Sobre | RECOMENDADO | `## Sobre este arquivo` | Explica o propósito do arquivo |
| Notas | PODE | `## Notas` | Contexto livre do controlador |
| Histórico | RECOMENDADO | `## Histórico de Alterações` | Tabela com data e descrição de mudanças |

O corpo Markdown existe para:
1. Dar contexto que não cabe em campos estruturados
2. Ser legível por LLMs que processam o arquivo como texto
3. Permitir que o controlador comunique intenções e compromissos

---

## Descoberta

Ferramentas e crawlers DEVEM verificar a existência do arquivo em:

```
GET https://{domínio}/lgpd.md
```

Se o servidor retornar `200 OK` com Content-Type `text/markdown`, o arquivo existe e é válido para parsing.

Mecanismos adicionais de descoberta (opcionais):

1. **HTML `<link>`**: `<link rel="lgpd" href="/lgpd.md" type="text/markdown">`
2. **HTTP Header**: `Link: </lgpd.md>; rel="lgpd"; type="text/markdown"`
3. **`robots.txt`**: incluir `# lgpd.md: /lgpd.md` como comentário informativo

---

## Validação

### JSON Schema

O frontmatter YAML é validável contra o schema publicado em:

- https://github.com/lgpd-app/lgpd-md/blob/main/schema.json

### Níveis de Validação

| Nível | O que verifica | Exemplo de erro |
|-------|---------------|-----------------|
| **Sintaxe** | YAML válido, tipos corretos | `prazo: "quinze"` (deveria ser integer) |
| **Completude** | Campos obrigatórios presentes | `encarregado` ausente |
| **Consistência** | Valores coerentes | `expira` no passado, URL inacessível |
| **Conformidade** | Campos condicionais | `cookies` ausente mas site usa GA4 |


---

## Relação com Outros Padrões

| Padrão | Relação com lgpd.md |
|--------|-------------------|
| `robots.txt` | Coexiste. lgpd.md não controla acesso de crawlers |
| `security.txt` | Complementar. security.txt trata vulnerabilidades; lgpd.md trata privacidade |
| `llms.txt` | Complementar. LLMs podem referenciar lgpd.md para entender práticas de dados |
| `privacy.txt` (IETF draft) | lgpd.md é mais específico (LGPD), mais rico (YAML + Markdown), e em português |
| Política de privacidade | lgpd.md complementa com dados estruturados; a política é o documento legal completo |

---

## Segurança

- O arquivo NÃO DEVE conter dados pessoais de titulares
- O arquivo NÃO DEVE conter credenciais, tokens ou chaves
- O arquivo DEVE ser servido sobre HTTPS
- O arquivo NÃO requer assinatura digital (diferente do security.txt)
- O campo `expira` mitiga o risco de informações desatualizadas

---

## Exemplos

### Blog pessoal (mínimo)

```yaml
---
spec: "1.0"
atualizado: 2026-05-30
expira: 2027-05-30
status: conforme

controlador:
  nome: "Maria Santos"
  contato: "maria@meublog.com.br"

encarregado:
  nome: "Maria Santos"
  contato: "maria@meublog.com.br"

direitos:
  canal: "maria@meublog.com.br"
  prazo: 15

politica:
  url: "https://meublog.com.br/privacidade"

bases_legais:
  - finalidade: "Comentários no blog"
    base: "consentimento"
    dados: ["nome", "email"]
---

# lgpd.md

> Declaração de conformidade LGPD do blog pessoal de Maria Santos.
```

### E-commerce

```yaml
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
```

### SaaS B2B

```yaml
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
```

---

## Adoção

### Para desenvolvedores

1. Crie um arquivo `lgpd.md` na raiz pública do seu site
2. Preencha o frontmatter YAML com os dados do seu projeto
3. Valide comparando com schema.json
4. Faça deploy junto com o site
5. Adicione `<link rel="lgpd" href="/lgpd.md">` no `<head>`

### Para ferramentas de auditoria

1. Verifique `GET https://{domínio}/lgpd.md`
2. Parse o frontmatter YAML
3. Valide contra o JSON Schema
4. Compare declarações com o comportamento real do site
5. Reporte discrepâncias

### Para reguladores e DPOs

O arquivo oferece uma visão rápida e padronizada do estado de conformidade de qualquer site, sem necessidade de ler políticas de privacidade extensas. Pode ser usado como ponto de partida para auditorias.

---

## Governança

Esta especificação é mantida como projeto open source em:

- **Spec**: https://github.com/lgpd-app/lgpd-md e https://lgpd.md 
- **Schema**: https://lgpd.md/schema.json
- **Discussões**: GitHub Issues e Discussions

Contribuições são bem-vindas via Pull Request. Mudanças na spec seguem versionamento semântico.

---

## Referências Legais

- [Lei 13.709/2018 (LGPD)](http://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/L13709compilado.htm)
- [Resolução CD/ANPD nº 19/2024](https://www.gov.br/anpd/pt-br) — Cláusulas contratuais padrão para transferência internacional
- [Resolução CD/ANPD nº 32/2026](https://www.gov.br/anpd/pt-br) — Decisão de adequação mútua Brasil-UE
- [Resolução CD/ANPD nº 33/2026](https://www.gov.br/anpd/pt-br) — ANPD como agência reguladora independente

---

## Inspirações

| Padrão | O que inspirou |
|--------|---------------|
| security.txt (RFC 9116) | Campo `expira`, localização na raiz, simplicidade |
| llms.txt | Formato Markdown, legibilidade por LLMs, seções com H2 |
| SKILL.md | YAML frontmatter + Markdown body, JSON Schema para validação |
| DESIGN.md (Google) | Dados estruturados + contexto qualitativo no mesmo arquivo |
| auth.md (WorkOS) | Spec voltada para agentes de IA parsearem |
| privacy.txt (IETF draft) | Conceito de privacidade machine-readable (expandido e localizado) |

## Disclaimer

Esta proposta de padrão e suas ferramentas propostas não substituem a assessoria de um advogado especialista. É um mecanismo para facilitar a descoberta de informações de privacidade em sites brasileiros. O usuário é o único responsável pelas informações fornecidas por ele mesmo em seu site na web.