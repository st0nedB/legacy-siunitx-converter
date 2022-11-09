# legacy-siunitx-converter
Simple Tex code to convert new siunitx commands to their legacy counterparts.
Its a quick fix so I don't have to maintain multiple versions of my papers for legacy TexLive environements not under my control (as in I can't update them).
There is certainly a better way to do this, but I didn't find it. 

## Why is this necessary?
The `siunitx`-versions (< 3.0) in TexLive 2020 (and earlier) are not compatible with `siunitx`-versions > 3.0 present in TexLive 2021 (and later).
This raises issues with the package, which can be annoying if the build-system is not under your control. 
If re-writing and maintaning multiple versions of your TeX-code is not an option, you can use this small script. 
It maps the new commands to their corresponding legacy-commands.

## Limitations
Optional arguments of the new commands are ignored as new `siunitx` has either renamed them and/or added new ones.
This may change the formatting of the numbers. Don't use this code in this case.
If you have time to help and improve the code feel free to create a Pull-Request. 

## Usage
Simply copy the script into your documents root directory and input the it into your main `*.tex` file after the `\usepackage{siunitx}` but before `\begin{document}`.
```tex
\documentclass{article}
\usepackage{siunitx}

\input{legacy-siunitx-converter}

\begin{document}
...
```

## How it works
The script differs between two cases. If you need to add more commands, you can find examples here.
Note that the code uses the `\qty` command (exists only in > v3) to determine if its running with an old or new version of siunitx.

### Case 1: Command-Names that exists in both versions
Simply redefine the command using a combination of `\let` and `\renewcommand`.
In this case the optional arguments will cause clashes as they might not exist in older versions.
A quick-and-dirty solution is to simply ignore them and go on with the result.
E.g. for the `\num` command
```tex
\let\oldnum\num
\renewcommand{\num}[2][ignored]{\oldnum{#2}}
```

### Case 2: Command-Names that only exist in newer siunitx versions
Define a new command using a `\newcommand`.
Optional arguments are ignored.
```tex
\newcommand{\qtyrange}[4][ignored]{\SIrange{#2}{#3}{#4}}
```
