# Drawing math illustration

## Drawing conceptual illustration to explain math problem

Use [inkscape and $\LaTeX$](./inkscape_latex_local.md).

## Drawing exact figure/diagram/graph

And also geogebra+

## TikZ

`\usepackage{tikz}` の時、~~`pdflatex`~~ (Not good for Japanese) と `lualatex` の場合は、

```latex
\documentclass{article}
\usepackage{tikz}
\begin{document}
    ...
\end{document}
```

で良いのですが、`platex` でコンパイルするときは、`dvipdfmx` ドライバを明示的に指定する必要があります：

```latex
\documentclass[dvipdfmx]{article}
\usepackage{tikz}
    ...
```

See [texwiki/tikz](https://texwiki.texjp.org/TikZ#e634a5d8).
