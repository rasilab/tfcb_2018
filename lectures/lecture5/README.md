# Organizing Projects and Working from Command Line

**Trevor Bedford ([@trvrb](https://twitter.com/trvrb), [bedford.io](https://bedford.io))**

## Lecture outline

1. Reproducibility and collaborative science
2. File organization and naming
3. Git and GitHub
4. Intro to the command line

## Reproducible science

### Motivation

There is a lot of interest and discussion of the reproducibility "crisis". In one example, [Estimating the reproducibility of psychological science (Open Science Collaboration, Science 2015)](https://doi.org/10.1126/science.aac4716), authors attempt to replicate 100 studies in psychology and find that only 36 of the studies had statistically significant results.

![](figures/reproducibility_psychology.png)

The Center for Open Science has also embarked on a [Reproducibility Project for Cancer Biology, with results being reported in an ongoing fashion](https://elifesciences.org/collections/9b1e83d1/reproducibility-project-cancer-biology).

There are a lot of factors at play here, including "_p_ hacking" lead by the "garden of forking paths" and selective publication of significant results. I would call this a crisis of _replication_ and have this as a separate concept from _reproducibility_.

Reproducibility is also difficult to achieve. In ["An empirical analysis of journal policy effectiveness for computational reproducibility" (Stodden et al, PNAS 2018)](https://doi.org/10.1073/pnas.1708290115), Stodden, Seiler and Ma:

>Evaluate the effectiveness of journal policy that requires the data and code necessary for reproducibility be made available postpublication by the authors upon request. We assess the effectiveness of such a policy by (i) requesting data and code from authors and (ii) attempting replication of the published findings. We chose a random sample of 204 scientific papers published in the journal Science after the implementation of their policy in February 2011. We found that we were able to obtain artifacts from 44% of our sample and were able to reproduce the findings for 26%.

They get responses like:

>"When you approach a PI for the source codes and raw data, you better
explain who you are, whom you work for, why you need the data and
what you are going to do with it."

>"I have to say that this is a very unusual request without any explanation!
Please ask your supervisor to send me an email with a detailed,
and I mean detailed, explanation."

[_Tables in paper are very informative._](http://www.pnas.org/content/pnas/115/11/2584.full.pdf)

At the _very_ least, it should be possible to take the raw data that forms the the basis of a paper and run the same analysis that the author used and confirm that it generates the same results. This is my bar for reproducibility.

### Reproducible science guidelines

My number one suggestion for reproducible research is to have:

**_One paper = One GitHub repo_**

Put both data and code into this repository. This should be all someone needs to reproduce your results. This has a few added benefits:

1. Versioning data and code through GitHub allows you to _collaborate_ with colleagues on code. It's extremely difficult to work on the same computational project otherwise. Even Dropbox is messy.

2. You're always working with future you. Having a single clean repo with documented readme makes it possible to come back to a project years later and actually get something done.

3. Other people can build off your work and we make science a better place.

I have a couple examples to look at here:

* My 2015 paper on global migration of influenza viruses: [github.com/blab/global-migration](https://github.com/blab/global-migration)
* [Sidney Bell's](https://bedford.io/team/sidney-bell/) 2018 paper on dengue evolutionary dynamics: [github.com/blab/dengue-antigenic-dynamics](https://github.com/blab/dengue-antigenic-dynamics)

Some things to notice:
* There is a top-level `README.md` file that describes what the thing is, provides authors, provides a citation and link to the paper and gives an overview of project organization.
* There is a top-level `data/` directory.
* Each directory should have a `README.md` file describing its contents.
* The readme files should specify commands to run, or point to the make file that does.
* Versioned Jupyter notebooks are viewable in the browser, as are `.tsv` files.
* Relative links throughout.
* Include language-specific dependency install file, Python uses `requirements.txt` as standard.
* Figures are embedded directly in the readme files via Markdown.

With GitHub as lingua franca for reproducible research, there are no services built on top of this model. For example:

* [Zenodo](https://zenodo.org/) allows you to mint DOIs from GitHub releases.
* In [github.com/cboettig/noise-phenomena](https://github.com/cboettig/noise-phenomena), Carl Boettiger provides `.Rmd` of code, but also uses [Binder](https://mybinder.org/) to launch an interactive RStudio session. Binder is described in more detail [here](https://elifesciences.org/labs/a7d53a88/toward-publishing-reproducible-computation-with-binder).
* We've built [Nextstrain](https://nextstrain.org) to look for results files in public GitHub repos to provide interactive figures. For example [nextstrain.org/community/blab/zika-colombia](https://nextstrain.org/community/blab/zika-colombia) provides an interactive visualization of [Allison Black's](https://bedford.io/team/allison-black/) paper on Zika in Colombia.

### Further reading

Some suggested readings on reproducible research include:

* [Excellent advice from Karl Broman on initial steps to reproducible research](https://kbroman.org/steps2rr/)
