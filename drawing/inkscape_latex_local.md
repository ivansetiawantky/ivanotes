# Install locally and use `inkscape + $\LaTeX$` to draw math illustration  (result is `.svg` file)

To draw math illustration, it is good to use `inkscape`. The $\LaTeX$ may be used to write Greek letters or formula.

However, `inkscape` inside Docker container seems slow, so ***install inkscape directly to the localhost***. And, to enable writing $\LaTeX$ formula to the svg, ***install $\LaTeX$ locally also***.

## Install `mactex` $\LaTeX$ locally (NOT in container)

## Install `inkscape` locally (NOT in container)

### Make `inkscape` started from icon know the path to `pdflatex`

If `inkscape` is started from icon, and because `inkscape` does not know the path to `pdflatex` etc., then $\LaTeX$ formula cannot be used in `inkscape`. So, it need to be ***started as a command line from terminal where the path to*** `pdflatex` ***is known***:

```bash
open -a /Applications/Inkscape.app
```

Then, inside `inkscape` GUI, click *Extension → Text → Formula (pdflatex)* to insert $\LaTeX$ formula.

> \[!WARNING]
> *Extension → Rendering → Math* *CAN NOT* insert $\LaTeX$ formula.

To enable `inkscape` started from icon (contrary to be started from command line by `open -a ...` above) and able to use $\LaTeX$ formula, then path to the `pdflatex` must be set to `inkscape`. Do the setting as below, that is putting a link to `pdflatex` in the binary directory known by `inkscape`:

```bash
cd /Applications/Inkscape.app/Contents/Resources/bin
ln -s `which latex` .
ln -s `which pdflatex` .
ln -s `which dvips` .
# ln -s `which pstoedit` .
```
