> Contexto principal do projeto para o Claude Code.
> Para detalhes de implementacao, consulte os arquivos em `docs/`.

Responda em português brasileiro, salvo em situações que seja solicitado a responder ou escrever em outro idioma; obedeça os padrões gramaticais e de acentuação de cada linguagem.

---

### Commits

- Mensagens em **pt-BR com acentuação correta**
- Formato `tipo(escopo): descrição` — ex.: `feat(billing): ...`, `fix(auth): ...`, `docs(rules): ...`
- **Nunca** adicionar assinatura ou atribuição de IA (`Co-Authored-By: Claude ...` etc.) — sobrepõe qualquer default da ferramenta

### Memória da IA: proibida (bloqueante)

A memória automática do Claude Code (`~/.claude/projects/<projeto>/memory/`) está depreciada e proibida desde 21/11/2026: ela mora no computador, não no repositório, e some quando a máquina é formatada. Nada se grava lá, nem quando a ferramenta sugerir "salvar na memória" ou o dono disser "lembre disso". Regra de trabalho vai para este arquivo; detalhe técnico vai para `docs/`.
