# Tutorial on how to write TikZ in $\LaTeX$ source file

Because `pdflatex` is NOT GOOD FOR Japanese, here we only provide souces for `lualatex` and `uplatex`.

## Template of TikZ for lualatex with documentclass `jlreq`

```latex
\documentclass[lualatex]{jlreq}
\usepackage{tikz}
\begin{document}
    ...
\end{document}
```

The tutorial lualatex source is available in [./luatikz/luatikztutorial.tex](./luatikz/luatikztutorial.tex).

The file [./luatikz/.latexmkrc](./luatikz/.latexmkrc) in the same `./luatikz/` directory, set the engine to `lualatex` (`$pdf_mode = 4;`).

## Template of TikZ for uplatex with documentclass `jlreq`

When using `uplatex` or `platex` engine to compile $\LaTeX$ source, the [`dvipdfmx` driver must be explicitly specified like below](https://texwiki.texjp.org/TikZ#e634a5d8):

```latex
\documentclass[uplatex,dvipdfmx]{jlreq}
\usepackage{tikz}
\begin{document}
    ...
\end{document}
```

The tutorial uplatex source is available in [./uplatikz/uplatikztutorial.tex](./uplatikz/uplatikztutorial.tex).

The file [./uplatikz/.latexmkrc](./uplatikz/.latexmkrc) in the same `./uplatikz` directory, set the engine to `uplatex` (`$pdf_mode = 3;`). So, the $\LaTeX$ engine switch should be seamless.

## References

* https://math-note.xyz/latex/tikz/tikz-line/<!-- markdownlint-disable-line MD034-->
* https://texample.net/
* https://note.com/dan_oyama/n/n6c3ae2a270ac
* https://alg-d.com/math/tikz.pdf
* http://phys.co-suite.jp/tex/Intro_Tikz.pdf
* https://www.overleaf.com/learn/latex/LaTeX_Graphics_using_TikZ%3A_A_Tutorial_for_Beginners_(Part_1)%E2%80%94Basic_Drawing
* https://www.ams.org/arc/resources/pdfs/tikz_tutorials_all-brown.pdf
