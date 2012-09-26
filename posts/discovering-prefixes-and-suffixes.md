---
title:
date: '2012-09-25'
description:
categories:
tags:
  - language
  - text processing
  - morphology
  - coding
  - unix
  - shell script
---
English is a language that has a concatinative morphology. For example, the word *preinstallation* can be split into three parts -- a prefix *pre*, a stem *install* and a suffix *ation*. Given a corpus of english words, how do we discover the prefixes and suffixes computationally? This is an interesting question, for we can use similar methods to analyze other languages that also share a concatinative morphology.

The corpus of english words can be represented by a forward tree (and a backward tree). The root of the forward tree is the token that denotes the start of word. Each node in the tree corresponds to a character. A word in the corpus is therefore a path starting from the root. Similarly, for the backward tree, the root is the end of word token. A path starting from the root corresponds to a word reading backwards.

After building this tree representation, we can calculate the frequency of each node, which is defined as the frequency of the path starting from the root to that node. Since prefixes(or suffixes) tend to appear more often, high frequency candidates are more likely to be considered prefixes(or suffixes) given a fixed character sequence length, or equivalently, a fixed depth in the tree. For instance, for a forward tree, we sort the candidates with tree depth 5 by their frequencies, we get (below the root, i.e. start of word token, is ignored)

> candidate,frequency

> OVER,378

> INTE,338

> COMP,233

> CONS,212

> UNDE,205

> TRAN,183

> CONT,179

> STRA,150

> COMM,142

> PRES,133

> ...

It seems that we can capture some prefixes solely by their frequencies. The guy on the top of the list -- *OVER* is a common prefix. However, a lot of the high frequency stuff are not actually prefixes, like *INTE* and *UNDE*. Why do they have a high frequency though? That is because *INTER* and *UNDER* are prefixes. So high frequency itself is not a sufficient criterion for a prefix.

It turns out we can eliminate the bad ones by the distribution of their children. For all the characters following *INTE*, most of their occurancies should be *R*, therefore the distribution of the children of *INTE* should be highly biased. A measure that people usually use to discribe this is called **Entropy**. It is defined as $-\sum\_ip\_i \ln p\_i$, where $p\_i$ is the probability of the i-th child. A high entropy implies the children are evenly distributed. Candidate with a high entropy is evidence for a good cut between prefix and stem. That is because varies stems can be attached to the same prefix, so the character following the prefix is rather chaotic.

It turns out there exist some cases where the boundary of a prefix or suffix does not have a high entropy. The suffix *-ing* is a good example. Unfortunately, *ling* appears more often than other *-ing*'s, like *ting* or *ming*. Therefore if you look at *ing*, it doesn't have a very high entropy. Combining the frequency and entropy criterions helps a lot, but there are still some corner cases.

What if we print the frequency and entropy path of each candidate? Below we selected the depth-6 candidates that have both high frequencies and high entropies. We print the frequency and entropy path of each candidate:

> B L A C K 62 2.44667
> B L A C	63 0.0815104
> B L A	235 2.2194
> B L	642 1.52547
> B	8561 1.94162

> C R O S S 42 2.58178
> C R O S	57 1.11551
> C R O	210 2.44689
> C R	862 1.68729
> C	9225 1.84258

> E X T R A 39 2.25089
> E X T R	61 1.11793
> E X T	115 1.21067
> E X	573 1.98306
> E	3922 2.74215

> G R A N D 60 2.69772
> G R A N	118 1.94621
> G R A	415 2.48571
> G R	1114 1.54845
> G	5006 2.0093

> G R E E N 72 2.51971
> G R E E	94 0.993835
> G R E	217 2.10298
> G R	1114 1.54845
> G	5006 2.0093

> I N T E R 275 2.71212
> I N T E	338 0.745482
> I N T	439 0.818788
> I N	1663 2.41109
> I	2849 1.64657

> M I C R O 87 2.63535
> M I C R	87 0
> M I C	179 1.33012
> M I	1377 2.23578
> M	8424 1.7602

> M I L L I 41 2.42468
> M I L L	87 1.68536
> M I L	212 2.2017
> M I	1377 2.23578
> M	8424 1.7602

> M O N T E 39 2.61507
> M O N T	108 2.08311
> M O N	330 2.28117
> M O	1369 2.53725
> M	8424 1.7602

> M U L T I 62 2.37335
> M U L T	64 0.160722
> M U L	144 1.77535
> M U	633 2.32698
> M	8424 1.7602

> N O R T H 41 2.251
> N O R T	45 0.349945
> N O R	185 2.46898
> N O	689 2.34442
> N	2576 1.62171

> P E T R O 44 2.34169
> P E T R	90 1.44709
> P E T	189 1.71305
> P E	1235 2.18293
> P	7159 2.03942

> P H O T O 37 2.30305
> P H O T	39 0.202273
> P H O	80 1.36333
> P H	296 1.71338
> P	7159 2.03942

> R O S E N 46 2.41789
> R O S E	89 1.74483
> R O S	203 1.88389
> R O	1140 2.8455
> R	6203 1.53285

> S H O R T 40 2.44971
> S H O R	56 1.01256
> S H O	234 2.55013
> S H	1244 1.76214
> S	12308 2.54537

> S T O C K 44 2.48733
> S T O C	46 0.208982
> S T O	288 2.58185
> S T	2136 1.75754
> S	12308 2.54537

> S U P E R 114 2.60547
> S U P E	114 0
> S U P	175 0.925686
> S U	988 2.47639
> S	12308 2.54537

> T R A N S 163 2.53122
> T R A N	183 0.565372
> T R A	416 2.07768
> T R	1053 1.56747
> T	4882 2.04851

