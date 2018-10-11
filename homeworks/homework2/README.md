TFCB 2018: Homework 2
================
Due 12pm, Oct 18, 2018

# Bioconductor and sequence motifs

In this homework, we will learn to use Bioconductor functions to identify sequence motifs around well-defined transcription start sites in the human genome.

First, load the packages that we will use.

1.  `tidyverse`
2.  `rtracklayer`
3.  `plyranges`
4.  `Biostrings`

Problem 1
=========

**10 points**

Read in the annotations of the transcription start sites identified in the FANTOM5 dataset into a `tibble`. This file is available at <http://fantom.gsc.riken.jp/5/datafiles/latest/extra/CAGE_peaks/hg19.cage_peak_phase1and2combined_ann.txt.gz>.

Note that the above file has several "comment" lines. Look at the documentation of the function to read `TSV` files to figure out how to ignore these lines while reading the file.

Filter to all transcription start sites of the `GATA1` gene. You need to first figure out which columns contain gene names. You might then find the `str_detect` function from `tidyverse` useful for doing the filtering.

Use `print()` function to display the contents of the final `tibble` containing the transcription start sites of *GATA1*.

Problem 2
=========

**10 points**

Read in the coordinates of the transcription start sites identified in the FANTOM5 dataset. This file is available at <http://fantom.gsc.riken.jp/5/datafiles/latest/extra/CAGE_peaks/hg19.cage_peak_phase1and2combined_coord.bed.gz>.

Filter for transcription start site peaks that are of just 1nt width and on the positive strand. Note that both `width` and `strand` are default columns of `GRanges` even if they are not displayed.

Stretch the resulting `GRanges` by 10nt on either side using an appropriate function from `plyranges` (<https://sa-lee.github.io/plyranges/reference/index.html>).

Use `print()` function to display the contents of the final `GRanges`.

Problem 3
=========

**10 points**

Retrieve the DNA sequence of the 21 nt region around each transcription start site that you obtained in Problem 2. You will find the `getSeq` function in the `Biostrings` package useful.

Use the `BSgenome.Hsapiens.UCSC.hg19` package for the human genome sequence.

Use `print()` function to display the contents of the `DNAStringSet` output of `getSeq`.

Problem 4
=========

**10 points**

We will make "logo" plots of the above sequences using a new package called `ggseqlogo`.

Figure out how to install `ggseqlogo` and show that you installed it correctly by making an example sequence logo plot using `ggseqlogo`. You are allowed to use any example from the web, but cite your source as a comment.

Problem 5
=========

**10 points**

Make and display a sequence logo plot of the sequences around transcription start sites.

First convert the `DNAStringSet` from Problem 3 to R `character` using the function `as.character()`.

Then use this for plotting in `ggseqlogo`.

5 bonus points if you use can adjust the X tick labels to go from -10 to +10!

# Git and GitHub

Problem 6
=========

**10 points**

Make a GitHub account and populate your bio. Here's an example [github.com/trvrb/](https://github.com/trvrb/). Please provide the link to yours.

Problem 7
=========

**10 points**

Make a new project directory using the material from [lecture 3](../../lectures/lecture3) as basis. Call this directory `tfcb-homework2`. Take files in `lecture3/tables/` and move them to a `data/` directory under `tfcb-homework2`. Include a `README.md` under `tfcb-homework2/` that briefly describes this as homework 2 from TFCB and gives your name.

Make this directory a Git repository, commit the readme file as well as the `data/` files and push it to your GitHub account.

Problem 8
=========

**10 points**

Make an `analysis/` directory under `tfcb-homework2/` and include an R Markdown script to read and operate on these tables following step-by-step instructions from [lecture 4](../../lectures/lecture4/lecture4.pdf). Make the `analysis/` directory your working directory. You'll need to change calls to `read_tsv` in the PDF from:
```
data <- read_tsv("tables/example_dataset_2.tsv")
```
to
```
data <- read_tsv("../data/example_dataset_2.tsv")
```

The file should look something like:
````
---
title: "Homework 2"
output: github_document
---

```{r}
library(tidyverse)
```

```{r}
data <- read_tsv("tables/example_dataset_2.tsv") %>% print()
```
````

Stage and commit this `.Rmd` script and then push changes to your GitHub account.

Problem 9
=========

**10 points**

Additionally commit and push the resulting knitted version of the Rmd analysis. It will be a file called `.md`.
