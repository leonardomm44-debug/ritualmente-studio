# ritualmente-studio
ritualmente-studio  /docs    Brand OS    Editorial OS    Design OS  /design-system    Colors    Typography    Components  /prompts    Claude    ChatGPT  /assets    Icons    Illustrations    Frames  /templates    Carousel    Story    Reel  /automation    n8n  /content    Eclipse    Luna    Tarot    Numerología

## Claude SEO

[claude-seo](https://github.com/AgriciDaniel/claude-seo) v2.2.4 (MIT) is installed
project-level: 25 skills in `.claude/skills/` and 18 specialist subagents in
`.claude/agents/`. The skills load automatically in Claude Code — ask for an SEO
audit, content brief, schema markup, topic cluster, and so on.

Several skills shell out to bundled Python tools (page fetching, HTML parsing,
Core Web Vitals, report generation). Those need a one-time runtime setup per
machine:

```bash
.claude/skills/seo/bin/claude-seo setup    # builds an isolated venv
.claude/skills/seo/bin/claude-seo doctor   # verify
```

In this "manual" install mode the runtime's data directory is the skill directory
itself, so the venv and Playwright's Chromium land in
`.claude/skills/seo/` — gitignored, never committed. To keep them out of the
working tree entirely, point the runtime elsewhere:

```bash
export CLAUDE_SEO_DATA_DIR="$HOME/.local/share/claude-seo"
```

Skills that are purely prompt-driven (briefs, clustering, schema authoring) work
without it.

### Updating

```bash
npx skills add AgriciDaniel/claude-seo --skill '*' --agent claude-code --copy -y
```

Pass `--agent claude-code` explicitly. **Do not use `--all`** — it expands to
`--agent '*'` and fans the 25 skills out to every supported agent directory
(`.cursor/`, `.windsurf/`, `.qwen/`, `agent/`, `data/`, `skills/`, … 54 of them:
~8400 files, 69 MB of identical copies).

Re-running the install also overwrites `.claude/skills/seo/`, which **deletes the
bundled runtime** (`bin/`, `scripts/`, `hooks/`, `schema/`, `pdf/`,
`requirements.txt`) along with the venv — the skills CLI only restores what
upstream ships under `skills/seo/`. Those files are committed, so after any
update restore them and rebuild:

```bash
git checkout -- .claude/            # restore the runtime support files
.claude/skills/seo/bin/claude-seo setup
```
