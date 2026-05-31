---
name: lgpd-md
description: "Gera, valida e compara arquivos lgpd.md — o padrão aberto de declaração de conformidade LGPD (Lei 13.709/2018). Valida arquivos locais ou online contra o JSON Schema oficial. Gera novos arquivos a partir de políticas de privacidade e documentos fornecidos via entrevista estruturada. Produz relatórios HTML comparáveis entre execuções. Use quando o usuário mencionar 'lgpd.md', 'gerar lgpd.md', 'validar lgpd.md', 'declaração de conformidade', 'padrão lgpd.md', 'arquivo de conformidade LGPD', ou quiser criar/auditar um arquivo lgpd.md para seu site."
---

# lgpd-md

Gerador, validador e auditor de arquivos `lgpd.md` — o padrão aberto de declaração de conformidade LGPD para websites.

---

## Modos de Operação

### 1. Validar (`validar`)

Valida um arquivo `lgpd.md` existente contra o JSON Schema oficial e regras de consistência.

### 2. Gerar (`gerar`)

Gera um arquivo `lgpd.md` completo a partir de documentos fornecidos, via entrevista estruturada.

### 3. Relatório (`relatorio`)

Produz relatório HTML padronizado. Se um relatório anterior existir, compara item a item.

---

## Modo 1: Validar

<what-to-do>

Validar um arquivo `lgpd.md` contra o schema e regras de conformidade.

### Entrada aceita

- Caminho local: `./lgpd.md`, `/path/to/lgpd.md`
- URL: `https://exemplo.com.br/lgpd.md`

### Procedimento

1. **Obter o arquivo** — ler do disco ou fazer GET na URL
2. **Extrair frontmatter YAML** — separar do corpo Markdown
3. **Validar sintaxe** — YAML válido, parseable
4. **Validar contra schema** — usar `references/schema.json` como referência:
   - Campos obrigatórios presentes (`spec`, `atualizado`, `expira`, `status`, `controlador`, `encarregado`, `direitos`, `politica`, `bases_legais`)
   - Tipos corretos (string, integer, boolean, array, date)
   - Enums válidos (`status`: conforme/parcial/em-adequação; `base`: 10 valores do Art. 7)
   - Formato de datas ISO 8601 (YYYY-MM-DD)
   - CNPJ no formato `XX.XXX.XXX/XXXX-XX`
   - `prazo` entre 1 e 15
5. **Validar consistência**:
   - `expira` não está no passado
   - `atualizado` não está no futuro
   - Se `cookies` ausente mas `scripts_terceiros` contém item com `consentimento_necessario: true` → alertar
   - Se `transferencia_internacional.transfere: true` → `destinos` e `mecanismo` devem existir
   - Se `status: conforme` mas há campos condicionais ausentes → sugerir `parcial`
6. **Validar corpo Markdown**:
   - H1 presente (`# lgpd.md`)
   - Blockquote de resumo recomendado
7. **Emitir resultado** com score e recomendações

### Formato de saída

```
═══════════════════════════════════════════
  VALIDAÇÃO lgpd.md — {domínio ou arquivo}
═══════════════════════════════════════════

✅ Sintaxe: YAML válido
✅ Schema: todos os campos obrigatórios presentes
✅ Tipos: formatos corretos
⚠️ Consistência: arquivo expira em 30 dias
❌ Conformidade: cookies ausente mas GA4 declarado em scripts_terceiros

Score: 85/100
Status declarado: conforme
Status sugerido: parcial (cookies não declarado)

Recomendações:
1. Adicionar seção `cookies` com declaração de banner e opt-in
2. Renovar antes de {data_expira}
```

</what-to-do>

---

## Modo 2: Gerar

<what-to-do>

Gerar um arquivo `lgpd.md` completo via entrevista estruturada, usando documentos fornecidos como base.

### Entradas aceitas

- Política de privacidade (URL ou arquivo local)
- Documentos internos: RIPD, registro de tratamento, DPA, termos de uso
- Site do controlador (para extrair informações públicas)
- `lgpd.md` existente (para atualização)

### Procedimento — Entrevista Estruturada

Opere como um DPO experiente entrevistando o controlador. Faça perguntas uma a uma, aguardando resposta antes de prosseguir. Para cada pergunta, ofereça sua recomendação baseada nos documentos fornecidos.

**Se documentos foram fornecidos**, extraia o máximo de informações antes de perguntar. Só pergunte o que não pode ser inferido dos documentos.

**Se nenhum documento foi fornecido**, conduza a entrevista completa.

#### Sequência de perguntas (resolver uma a uma):

**Bloco 1 — Identificação**
1. Razão social / nome do controlador
2. CNPJ (se pessoa jurídica)
3. Email/URL de contato para privacidade
4. Nome do Encarregado (DPO)
5. Contato do Encarregado

**Bloco 2 — Direitos e Política**
6. URL da política de privacidade
7. Canal para exercício de direitos do titular (URL ou email)
8. Prazo de resposta (máximo 15 dias conforme Art. 19, II)

**Bloco 3 — Dados e Bases Legais**
9. Quais dados pessoais são coletados? (listar categorias)
10. Para cada categoria: qual a finalidade e base legal?
11. Há dados sensíveis (Art. 11)? Se sim, qual a base legal específica?

**Bloco 4 — Cookies e Scripts**
12. O site usa cookies não essenciais?
13. Há banner de consentimento com opt-in prévio?
14. Opção de rejeitar todos com mesma proeminência?
15. Quais scripts de terceiros estão presentes? (GA4, Meta Pixel, Hotjar, etc.)
16. Quais requerem consentimento prévio?

