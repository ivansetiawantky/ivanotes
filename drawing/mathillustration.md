# Drawing math illustration

## Drawing conceptual illustration to explain math problem

Use [inkscape and $\LaTeX$](./inkscape_latex_local.md).

## Drawing exact figure/diagram/graph

And also geogebra+

## TikZ

TikZ is a $\LaTeX$ package to create drawing. TikZ can be included in markdown, as explained in ["How to preview TikZ ($\LaTeX$ drawing package) inside `.md` file"](./tikzinmd.md).

Tutorial of tikzinlatex is here (to be added add to README.md)

`\usepackage{tikz}` の時、~~`pdflatex`~~ (Not good for Japanese) と `lualatex` の場合は、

```latex
\documentclass[lualatex]{jlreq}
\usepackage{tikz}
\begin{document}
    ...
\end{document}
```

で良いのですが、`platex` でコンパイルするときは、`dvipdfmx` ドライバを明示的に指定する必要があります：

```latex
\documentclass[uplatex,dvipdfmx]{jlreq}
\usepackage{tikz}
\begin{document}
    ...
\end{document}
```

See [texwiki/tikz](https://texwiki.texjp.org/TikZ#e634a5d8).
