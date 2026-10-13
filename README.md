# show-math

An agent skill that shows math in [T3 Code](https://github.com/pingdotgg/t3code). T3 chat doesn't render LaTeX, so formulas show up as raw source. This skill has the agent put its math on a small HTML page typeset with KaTeX, which T3 Code shows inline above the reply. The reply itself stays plain prose.

## Install

```sh
npx skills add hkarlsen06/show-math
```

Or copy `skills/show-math/SKILL.md` to `~/.claude/skills/show-math/` (Claude Code) or `~/.agents/skills/show-math/` (Codex and other agents).

The agent uses the skill on its own when a reply needs an equation, derivation, matrix, or other notation. You can also ask for it by name.

## How it works

1. The agent loads `html_render` from T3 Code's built-in `t3-code` MCP server.
2. It fills a fixed template with its formulas: `$$…$$` for display math, `\(…\)` for inline math, each with a short label.
3. T3 Code renders the page in a sandboxed frame above the reply and sizes it to fit. KaTeX loads from jsDelivr, and the page uses T3's theme variables, so it matches light and dark mode.
4. Each display formula gets a **Copy** button that copies its LaTeX source.

Plots and diagrams can go on the same page as inline SVG.

## Requirements

T3 Code with the `html_render` tool, so the agent has to run inside T3 Code. Elsewhere the skill has nothing to render with.