> U N D E R 187 2.67801
> U N D E	205 0.466284
> U N D	247 0.711374
> U N	1109 2.68328
> U	1608 1.33352

> W A T E R 51 2.52729
> W A T E	51 0
> W A T	99 1.67263
> W A	854 2.45031
> W	3543 1.7672

> W H I T E 43 2.35619
> W H I T	108 2.0792
> W H I	196 1.60616
> W H	337 1.15808
> W	3543 1.7672

The first number following each candidate is the frequency, and the second is entropy. The feature becomes clear -- a prefix should be one such that its parent has a low entropy and itself has a high entropy. The same applies to suffixes. Now if we look at the entropy path of *ing*, we find

> ING,2.66174

> NG,0.282628

> G,0.646555

Since *NG* has a low entropy and *ING* has a high entropy, it becomes clear that *ING* should be a suffix.

The experiment above uses the corpus [CMU Pronouncing Dictionary](http://www.speech.cs.cmu.edu/cgi-bin/cmudict). Below is the shell script to generate the frequencies and entropies for the forward and backward tree:
<pre>
\#!/bin/sh
\#--Generate frequency and entropy for forward&backward tree.--
\# == parameters ==
DICT=../data/cmudict.0.7a.txt
ORDER=7
\# == function ngramFrequencyEntropy ==
\# Generate 1-$2 order grams with frequency and entropy
\# $1 : file name
\# $2 : order
ngramFrequencyEntropy(){
  CMD="ngram-count -text $1 -order $2"
  for (( i = 1; i <= $2; i ++ )); do
    CMD=$CMD" -write$i $1.ORDER$i"
  done
  eval $CMD
  for (( i = 1; i <= $2; i ++ )); do
    grep '^\<s\>' $1.ORDER$i > $1.ORDER$i.PRE
  done  
  eval "cat $1.ORDER[1-$(($ORDER-1))].PRE > $3" \# cat all the prefixes
  \# add the entropy of each node
  LINENUM=1
  AWKCMD='{sum+=$NF; a[++i]=$NF } END ' \# simply because its too long...
  AWKCMD+='{ for(j=1;j<=i;j++) {p = a[j]/sum; entropy += -p\*log(p)} } END '
  AWKCMD+='{print entropy}'
  for (( i = 1; i < $2; i ++ )); do
    while IFS= read -r line; do 
      ENTROPY=`echo "$line" | rev | cut -f2- | rev | \
      grep -f- $1.ORDER$(($i+1)).PRE | awk "$AWKCMD"`
      if [ "x$ENTROPY" = "x" ]; then
        ENTROPY=0
      fi
      awk -v ent=$ENTROPY -v ln=$LINENUM \
      '{if (NR==ln) {print $0, ent} else {print $0}}' $3 > TEMP && mv TEMP $3
      LINENUM=$(($LINENUM+1))
    done < $1.ORDER$i.PRE
  done 
}
\# == Generate phoneme and grapheme suffix and prefix trees ==
echo 'Generating grapheme trees ...'
sed '/\;\;\;/d;s/  .\*//g;/[^A-Za-z]/d' $DICT | \
uniq | sed 's/./ &/g;s/^ //' > CH \# char-only
ngramFrequencyEntropy CH $ORDER ch.order$ORDER
rev CH > CH\_REV
ngramFrequencyEntropy CH\_REV $ORDER ch\_rev.order$ORDER
echo 'Generating phoneme trees ...'
sed '/\;\;\;/d;s/.\*  //g' $DICT > PH \# phone-only
ngramFrequencyEntropy PH $ORDER ph.order$ORDER
rev PH > PH\_REV
ngramFrequencyEntropy PH\_REV $ORDER ph\_rev.order$ORDER
\# == Remove temp files ==
rm CH CH_REV PH PH_REV \*ORDER\*
</pre>
Below is the code that generates possible candidates for prefixes and suffixes by selecting those that have both high frequencies and high entropies:
<pre>
\#!/bin/sh
\# -- generate the candidates using frequency and entropy --
\# == parameters ==
ORDER=7
FREQ\_CUTOFF=50
ENTR\_CUTOFF=50
topFreqEntr(){
  for (( i = 3; i < $2; i ++ )); do
    cat $1.order$i | sort -k$((i+1)) -r -n | head -$FREQ\_CUTOFF > topfreq
    cat $1.order$i | sort -k$((i+2)) -r -n | head -$ENTR\_CUTOFF > topentr
    sort -o topfreq topfreq
    sort -o topentr topentr
    comm -12 topfreq topentr | sed "s/^\t//g" \> $1.order$i.candidates
  done
}
topFreqEntr ch $ORDER
topFreqEntr ph $ORDER
topFreqEntr ch\_rev $ORDER
topFreqEntr ph\_rev $ORDER
\# == clear junk files == 
rm topfreq topentr
</pre>
Below is the code that prints the frequency and entropy path of candidates:
<pre>
\#!/bin/sh
\# -- print the path of the candidates --
\# == parameters ==
ORDER=7
printPath(){
  while read line; do
    NUM_COL=`expr $2 - 1`
    echo $line
    while [ $NUM_COL -ge 2 ]; do
      echo $line | cut -d' ' -f 1-$NUM\_COL | grep -f- $1.order$NUM\_COL
      NUM\_COL=`expr $NUM\_COL - 1`
    done
    echo ""
  done < $1.order$2.candidates
}
for (( i = 3; i < $ORDER; i++ )); do
  printPath ch $i > ch.order$i.candidates.path
  printPath ph $i > ph.order$i.candidates.path
  printPath ch\_rev $i > ch\_rev.order$i.candidates.path
  printPath ph\_rev $i > ph\_rev.order$i.candidates.path
done
</pre>
