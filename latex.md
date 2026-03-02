# How to $\LaTeX$ using Docker Container

## Quickstart to compile one $\LaTeX$ source file `sample.tex`

1. Just pull the image: `docker pull ghcr.io/being24/latex-docker:latest` (Check with `docker image inspect ghcr.io/being24/latex-docker:latest | jqosarc`)
1. 

For more details please refer to [this](https://zenn.dev/being/articles/how-to-use-my-latex).

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

Is the `id -u` necessary?

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

