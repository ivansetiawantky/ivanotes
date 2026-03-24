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

* [LaTeXで図を直接描けるTikZの使い方 by TikZ職人山本拓人](https://math-note.xyz/latex/tikz/tikz-line/)
* [TikZ 入門(1) ～線を描く～ by 大山壇](https://note.com/dan_oyama/n/n6c3ae2a270ac)
* [Ti$k$Z の使い方](https://alg-d.com/math/tikz.pdf)
* [Ti$k$Z への入門](http://phys.co-suite.jp/tex/Intro_Tikz.pdf)
* [LaTeX Graphics using TikZ: A Tutorial for Beginners (Part 1)—Basic Drawing](https://www.overleaf.com/learn/latex/LaTeX_Graphics_using_TikZ%3A_A_Tutorial_for_Beginners_(Part_1)%E2%80%94Basic_Drawing)
* [LaTeX Graphics using TikZ: A Tutorial for Beginners (Part 2)—Generating TikZ Code from GeoGebra](https://www.overleaf.com/learn/latex/LaTeX_Graphics_using_TikZ%3A_A_Tutorial_for_Beginners_(Part_2)%E2%80%94Generating_TikZ_Code_from_GeoGebra)
* [TikZ tutorials](https://www.ams.org/arc/resources/pdfs/tikz_tutorials_all-brown.pdf)
* [A lot of example in texample.net](https://texample.net/)
