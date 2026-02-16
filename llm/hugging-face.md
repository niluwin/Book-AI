# Hugging Face

models

datasets

spaces



### Libraries

1. hub
2. datasets
3. transformers
4. peft
5. trl
6. accelerate

### Tokenizer

encode

decode

batch\_decode

get\_added\_vocab

vocab

apply chat template

### Quantization

BitsAndBytesConfig

### Model Internals

embessing: dimensionality,

1. llama: rotary embedding

layers:

1. decoder layers
   1. attention layer: query, key, value and output for self attention layer
   2. multi layer parceptron (mlp) layer, activation layer

lm\_head: fully connected layer, classification layer gives probability of next token out of vocab



### Streaming

### Comparing between models

<figure><img src="../.gitbook/assets/image (165).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (164).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (166).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (167).png" alt=""><figcaption></figcaption></figure>

#### Chinchilla scaling law

### Benchmarks

<figure><img src="../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>

### &#x20;Leaderboard

Artificial Analysis

Vellum&#x20;

scale.com

hugging face

live bench

### LM Arena

