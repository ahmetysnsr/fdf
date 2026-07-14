# FDF

`fdf` is a wireframe map renderer built in C with MiniLibX. It reads a height map from a `.fdf` file, projects the points in isometric view, and draws the result in a graphical window.

## Overview

The project parses a 2D grid of altitude values and optional colors, stores the data in a matrix, computes an automatic view, and renders the map as connected line segments.

The implementation includes:

- file parsing with `get_next_line`
- custom split and parsing helpers
- automatic window fitting
- isometric projection
- line drawing on an MLX image buffer
- optional per-point colors

## Project structure

```text
fdf/
├── main.c
├── fdf.h
├── fill_map.c
├── get_map_data.c
├── render_map.c
├── utils1.c
├── utils2.c
├── utils3.c
├── ft_split.c
├── GNL/
└── minilibx-linux/
```

## Build

This project depends on MiniLibX and the X11 libraries on Linux.

```bash
make
```

Common targets:

```bash
make clean
make fclean
make re
```

## Run

```bash
./fdf test_maps/42.fdf
```

Any valid `.fdf` file can be used. Example test maps are included in `test_maps/`.

## Input format

Each line in the input file represents a row of points. Values are separated by spaces.

Basic example:

```text
0 0 0
0 1 0
0 0 0
```

Color example:

```text
0,0xFF0000 10,0x00FF00 20,0x0000FF
```

Supported point syntax:

- `z`
- `z,0xRRGGBB`
- `z,0xrrggbb`

If no color is provided, the default color is white.

## Rendering pipeline

1. Validate the file extension.
2. Scan the file to determine map height and maximum row width.
3. Allocate a point matrix.
4. Parse each row into `z`, color, and validity data.
5. Compute a view that fits the model into a `1200x800` window.
6. Project each valid point with an isometric transform.
7. Draw horizontal and vertical connections between adjacent points.

The rendering uses an image buffer for drawing and then displays it in the MLX window.

## Controls

- `ESC` closes the window
- clicking the window close button exits cleanly

## Implementation notes

- The view is centered automatically.
- The Z axis is scaled separately from X/Y to keep the model readable.
- Missing points in shorter rows are treated as invalid and skipped during drawing.
- The code frees the map matrix and destroys the MLX window on exit.

## Test maps

Useful sample files are available in `test_maps/`:

```text
42.fdf
basictest.fdf
elem.fdf
mars.fdf
pyramide.fdf
```

These files are helpful for checking scaling, slope handling, color parsing, and map density.