**Bloco 5 — Transferência Internacional**
17. Dados são transferidos para fora do Brasil?
18. Se sim: quais países/regiões?
19. Qual mecanismo legal autoriza? (SCCs, adequação, consentimento)

**Bloco 6 — Status**
20. O controlador considera-se conforme, parcial ou em adequação?

#### Desafiar contra documentos

Quando a resposta do usuário contradiz o que está nos documentos fornecidos, apontar imediatamente:

> "Sua política de privacidade menciona compartilhamento com Google Analytics, mas você não listou isso nos scripts de terceiros. Qual está correto?"

> "O documento de registro de tratamento lista 'legítimo interesse' para marketing, mas sua política diz 'consentimento'. Qual base legal é a real?"

#### Gerar o arquivo

Após resolver todas as questões, gerar o arquivo `lgpd.md` completo com:
- Frontmatter YAML validável contra o schema
- Corpo Markdown com H1, blockquote, seção "Sobre", "Notas" e "Histórico"
- Salvar no caminho indicado pelo usuário (default: `./lgpd.md`)

</what-to-do>

---

## Modo 3: Relatório

<what-to-do>

Gerar relatório HTML padronizado de conformidade. Se relatório anterior existir, comparar item a item.

### Procedimento

1. **Validar** o arquivo (Modo 1)
2. **Gerar HTML** com estrutura padronizada (ver template abaixo)
3. **Buscar relatório anterior** no mesmo diretório (`lgpd-md-report-*.html`)
4. **Se encontrar anterior**: comparar campo a campo e incluir seção de diff
5. **Salvar** como `lgpd-md-report-{YYYY-MM-DD}.html`

### Estrutura do relatório HTML

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Relatório lgpd.md — {controlador} — {data}</title>
  <style>
    /* Estilos inline para portabilidade */
    body { font-family: system-ui, sans-serif; max-width: 800px; margin: 2rem auto; padding: 0 1rem; }
    .pass { color: #16a34a; } .warn { color: #d97706; } .fail { color: #dc2626; }
    .score { font-size: 3rem; font-weight: bold; text-align: center; }
    table { width: 100%; border-collapse: collapse; }
    th, td { padding: 0.5rem; border: 1px solid #e5e5e5; text-align: left; }
    .diff-added { background: #f0fdf4; } .diff-removed { background: #fef2f2; }
    .diff-changed { background: #fffbeb; }
  </style>
</head>
<body>
  <h1>Relatório de Conformidade lgpd.md</h1>
  <p><strong>Controlador:</strong> {nome}</p>
  <p><strong>Data:</strong> {data}</p>
  <p><strong>Arquivo:</strong> {caminho_ou_url}</p>

  <div class="score">{score}/100</div>

  <h2>Validação por Categoria</h2>
  <table>
    <tr><th>Categoria</th><th>Status</th><th>Detalhes</th></tr>
    <tr><td>Sintaxe YAML</td><td>{✅|❌}</td><td>...</td></tr>
    <tr><td>Campos obrigatórios</td><td>{✅|❌}</td><td>...</td></tr>
    <tr><td>Tipos e formatos</td><td>{✅|⚠️|❌}</td><td>...</td></tr>
    <tr><td>Consistência</td><td>{✅|⚠️|❌}</td><td>...</td></tr>
    <tr><td>Corpo Markdown</td><td>{✅|⚠️}</td><td>...</td></tr>
  </table>

  <h2>Campos Declarados</h2>
  <table>
    <tr><th>Campo</th><th>Valor</th><th>Válido</th></tr>
    <!-- Um row por campo do frontmatter -->
  </table>

  <!-- Se relatório anterior encontrado -->
  <h2>Comparação com Relatório Anterior ({data_anterior})</h2>
  <table>
    <tr><th>Campo</th><th>Anterior</th><th>Atual</th><th>Status</th></tr>
    <tr class="diff-added"><td>cookies.banner</td><td>—</td><td>true</td><td>Adicionado</td></tr>
    <tr class="diff-changed"><td>status</td><td>parcial</td><td>conforme</td><td>Alterado</td></tr>
  </table>

  <h2>Recomendações</h2>
  <ol>
    <!-- Lista de ações sugeridas -->
  </ol>

  <footer>
    <p>Gerado pela skill <a href="https://github.com/lgpd-app/skills">lgpd-md</a> em {datetime}</p>
  </footer>
</body>
</html>
```

### Lógica de comparação

Para cada campo do frontmatter:
- **Adicionado**: existe no atual, não existia no anterior
- **Removido**: existia no anterior, não existe no atual
- **Alterado**: valor mudou
- **Inalterado**: mesmo valor (não mostrar no diff)

Comparar também:
- Score anterior vs atual (melhorou/piorou)
- Status declarado anterior vs atual
- Campos que eram inválidos e agora são válidos (e vice-versa)

</what-to-do>

---

## Referências

| Arquivo | Descrição |
|---------|-----------|
| `references/schema.json` | JSON Schema oficial para validação do frontmatter |
| `references/LGPD-MD-SPEC.md` | Especificação formal completa do padrão |
| `references/example-blog.md` | Exemplo mínimo (blog pessoal) |
| `references/example-ecommerce.md` | Exemplo com cookies e transferência internacional |
| `references/example-saas.md` | Exemplo SaaS B2B com múltiplas bases legais |

---

## Regras de Conteúdo

- Idioma: PT-BR formal-técnico
- Referenciar artigos da Lei 13.709/2018 quando aplicável
- Não alucinar resoluções ou artigos — usar apenas os documentados na spec
- Valores de enum `base` devem ser exatamente os 10 do Art. 7 (ver schema)
- Datas sempre ISO 8601 (YYYY-MM-DD)
- CNPJ sempre no formato `XX.XXX.XXX/XXXX-XX`
