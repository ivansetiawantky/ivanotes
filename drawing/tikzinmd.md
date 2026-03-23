# How to preview TikZ ($\LaTeX$ drawing package) inside `.md` file

`Markdown Preview Enhanced (MPE)` VScode extension has code-chunk-execution functionality, so it can compile $\LaTeX$ code. Other than $\LaTeX$ compiler, must install also `pdf2svg` to preview the TikZ figure. See this [web page in case MPE cannot show TikZ in markdown file](https://qiita.com/cens-treye/items/cbd8da8157cb459a68e0).

> \[!NOTE]
> [Access here for MPE code-chunk](https://shd101wyy.github.io/markdown-preview-enhanced/#/code-chunk) explanation.

My host (local) has only `MacTeX` installed, but does NOT have `pdf2svg` installed. So, the host cannot preview TikZ in markdown file. So, use the [latex-docker-iv docker container](/dockerlatex.md#how-to-latex-using-docker-container).

## Steps to preview TikZ inside `.md` file

### 1. Change directory to where the `.md` file to be previewed is existing

Confirm that the current directory has the `.md` (which include TikZ) file to be previewed. For example, this file [`tikzinmd.md`](/drawing/tikzinmd.md).

```bash
$ ls ./tikzinmd.md
./tikzinmd.md
```

### 2. Run the `latex-docker-iv image` as `latexdockerivcont`

Then, run the docker container like below, such that the `tikzinmd.md` is in the container's `/workdir`. May need to use `-u root` in docker run.

```bash
docker run --rm -it --name latexdockerivcont -v $PWD:/workdir latex-docker-iv /bin/bash
```

### 3. Attach to the container

Press `Command+Shift+P` then select `Dev Containers: Attach to Running Container...`, select the above running container.

### 4. Open the file

Open the file in `/workdir`, then select `tikzinmd.md`.

### 5. Install MPE, and see the TikZ figure preview in `tikzinmd.md`

Because the container is NOT run with `.devcontainer/devcontainer.json` available, so we need to install VScode extension `Markdown Preview Enhanced` and `Markdownlint` again.

## Code chunk of $\LaTeX$ to be previewed with MPE

````markdown
```latex {cmd=true, latex_zoom=200%, hide=false}
\documentclass{standalone}
\begin{document}
  Hello \LaTeX and TikZ world!
\end{document}
```
````

`latex_zoom=200%` can be omitted. `hide=true` will hide the source, then the *Play* button to execute this $\LaTeX$ code-chunk will NOT appear, so we cannot execute it. Change to `hide=true` only when you want to release (*use multi cursor edit Command+d*)! The above code-chunk will be previewed as below (`hide=false`):

```latex {cmd=true, latex_zoom=200%, hide=false}
\documentclass{standalone}
\begin{document}
  Hello \LaTeX and TikZ world!
\end{document}
```

### Code-chunk for uplatex (the `dvipdfmx` driver is used)

````markdown
```latex {cmd=true, latex_zoom=100%, hide=false}
\documentclass[margin=0pt]{standalone}
\usepackage[dvipdfmx]{graphicx}
\usepackage{tikz}
\usetikzlibrary{positioning, intersections, calc, arrows.meta, math}

\begin{document}
\begin{tikzpicture}[scale=1]
  ...
\end{tikzpicture}
\end{document}
```
````

### Code-chunk for lualatex

````markdown
```latex {cmd=true, latex_zoom=100%, hide=false}
\documentclass[margin=0pt]{standalone}
\usepackage{tikz}
\usetikzlibrary{positioning, intersections, calc, arrows.meta, math}

\begin{document}
\begin{tikzpicture}[scale=1]
  ...
\end{tikzpicture}
\end{document}
```
````

## Some TikZ samples

```latex {cmd=true, latex_zoom=300%, hide=false}
\documentclass[margin=10pt]{standalone}
\usepackage{tikz}
\begin{document}
  \begin{tikzpicture}[samples=100]
    \draw[->,>=stealth,semithick] (-0.3,0)--(2,0) node[right]{$x$};
    \draw[->,>=stealth,semithick] (0,-0.3)--(0,3) node[above]{$y$};
    \draw (0,0) node[below left]{O};

    % 過剰和を赤色、不足和を青色で示す
    \foreach \x in {0.0, 0.2, ..., 1.6}
      \filldraw[fill=red!20] (\x,0) rectangle (\x+0.2,{max(\x*\x,(\x+0.2)*(\x+0.2))});
      
    \foreach \x in {0.0, 0.2, ..., 1.6}
      \filldraw[fill=blue!20] (\x,0) rectangle (\x+0.2,{min(\x*\x,(\x+0.2)*(\x+0.2))});

    \draw[domain=-0.3:1.85] plot(\x,{\x*\x});
  \end{tikzpicture}
\end{document}
```

```latex {cmd=true, latex_zoom=200%, hide=false}
\documentclass[margin=10pt]{standalone}
\usepackage{tikz}
\usepackage[active,tightpage]{preview}
\PreviewEnvironment{tikzpicture}
\setlength{\PreviewBorder}{10pt}%
\usetikzlibrary{calc}
\usepackage{amssymb}

\begin{document}
\begin{tikzpicture}
  \draw[dashed,color=gray] (0,0) arc (-90:90:0.5 and 1.5);% right half of the left ellipse
  \draw[semithick] (0,0) -- (4,1);% bottom line
  \draw[semithick] (0,3) -- (4,2);% top line
  \draw[semithick] (0,0) arc (270:90:0.5 and 1.5);% left half of the left ellipse
  \draw[semithick] (4,1.5) ellipse (0.166 and 0.5);% right ellipse
  \draw (-1,1.5) node {$\varnothing d_1$};
  \draw (3.3,1.5) node {$\varnothing d_2$};
  \draw[|-,semithick] (0,-0.5) -- (4,-0.5);
  \draw[|->,semithick] (4,-0.5) -- (4.5,-0.5);
  \draw (0,-1) node {$x=0$};
  \draw (4,-1) node {$x=l$};
\end{tikzpicture}
\end{document}
```

```latex {cmd=true, latex_zoom=200%, hide=false}
% Steradian cone in sphere
% Author: Bartman
\documentclass[tikz,border=10pt]{standalone}
\usepackage{sansmath}
\usetikzlibrary{shadings,intersections}
\begin{document}
\begin{tikzpicture}[font = \sansmath]
  \coordinate (O) at (0,0);

  % ball background color
  \shade[ball color = blue, opacity = 0.2] (0,0) circle [radius = 2cm];

  % cone
  \begin{scope}
    \def\rx{0.71}% horizontal radius of the ellipse
    \def\ry{0.15}% vertical radius of the ellipse
    \def\z{0.725}% distance from center of ellipse to origin

    \path [name path = ellipse]    (0,\z) ellipse ({\rx} and {\ry});
    \path [name path = horizontal] (-\rx,\z-\ry*\ry/\z)
                                -- (\rx,\z-\ry*\ry/\z);
    \path [name intersections = {of = ellipse and horizontal}];

    % radius to base of cone in ball
    \draw[fill = gray!50, gray!50] (intersection-1) -- (0,0)
      -- (intersection-2) -- cycle;
    % base of cone in ball
    \draw[fill = gray!30, densely dashed] (0,\z) ellipse ({\rx} and {\ry});
  \end{scope}

  % label of cone
  \draw (0.25,0.4) -- (0.9,0.1) node at (1.05,0.0) {$q$};

  % ball
  \draw (O) circle [radius=2cm];
  % label of ball center point
  \filldraw (O) circle (1pt) node[below] {$P$};

  % radius
  \draw[densely dashed] (O) to [edge label = $r$] (-1.33,1.33);
  \draw[densely dashed] (O) -- (1.33,1.33);

  % cut of ball surface
  \draw[red] (-1.35,1.47) arc [start angle = 140, end angle = 40,
    x radius = 17.6mm, y radius = 14.75mm];
  \draw[red, densely dashed] (-1.36,1.46) arc [start angle = 170, end angle = 10,
    x radius = 13.8mm, y radius = 3.6mm];
  \draw[red] (-1.29,1.52) arc [start angle=-200, end angle = 20,
    x radius = 13.75mm, y radius = 3.15mm];

  % label of cut of ball surface
  \draw (-1.2,2.2) -- (-0.53,1.83) node at (-1.37,2.37) {$A$};
\end{tikzpicture}
\end{document}
```

```latex {cmd=true, latex_zoom=100%, hide=false}
\documentclass[margin=50pt]{standalone}
\usepackage{tikz}

\begin{document}
\begin{tikzpicture}
  \coordinate (A) at (3, 5);
  \draw[thick, ->] (0, 0) -- (A);
  \node[above right] at (A) {$A = (1, 2)$};
\end{tikzpicture}
\end{document}
```

## TikZ tutorial in `tikzinmd.md`

From [大山壇](https://note.com/dan_oyama/n/n6c3ae2a270ac)

<div align="center"><!-- markdownlint-disable-line MD033 -->

```latex {cmd=true, latex_zoom=100%, hide=false}
\documentclass[margin=0pt]{standalone}
% \usepackage[dvipdfmx]{graphicx}
\usepackage{tikz}
\usetikzlibrary{positioning, intersections, calc, arrows.meta, math}

\begin{document}
\begin{tikzpicture}[scale=1]
  \draw[very thick,dotted] (0,0)--(3,2)--(5,-2);
  \draw[very thick,dashed] (0+6,0)--(3+6,2)--(5+6,-2);
  \draw[line width=5pt,double] (0+12,0)--(3+12,2)--(5+12,-2);
\end{tikzpicture}
\end{document}
```

```latex {cmd=true, latex_zoom=100%, hide=false}
\documentclass[margin=10pt]{standalone}
% \usepackage[dvipdfmx]{graphicx}
\usepackage{tikz}
\usetikzlibrary{positioning, intersections, calc, arrows.meta, math}

\begin{document}
\begin{tikzpicture}[scale=1]
  \draw[->,>=stealth,very thick](0,2)--(3,3);
  \draw[->,very thick](0,0)--(3,0);
\end{tikzpicture}
\end{document}
```

```latex {cmd=true, latex_zoom=100%, hide=false}
\documentclass[margin=10pt]{standalone}
% \usepackage[dvipdfmx]{graphicx}
\usepackage{tikz}
\usetikzlibrary{positioning, intersections, calc, arrows.meta, math}

\begin{document}
\begin{tikzpicture}[scale=1]
  \draw[line width=10pt] (0,0)--(3,2)--(5,-2)--(0,0)--cycle;
\end{tikzpicture}
\end{document}
```

```latex {cmd=true, latex_zoom=100%, hide=false}
\documentclass[margin=10pt]{standalone}
% \usepackage[dvipdfmx]{graphicx}
\usepackage{tikz}
\usetikzlibrary{positioning, intersections, calc, arrows.meta, math}

\begin{document}
\begin{tikzpicture}[scale=1]
  \coordinate (O) at (0,0);
  \draw (0,0)--(3,0);
  \draw[dashed] (0,0) to [out=30,in=150] (3,0) node at (1.5, 0.45) {3};
  \draw (1.5,0.45) node[fill=white]{3};
\end{tikzpicture}
\end{document}
```

```latex {cmd=true, latex_zoom=100%, hide=false}
\documentclass[margin=10pt]{standalone}
% \usepackage[dvipdfmx]{graphicx}
\usepackage{tikz}
\usetikzlibrary{positioning, intersections, calc, arrows.meta, math}

\begin{document}
\begin{tikzpicture}[scale=1]
  \draw (0,0)--(3,2)--(5,-2)--cycle;
  \draw (0,0) node[left]{A};
  \draw (3,2) node[above]{B$(\beta=3+2i)$};
  \draw (5,-2) node[below right]{C};
\end{tikzpicture}
\end{document}
```

</div>
