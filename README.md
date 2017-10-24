# QuickDraw.pde

QuickDraw.pde is a [Processing](https://www.procssing.org) library that makes it easy to interface with drawings from [Google's Quick, Draw! Experiment](https://quickdraw.withgoogle.com) data set in your own sketches. 

I hope that it enables you to new types of open source art and design as it will for myself. If you create something with this dataset, please let me know [by e-mail](mailto:cblewisnj@gmail.com) and consider passing it along to [Google](https://aiexperiments.withgoogle.com/submit).

## Content

- [Getting Started](#getting-started)
  - [Prerequsites](#prerequisites)
  - [Installing](#installing)
- [Reference](#reference)
  - [create()](#create)
  - [mode()](#mode)
  - [info()](#info)
  - [length()](#length)
  - [points()](#points)
  - [curves()](#curves)
  - [noCurves()](#nocurves)
- [License](#license)

# Getting Started

## Prerequisites

The [latest version](https://www.processing.org/download/) of Processing.

[Simplified Drawing Files](https://console.cloud.google.com/storage/browser/quickdraw_dataset/full/simplified) from the Google Quick, Draw! [dataset](https://github.com/googlecreativelab/quickdraw-dataset).

>**Simplified Drawing files (.ndjson)**
>
>We've simplified the vectors, removed the timing information, and positioned and scaled the data into a 256x256 region. The data is exported in ndjson format with the same metadata as the raw format. The simplification process was:
>
>1. Align the drawing to the top-left corner, to have minimum values of 0.
>2. Uniformly scale the drawing, to have a maximum value of 255.
>3. Resample all strokes with a 1 pixel spacing.
>4. Simplify all strokes using the [Ramer–Douglas–Peucker algorithm](https://en.wikipedia.org/wiki/Ramer%E2%80%93Douglas%E2%80%93Peucker_algorithm) with an epsilon value of 2.0.

For this example I've downloaded and will be using the [apple.ndjson](https://storage.googleapis.com/quickdraw_dataset/full/simplified/apple.ndjson) data file :apple:.

## Installing

Once you have finished downloading your file, begin by adding it to your sketch's data folder.
*(Sketch > Add File)*

The following lines have been included in the example file, but if you are starting with an empty sketch window, proceed by initializing a QuickDraw object from the class

```java
QuickDraw qd;
```

and calling it within `void setup()` in order to construct it.

```java
void setup() {
  qd = new QuickDraw("apple.ndjson");
}
```

By default, constructing the object will set the drawings on screen to have no fill. If you want to use filled shapes within your sketch, call the `fill()` function anywhere  either within `setup()` or `draw()` below the object's construction.

That's it! You're now ready to create drawings.

# Reference

The QuickDraw class currently has 7 functions for intefacing with the drawing data.

- [create()](#create)
- [mode()](#mode)
- [info()](#info)
- [length()](#length)
- [points()](#points)
- [curves()](#curves)
- [noCurves()](#nocurves)

## create()

Draws a Google Quick, Draw! drawing to the screen. This function was modeled by the Processing's built in `ellipse()` and `rect()` functions. By default, the first two parameters set the location of the upper-left corner, the third sets the width, and the fourth sets the height. The way these parameters are interpreted, however, may be changed with the `mode()` function.

The fifth parameter sets the index of the drawing you want to pull data from and is 0 by default. The sixth and seventh parameters set the start and stop position of the drawing and are respectively 0.0 and 1.0 by default.

#### Syntax

```
qd.create(x1, y1, x2, y2)
qd.create(x1, y1, x2, y2, index)
qd.create(x1, y1, x2, y2, index, stop)
qd.create(x1, y1, x2, y2, index, start, stop)
```

#### Parameters

```
 qd         QuickDraw: a QuickDraw object
 x1         float: x-coordinate of the drawing by default
 y1         float: y-coordinate of the drawing by default
 x2         float: width of the drawing's bounding box by default
 y2         float: height of the drawing's bounding box by default
 index      int: int between 0 and the object's source file length
 start      float: float between 0.0 and 1.0
 stop       float: float between 0.0 and 1.0
```

## mode()

Modifies the location from which drawings are drawn by changing the way in which parameters given to `create()` are interpreted. This function was modeled by the Processing's built in `ellipseMode()` and `rectMode()` functions.

The default mode is `rectMode(CENTER)`, which interprets the first two parameters of `create()` as the shape's center point, while the third and fourth parameters are its width and height.

`rectMode(CORNER)` interprets the first two parameters of `rect()` as the upper-left corner of the shape, while the third and fourth parameters are its width and height.

`rectMode(CORNERS)` interprets the first two parameters of `rect()` as the location of one corner, and the third and fourth parameters as the location of the opposite corner.

The parameter must be written in ALL CAPS because Processing is a case-sensitive language. The built in variables CENTER, CORNER, and CORNERS equate to the integers 3, 0, and 1 respectively, which can also be input as parameters.

#### Syntax

```
qd.mode(mode)
```

#### Parameters

```
qd          QuickDraw: a QuickDraw object
mode        int: CENTER, CORNER, or CORNERS
```

## info()

Returns a String of information about a specified drawing. By default, the function will return all available data on the drawing across multiple lines. Data points include what source file the drawing is found in, what index of the dataset the drawing is found on, how many points the drawing is made from, what word was the drawing is based on, what country the drawing is from, and what date and time the drawing was originally created at.

When using the string "length" as the data point, the function will return the amount of points in the drawing. When using the string "index" as the data point, the function will return the index, parameter.

#### Syntax

```
qd.info(index)
qd.info(index, "dataPoint")
```

#### Parameters

```
qd          QuickDraw: a QuickDraw object
index       int: int between 0 and the object's source file length
dataPoint   str: "source", "index", "length", "word", "countrycode", or "timestamp"
```

## length()

Returns an integer amount of drawings in the dataset or amount of drawn lines in an index. Used in `info()` to create the data point output as "length".

#### Syntax

```
qd.length()
qd.info(index)
```

#### Parameters

```
qd        QuickDraw: a QuickDraw object
index     int: int between 0 and the object's source file length
```

## points()

Returns an integer amount of points in a drawing index or one of the drawn lines within that index. Used in `info()` to create the data point output as "length".

#### Syntax

```
qd.info(index)
qd.info(index, line)
```

#### Parameters

```
qd          QuickDraw: a QuickDraw object
index       int: int between 0 and the object's source file length
line       int: int between 0 and the value of (qd.info(index) - 1)
```

#### curves()

Enables the default geometry used to smooth the lines drawn on screen within `create()`. Note that this behavior is active by default, so it only necessary to call the function to reactivate the behavior after calling `noCurves()`.

#### Syntax

```
qd.noCurves()
```

#### Parameters

```
qd          QuickDraw: a QuickDraw object
```

## noCurves()

Disables the default geometry used to smooth the lines drawn on screen via `create()`. Note that `curves()` is active by default, so it is necessary to call `noCurves()` to disable smoothing of lines.

#### Syntax

```
qd.noCurves()
```

#### Parameters

```
qd          QuickDraw: a QuickDraw object
```
