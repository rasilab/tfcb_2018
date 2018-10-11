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

More sophisticated examples will use a workflow manager like Snakemake to automate builds. For example:

* Alistair Russell and Jesse Bloom's recent work on single-cell sequencing of influenza: [github.com/jbloomlab/IFNsorted_flu_single_cell/](https://github.com/jbloomlab/IFNsorted_flu_single_cell/)

With GitHub as lingua franca for reproducible research, there are no services built on top of this model. For example:

* [Zenodo](https://zenodo.org/) allows you to mint DOIs from GitHub releases.
* In [github.com/cboettig/noise-phenomena](https://github.com/cboettig/noise-phenomena), Carl Boettiger provides `.Rmd` of code, but also uses [Binder](https://mybinder.org/) to launch an interactive RStudio session. Binder is described in more detail [here](https://elifesciences.org/labs/a7d53a88/toward-publishing-reproducible-computation-with-binder).
* We've built [Nextstrain](https://nextstrain.org) to look for results files in public GitHub repos to provide interactive figures. For example [nextstrain.org/community/blab/zika-colombia](https://nextstrain.org/community/blab/zika-colombia) provides an interactive visualization of [Allison Black's](https://bedford.io/team/allison-black/) paper on Zika in Colombia.

### Project communication

For me, as PI, I enforce a further rule:

**_One paper = One GitHub repo = One Slack channel_**

It's _much_ easier if all project communication goes in one place.

### Further reading

Some suggested readings on reproducible research include:

* [Excellent advice from Karl Broman on initial steps to reproducible research](https://kbroman.org/steps2rr/)

## Project and data organization

It's important to keep a tidy project directory, even if something is not as the stage of being versioned on GitHub.

Much of this advice is borrowed directly from [Karl Broman here](https://kbroman.org/dataorg/) and from [Ciera Martinez et al here](https://datacarpentry.org/rr-organization1/).

Some general advice:

1. **Encapsulate everything within one directory, which is named after the project.** Have a single directory for a project, containing all of the data, code, and results for that project. This makes it easier to find things, or to zip it all up and hand it off to someone else.
2. **Separate the data from the code.** I prefer to put code and data in separate subdirectories. I’ll often have a `data/` subdirectory and a `scripts/` (or `src`) subdirectory.
3. **Use relative paths (never absolute paths).** If you encapsulate all data and code within a single project directory, then you can refer to data files with relative paths (e.g., `../data/some_file.csv`). If you were to use an absolute path (like `~/Projects/SomeProject/data/some_file.csv` or `C:\Users\SomeOne\Projects\SomeProject\data\some_file.csv)` then anyone who wanted to reproduce your results but had the project placed in some other location would have to go in and edit all of those directory/file names.
4. **Write dates as YYYY-MM-DD**. This sorts properly and also avoids ambiguities.

#### File names

Borrowing excellent slide deck from Ciera Martinez and colleagues: [Reproducible Science Workshop: File Naming](https://rawgit.com/Reproducible-Science-Curriculum/rr-organization1/master/organization-01-slides.html#1)

#### File organization

Continuing slide deck from Ciera Martinez and colleagues: [Reproducible Science Workshop: Organization](https://rawgit.com/Reproducible-Science-Curriculum/rr-organization1/master/organization-02-slides.html)

### Further reading

Some suggested readings include:

* [More excellent advice from Karl Broman](https://kbroman.org/dataorg/)
* [Good Enough Practices for Scientific Computing by Wilson et al.](https://swcarpentry.github.io/good-enough-practices-in-scientific-computing/)

## Version control, Git and GitHub

* Why use version control?

* Introduction to Git and distributed version control: [Basics from Alice Bartlett with Git for Humans](https://speakerdeck.com/alicebartlett/git-for-humans)

* Introduction to GitHub

* GitKraken vs command line (and why not GitHub Desktop)

* Demonstration:
  - Make a new project on GitHub
  - Make a linear series of commits in GitKraken
  - Cover same actions on the command line: `git status`, `git add`, `git commit`, `git diff`
  - Safely exploring history and rolling back if necessary
  - Amend last commit  
  - Explain branching and merging in GitKraken
  - Remotes and push and pull from GitHub  
  - Pull Requests
  - Look at files under the hood

* Other details:
  - The importance of `.gitignore`
  - The importance of good commit messages, relevant [xkcd](https://xkcd.com/1296/)
  - Breaking up work and staging / committing different pieces separately

### Further reading

* [The Git parable by Tom Preston-Werner motivates many decisions made by Git](http://tom.preston-werner.com/2009/05/19/the-git-parable.html)
* [More by Karl Broman](https://kbroman.org/github_tutorial/)
* [A deeper guide to Git](https://matthew-brett.github.io/curious-git/index.html)

## Intro to the command line

["In the beginning was the command line" by Neal Stephenson](http://cristal.inria.fr/~weis/info/commandline.html)

![](https://i.ytimg.com/vi/MikoF6KZjm0/maxresdefault.jpg)
![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/DEC_VT100_terminal.jpg/1200px-DEC_VT100_terminal.jpg)

Some background:
* Unix vs DOS
* Unix commands are structured like lego blocks, example with `ls -lah | wc -l`

Basic navigation / exploration with:
* `ls` – "list" a directory
* `cd` - "change directory" and what `.` and `..` mean
* `open` - open in Finder
* `cat` - "concatenate" file and how to use write `>` and append `>>`
* `head` - "head" of file
* `tail` - "tail" of file
* `mkdir` - "make directory"
* `rm` - "remove" file or directory, _there are no guardrails!_
* `man` - "manual"
* Globbing with `*`
* Tab completion
* Accessing history with up and down arrows

### Further reading

* [Unix cheatsheet](http://cheatsheetworld.com/programming/unix-linux-cheat-sheet/)
* [Julia Evens writes some wonderful Linux / Unix zines](https://jvns.ca/linux-comics-zine.pdf)
