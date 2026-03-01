# How to $\LaTeX$ using Docker Container

[Refer this](https://zenn.dev/being/articles/how-to-use-my-latex)

## No need to use `pdf2svg`, just use `inkscape`

[Inkscape for doing pdf2svg](https://rooter.jp/data-format/pdf2svg-inkscape-cli/)

The `inkscape` program is a already available inside the `latex-docker` image. When using below command, the `text` tag is existing. But, confirm first that `inkscape --version` inside the container is 1.4 or later.
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

## Typical usage

```
docker run -u $(id -u):$(id -g) --rm -v $PWD:/workdir ghcr.io/being24/latex-docker latexmk main.tex
```

Is the `id -u` necessary?

### How to compile just 1 tex file

### Do I need to `Use this template`?

