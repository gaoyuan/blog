---
title:
date: '2012-08-17'
description:
categories:
tags:
  - one liner
  - coding
  - unix
  - text processing
---

I'm currently playing with the [CMU Pronouncing Dictionary](http://www.speech.cs.cmu.edu/cgi-bin/cmudict), which is a word-pronunciation dictionary that contains more than 100,000 items. I've learned some text processing techniques in dealing with the massive amount of data, which I think is quite useful because these kinds of tasks, i.e. processing data, are frequently encountered, and being efficient in text manipulation will certainly save our lives.

The first tool is *tr*, which can translate some chars into other chars. As far as I know, it does not support regex. For example, below is a segment of the n-gram counts of the phonemes:

> &lt;s&gt; T UH SH	1
>
> &lt;s&gt; T AE	71
>
> &lt;s&gt; T AE B	8
>
> &lt;s&gt; T AE N	9
>
> &lt;s&gt; T AE K	19

What if I want to keep only the numbers? The following command should work:

<pre> tr -cd '[:digit:]\n' &lt; inputfile </pre>

That means we delete(-d) all the chars except(-c) the numbers([:digit:], which is same as [0-9]) and line break(\n).

For the the same task we may also use the following *perl* command:

<pre> perl -i -pe 's/[^0-9\n]//g' inputfile </pre>

The -i argument means in-place edit, so the inputfile will simply be changed. You can write -i.bak to keep a backup, or simply ignore this option and the result will be displayed in the terminal. The -e argument makes perl code to execute on the command line. And the -p ensures the command to be executed per line. In princple, you can write any valid perl command in the quote. That implies, for instance, you can specify the substitution to happen conditionally on those lines that you desire.

Instead of using perl, we can simply use *sed* almost the same way. Sed is especially designed to do substitution, as its name suggests.

The *sort* command can sort the files by lines, with the -n argument for numerical sorting and -r for descending order. The -k option specify which column to sort. Quite convenient.

Another powerful command is *awk*. For example, you can obtain the sum of the first column of all the lines in a file by a one liner:

<pre> awk '{s+=$1} END {print s}' inputfile </pre>

Some other command line tools are also very helpful:

* cat: display files (cat filename), set up a sketch (cat >> filename), or concatenate multiple files (cat file1 file2).

* wc: count the lines, words and chars of a file.

* paste: paste multiple files line by line.
