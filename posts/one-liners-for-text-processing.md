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

I'm currently playing with the [CMU Pronouncing Dictionary](http://www.speech.cs.cmu.edu/cgi-bin/cmudict), which is a word-pronunciation dictionary that contains more than 100,000 items. In dealing with the massive amount of data, I've learned some text processing techniques which I think is quite useful because these kinds of tasks, i.e. processing data, are frequently encountered, and being efficient in text manipulation will certainly save our lives.

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

Below is a piece of code I wrote this afternoon for exploring phoneme and character feature representation of each word. The program *ngram-count* generates all the ngrams based on the training text. Only the CMU dictionary is required as an input, and the code outputs a sparse matrix representation of each word, in terms of phoneme features and character features respectively.

<pre>
#!/bin/sh
#--Generate phone and char feature representations.--
#Using ngram-count
#Removing unigram features and features below threshold.

# == Generate features(sorted by frequency) ==

# --
echo 'Generating phoneme features ...'

sed '/\;\;\;/d;s/.*  //g' cmudict.0.7a.txt > PH # phone-only

cat PH | ngram-count -text - -order 4 | \
awk 'BEGIN{FS=" "; OFS="|"}{print $NF, $0}' | sort -n -r -t'|' -k1 | \
awk 'int($1) >=10' | cut -d'|' -f2 | rev | cut -f2- | rev | grep ' ' > PH_F

PH_SIZE=`cat PH_F | wc -l | cut -d' ' -f1`
echo "$PH_SIZE phoneme features generated."
# --
echo 'Generating character features ...'

sed '/\;\;\;/d;s/  .*//g;/[^A-Za-z]/d' cmudict.0.7a.txt | uniq | \
sed 's/./ &/g;s/^ //' > CH # char-only

cat CH | ngram-count -text - -order 4 | \
awk 'BEGIN{FS=" "; OFS="|"}{print $NF, $0}' | sort -n -r -t'|' -k1 | \
awk 'int($1) >=10' | cut -d'|' -f2 | rev | cut -f2- | rev | grep ' ' > CH_F

CH_SIZE=`cat CH_F | wc -l | cut -d' ' -f1`
echo "$CH_SIZE character features generated."
# --

# == Get feature representations ==

rm -f ph_sp ch_sp # clear if exists

cat cmudict.0.7a.txt | grep -v '[^A-Za-z0-9 ]' > CHPH # char-phone dict

CNT=1 # loop counter

while IFS= read -r line; do
  # phone
  echo "$line" | sed 's/.*  //g' | ngram-count -text - -order 4 | \
   rev | cut -f2- | rev | grep ' ' > TMP;
  diff --unchanged-group-format=''$CNT' %df 1|' --old-group-format='' \
   --new-group-format='' --changed-group-format='' PH_F TMP | \
   sed 's/|/\n/g' >> ph_sp
  # char
  echo "$line" | sed 's/  .*//g' | sed 's/./ &/g;s/^ //' | \
   ngram-count -text - -order 4 | rev | cut -f2- | rev | grep ' ' > TMP;
  diff --unchanged-group-format=''$CNT' %df 1|' --old-group-format='' \
   --new-group-format='' --changed-group-format='' CH_F TMP | \
   sed 's/|/\n/g' >> ch_sp
  # counter++
  CNT=`expr $CNT + 1`
done < CHPH

# == Remove temp files ==

rm CH CH_F PH PH_F CHPH TMP
</pre>
