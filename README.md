# ivanotes

Repository for my notes

Mainly how-to notes.

## Preparation 1. vscode extension to be installed and set

* Markdown All in One (To preview md, print md to html)
  - Insert table of contents (TOC)<!-- markdownlint-disable-line MD004 -->
    + github can creates TOC (by clicking Outline menu icon at top right) for markdown docs, so no need for github docs.<!-- markdownlint-disable-line MD004 -->
  - [How to suppress toc detection](https://markdown-all-in-one.github.io/docs/guide/table-of-contents.html#suppressing-toc-detection)<!-- markdownlint-disable-line MD004 -->
    + Add a comment `<!-- omit in toc -->` at the end of a heading or above it.<!-- markdownlint-disable-line MD004 -->
* Markdown Preview Enhanced (Set `true` to *Markdown-preview-enhanced: Enable Script Execution*)
* Markdownlint
* Indent Rainbow
* Trailing Spaces
* ~~Dictionary Completion~~ **DO NOT** install this extension because it will hide suggesting word inside the current file.

## Preparation 2. Set vscode built-in `IntelliSense` (***NOT IntelliCode***)

In the command palette (Command+Shift+P), select *Preferences: Open User Settings (UI)*, then search for "suggest". Select the item to set. Then, also search for "wordBasedSuggestions", and set to "allDocuments".

It seems that `markdown` is by default NOT activating `IntelliSense` suggestion. So, again from the command palette (Command+Shift+P), select *Preferences: Open User Settings (JSON)*, and edit to be like below:

```json
  "[markdown]": {
    "editor.quickSuggestions": {
      "comments": "on",
      "strings": "on",
      "other": "on"
    }
  },
  "editor.suggestSelection": "recentlyUsed",
  "editor.quickSuggestions": {
    "other": "on",
    "comments": "off",
    "strings": "off"
  },
  "editor.wordBasedSuggestions": "allDocuments"
```

May need to add *editor.quickSuggestions* also to *"[go]"* in the `settings.json`.

## Table of Contents

* [How to prepare & use $\LaTeX$ docker environment](./dockerlatex.md)
* How to create drawings, diagram, figure:
  - [Use `mermaid` notation to create diagram in Markdown (result is `.md` text file)](drawing/mermaid.md)<!-- markdownlint-disable-line MD004 -->
  - [Use `draw.io` to create diagram with GUI in vscode (result is `.drawio.svg` file)](drawing/drawiosvg.md)<!-- markdownlint-disable-line MD004 -->
  - [Use `DOT language` (graph description language, part of Graphviz project) to draw tree/graph (result is `.gv` text file)](drawing/dotgraphviz.md)<!-- markdownlint-disable-line MD004 -->
  - [Install locally and use `inkscape + $\LaTeX$` to draw math illustration (result is `.svg` file)](drawing/inkscape_latex_local.md)<!-- markdownlint-disable-line MD004 -->
  - [How to preview TikZ ($\LaTeX$ drawing package) inside `.md` file](drawing/tikzinmd.md)<!-- markdownlint-disable-line MD004 -->
  - [How to write TikZ in $\LaTeX$](drawing/tikzinlatex/tikztutorial.md)<!-- markdownlint-disable-line MD004 -->
  - [TODO Put `geogebra` link here](drawing/geogebra.md)<!-- markdownlint-disable-line MD004 -->
  - [Conclusion: Use `inkscape` to draw conceptual math illustration; use $\LaTeX$'s `\TikZ` or `geogebra` to draw exact math figure/graph](drawing/mathillustration.md)<!-- markdownlint-disable-line MD004 -->
