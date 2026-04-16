# Use `geogebra` for plotting function or accurate illustration

Use `geogebra` for plotting function or more accurate illustration. For example, draw exactly angle of 60 degree. `geogebra` is good for illustration which is NOT needing mixing of stroke (line) style.

In some cases, the svg result from `geogebra` is polished using `inkscape`. For example, to put annotation using $\LaTeX$. The $\LaTeX$ font in `inkscape` is better.

Access [geogebra](https://www.geogebra.org/), then click "Start calculator"「アプリの開始」.

## `geogebra` web service tutorial

* [GeoGebra が角度の作画に思いのほか便利で驚いた](https://qiita.com/syuuu/items/db75cf339183940d23f2):
  * ***In "Geometry":***
  * Select first *Basic Tools → Move*, then select angle, point, etc. The angle unit (degree or radian) is set in *Setting → Algebra (数式)*. In "Geography"
* `geogebra` is good for interactive plotting function, for example plotting $g(x) = \sin(x^2)$ as explained in [Programming for Mathematicians day 33](https://www.youtube.com/watch?v=RJBo795ZmII):
  * ***In "Graphing":***
  * Show the "Input" on the left, then input function here
  * We can create slider just be put "a" in the "Input"
  * In Input, write x(A) to know the abscissa of point A.
  * ***In "Geometry":***
  * Can show/hide object
  * ***In "3D Calculator:***
  * Input: $z^2=x^2+y^2$
  * Input point (1,0,0), (0,1,0), (0,0,1), then plane trough 3 points
  * ***In "Geometry":***
  * From 28:00 to 32:20, plotting **ray** and `how to cut ray` (actually, draw a segment on the portion of ray, then hide (uncheck "Show Object") the ray...)
  * Save to [angledemo.ggb](angledemo.ggb) extension, then using geogebra menu, download as [angledemo-ggb.svg](angledemo-ggb.svg). Then see [How to open `geogebra`-produced svg file in `inkscape`](#how-to-open-geogebra-produced-svg-file-in-inkscape)
  * *Edit → XML Editor*: Good for changing text only, for example.
  * *Save a Copy → Optimized svg*
  * *Save a Copy → EPS (encapsulated post script)*
  * "Export" to plain.svg.
  * ![angledemo.svg](angledemo.svg)
* ***CAS***: Computer Algebra System, to do symbolic math.
* [Using `geogebra.org` to make scale drawings](https://www.youtube.com/watch?v=cKpqVOmqRb0)
  * Don't forget to always go back to "Arrow".

## Use `geogebra` then `inkscape`

### How to open `geogebra`-produced svg file in `inkscape`

This method is from [Programming for Mathematicians day 33](https://www.youtube.com/watch?v=RJBo795ZmII) from 37:50:

1. Copy `angledemo-ggb.svg` to `angledemo.svg`. We will operate and save to `angledemo.svg`.
2. Then open [angledemo.svg](./angledemo.svg) with `inkscape`, we can see that it is in one group.
3. Ungroup the group (Command+Shift+g). Click the white-background ONLY, and delete the white background. This is an important step!
4. Create a canvas at (0,0) with size of 3 by 3 inch (for example):
   1. *File →　Document Properties*, select *Front Page*, size as *Custom*, set unit to "inch" first, then 3 by 3.
   2. Be careful, the svg object background is still transparent.
5. ***Add a layer for white background image*** (to prevent transparent background) ***of size A4***:
   1. Add a layer in ***the most bottom***.
   2. In this *BackgroundLayer*, add a rectangle, with stroke width 0, at (0,0), with size (210x297), fill color as white. The svg shown will be with canvas size, so A4 size should be no problem (if you ***export from "Page" tab (OR "Selection" tab***).
   3. Lock this BackgroundLayer. Also lock the white rectangle object.

### How to export selections across layer, where 1 layer is the background white-fill rectangle

Below is a method in `inkscape` to export selections across layers so that the background is also selected:

1. Select the object in a layer.
2. Then go to "Layers and Objects", press Shift and select the white rectangle object in the *BackgroundLayer*.
3. In "Export", ***"Page" tab (OR "Selection" Tab)***, check "Export Selected Only" to plain svg.

### How to export 1 drawing in an svg file and give a white background color (***NOT TRANSPARENT background)

In case a lot of drawings are existing in one svg file, and we want to export just 1 drawing only:

1. Create a rectangle (Region Of Interest) that covering the drawing we want to export,
2. Then move the rectangle to the bottom of the drawing.
3. Change the fill color of the rectangle to white, with 0-width stroke.
4. Select the drawing and the rectangle.
5. Export ("Page" tab) with "Export Selected Only" to plain svg (independent of DPI). In case of export to png, set the DPI to 300.

### Another example of using `geogebra` + `inkscape`

* [Draw a racing circuit with `geogebra`, then refine with `inkscape`](https://www.youtube.com/watch?v=-WWeOlN-wck)
