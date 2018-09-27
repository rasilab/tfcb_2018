# Lecture 1

## How do you learn computational biology
Have a basic idea of the main approaches and tools.

Learn to use Google. Especially Stackoverflow.

Look at the code that others have written.

Let others look at your code.

Understand what good code looks like.

## Markdown
Rich text format.

The best way to learn about Markdown is to Google "Markdown".
You will get pages [like this](https://en.wikipedia.org/wiki/Markdown).

### subtitle
#### smaller subtitle

**bold**

*italic*

You can make a list:

 1. item 1
 2. item 2

Or not numbered:

 - item
 - another item

You can write "literal text" `like this` for a few words.

Or like this (with a colon and indentation):

    for a block of text

## Notebook formats
What are they good for?

Are good for mixing explanation, computer code, and plots.

They are not so good for writing complex programs.

On three languages:

 - the command line

 - Python: Jupyter notebook

 - R: RStudio or Jupyter notebook 

## Good versus bad code
See [good_vs_bad_code.ipynb](good_vs_bad_code.ipynb).

## Example code in R
We are now going to go through a real data analysis in R.

Again, you can do "notebooks" in R in Jupyter or in RStudio.

If you want to use R in Jupyter, you need to add `IRkernel`.
[Here are instructions](https://github.com/IRkernel/IRkernel). They are as follows.
First, you open R. Then you type:

    install.packages("devtools")
    devtools::install_github('IRkernel/IRkernel')
    IRkernel::installspec()

Our example analysis is in the Jupyter notebook at [example_R_analysis.ipynb](example_R_analysis.ipynb).
