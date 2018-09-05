---
output: github_document
---

<!-- README.md is generated from README.Rmd. Please edit that file -->



[![Travis-CI Build Status](https://travis-ci.org/bodkan/admixr.svg?branch=master)](https://travis-ci.org/bodkan/admixr)
[![Coverage status](https://codecov.io/gh/bodkan/admixr/branch/master/graph/badge.svg)](https://codecov.io/github/bodkan/admixr?branch=master)
[![lifecycle](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://www.tidyverse.org/lifecycle/#maturing)

# admixr

_**"Make ADMIXTOOLS fun again!"**_

**TL;DR** Perform ADMIXTOOLS analyses directly from R, without having to deal
with par/pop/log files ever again.

## Introduction

[ADMIXTOOLS](https://github.com/DReichLab/AdmixTools/) is a widely used
software package for calculating admixture statistics and testing population
admixture hypotheses. However, although powerful and comprehensive, it is not
exactly known for being user-friendly.

A typical ADMIXTOOLS workflow often involves a combination of `sed`/`awk`/shell
scripting and manual editing to create different configuration files. These are
then passed as command-line arguments to one of ADMIXTOOLS commands, and
control how to run a particular analysis. The results are then redirected to
another file, which has to be parsed by the user to extract values of interest,
often using command-line utilities again or (worse) by manual copy-pasting.
Finally, the processed results are analysed in R, Excel or another program.

This workflow is very cumbersome, especially if one wants to explore many
hypotheses involving different combinations of populations. Most importantly,
however, it makes it difficult to follow the rules of best practice for
reproducible science, as it is nearly impossible to construct reproducible
automated "pipelines".

This R package makes it possible to perform all stages of an ADMIXTOOLS
analysis entirely from R. It provides a set of convenient functions that
completely remove the need for "low level" configuration of individual
ADMIXTOOLS programs, allowing users to focus on the analysis itself.

## Installation instructions

To install `admixr` from Github you need to install the package `devtools`
first. You can simply run:


```r
install.packages("devtools")
devtools::install_github("bodkan/admixr")
```

**Note that in order to use the `admixr` package, you need a working
installation of ADMIXTOOLS!** You can find installation instructions
[here](https://github.com/DReichLab/AdmixTools/blob/master/README.INSTALL).

**Furthermore, you need to make sure that R can find ADMIXTOOLS binaries on the
`$PATH`.** If this is not the case, running `library(admixr)` will show a
warning message with instructions on how to fix this.

## Example

This is all the code that you need to perform ADMIXTOOLS analyses using this
package! No shell scripting, no copy-pasting and manual editing of text files.
The only thing you need is a working ADMIXTOOLS installation and a path to
EIGENSTRAT data (a trio of ind/snp/geno files), which we call `prefix` here.


```r
library(admixr)

# download a small testing dataset to a temporary directory and return its
# EIGENSTRAT prefix (i.e. shared path to the trio of ind/snp/geno files)
eigenstrat_prefix <- download_data()

result <- d(
    W = c("French", "Sardinian"), X = "Yoruba", Y = "Vindija", Z = "Chimp",
    prefix = eigenstrat_prefix
)

result
#>           W      X       Y     Z      D   stderr Zscore  BABA  ABBA  nsnps
#> 1    French Yoruba Vindija Chimp 0.0313 0.006933  4.510 15802 14844 487753
#> 2 Sardinian Yoruba Vindija Chimp 0.0287 0.006792  4.222 15729 14852 487646
```

Note that a single call to the `d` function generates all required intermediate
config and population files, runs ADMIXTOOLS, parses its log output and returns
the result as a `data.frame` object. It does all of this behind the scenes,
without the user having to deal with low-level technical details.

**To see many more examples, please check out the [tutorial
vignette](https://bodkan.net/admixr/articles/tutorial.html).**
