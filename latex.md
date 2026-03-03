# How to $\LaTeX$ using Docker Container

## Quickstart to compile 1 simple $\LaTeX$ source file "hello.tex"

```LaTeX
\documentclass{article}

\begin{document}
Hello, \LaTeX World!
\end{document}
```

### 1. Just pull the image
```bash
docker pull ghcr.io/being24/latex-docker:latest
```
Check the image with:
```bash
docker image inspect ghcr.io/being24/latex-docker:latest | jqosarc
```

### 2. Compile "hello.tex" to obtain "hello.pdf"
```bash
docker run -u $(id -u):$(id -g) --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk -pdf hello.tex
```
### 3. Cleaning up

To remove intermediate files only:
```bash
docker run -u $(id -u):$(id -g) --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk -c
```

To remove everything unless the tex source file:
```bash
docker run -u $(id -u):$(id -g) --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk -C
```

For more details please refer to [this](https://zenn.dev/being/articles/how-to-use-my-latex) and [latexmk-howto-page](https://mgeier.github.io/latexmk.html).

## What about using the template?

latex-template

## Typical usage

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

## No need to use `pdf2svg`, just use `inkscape`

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

## Misc

Command to remove everything except files suffixed by ".tex".
```bash
find . -maxdepth 1 -type f ! -name '*.tex' -delete
```