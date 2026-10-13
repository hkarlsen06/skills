---
name: show-math
description: Show math to the user in T3 Code, whose chat renders no LaTeX. Use whenever a reply would contain an equation, formula, derivation, proof step, matrix, or any math notation beyond plain arithmetic.
---

# Show math

T3 chat prints LaTeX as raw source. Math goes in an inline HTML page typeset with KaTeX, shown above your reply with `html_render`. Chat text stays plain words.

## Steps

1. **Load the tool**: `html_render` from the `t3-code` MCP server. In Claude Code it is deferred: `ToolSearch` with `select:mcp__t3-code__html_render`. Other harnesses may normalize the prefix (`mcp__t3_code__…`); same tool.
2. **Fill the template** below. Copy it verbatim and replace only the example content between `<body>` and `<script>`; the head, styles, and script stay exactly as written. Put each formula in `$$...$$` for display math (gets a Copy button) or `\(...\)` for inline math inside prose. Give each display formula a short `<h3>` label and, if useful, a one-line `<p>`.
3. **Render** with `html_render`, `height: 2000`. T3 shrinks the frame to fit the page; the height is only a cap.
4. **Reply** in plain prose that adds what the page doesn't say. Refer to formulas by their label ("the Basel sum"); the page already shows them.

Plots or diagrams go in the same page as inline SVG using the theme variables (`--chart-1` … `--chart-6`, `--border`, `--muted-foreground`). Hand-drawn layout is worth one `html_preview` check before rendering; plain math is not.

## HTML escaping

The formulas sit in HTML, so write `<` as `\lt`, `>` as `\gt`, and `&` (alignment in `aligned`/matrices) as `&amp;`. Single `$` is plain text, so prices in prose are safe.

## Template

```html
<!doctype html>
<html><head>
<meta charset="utf-8">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/katex.min.css">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/katex.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/contrib/auto-render.min.js"></script>
<style>
  body { font-family: var(--font-sans); color: var(--foreground); line-height: 1.55; }
  h3 { margin: 22px 0 6px; font-size: 15px; font-weight: 600; }
  h3:first-child { margin-top: 0; }
  p { margin: 4px 0; color: var(--muted-foreground); font-size: 14px; }
  .katex-display { margin: 10px 0; position: relative; padding: 0 60px; }
  .katex-display > .katex { overflow-x: auto; overflow-y: hidden; padding: 2px 0; }
  .katex { font-size: 1.12em; }
  .copy { position: absolute; right: 0; top: 50%; transform: translateY(-50%);
    font: 12px var(--font-sans); color: var(--muted-foreground); background: var(--background);
    border: 1px solid var(--border); border-radius: var(--radius); padding: 3px 9px; cursor: pointer; }
  .copy:hover { color: var(--foreground); border-color: var(--ring); }
</style></head>
<body>

<h3>Euler's identity</h3>
$$e^{i\pi} + 1 = 0$$

<h3>Basel problem</h3>
<p>The sum over \(1/n^2\) converges to:</p>
$$\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{6}$$

<script>
// Clipboard API may be blocked in the sandboxed frame; fall back to execCommand.
async function copy(text) {
  try { await navigator.clipboard.writeText(text); return; } catch {}
  const t = Object.assign(document.createElement('textarea'), { value: text });
  document.body.append(t); t.select(); document.execCommand('copy'); t.remove();
}
addEventListener('DOMContentLoaded', () => {
  renderMathInElement(document.body, {
    delimiters: [{left:'$$',right:'$$',display:true},{left:'\\(',right:'\\)',display:false}]
  });
  for (const d of document.querySelectorAll('.katex-display')) {
    const tex = d.querySelector('annotation[encoding="application/x-tex"]').textContent;
    const b = Object.assign(document.createElement('button'), { className: 'copy', textContent: 'Copy' });
    b.title = 'Copy LaTeX';
    b.setAttribute('aria-label', 'Copy LaTeX: ' + tex);
    b.onclick = async () => { await copy(tex); b.textContent = 'Copied'; setTimeout(() => b.textContent = 'Copy', 1200); };
    d.append(b);
  }
});
</script>
</body></html>
```
