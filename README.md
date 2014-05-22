UC Berkeley Thesis Templates
==============================

Warning
-----------

This package was last updated on May 22, 2014.
Before submitting the final draft of your thesis,
double check with the Graduate Division that everything is kosher (e.g., the
margins are properly sized, the front page is properly spaced, etc.).

To access these templates, by themselves, just download (or clone) this repo,
and check out the templates in the `inst/` directory.


Installation
--------------
### xelatex
The package functions `rnw2pdf` and `rmd2pdf` make system calls to xelatex 
(and biber). These normally come installed on OS X and Linux systems. 
Double check that they are accessible by running the following at the command
line:
```
which xelatex
which biber
```

### pandoc
This package uses [pandoc](http://johnmacfarlane.net/pandoc/installing.html) to
convert markdown (.md) files to latex (.tex) files. `rmd2pdf` will not run 
unless `pandoc` is accesible via the command-line. I.e., `which pandoc` 
returns a non-null string.

This version was tested on pandoc version 1.12.4, however the version available
in the ubuntu software center (1.12.3) should suffice. 
If it doesn't you'll have to manually install pandoc and create system links via
```
sudo ln -s /full/path/to/.cabal/.bin /usr/local/bin
```
And then double check that everything works with
```
which pandoc
```

### the actual package
To install this as an R package, and therefore access the templates, as well as
the helper functions `rnw2pdf` and `rmd2pdf`, you can either clone this repo,
and build the package manually, or use `devtools` via:

```
library(devtools)
devtools::install_github(repo = "ucbthesis", username = "stevenpollack")
```

I don't forsee this package every making it to CRAN (though it passes all the
checks), so this is your best means of obtaining this code.

Usage
-----------------
### Latex
If you know latex, the R code in this package is worthless. You'll want to pull
down the files in `inst/latex` and modify those files accordingly.

### Knitr
If knitr is your bag, the templates in `inst/knitr` are what you need. The
files `chap1.Rnw`, `chap2.Rnw` and `abstract.Rnw` demonstrate the [parent-child
document model](http://yihui.name/knitr/demo/child/) implemented by knitr. Be
warned: child documents are sensititve to white space at their head's, so
make sure there are no lines ahead of the code chunk:
```
<<cache=FALSE, echo=FALSE>>
set_parent("parent-document.Rnw")
@
```
in the child document.

Once you've written your .Rnw and want to compile it into a .pdf to see that
everything's a-okay you have two options:

1. If you're using RStudio: just hit the compile button (CTRL+SHIFT+I).
2. If you're not using RStudio: change your working directory to the location of your .Rnw (`yourFile.Rnw`) file and run `rnw2pdf('yourFile.Rnw')`. See the help
documentation for `rnw2pdf` for more details.

### R Markdown
If you're using R Markdown (and I don't necessarily suggest you do, as this is
your _thesis_), then you'll want to check out the use case in `inst/rmarkdown`.

This package includes the function `rmd2pdf` which performs the
following:

```
.Rmd
  \
   ----- knitr ---> .md
                    /
.tex <-- pandoc ---
 \
 xelatex + biber 
            \
             --> .pdf
```

`inst/rmarkdown` includes a file, `thesis_template.latex` which is a
pandoc template to facilitation the conversion from .md to .tex. This template
is how you specify the latex preamble to that xelatex processes to make the 
final .pdf. Modify the preamble in this document as you would with any latex
document. For example, if you need the `amsmath` package for your dissertation,
add `\usepackage{amsmath}` somewhere before `\begin{document}` in the .latex
template.

