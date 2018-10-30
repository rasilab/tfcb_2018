# Homework 4

## What to turn in
Write a Jupyter notebook that does the analysis, and also turn in answers to the specific questions.

## Dates
Assigned: Oct-26-2018

Due: Nov-2-2018, noon

## Question 1: Parsing barcodes from a FASTQ file
We are going to extend the analysis from [lecture 8](https://github.com/rasilab/tfcb_2018/tree/master/lectures/lecture8).

If you recall, at the end of that lecture we were parsing barcodes from the FASTQ file that you can download by clicking [here](https://github.com/rasilab/tfcb_2018/raw/master/lectures/lecture8/barcodes_R1.fastq).

As described in the text above cell 24 of the [lecture 8 Jupyter notebook](https://github.com/rasilab/tfcb_2018/blob/master/lectures/lecture8/lecture_8.ipynb), the barcodes consist of the termini of either flu HA or NA, a short constant sequence, and then 16 `N` nucleotides.

In the lecture, we used BioPython to read in the FASTQ files and convert them to strings (cells 25 and 26 of the [lecture 8 Jupyter notebook](https://github.com/rasilab/tfcb_2018/blob/master/lectures/lecture8/lecture_8.ipynb)).
We then got their reverse complements (cell 28 of the [lecture 8 Jupyter notebook](https://github.com/rasilab/tfcb_2018/blob/master/lectures/lecture8/lecture_8.ipynb)), and created a regular expression match (cell 29 the [lecture 8 Jupyter notebook](https://github.com/rasilab/tfcb_2018/blob/master/lectures/lecture8/lecture_8.ipynb)) that matched all reads that contained a valid barcode.

But there are a few things we did **not** do in that example, and performing those tasks is the subject of this question.

### Question 1.1: Extending the match
The regular expression match in the homework only requires a match with the `AGGCGGCCGC` sequence upstream of the barcode.
But as explained in the description of the reads in the [lecture 8 Jupyter notebook](https://github.com/rasilab/tfcb_2018/blob/master/lectures/lecture8/lecture_8.ipynb), we further know that this upstream sequence should match the 3' end of HA or NA (the two possible expected sequences are given in the lecture 8 Jupyter notebook).
Write a new regular expression object that uses the _or_ operator to require that upstream of the `AGGCGGCCGC` we find either the expected HA or NA sequence. 
Read [this](https://stackoverflow.com/questions/8609597/python-regular-expressions-or) or [this](ocs.python.org/3/library/re.html#regular-expression-syntax) to learn how to do that. 
Call the group that consists of this upstream sequence `ha_or_na` (recall that this is done by using a `?P<ha_or_na>` at the beginning of the group).

Then answer the following question: 
**How many of the 10,000 reads fully match the expected pattern when we include the requirement for matching the 3' end of HA or NA?**

### Question 1.2: Counting HA and NA matches
**How many of the barcode matches have an HA upstream sequence, and how many have a NA upstream sequence?**

To determine this, go through all of the reads that matched in _Question 1.1_, and check if the identity of the `ha_or_na` group matches the HA or NA sequence. 
You can count up the matches with each in a _dict_.

### Question 1.3: Counting number of barcodes for HA and NA.
In cell 30 of the [lecture 8 Jupyter notebook](https://github.com/rasilab/tfcb_2018/blob/master/lectures/lecture8/lecture_8.ipynb), we counted the number of sequences with each barcode.

Now we want to determine the same information but separately for the barcodes with HA and NA upstream.

Answer the two following questions. 
Do this not by duplicating the same code for HA and NA, but by using a function or for loop to apply the same code once to HA and once to NA.

**How many unique barcodes have HA as the upstream, and how many have NA as the upstream?**
Hint: You can get the number of entries in a _dict_ that is keyed by the barcodes using the _len_ function, as in `len(barcode_counts)`.

In reality, we might only care about barcodes that are observed more than once (have a value greater than 1 in a dictionary like the `barcode_counts` one in the [lecture 8 Jupyter notebook](https://github.com/rasilab/tfcb_2018/blob/master/lectures/lecture8/lecture_8.ipynb).
The reason is that singleton barcodes might be generating just by sequencing errors.
**How many unique barcodes are there for HA and NA that have more than one occurrence?**
Hint: you can do this like before, but loop through your dictionary and only count keys that have values greater than 1.
