# LGPD Skills

> 🇬🇧 *AI agent skills for LGPD (Brazil's General Data Protection Law) compliance. Install with `npx skills add lgpd-app/skills`.*

---

Coleção de skills de agente IA para conformidade com a Lei Geral de Proteção de Dados (Lei 13.709/2018).

## Skills Disponíveis

| Skill | Descrição | Instalação |
|-------|-----------|------------|
| **lgpd-check** | Audita websites para conformidade LGPD. Gera relatório com score e correções prioritárias. | `npx skills add lgpd-app/skills --skill "LGPD Check"` |
| **lgpd-md** | Gera e valida arquivos `lgpd.md` a partir de documentos e política de privacidade. | `npx skills add lgpd-app/skills --skill lgpd-md` |

Instalar todas:

```bash
npx skills add lgpd-app/skills
```

Compatível com: Claude Code, Kiro CLI, Cursor, Codex, GitHub Copilot, Windsurf, Cline, e [outros agentes](https://skills.sh).

---

## lgpd-check

Audita websites para conformidade com a LGPD. Verifica 6 módulos:

| # | Módulo | O que audita |
|---|--------|-------------|
| 1 | **Política de Privacidade** | Existência, acessibilidade, identificação do controlador, bases legais, DPO |
| 2 | **Consentimento de Cookies** | Modelo opt-in, banner, botão rejeitar, dark patterns |
| 3 | **Minimização de Dados** | Campos desnecessários, dados sensíveis, proporcionalidade |
| 4 | **Transferência Internacional** | SCCs, adequação, mecanismos legais |
| 5 | **Direitos do Titular** | Canal de exercício, prazo de resposta, portabilidade |
| 6 | **Scripts de Terceiros** | Analytics, pixels, CDNs — consentimento necessário |

### Uso

```
auditar LGPD https://exemplo.com.br
lgpd check https://exemplo.com.br
```

---

## lgpd-md

Gera e valida arquivos `lgpd.md` — o padrão aberto de declaração de conformidade LGPD para repositórios e projetos.

Usa como base:
- Documentos internos de conformidade
- Política de privacidade do site/produto
- Spec do padrão lgpd.md

### Uso

```
gerar lgpd.md
validar lgpd.md
```

---

## Contexto Regulatório (2026)

- ANPD como agência reguladora independente (Resolução 33/2026)
- Decisão de adequação mútua Brasil-UE (Resolução 32/2026)
- SCCs obrigatórias para transferências fora do eixo Brasil-UE (Resolução 19/2024)
- Sanções: multa até 2% da receita no Brasil (teto R$50M por infração)

## Links

- 🌐 **Site**: [lgpd.app](https://lgpd.app)
- 📄 **Padrão lgpd.md**: [lgpd.md](https://lgpd.md)
- 📦 **skills.sh**: [skills.sh](https://skills.sh)

## Licença

[Apache 2.0](LICENSE)
