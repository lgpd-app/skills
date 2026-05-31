---
description: "Gera e valida arquivos lgpd.md para conformidade com a LGPD brasileira (Lei 13.709/2018). Usa documentos internos e política de privacidade como base para produzir declarações de conformidade no padrão aberto lgpd.md."
---

# lgpd-md

Gerador e validador de arquivos `lgpd.md` — o padrão aberto de declaração de conformidade LGPD para repositórios e projetos.

## Quando usar

- Gerar um arquivo `lgpd.md` para um projeto/repositório
- Validar um `lgpd.md` existente contra o schema do padrão
- Atualizar um `lgpd.md` após mudanças na política de privacidade

## Entradas aceitas

- Política de privacidade (URL ou texto)
- Documentos internos de conformidade (RIPD, registro de tratamento, etc.)
- `lgpd.md` existente (para validação ou atualização)

## Comandos

```
gerar lgpd.md
validar lgpd.md
atualizar lgpd.md
```

## Output

Arquivo `lgpd.md` no formato do padrão, contendo:

```yaml
---
versao: "1.0"
controlador: "Nome da Empresa"
cnpj: "00.000.000/0001-00"
dpo:
  nome: "Nome do DPO"
  contato: "dpo@empresa.com.br"
ultima_atualizacao: "2026-05-31"
---
```

Seguido das seções obrigatórias:
1. Dados coletados e finalidades
2. Bases legais utilizadas
3. Compartilhamento e transferência internacional
4. Direitos do titular e como exercê-los
5. Retenção e eliminação
6. Segurança e incidentes

## Validação

Verifica contra o JSON Schema do padrão:
- Campos obrigatórios do frontmatter
- Seções obrigatórias presentes
- Formato de datas e CNPJs
- Consistência entre bases legais declaradas e dados coletados

## Referências

- Spec do padrão: [lgpd.md](https://lgpd.md)
- JSON Schema: `references/schema.json`
- Lei 13.709/2018 (LGPD)
- Resoluções ANPD vigentes
