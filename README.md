# skills

Agent skills I use with [T3 Code](https://github.com/pingdotgg/t3code), Claude Code, and Codex.

Install all of them:

```sh
npx skills add hkarlsen06/skills
```

Or one by name, with `--skill <name>`. To install by hand, copy a skill's folder to `~/.claude/skills/` (Claude Code) or `~/.agents/skills/` (Codex and other agents).

## show-math

```sh
npx skills add hkarlsen06/skills --skill show-math
```

Shows math in T3 Code. T3 chat doesn't render LaTeX, so formulas show up as raw source. With this skill, the agent puts its math on a small HTML page typeset with KaTeX, and T3 Code shows that page inline above the reply. The reply itself stays plain prose.

The agent uses it on its own when a reply needs an equation, derivation, matrix, or other notation. You can also ask for it by name.

How it works:

1. The agent loads `html_render` from T3 Code's built-in `t3-code` MCP server.
2. It fills a fixed template with its formulas: `$$…$$` for display math, `\(…\)` for inline math, each with a short label.
3. T3 Code renders the page in a sandboxed frame above the reply and sizes it to fit. KaTeX loads from jsDelivr, and the page uses T3's theme variables, so it matches light and dark mode.
4. Each display formula gets a **Copy** button that copies its LaTeX source.

Plots and diagrams can go on the same page as inline SVG.

It needs the `html_render` tool, so the agent has to run inside T3 Code.
