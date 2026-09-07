# Simplified Software LaTeX Report Template

### Disclaimer!!!

This project contains portions derived from a template originally created by **Jesper Kjær Nielsen** in 2020. _Github: https://github.com/jkjaer/aauLatexTemplates._

Those portions are licensed under the **BSD 3-Clause License.** See **License.tex** for the full license.

----------

# Contents

1.  [Macro usage and API guide](#macro-api-guide)
    - [Displaying images](#displaying-images)
    - [How to Reference/Cite?](#how-to-referencecite)
    - [The MoSCoW model](#the-moscow-model)
    - [Block Quotes](#block-quotes)
    - [How to use Code Blocks](#how-to-use-code-blocks)
    - [Creating tables](#creating-tables)
    - [Using Math Blocks](#using-math-blocks)
      
2.  [File structure](#file-structure)
    - [How to import pictures?](#how-to-import-pictures)
    - [How to create new Chapters?](#how-to-create-new-chapters)
    - [Settings Guide](#settings-guide)
    - [Filling out project info](#filling-out-project-info)
    - [Bibliography](#bibliography)
    - [Themes](#themes)
      
3.  [About](#about)

----------

## MACRO API GUIDE

This template wraps the "raw" LaTeX packages (`graphicx`, `listings`, `wrapfig`, `biblatex`, ...) in a small set of custom macros. **You should avoid writing vanilla `\begin{figure}` or `\begin{lstlisting}[language=...]` blocks by hand**. If you stick to the macros below, the styling will stay consistent across the whole report.

### Displaying images

All your images live in `Images/` and are inserted with either `\newimage` or `\newDoubleImage`. Both macros automatically label, just reference `fig:<filename>.`Every image should only be displayed once, then just reference it later if you need; else you will get a you will get a `Label 'fig:Placeholder' multiply defined` warning (which isnt the end of the world).

#### Single image (left, right, or centered)

```latex
\newimage{position}{scale}{path}{caption}
```
Position has 3 options; `left, right, center` 
`left`/`right` will also wrap body text around the image.
`center` places it as a normal full-width figure, in the center, no text wrap.

`scale` goes from 0.0 to 1.0, it will stay between margins. 

`path` to the image, e.g. *Images/Placeholder.jpg*

`caption`text shown under the image.

Example: `\newimage{right}{0.5}{Images/Placeholder.jpg}{Lorem ipsum}`

#### Dual images (side by side)

```latex
\newDoubleImage{width1}{path1}{caption1}{width2}{path2}{caption2}
```
Same as single image, but images are always centered. If the two widths total to something over 1 you probably will get some weird outcomes.

*Example:*
```latex
\newDoubleImage
    {0.45}{Images/Placeholder.jpg}{Left description}
    {0.45}{Images/Placeholder.jpg}{Right description}
```
----------

### How to Reference/Cite?

**Images:** `\ref{fig:<filename>}` or `\autoref{fig:<filename>}`.

**Sections and Chapters:** `\autoref{sec:...}` 

**Bibliography:** `\cite{key}`, where `key` matches an entry in `ZZ - Bibliography.bib`.

**MoSCoW model** `\ref{tab:moscow:<Letter><n>}` or `\hyperref[tab:moscow:<Letter><n>]{text}`.

----------

### The MoSCoW model

Requirement tables (Must have / Should have / Could have / Won't have) are generated with a single macro instead of a hand-built table:

```latex
\moscow
    {Must-have item 1;Must-have item 2}
    {Should-have item 1;Should-have item 2}
    {Could-have item 1;Could-have item 2}
    {Won't-have item 1;Won't-have item 2}

```

-   The macro takes **exactly 4 arguments**, one per MoSCoW category, in order: **M**ust, **S**hould, **C**ould, **W**on't.
-   Inside each argument, separate individual requirements with a semicolon (`;`). Whitespace around each item is trimmed automatically.
-   The macro builds one shaded, alternating-row table and automatically numbers and labels every requirement as `tab:moscow:<Letter><n>` — for example, the second "Should" item becomes `tab:moscow:S2`.
-   You can then reference any single requirement anywhere in the report:
-   This Macro should only be called once, otherwise numbering breaks.

*Example:*
```latex
Requirement \hyperref[tab:moscow:M1]{M1} must ship before \hyperref[tab:moscow:S2]{S2}.
```

----------

### Block Quotes

For a stylized, centered block quote, use:

```latex
\AAUquote{The quoted text goes here.}

```
----------

### How to use Code Blocks

Code is inserted with the `lstlisting` environment. Notable languages included is 
-   C / C++
-   Java
-   Python
-   JavaScript / Typescript
-   HTML
-   XML
-   SQL
-   Bash / shell
-   Assembler

For more languages or help defining your own, ill refer you to the package documentation.
https://tug.ctan.org/macros/latex/contrib/listings/listings.pdf 

```latex
\begin{lstlisting}[language=Java]
if (Objects.equals(field, "tags")) {
    // your code here
}
\end{lstlisting}
```
----------

### Creating tables

`\AAUtable{<mode>}{<table contents>}{<caption>}`
`<mode>` can be:
-   noheader — no header row
-   header — first row is styled as a header
-   side — first row and first column are styled as headers

`<table contents>` contains the table rows using normal tabular syntax.
    ```latex
    \AAUtable{noheader}{
        a & b & c \\
        d & e & f \\
        g & h & i
    }{}
    ```   
`<caption>` is optional. Leave it empty ({}) if no caption is wanted.

### Using Math Blocks

Theres two math blocks a simplistic one: `\AAUmath{<equation>}`
And a themed bigger one: `\AAUmathblock{<equation>}`

----------

## FILE STRUCTURE

```
├── Template/            // <- should stay folded; change in SETTINGS 
│   ├── Assets/
│   │   ├── aau_logo_circle_en.eps
│   │   ├── aau_logo_en.eps
│   │   └── aau_waves.eps
│   │
│   ├── License.tex
│   ├── Macros.tex
│   └── Styling.tex
│
├── Images/                 // <- images go here
│   └── Placeholder.jpg
│
├── 0.1 - Frontpage.tex
├── 0.2 - Titlepage.tex
│
├── 1 - Introduction.tex    // <- new chapters go here, format: "[chapter number] - Name of Chapter.tex"
│                           //    Introduction is a placeholder that shows the macros visually.
├── A - Appendix.tex        //    for appendixes use "[Alphabet] - Name of appendix.tex"
│
├── Z - Main.tex            // <- REMEMBER to add new chapters here with \input{}
├── ZZ - Bibliography.bib   // <- your BibTeX/biblatex references go here
├── ZZZ - Packages.tex      // <- add any extra packages you need here
└── ZZZZ - Readme.md
```

**Why the numbering?** Files are prefixed with numbers/letters, purely so they sort in reading/compile order inside your file explorer.
 - Front matter; 0.X - name of file
 - Chapters; X - name of chapter
 - Appendixes; should be named A, B, C... - Appendix/or name
 - The Z's are reserved for organizing templates included files.


### How to import pictures?

**Image formats:** `.jpg` / `.jpeg`, `.png`, `.eps`, and `.pdf`. 
Vector formats (`.eps`/`.pdf`) are recommended for logos, diagrams, and screenshots of UML/flowcharts, since they stay sharp at any size.

Drop your file into the `Images/` folder and reference it by path in e.g. `Images/Wireframe.png`. 

The filename becomes the figure's label, so name it descriptively this will make referencing easier later. This means that `Images/Placeholder.jpg` would become `fig:Placeholder` when referencing.

### How to create new Chapters (or appendixes)?

1.  **Create a new file at the project root** named `[chapter number] - Chapter Name.tex`, e.g. `2 - Analysis.tex`, `3 - Design.tex`, `4 - Implementation.tex`. The leading number keeps chapters sorted in your file explorer in the order they'll appear in the report — it has no effect on LaTeX itself, only on the filename ordering. (for appendixes use A, B, C... this will make them show up last)

2.  **Register the chapter in `Z - Main.tex`.** Creating the `.tex` file is not enough — LaTeX only compiles files it's told to include. Add an `\input{}` line for your new chapter, in the order you want it to appear, inside the `%------------CHAPTERS--------------%` block:
    
    ```latex
    %------------CHAPTERS--------------%
    \input{1 - Introduction}
    \input{2 - Analysis}
    \input{3 - Design}
    %----------------------------------%
    ```
    
3.  Structure the chapter's content with the usual sectioning commands:
    
    ```latex
    \chapter{Analysis} %this also sets the chapter's title in the Table of Contents
    \section{Requirements}
    \subsection{Functional requirements}
    %\subsubsection{Edge cases} //usually should not ever be needed.
    ```    

### Filling out project info

Project metadata is split across two files, both at the project root:

-   **`0.1 - Frontpage.tex`** — the plain navy cover page.

-   **`0.2 - Titlepage.tex`** — the detailed info page with the abstract box.

Both macros live in `Template/Macros.tex` and shouldn't need to be edited, only the arguments you pass to them in these two files.

### Bibliography

References are kept in **`ZZ - Bibliography.bib`**, using standard BibLaTeX entries, e.g.:

```bibtex
@online{smith2020,
  author  = {Smith, John},
  title   = {An Example Reference},
  year    = {2020},
  url     = {https://example.com},
  urldate = {2026-08-28}
}
```

Cite any entry from anywhere in the report with `\cite{smith2020}`. The full reference list is printed automatically by the line already present in `Z - Main.tex`:

```latex
\printbibliography[heading=bibintoc, title=Bibliography]
```
Only sources in `ZZ - Bibliography.bib` which has been used with `\cite{}` will be displayed

### Themes

`\theme{...}` sets the global color scheme used several macros, it can be found in `Z - Main.tex`

```latex
theme{frost} %Options: sakura, clean and frost.
```
`sakura` is cute and pink.

`clean` is high contrast no background

`frost` is muted blue/grey  (default).

----------

## About

This template, is intended for software/compsci(datalogi) students at AAU. Its intended to be used in Overleaf.
It was created as i found that the other templates, tend to get rather cluttered quickly. The layout, and packages used, have been cut down upon, changed and modified, and ive also made some additional software relevant macros.

If you need anything changed or have suggestions, contact one of the maintainers.
Ive hosted this on my own github page, i will transfer ownership when i stop studying on AAU.
If you have any suggestions feel free to open a github issue, or contribute.

**Contact me** 
For inquiry or in case i fucked up with the bsd3 copyright thingie:
Luna Winther, LunaWinther@proton.me.
Dont send mail regarding changes on there tho, just open an issue on github.
