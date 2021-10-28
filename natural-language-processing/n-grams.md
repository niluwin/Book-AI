# N-Grams

## Overview

A language model is a tool that's calculates the probabilities of sentences. Language models can also estimate the probability of an upcoming word given a history of previous words.

N-grams are fundamental and give you a foundation that will allow you to understand more complicated NLP models. These models allow you to calculate probabilities of certain words happening in a specific sequence. Using that, you can build an auto-correct or even a search suggestion tool.&#x20;

Other applications of N-gram language modeling include:

![](<../.gitbook/assets/image (70) (1).png>)

## N-Grams

![](<../.gitbook/assets/image (72) (1).png>)

Now given the those definitions, we can label a sentence as follows:

![](<../.gitbook/assets/image (71) (1) (1).png>)



In other notation you can write:&#x20;

* $$w_1^m = w_1 w_2 w_3 .... w_m$$​
* $$w  ^1_ 3 ​  =w _1 ​  w _2 ​  w_ 3 ​$$​
* $$w_{m−2}^m ​  =w _{m−2} w _{m−1} ​w_m$$

## ​N-Gram probabilities

Given the following corpus:_ <mark style="color:blue;">I am happy because I am learning.</mark>_

### Unigram Probability_ _

* Size of corpus <mark style="color:blue;">m = 7</mark>.
* ​$$P(I)= 2/7$$
* P(happy) = $$1/7$$​

To generalize, the probability of a unigram is $$P(w) = \frac{C(w)}{m}$$

### Bigram Probability

\


![](<../.gitbook/assets/image (74) (1) (1).png>)

### Trigram Probability

​To compute the probability of a trigram:

* $$P\left(w_{3} \mid w_{1}^{2}\right)=\frac{C\left(w_{1}^{2} w_{3}\right)}{C\left(w_{1}^{2}\right)}$$​
* $$C\left(w_{1}^{2} w_{3}\right)=C\left(w_{1} w_{2} w_{3}\right)=C\left(w_{1}^{3}\right)$$​

### N-Gram Probability

* $$P\left(w_{N} \mid w_{1}^{N-1}\right)=\frac{C\left(w_{1}^{N-1} w_{N}\right)}{C\left(w_{1}^{N-1}\right)}$$​
* $$C\left(w_{1}^{N-1} w_{N}\right)=C\left(w_{1}^{N}\right)$$​

## Sequence Probabilities

To compute the probability of a sentence: _The teacher drinks tea, _We will make use of the following:

* $$P(B \mid A)=\frac{P(A, B)}{P(A)} \Longrightarrow P(A, B)=P(A) P(B \mid A)$$
* $$P(A, B, C, D)=P(A) P(B \mid A) P(C \mid A, B) P(D \mid A, B, C)$$​

To compute the probability of a sequence, you can compute the following:

$$P( the teacher drinks tea )= P(the)P( teacher|the)P( drinks | the teacher)P(tea∣the teacher drinks )$$

One of the main issues with computing the probabilities above is the corpus rarely contains the exact same phrases as the ones you computed your probabilities on. Hence, you can easily end up getting a probability of 0. The _Markov assumption _indicates that only the last word matters. Hence:

* $$\text { Bigram } \quad P\left(w_{n} \mid w_{1}^{n-1}\right) \approx P\left(w_{n} \mid w_{n-1}\right)$$​
* $$\text { N-gram } \quad P\left(w_{n} \mid w_{1}^{n-1}\right) \approx P\left(w_{n} \mid w_{n-N+1}^{n-1}\right)$$​

You can model the entire sentence as follows:

* $$P\left(w_{1}^{n}\right) \approx \prod_{i=1}^{n} P\left(w_{i} \mid w_{i-1}\right)$$
* $$P\left(w_{1}^{n}\right) \approx P\left(w_{1}\right) P\left(w_{2} \mid w_{1}\right) \ldots P\left(w_{n} \mid w_{n-1}\right)$$​

### Starting and Ending Sentences

The N-gram Language Model\



### &#x20;

\
