# Use `draw.io` to create diagram with GUI in vscode (result is `.drawio.svg` file)

`draw.io` (now diagrams.net) is a web-based tool used to create diagrams, including flowcharts, network diagrams, organizational charts, UML, and floor plans. Also used to create technical and business diagrams such as Entity Relationship Diagrams (ERD), process maps, Venn diagrams, and mind maps.

However, for mathematical/conceptual illustration (e.g., drawing triangle etc. for explaining a math problem), I think `inkscape` is better. Anyway, use `draw.io` for technical illustration (flowcharts, etc.).

## Use draw.io from Visual Code Studio (vscode)

First install vscode-extension `Draw.io Integration` by Henning Dieterichs.

Then, create a new `*.drawio.svg` file (the filename-extension is `drawio.svg`). This is a valid svg file.

The `Draw.io Integration` vscode-extension will start automatically when opening file with filename-extension `drawio.svg`, but NOT with filename-extension `.svg` only. This is because `draw.io` can only edit `svg` created by `draw.io`, not any general `svg`. So, strictly follow below rule:

> \[!IMPORTANT]
> When using "draw.io" to create "svg" file, create the file with filename-extension "drawio.svg"!

Use browser to export "svg" file to "pdf":
> \[!TIP]
> The exported "svg" file created by "draw.io" is meant to be displayed in web pages.\
> So, to export the "svg" as "pdf", first open the exported "svg" file with a browser.\
> Then use browser's print-to-pdf to convert the "svg" to "pdf".

## Setting the diagram editor theme

In the vscode menu, click *Code* → *Preferences* → *Settings*.

Select *Extensions* → *Draw.io Integration*.

Set `Hediet > Vscode-drawio: Theme` to `kennedy`.

## Mathematical typesetting in `Draw.io Integration` vscode-extension

Read [this `draw.io のメモ`](https://sogog.hatenablog.com/entry/2022/06/06/232723).

It is better to use inside the [$\LaTeX$ devcontainer](../latex.md/#3-use--template).

Open a `*.drawio.svg` file. Make sure that *Extras* →  *Mathematical Typesetting*, is checked.

Click `+` → *Text*, input for example `$$\alpha = x^2$$`.

See [this FAQ](https://www.drawio.com/doc/faq/math-typesetting), in case the math equation is NOT rendered. There is probably hidden html tags.

## mermaid notation can be used also

Click `+` → *Advanced* → *Mermaid*
