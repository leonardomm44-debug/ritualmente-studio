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
.claude/skills/seo/bin/claude-seo setup    # builds an isolated venv under ~/.local/share
.claude/skills/seo/bin/claude-seo doctor   # verify
```

Skills that are purely prompt-driven (briefs, clustering, schema authoring) work
without it. To update: `npx skills add AgriciDaniel/claude-seo --all --copy`.
