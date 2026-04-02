# Install locally and use `inkscape + $\LaTeX$` to draw math illustration  (result is `.svg` file)

To draw math illustration, it is good to use `inkscape`. The $\LaTeX$ may be used to write Greek letters or formula.

However, `inkscape` inside Docker container seems slow, so ***install inkscape directly to the localhost***. And, to enable writing $\LaTeX$ formula to the svg, ***install $\LaTeX$ locally also***.

## Install `MacTeX` $\LaTeX$ in host locally (NOT in container)

### Make sure there is no $\TeX$ system installed using `brew`

Execute below commands to check whether there is $\TeX$ installed using `brew`.

```bash
arch -x86_64 /usr/local/bin/brew list | grep tex
arch -arm64 /opt/homebrew/bin/brew list | grep tex
```

### If some $\TeX$ system is already installed, then remove it

> \[!TIP]
> See [TeX User Grup (TUG) uninstallation page](https://www.tug.org/mactex/uninstalling.html).\
> See also [mac で tex環境を削除 (***REMOVE ALL, CLEAN***)](https://qiita.com/tetsuo_jp/items/04a66b8f42946b5c5a5a).

Execute below command to confirm/remove `basictex`, `ghostscript`, and `imagemagick`, which may be installed using `brew`. If needed, use `.dmg` package to install `ghostscript` or `imagemagick`.

```bash
brew uninstall basictex
brew list --cask | grep tex
brew list --cask | grep ghost
brew list | grep ghost
brewx86 list | grep ghost
brew uninstall --force imagemagick
brew uninstall ghostscript (Better: brew uninstall --ignore-dependencies ghostscript)
brewx86 uninstall --force imagemagick
brewx86 uninstall ghostscript
```

> \[!CAUTION]
> Doing `brew uninstall --force imagemagick` might erase `python3.9`. To install it again, execute:
>
> ```bash
> brewx86 install python@3.9
> ```

### Uninstall, remove all of `MacTeX` (& also its largest piece, `TeX Live`)

Following the [TIP for uninstalling $\TeX$](#if-some-tex-system-is-already-installed-then-remove-it), execute belows (confirm first the config file location from `/usr/local/texlive/2024/texmf.cnf`):

```bash
sudo rm -rf /usr/local/texlive
rm -rf ~/Library/texmf
rm -rf ~/Library/texlive
rm -rf ~/Library/TeXShop
sudo rm -rf /Applications/TeX
sudo rm -rf /Library/TeX/
```

### Install `MacTeX`

Install MacTeX from [The MacTeX-20?? Distribution](https://tug.org/mactex/). From [MacTeX download page](https://tug.org/mactex/mactex-download.html), download `MacTeX.pkg`. Read this download page!

By standard `MacTeX` installation, `ghostscript` will be installed. So, no need to install `ghostcript` using `brew`, NOR remove the old version of `ghostscript`. After installation, check whether ghostscript is updated (`gs --version`).

Use ***TeX Live Utility*** in `/Applications/TeX` to update programs to the current version.

### How to set VScode to use `MacTeX` in host (NOT in the container)

The $\LaTeX$ source code for testing is available in `hostlatex` directory. Put `.latexmkrc` file in this directory, so the source $\LaTeX$ file and the `.latexmkrc` file is ***IN THE SAME DIRECTORY***. Use `hello.tex` to know which $\LaTeX$ engine is used, as in [the TIP here](../dockerlatex.md#b-compile-hellotex-to-obtain-hellopdf).

Install vscode-extension Latex-Workshop. Then, create Workspace [`settings.json`](../.vscode/settings.json) as below:

```json:settings.json
{
    // This is only a part of Workspace's settings.json.
    "[latex]": {
        "editor.formatOnSave": false,
    },
    "latex-workshop.latex.recipe.default": "latexmk (latexmkrc)",
    "latex-workshop.latex.autoBuild.run": "onSave",
}
```

Then also set the default recipe to look at the `.latexmkrc` because by default Latex-Workshop will use `pdflatex`.

> \[!CAUTION]
> Set the preferences of the ***Workspace Settings*** (i.e., this `ivanotes` workspace), instead of the ***User Settings***.\
> This is in order NOT to mix-up dockerized-Latex and host (local)-Latex settings.

See below TIP for opening the `settings.json` file.

> \[!TIP]
> Open ***Workspace Settings***: Command+Shift+P, then Preferences: Open Workspace Settings (JSON)\
> Open ***User Settings***: Command+Shift+P, then Preferences: Open User Settings (JSON)\
> *User Settings* has a wider influence, but can be overriden by *Workspace Settings*.\
> The location of *User Settings' settings.json* is somewhere below home directory, while the location of *Workspace Settings' settings.json* is in the root directory of the project.

## Install `inkscape` locally (NOT in container)

### Download inkscape `Inkscape-1.4.3_arm64.dmg`

Go to [inkscape website](https://inkscape.org/), then download `Inkscape-1.4.3_arm64.dmg` and double-click.

### Make `inkscape` started from icon know the path to `pdflatex`

If `inkscape` is started from icon, and because `inkscape` does not know the path to `pdflatex` etc., then $\LaTeX$ formula cannot be used in `inkscape`. So, it need to be ***started as a command line from terminal where the path to*** `pdflatex` ***is known***:

```bash
open -a /Applications/Inkscape.app
```

Then, inside `inkscape` GUI, click *Extension → Text → Formula 公式 (pdflatex)* to insert $\LaTeX$ formula.

> \[!WARNING]
> *Extension → Rendering → Mathematics* *CAN NOT* insert $\LaTeX$ formula.\
> Use *Extension → Text → Formula 公式 (pdflatex)* to insert $\LaTeX$ formula.

To enable `inkscape` started from icon (contrary to be started from command line by `open -a ...` above) and able to use $\LaTeX$ formula, then path to the `pdflatex` must be set to `inkscape`. Do the setting as below, that is putting a link to `pdflatex` in the binary directory known by `inkscape`:

```bash
cd /Applications/Inkscape.app/Contents/Resources/bin
ln -s `which latex` .
ln -s `which pdflatex` .
ln -s `which dvips` .
# ln -s `which pstoedit` .
```

## Install Inkscape extension TexText

### Problem with *Extension → Text → Formula 公式 (pdflatex)*

Once the $\LaTeX$ equation is rendered, the equation source code is eliminated, so that we cannot re-edit the equation anymore. To prevent this, use *Extension → Text → TexText*.

### URL for downloading TexText and installation

Download and install, following [the macos installation of TexText](https://textext.github.io/textext/install/macos.html).

> \[!CAUTION]
> For inkscape ver 1.4.3, use TexText ver 1.13.0

Unzip the zip source of TexText, and execute below commands. The later command may be not necessary in the future:

```bash
python3.10 setup.py --lualatex-executable=$(which lualatex) --pdflatex-executable=$(which pdflatex) --skip-requirements-check
mv /Applications/Inkscape.app/Contents/Resources/lib/python3.10/site-packages/gi/overrides/GioUnix.py /Applications/Inkscape.app/Contents/Resources/lib/python3.10/site-packages/gi/overrides/GioUnix.py.disabled
```

### Insert $\LaTeX$ equation with *Extension → Text → TexText*

See [the manual for using the TexText Inkscape-extension UI](https://textext.github.io/textext/usage/gui.html#gui)

Use `lualatex` as the $\LaTeX$ equation compiler. Use `$ $` or `\[ \]` or `\( \)`.

> \[!CAUTION]
> TexText editor cannot input backslash, it will show yen mark.\
> Press Alt(Option) + \\, to input "\\".

### How to re-edit the equation?

In Layer and Object tab, select the equation group (g243 etc), then select *Extension → Text → TexText*.

## `inkscape` flow for creating `.svg` file to be included in `.md` file or $\LaTeX$

There is 2 types of `svg` file, that is:

1. `Source inkscape svg` file (an `svg` file edited using `inkscape`; maybe edited later),
2. `Exported plain svg` file (an `svg` file exported by `inkscape`; NOT to be edited further; FINAL READ-ONLY `plain svg`).

First, create the `Source inkscape svg` file. Then, using `inkscape`, select the region-of-interest, and export to `Exported plain svg` file. Include the `Exported plain svg` file to `.md` or $\LaTeX$.

### Initial setting when creating svg file

#### Size in ***pixel (px)***

0. *表示 → ウィドスクリーン*
1. Open `inkscape`, *File → Document Properties → **Set unit to pixel***. Confirm size is A4 and scale (尺度) is 1.0.
2. Save to svg file.

#### Basic knowledge for using `inkscape`

* Object, Path, Stroke
  - Path: a set of nodes (or points) and segments forming a curve, which can be filled or stroked. A path itself has no width.<!-- markdownlint-disable-line MD004 -->
  - Stroke: A contour or outline applied to a vector path or shape, which can have varying widths, colors, and styles.<!-- markdownlint-disable-line MD004 -->
  - Object to path: converts parametric shapes (like circles or rectangles) into editable path nodes, but ignores the stroke.<!-- markdownlint-disable-line MD004 -->
* ***Press shift & left-click to specify stroke (outline) color. Just left-click to specify fill color.***
* Be careful with ***Snap controls*** (The magnet icon on top right).
* ***Ctrl***: draw square/circle (instead of rectangle/ellipse), move (handles) to horizontal/vertical only, keep proportion, 15 degree step rotation, change length/width only.
* ***Shift***: draw from center, select multiple objects.
* To rotate, skew, or change size of the object: change to selector tool (arrow), then ***1 click, 2 times***.
* To show handles to modify object: ***double click the object***. Then, must select the appropriate object (select Circle-shape tools to modify circle, arc, etc.)
* 3 types of handles: tiny square, circle, diamond.
* Notice the ***tool controls bar***! Change shape to arc, circle, etc, here.
* Depending on the mouse position (inside or outside imaginary circle) when dragging circle handle, circle can become arc or segment (pie wedge).
* ***Pencil tool is for drawing free curve*** with click and dragging mouse. ***Pen tool is for drawing segments/polyline (折れ線)*** with double click to end.
* Select the object with selector tool (arrow), then in the menu select *Path → Object to path*.

### Create the `Source inkscape svg` file (MAYBE EDITED LATER)

The `Source inkscape svg` is like below, is having a transparent background, with A4 size:
![inkscape.svg](inkscape_source.svg)

### Set `Exported plain svg` image background to white (NOT TO BE EDITED, FINAL READ-ONLY FILE; ***after doing this, the `plain svg` file CAN NOT be edited using `inkscape`***)

By default, the background of `Exported plain svg` file is transparent. This will make it look bad for a dark theme browser. So, we need to change the background color to white, but in svg, it seems *NOT* possible.

Refering to [stackoverflow's background color of svg root-element](https://stackoverflow.com/a/69899106), we can add a very big circle (below) as the first child of svg tag:

```svg
<circle r="1e5" fill="red"/>
```

Use text editor to edit the svg (which is a text file), so the final svg file will look like this:

```svg
<?xml version="1.0" encoding="UTF-8"?>
<svg version="1.1" xmlns="http://www.w3.org/2000/svg">
  <circle r="1e5" fill="white"/>
  <!-- circle must be the first child, so it is placed right after svg tag -->
  ...
</svg>
```

> \[!NOTE]
> It seems using `inkscape` *File → Document Properties → Display (表示)* CANNOT change the background color to white. It keeps transparent.

But be careful, as stated above, this `plain svg` CAN NOT be edited using `inkscape` anymore:
> \[!CAUTION]
> Due to the size of *very big* background circle object, this circle will prevent other objects selection inside `inkscape` UI. Re-export again from the source `inkscape svg`, when needed.

The above A4 svg equation region is selected, then export as plain svg. The background is then set to yellow circle:\
![ink_src_1_exported_plain.svg](ink_src_1_exported_plain.svg)

The above A4 svg image region is selected, then export as plain svg. The background is then set to white circle:\
![ink_src_2_exported_plain.svg](ink_src_2_exported_plain.svg)

## `inkscape` tutorial

`Inkscape` is used to draw a math conceptual illustration, which possibly include $\LaTeX$ formula. The flow for creating the source `.svg` file (and exporting to `plain svg` file for inclusion) is mentioned [above](#inkscape-flow-for-creating-svg-file-to-be-included-in-md-file-or-latex).

### References

* Read ["Welcome to the Inkscape Beginners' Guide!"](https://inkscape-manuals.readthedocs.io/en/latest/)
* How to separate $\LaTeX$ and figure (but, I don't think will adopt this method) in [How I draw figures for my mathematical lecture notes using Inkscape](https://castel.dev/post/lecture-notes-2/)
* VERY GOOD inkscape tutorial by DrChangMathGuitar [Inkscape for Math Instructors](https://www.youtube.com/playlist?list=PLdr5XE6u9kEpOlqaxqaE9y41ac0FBYAZk)

### In inkspace, how to split arc, so we can have 2 stroke style of ellipse (one with dash-style, one with real-line) ?

Watch [2 Ways to Create Arc in Inkscape](https://www.youtube.com/watch?v=-0SBABNtXJc)

1. Draw ellipse with no fill
2. Ensure snap, 境界枠 (エッジ、角、中央)、ノード is ON
3. Use pen tool and draw the path (i.e., the cutter-path) to cut the ellipse
4. Press shift, select the cutter-path and ellipse (or from layer & object tab, select both paths)
5. Then click menu *Path → Cut path (パスをカット)*. Use real-line stroke style for a path, and dash line for the other path.
