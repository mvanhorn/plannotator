Title: Plan and annotate: optional source line numbers beside each block

Settings → Display has a Source line numbers switch, off by default, under Document font. Turn it on and each rendered block in plan review and annotate shows its source lines in a quiet gutter: `42` for one line, `42–45` for a span. The span is the same `startLine` / `sourceLineCount` / `blockEndLine` math exported feedback has used since #623.

The label hangs into the page padding, so the document column does not shift. It is `select-none` and `annotation-exclude`, so it stays out of copied HTML and out of a new annotation's quote. Code fences, tables, lists, quotes, diagrams, math, questions, HTML blocks and YAML frontmatter get one label for the block as a whole, not a number per wrapped line. On phone widths and in print the gutter is hidden.

Cookie `plannotator-document-source-line-numbers`, browser-only, same store as Text size. No server change.

Closes #1650.

## Tests

- `bun run typecheck` — pass
- `bun run test:sandbox` over `packages/ui` — 2069 pass / 2 skip (146 files), 2464 pass / 1 skip (260 DOM files), 7 pass (`useFileBrowser`); 0 fail
- New files: `documentSourceLineNumbersSetting.test.ts` (2), `formatBlockSourceLineLabel.test.ts` (6), `Settings.documentSourceLineNumbers.test.tsx` (2), `Viewer.sourceLineNumbers.test.tsx` (3)
- `bun run --cwd apps/review build && bun run build:hook && bun run build:opencode` — pass

Not verified: Pi extension UI, Windows, a physical phone.

## Demo

![demo](https://raw.githubusercontent.com/mvanhorn/pr-demo-assets/main/osc-bulk-1114/doc-source-line-numbers/preview.gif)

<video src="https://raw.githubusercontent.com/mvanhorn/pr-demo-assets/main/osc-bulk-1114/doc-source-line-numbers/out.mp4" controls width="720"></video>
