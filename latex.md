# How to $\LaTeX$ using Docker Container

## 1. Quickstart to compile 1 simple $\LaTeX$ source file "hello.tex"

```LaTeX
\documentclass[uplatex,dvipdfmx]{jsarticle}

\begin{document}
Hello, \LaTeX World!\\
And this is 日本語.
\end{document}
```

### A. Just pull the image
```bash
docker pull ghcr.io/being24/latex-docker:latest
```
Check the image with:
```bash
docker image inspect ghcr.io/being24/latex-docker:latest | jqosarc
```

### B. Compile "hello.tex" to obtain "hello.pdf"
```bash
docker run -u $(id -u):$(id -g) --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk -pdfdvi hello.tex
```

> \[!CAUTION]
> Depends on the $\LaTeX$ source code type, matching $\LaTeX$ engine MUST be used.\
> There are 3 types: `platex` (or its Unicode version `uplatex`), `pdflatex`, and `lualatex`.\
> Set accordingly in the [`.latexmkrc`](#2-it-is-better-to-provide-latexmkrc) file's `$pdf_mode`, OR use below `latexmk` command line option (as [above](#1-quickstart-to-compile-1-simple--source-file-hellotex)'s `docker run ... latexmk -pdfdvi ... `):
> * ~~`-pdf`: Use `pdflatex` to produce pdf (`$pdf_mode=1`)~~ 日本語サポート無し
> * `-pdfdvi`: Use `uplatex` to produce pdf (`$pdf_mode=3`)
> * `-pdflua` OR `-lualatex`: Use `lualatex` to produce pdf (`$pdf_mode=4`)
>
> See [texwiki/latexmk](https://texwiki.texjp.org/?Latexmk).

> \[!IMPORTANT]
> Due to their Japanese support, use either `uplatex` (`-pdfdvi`, `$pdf_mode=3`) or `lualatex` (`-pdflua`, `-lualatex`, `pdf_mode=4`).\
> `platex` and `uplatex` are very old, so use [`lualatex`](#b-use-lualatex-engine-as-possible) as possible.

> \[!TIP]
> [自分がどのLaTeXを使ってるか知りたい話](https://qiita.com/zr_tex8r/items/a924be192ecea7e6bbe4)にリンクされる `hello.tex` を使って、自分の $\LaTeX$ を確認できる。\
> `latexmk hello.tex` のを実行し、`hello.pdf` に `latexmk` が実行した $\LaTeX$ エンジンがわかる。\
> もちろん `latexmk -{pdfdvi|pdf|pdflua} hello.tex`を実行すると、出来上がった `hello.pdf` は、それぞれ `uplatex+dvipdfmx`、`pdflatex`と、`lualatex`となる。

> \[!CAUTION]
> `lualatex` needs to create fonts and put it to directory writable only by root. So, ***DO NOT*** `docker run` ***WITH*** `-u $(id -u)` ***!!!***\
> Use below `docker run` WITHOUT `-u $(id -u)` for executing `lualatex`.
> ```bash
> docker run --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk -pdflua luahello.tex
> ```

### C. Cleaning up

To remove intermediate files only:
```bash
docker run -u $(id -u):$(id -g) --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk -c
```

To remove everything but the tex source file:
```bash
docker run -u $(id -u):$(id -g) --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk -C
```

For more details please refer to [this](https://zenn.dev/being/articles/how-to-use-my-latex) and [latexmk-howto-page](https://mgeier.github.io/latexmk.html).

## 2. It is better to provide ".latexmkrc"

In the same directory where the tex source file exist, put below as ".latexmkrc" file.
```perl:.latexmkrc
#!/usr/bin/env perl

$do_cd = 1;

$pdflatex = 'pdflatex -synctex=1 -interaction=nonstopmode -file-line-error -halt-on-error %O %S';
# $latex = 'platex -synctex=1 -interaction=nonstopmode -file-line-error -halt-on-error %O %S';
$latex = 'uplatex -synctex=1 -interaction=nonstopmode -file-line-error -halt-on-error %O %S';
$lualatex = 'lualatex -synctex=1 -interaction=nonstopmode -file-line-error -halt-on-error --shell-escape %S';
$dvipdf = 'dvipdfmx %O -o %D %S';
$makeindex = 'makeindex %O -o %D %S';

$bibtex_use=2;
$bibtex = 'upbibtex %O %S';
$biber = 'biber --bblencoding=utf8 -u -U --output_safechars %O %S';

$clean_ext="$clean_ext run.xml dvi synctex.gz";

# pdflatexは1,uplatexは3,lualatexは4
$pdf_mode = 3;
$max_repeat = 10;
```
Note that I modified `$clean_ext` to remove files with extensions dvi and synctex.gz also.

Depending on the tex source file (written for compilation with platex, lualatex, etc.), configurations that may need to be set properly are `$latex`, `$pdf_mode`.

The ".latexmkrc" may need to be updated when the "latex-docker" image or "latex-template-ja" is updated. Find the latest ".latexmkrc" [here](https://github.com/being24/latex-template-ja/blob/master/.latexmkrc).

The command to compile and get pdf is:
```bash
docker run -u $(id -u):$(id -g) --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk texfile.tex
```

Then, clean up:
```bash
docker run -u $(id -u):$(id -g) --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk -c
```
Or remove also the pdf with:
```bash
docker run -u $(id -u):$(id -g) --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk -C
```

## 3. Use $\LaTeX$ template

For a real $\LaTeX$ source writing (NOT just compiling as above), it is better to use template as explained in 「[VSCodeとDockerでLaTeXを用いた多機能論文執筆環境を整える](https://zenn.dev/being/articles/」how-to-use-my-latex)」. The "latex-template-ja" is available [here](https://github.com/being24/latex-template-ja).

### A. "Use this template"

Login to github, go to [https://github.com/being24/latex-template-ja](https://github.com/being24/latex-template-ja) then click "Use this template". Select "Create a new repository", and give a name aligned with your project name. For example "awesame-proj".

Clone the above "awesome-proj" to local host. Then remove tex file, "main.tex", "main.pdf", even class that has nothing to do with your project. This repository is now the repository for your "awesome-proj".

If you want to create tex for other project, then repeat "Use this template" and give a name aligned with the other project.

VScode settings, ".vscode/settings.json" and ".devcontainer/devcontainer.json" etc., are already provided! 

### B. Open the folder (of the cloned repository) with VScode

But first, don't forget to run Docker Desktop.

Because the folder has ".devcontainer/devcontainer.json", when VScode open the folder, then the "ghcr.io/being24/latex-docker:latest" image will be run. VScode will ask whether to open in a new container. Answer "yes".

### C. Create a new tex file and build this new tex file

The build button is the green-play-button. You can preview the output pdf also. Click "TEX" (LaTeX Workshop extension) then select "Clean up auxiliary files" to remove build intermediate files.

### D. My customization

#### D.1. Customize my `.latexmkrc`

* `latexmk -c` will clean `dvi`, s`ynctex.gz` also:
  ```
  $clean_ext="$clean_ext run.xml dvi synctex.gz";
  ```
* Use `lualatex` as default:
  ```
  $pdf_mode = 4;
  ```

#### D.2. Customize my `.vscode/settings.json`

To automatically build after saving tex file, change ".vscode/settings.json" below configuration:
```json
"latex-workshop.latex.autoBuild.run": "never",
```
to
```json
"latex-workshop.latex.autoBuild.run": "onSave"
```

#### D.3. Customize my `.devcontainer/devcontainer.json`

Install vscode-extension to be persistent inside the container. [Required vscode-extensions](./drawing/mermaid.md/#vscode-extension-for-markdown-must-install--use) are:
* vscode-pdf
* Markdown All in One
* Markdown Preview Enhanced
* Markdownlint

To install it persistently (so it will be installed also next time the container is run), follow below steps:
1. In vscode window, click *Extensions* on the left
2. Search Markdown Preview Enhanced
3. Click the *Settings Gear* icon, then select *Add to devcontainer.json*

> \[!CAUTION]
> In Markdown Preview Enhanced setting, use `lualatex` as $\LaTeX$ engine instead of the default `pdflatex`. Set for both *User* and *Remote [Dev Container:...]* settings.\
> May need to add below in `.vscode/settings.json`:\
> `"markdown-preview-enhanced.latexEngine": "lualatex"`

## 4. Typical usage

```bash
docker run -u $(id -u):$(id -g) --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk main.tex
```
```bash
docker run --rm -v $PWD:/workdir ghcr.io/being24/latex-docker bash -c "inkscape --version"
```
```bash
docker run -it --rm --name latex-template-ja --user root ghcr.io/being24/latex-docker:latest /bin/bash
```
```bash
docker run -it --rm --name latex-template-ja -v $PWD:/workdir ghcr.io/being24/latex-docker /bin/bash
```

## 5. No need to use `pdf2svg`, just use `inkscape`

[Explanation for using Inkscape to do pdf2svg](https://rooter.jp/data-format/pdf2svg-inkscape-cli/).

The `inkscape` program is a already available inside the `latex-docker` image. When using below command, the `text` tag is existing in the output svg. But, confirm first that `inkscape --version` inside the container is 1.4 or later.
```bash
inkscape simple.pdf --pdf-font-strategy=keep --export-filename=simple_font_keep.svg
```

But, there are warnings:
```
** (inkscape:11): WARNING **: 09:33:47.157: Failed to wrap object of type 'PangoFT2FontMap'. Hint: this error is commonly caused by failing to call a library init() function.
** (inkscape:11): WARNING **: 09:33:47.236: No pages selected, getting first page only.
** (inkscape:11): WARNING **: 09:33:47.245: Failed to wrap object of type 'GtkRecentManager'. Hint: this error is commonly caused by failing to call a library init() function.
```

The 1st and 3rd are warnings due to headless CLI usage (NO X-server), instead of GUI usage of inkscape, so should be no problem.

For the second warning, first we must separate the pdf, select the desired page:
```bash
pdfseparate -f 2 -l 2 input.pdf onepage.pdf
```
Then, do as above.

## 6. Knowledge about $\LaTeX$

### A. Some $\LaTeX$ engines and `\documentclass` for Japanese

Read this [$\LaTeX$エンジンと文書クラスについて (engine and `\documentclass`)](https://qiita.com/kou-JP/items/60b63963f2ae7fdefddd) first.

日本語が扱えるおすすめの `\documentclass` は [`jlreq`](https://github.com/abenori/jlreq/blob/master/README-ja.md)。使いたい $\LaTeX$ engine は、`[option]` として渡す。Default の `jlreq` は、`article` 相当のスタイルとなる。他のスタイル `book` や `report` は、engine の種類と同じく `[option]` として渡す。

以下のソースは、`uplatex+dvipdfmx` でコンパイル (`latexmk -pdfdvi uplhello.tex`):
```latex:uplhello.tex
\documentclass[uplatex,dvipdfmx]{jlreq}

\begin{document}
Hello, \LaTeX World!\\
And this is 日本語.
\end{document}
```

以下のソースは、`lualatex` でコンパイル (`latexmk -pdflua luahello.tex`):
```latex:luahello.tex
\documentclass[lualatex]{jlreq}

\begin{document}
Hello, \LaTeX World!\\
And this is 日本語.
\end{document}
```

### B. Use `lualatex` engine as possible

[pLaTeX から LuaLaTeX に移行](https://qiita.com/zr_tex8r/items/ac9176e4611bf233a3e0)を参考に `lualatex` のソースを書けば良い。

## 7. Misc

Command to remove everything except files suffixed by ".tex". NOT TOO GOOD, because ".latexmkrc" will be removed also.
```bash
find . -maxdepth 1 -type f ! -name '*.tex' -delete
```