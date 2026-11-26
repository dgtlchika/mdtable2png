> このフォークについて：デジタル地下室（note: sumaho_lab）が自前環境で使うためのフォークです。上流は H1M4W4R1/mdtable2png（https://github.com/H1M4W4R1/mdtable2png）です。

# mdtable2png

`mdtable2png` is a small command-line tool for turning the first Markdown table in a file into image output.

It reads a Markdown file, writes an SVG version of the table, and renders that SVG to a PNG.

## Features

- Converts the first Markdown table found in an input file.
- Writes both PNG and SVG output.
- Supports Markdown column alignment:
  - `:---` for left alignment
  - `:---:` for center alignment
  - `---:` for right alignment
- Supports simple inline formatting inside cells:
  - `**bold**`
  - `*italic*`
  - `<b>...</b>`
  - `<i>...</i>`
  - `<span style="color:#0969da">...</span>`
  - `[color=red]...[/color]`

## Requirements

- CMake 3.16 or newer
- A C++98-capable compiler
- Windows for PNG rendering

PNG rendering is currently implemented with Windows GDI+. Non-Windows builds can still compile the project, but PNG rendering will report that Windows is required.

## Build

From the repository root:

```sh
cmake -S . -B build
cmake --build build
```

On Windows, the resulting executable is typically placed under a configuration directory such as:

```text
build/Debug/mdtable2png.exe
```

or:

```text
build/Release/mdtable2png.exe
```

## Usage

```sh
mdtable2png <input.md> [output.png] [--svg <output.svg>]
```

or:

```sh
mdtable2png --input <input.md> [--output <output.png>] [--svg <output.svg>]
```

If no PNG path is provided, `mdtable2png` writes the PNG next to the input file using the `.png` extension. If no SVG path is provided, it writes the SVG next to the PNG using the `.svg` extension.

## Example

Given `example.md`:

```md
| Name | Status | Score |
| :--- | :----: | ----: |
| Alpha | **Ready** | 42 |
| Beta | [color=red]Blocked[/color] | 7 |
```

Run:

```sh
mdtable2png example.md
```

This writes:

```text
example.png
example.svg
```

To choose both output paths:

```sh
mdtable2png --input example.md --output table.png --svg table.svg
```

## Command Reference

```text
mdtable2png --help
mdtable2png --version
mdtable2png <input.md> [output.png] [--svg <output.svg>]
mdtable2png --input <input.md> [--output <output.png>] [--svg <output.svg>]
```

Exit codes:

```text
0  Success, help, or version output
1  Input, parsing, SVG writing, or PNG rendering failure
2  Invalid command-line arguments
```

## Notes

- Input files are limited to 4 MiB.
- Only the first Markdown table is converted.
- Very wide cell content is constrained by the renderer.
- User-facing errors are written to stderr.

## License

This project is licensed under the MIT License. See [LICENSE.MD](LICENSE.MD).
