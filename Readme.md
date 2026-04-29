# Setutila JSON data definition

This repo produces JSON files representing the text in a **hierarchical** form that is easy to render or transform.

## Schema

Use the JSON Schema in:

- `docs/json/setutila-content.schema.json`

It describes:

- Root fields (`title`, `content`, optional `content_flat`)
- The hierarchical tree shape: `content_type → pramana → padya → leaf rows`
- Leaf row fields such as `text`, `kutra`, `bg_patha`, `trk_patha`, `footnote`, etc.

## Key ideas

- `content` is an **array of blocks** grouped by `content_type` (e.g. `Mula`, `Heading3`, `Sarvamula`).
- Inside each `content_type` block, content is nested under:
  - a `{"pramana": true}` node when in pramāṇa context
  - a `{"padya": true}` node when in padya context
- Rows that are marked `Inherit` / `Continue` in the source are **not emitted as values**; they become **children** of the currently active node.
- Rows marked `Inline` padya are always emitted as **children** under the current padya node.

## Parsing tips

Most consumers should ignore `content_flat` and only traverse `content`.

Pseudo-traversal:

1. For each `content_type` block in `content`:
2. Depth-first traverse `children`
3. When you hit a leaf node, render/collect its `text` and optional metadata.

