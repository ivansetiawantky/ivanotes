# ivanotes

Repository for my notes

Mainly how-to notes.

## VScode related preparation

### 1. Install and set vscode extensions below

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
* LaTeX Workshop

### 2. Set vscode built-in `IntelliSense` (***NOT IntelliCode***)

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

### 3. Set LaTeX Workshop linter `chktex` in Workspace (NOT User) `settings.json`

DO NOT SET IN ***User*** `settings.json` because it will disturb the Workspace `settings.json` in being24 or latex-docker-iv. Open command palette (Command+Shift+P), select *Preferences: Open Workspace Settings (JSON)*, then add below to the `settings.json`:

```json
"latex-workshop.check.duplicatedLabels.enabled": true,
"latex-workshop.intellisense.update.aggressive.enabled": true,
"latex-workshop.linting.chktex.enabled": true,
"latex-workshop.linting.run": "onType"
```

### 4. Set LaTeX Workshop formatter `latexindent` in Workspace (NOT User) `settings.json`

Open command palette (Command+Shift+P), select *Preferences: Open Workspace Settings (JSON)*, then edit `settings.json` which finally will become like below:

```json:settings.json
{
    "[latex]": {
        "editor.formatOnSave": false,
        "editor.defaultFormatter": "James-Yu.latex-workshop",
        "editor.wordSeparators": "./\\()\"'-:,.;<>~!@#$%^&*|+=[]{}`~?．。，、（）「」［］｛｝《》てにをはがのともへでや",
        "editor.folding": true,
        "editor.showFoldingControls": "always",
        "editor.wordWrap": "on",
        "files.trimTrailingWhitespace": true,
        "files.trimFinalNewlines": true,
    },
    "latex-workshop.latex.recipe.default": "latexmk (latexmkrc)",
    "latex-workshop.latex.autoBuild.run": "onSave",
    "latex-workshop.latex.autoClean.run": "onFailed",
    "latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.fdb_latexmk",
        "*.synctex.gz",
        "_minted*",
        "*.nav",
        "*.snm",
        "*.vrb",
        "*.run.xml",
        "*.dvi",
        "*.fls",
        "*.toc",
        "*.bcf",
        "*.run.xml",
        //"*.log",
        //"*/*.log"
    ],
    "latex-workshop.check.duplicatedLabels.enabled": true,
    "latex-workshop.intellisense.update.aggressive.enabled": true,
    "latex-workshop.linting.chktex.enabled": true,
    "latex-workshop.linting.run": "onType",
    "latex-workshop.formatting.latex": "latexindent",
    "latex-workshop.format.fixQuotes.enabled": true,
    "latex-workshop.format.fixMath.enabled": true,
    "latex-workshop.formatting.latexindent.path": "latexindent",
    "latex-workshop.formatting.latexindent.args": [
        "-c",
        "%DIR%/",
        "-l",
        "%WORKSPACE_FOLDER%/.vscode/localSettings.yaml",
        // "-w",
        // "-s",
        "-r",
        "%DOC%"
    ],
    "latex-workshop.intellisense.includegraphics.preview.enabled": true,
    "latex-workshop.linting.chktex.exec.args": [
        "-wall",
        "-n19",
        "-n22",
        "-n30",
        "-e16",
        "-q"
    ],
}
```

> \[!CAUTION]
> When executing `latexindent file.tex`, errors like:
>
> * `[Format][latexindent] Error when removing temporary file Error: ENOENT: no such file or directory, unlink '%WS1%/drawing/tikzinlatex/luatikz/indent.log' Error: ENOENT:`
> * `[Format][latexindent] stderr: Can't locate File/HomeDir.pm in @INC (you may need to install the File::HomeDir module)`
>
> may appears. This is due to Perl module File::HomeDir is NOT available.\
> Install using below command. ***May need to repeat this command several times!***:
>
> `sudo cpan -i File::HomeDir`

Again, be careful:
> \[!CAUTION]
> `formatOnSave` must be set to `false` when `autoBuild.run` is `onSave`, to prevent race condition of formatter and builder from writing the same file.\
> Run formatter by right-click the $\TeX$ source file and select *Format Document*.

Maybe now we are using a better $\LaTeX$ formatter:
> \[!TIP]
> Set the *latex-workshop.formatting.latexindent.args* in Workspace `settings.json` as above. Use the configuration of `latexindent` in [/.vscode/localSettings.yaml](/.vscode/localSettings.yaml).

### 5. Some tips of $\LaTeX$ Workshop in vscode

* Mouseover to see the rendered equation
* Use $\TeX$ → MATH SYMBOL, to input $\int_0^\infty$ for example
* Select region, then *Command palette → Surround selection with $\LaTeX$ command → \\(*
* OR, while in-selecting, go to above the selection, and then type **\section**, will also give the same effect like *surround selection with...*.
* [Good $\LaTeX$ preambule](https://qiita.com/moinslut/items/bc1d1b1e13cb38377406#%E3%83%97%E3%83%AA%E3%82%A2%E3%83%B3%E3%83%96%E3%83%AB)
* Type `@a` it will change to `\alpha` etc.
* Type `SCH`, `SSE` it will change to `\chapter{}`, `\section{}` etc. See the TEX Structure.
* `\equation` will expand bo begin-end environment.

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
