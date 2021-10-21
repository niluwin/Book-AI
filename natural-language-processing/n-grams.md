# N-Grams

## verView

A language model is a tool that's calculates the probabilities of sentences. Language models can also estimate the probability of an upcoming word given a history of previous words.

N-grams are fundamental and give you a foundation that will allow you to understand more complicated NLP models. These models allow you to calculate probabilities of certain words happening in a specific sequence. Using that, you can build an auto-correct or even a search suggestion tool.&#x20;

Other applications of N-gram language modeling include:

![](<../.gitbook/assets/image (70).png>)

## N-Grams

![](<../.gitbook/assets/image (72).png>)

Now given the those definitions, we can label a sentence as follows:

![](<../.gitbook/assets/image (71).png>)



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


![](<../.gitbook/assets/image (74).png>)

### Trigram Probability

​To compute the probability of a trigram:

* $$P\left(w_{3} \mid w_{1}^{2}\right)=\frac{C\left(w_{1}^{2} w_{3}\right)}{C\left(w_{1}^{2}\right)}$$​
* $$C\left(w_{1}^{2} w_{3}\right)=C\left(w_{1} w_{2} w_{3}\right)=C\left(w_{1}^{3}\right)$$​

### N-Gram Probability

* $$P\left(w_{N} \mid w_{1}^{N-1}\right)=\frac{C\left(w_{1}^{N-1} w_{N}\right)}{C\left(w_{1}^{N-1}\right)}$$​
* $$C\left(w_{1}^{N-1} w_{N}\right)=C\left(w_{1}^{N}\right)$$​

\


\


\
