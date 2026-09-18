# Exporting to PNG

Why this exists: native Mermaid `C4Context`/`C4Container`/`C4Component`
diagrams render live in VS Code (with the Mermaid extension) and in the
Mermaid Live Editor, but **GitHub does not render them** — GitHub's bundled
Mermaid renderer excludes the C4 extension, so a `C4Context` code block just
shows as raw text there (regular `flowchart`/`sequenceDiagram` blocks are
unaffected). Exporting a PNG alongside the source is what makes the diagram
actually visible anywhere that can't render Mermaid's C4 syntax.

## One-time setup

Requires Node.js. Either install the CLI globally once:

```
npm install -g @mermaid-js/mermaid-cli
```

or skip the install and invoke it on demand via `npx` each time:

```
npx @mermaid-js/mermaid-cli -i <file>.mmd -o <file>.png
```

Both are free and run entirely locally — no account, no cloud service, no
diagram source ever leaves the machine.

## Exporting

Save the Mermaid source to a `.mmd` file (or point at the `.md` file
containing the fenced ` ```mermaid ` block), then run:

```
mmdc -i <file>.mmd -o <file>.png
```

Save the PNG next to the source file with the same base name, so the pair is
easy to find together (e.g. `taskflow-context.mmd` + `taskflow-context.png`).

## Why PNG, not JPEG

`mmdc` doesn't produce JPEG output directly, and that's not a limitation
worth working around: JPEG's lossy compression introduces visible artifacts
on the sharp lines and small text that diagrams are made of, which is why
diagram tooling generally standardizes on PNG (crisp, lossless raster) or
SVG (vector, scales without quality loss) instead. If a specific downstream
tool truly requires JPEG, convert the PNG afterward with a general image
tool rather than asking `mmdc` to do it.

If file size or infinite scaling matters more than universal raster support,
SVG is also a one-flag change:

```
mmdc -i <file>.mmd -o <file>.svg
```
