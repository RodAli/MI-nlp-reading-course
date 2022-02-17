Week 6 - NLP Reading Course Notes
================
2022-02-18

This Rmarkdown includes the notes and reading responses for my course
readings in **INF2010 - Reading Course: Natural Language Processing**.

This week’s readings include:

-   Hvitfeldt, Emil & Julia Silge, 2020, Supervised Machine Learning for
    Text Analysis in R, Chapters 7-9, <https://smltar.com>.
-   Jurafsky, Dan, and James H. Martin, 2020, Speech and Language
    Processing, 3rd ed., Chapters 6 and 7,
    <https://web.stanford.edu/~jurafsky/slp3/>.
-   Zhao, Jieyu, Tianlu Wang, Mark Yatskar, Vicente Ordonez and Kai-Wei
    Chang,2017, ‘Men Also Like Shopping: Reducing Gender Bias
    Amplification using Corpus-level Constraints’, Proceedings of the
    2017 Conference on Empirical Methods in Natural Language Processing,
    pp. 2979–2989, <https://aclweb.org/anthology/D17-1323.pdf>.
-   Wang WY, 2017, “Liar, liar pants on fire”: A new benchmark dataset
    for fake news detection, arXiv, <https://arxiv.org/abs/1705.00648>.

## Supervised Machine Learning for Text Analysis in R

### Chapter 7 - Classification

**recipes** package allows us to create specification of preprocessing
steps we want to perform.

Evaluation Metrics:

-   **accuracy**: is the proportion of the data that is predicted
    correctly. Can be misleading for imbalanced data sets.
-   **precision**
-   **recall**
-   **ROC-AUC**: measures how well a classifier performs at different
    thresholds. The ROC curve plots the true positive rate against the
    false positive rate. AUC closer to 1 indicates a better-performing
    model, while AUC closer to 0.5 indicates a model close to random
    guessing.

### Chapter 8 - Dense Neural Networks

**Deep learning models** are “deep” in the sense they use multiple
layers to learn how to map from input features to output outcomes.

Neural networks model sequences, so we have to choose the length of
sequences we would like to include. This sequence length is a
hyperparameter of the model.

Optimization algorithm: when a neural network passing a batch of data
through the network, it needs a way to use the difference between the
predicted values and true values to update the network’s weights.

Loss function: the quantity that we want to have minimized during the
training of a neural network

**One-hot encoding** is similar to dummy variables. Where you have a
vector such that only one value is 1 (true) and all others are 0
(false). Can be used to represent categorical variables, where each
value is a one-hot encoded column.

Limitations of Deep Learning

-   interpretability of the models is difficult, models are black box,
    hard to know how the model is working
-   do not faciliatate comprehensive theoretical understanding or
    learning of their inner organization
-   deep learning models tend to require more training data that
    traditional statistical machine learning models

### Chapter 9 - Long Short-Term Memory (LSTM)

An **LSTM** is a specific kind of network architecture with feedback
loops that allow information to persist through steps14 and memory cells
that can learn to “remember” and “forget” information through sequences.

Helpful resource:
<https://colah.github.io/posts/2015-08-Understanding-LSTMs/>

A **bidirectional LSTM** allows the network to have both the forward and
backward information about the sequences at each step.

#### Training a LSTM model

``` r
# Import libraries
library(scotus)
library(tidytext)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
    ## ✓ readr   2.1.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(tidymodels)
```

    ## Registered S3 method overwritten by 'tune':
    ##   method                   from   
    ##   required_pkgs.model_spec parsnip

    ## ── Attaching packages ────────────────────────────────────── tidymodels 0.1.4 ──

    ## ✓ broom        0.7.10     ✓ rsample      0.1.1 
    ## ✓ dials        0.1.0      ✓ tune         0.1.6 
    ## ✓ infer        1.0.0      ✓ workflows    0.2.4 
    ## ✓ modeldata    0.1.1      ✓ workflowsets 0.1.0 
    ## ✓ parsnip      0.1.7      ✓ yardstick    0.0.9 
    ## ✓ recipes      0.1.17

    ## ── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
    ## x scales::discard() masks purrr::discard()
    ## x dplyr::filter()   masks stats::filter()
    ## x recipes::fixed()  masks stringr::fixed()
    ## x dplyr::lag()      masks stats::lag()
    ## x yardstick::spec() masks readr::spec()
    ## x recipes::step()   masks stats::step()
    ## • Use suppressPackageStartupMessages() to eliminate package startup messages

``` r
library(textrecipes)
library(keras)
```

    ## 
    ## Attaching package: 'keras'

    ## The following object is masked from 'package:yardstick':
    ## 
    ##     get_weights

``` r
# Clean and split data into training and test
scotus_split <- scotus_filtered %>%
  mutate(
    year = (as.numeric(year) - 1920) / 50,
    text = str_remove_all(text, "'")
  ) %>%
  initial_split(strata = year)

scotus_train <- training(scotus_split)
scotus_test <- testing(scotus_split)

max_words <- 2e4
max_length <- 1e3

# Preprocess data: tokenize, filter, and oneshot encode
scotus_rec <- recipe(~ text, data = scotus_train) %>%
  step_tokenize(text) %>%
  step_tokenfilter(text, max_tokens = max_words) %>%
  step_sequence_onehot(text, sequence_length = max_length)

scotus_prep <- prep(scotus_rec)
scotus_train_baked <- bake(scotus_prep,
                           new_data = scotus_train,
                           composition = "matrix")
scotus_test_baked <- bake(scotus_prep,
                          new_data = scotus_test,
                          composition = "matrix")

# Use LSTM model
scotus_mod <- keras_model_sequential() %>%
  layer_embedding(input_dim = max_words + 1, output_dim = 64) %>%
  layer_lstm(units = 32, dropout = 0.4, recurrent_dropout = 0.4) %>%
  layer_dense(units = 1)

scotus_mod %>%
  compile(
    optimizer = "adam",
    loss = "mse",
    metrics = c("mean_squared_error")
  )

# Train the LSTM
scotus_history <- scotus_mod %>%
  fit(
    scotus_train_baked,
    scotus_train$year,
    epochs = 10,
    validation_split = 0.25,
    verbose = FALSE
  )

# Test our model
scotus_res <- tibble(year = scotus_test$year,
                     .pred = predict(scotus_mod, scotus_test_baked)[, 1]) %>%
  mutate(across(everything(), ~ . * 50 + 1920))

scotus_res %>% metrics(year, .pred)
```

## Speech and Language Processing

### Chapter 6 - Vector Semantics and Embeddings

**Vector Semantics** is the standard way to represent word meaning in
NLP.

Co-occurence matrices:

-   term-document matrix
-   term-term matrix

**Term-document matrix** each row represents a word in the vocabulary
and each column represent a document from some collection of documents.
We can think of a document as a point in \|V\|-dimensional space, where
\|V\| is the vocabulary size. From this term-document matrix we can
derive **document vectors** (using columns) and **word vectors** (using
rows).

**Term-term matrix**, also known as word-word matrix or term-context
matrix, which columns are labeled by words.

**Cosine similarity** is based on the *normalized dot product* (inner
product) of two vectors of same length.

cosine(v,w) = (v . w) / (\|v\|\*\|w\|)

**Term-frequency** is the frequency of word t in the document d. tf(t,
d) = count(t, d). Some times we may take the log to squash extreme
values, and add 1 to not multiply with 0. tf(t, d) = log(count(t, d) +
1).

**Document frequency** of a term t is the number of documents term t
occurs in.

**Inverse document frequency** is the inverse of document freqency. Log
is often used to squash large values.

**tf-idf** is the multiplication of term frequency with inverse document
frequency.

**Pointwise Mutual Information (PMI)** is a measure of how often two
events x and y occur, compared with what we would expect if they were
independent.

**Positive Pointwise Mutual Information (PPMI)** replaces all negative
values of PMI with 0 because it is not clear to measure ‘unrelatedness’.

**Centroid** of a set of vectors S is a single vector that has the
minimum sum of squared distances to each of the vectors in S.

**Embeddings** are short dense vectors. The number of dimensions range
from 50 - 100, rather than the large vocabulary size of \|V\|. These d
dimensions don’t have a clear interepretation. And these vectors are
dense, instead of being spares (mostly 0 counts).

**skip-gram with negative sampling (SGNS)** a method for computing
embeddings. A part of the package **word2vec**. and sometimes the
algorithm is loosely refered to as word2vec. The intuition of skip-gram
is:

1.  Treat the target word and a neighboring context word as positive
    examples.
2.  Randomly sample other words in the lexicon to get negative samples.
3.  Use logistic regression to train a classifier to distinguish those
    two cases.
4.  Use the learned weights as the embeddings.

**Fasttext** is an another static embedding extension of word2vec that
addresses the problem with unknown words - words that ppear in a test
corpus but were unseen in the training corpus. Link:
<https://fasttext.cc/>.

**GloVe** short for global vectors is another static embedding model
that captures corpus statistics.

Visualizing embeddings:

1.  Listing the most similar words to a word w in a list using cosine
    similarity
2.  Use a clustering algorithm to show a hierarchical tree representatio
    of the words

**Parallelogram model** is used for solving simple analogy problems of
the form a is to b as a\* is to \_\_?

#### Bias and Embeddings

**Allocational Harm** when a system allocates resources unfairly to
different groups.

It turns out that embeddings don’t just reflect the statistics of their
input, but also *amplify* bias.

**Representational Harm** is a harm caused by a system demeaning or even
ignoring some social groups.

### Chapter 7 - Neural Networks and Neural Language Models

The building block of a neural network is a **single computational
unit**.

Activation functions:

-   sigmoid function
-   tanh
-   rectified linear unit (ReLI)

**The XOR Problem** it is not possible to make a perceptron that
computes a logical XOR. XOR is not **linearly seperable**, therefore it
is not able to be modeled by a single perceptron.

**Feedforward Network** is a multilayer network in which the units are
connected with no cycles; the outputs from units in each layer are
passed to units in the next higher layer, and no outputs are passed back
to lower layers.

**Multi-layer perceptions (MLPs)** are another name for feedforward
networks that have more than 1 layer.

**Softmax** function is a convenient normalizing function of a vector of
real values to a probability distribution.

Advantages of Neural Language Models over n-gram models:

-   can handle much longer histories
-   can generalize better over contexts of similar words
-   more accurate with word prediction

Disadvantages of Neural Language Models over n-gram models:

-   more complex
-   slower and need more energy to train
-   less interpretable than n-gram models

**Forward inference** is the task, given an input, of running a forward
pass on the network to produce a probability distribution over possible
outputs.

**Error backpropagation**, backward differentiation on a computation
graph, is used to compute the gradients of the loss function for a
network.

## Men Also Like Shopping: Reducing Gender Bias Amplification using Corpus-level Constraints

### Reading Response

Zhao et al. analyze image datasets with multilabel label object
classification and visual semantic role labelling. What they notice in
these datasets are gender biases that associate men and women with
certain action verbs and objects. For example, they found that “cooking
is over 33% more likely to involve females than males.” Not only did
they find that training data was biased, but existing models that were
trained on this data only amplified the bias further. They found that “a
trained model further amplifies the disparity to 68% at test time.” They
measured bias by defining a bias score. This bias score is calculated by
finding the proportion of verbs with gendered words (man, woman)
relative to the total uses of that verb in the corpora. Using this
metric, they developed a Reducing Bias Amplification (RBA) model, which
decreases bias by 47.5% for multilabel classification and 40.5% for
visual semantic role labelling.

What was particularly interesting was the finding that models trained on
imbalanced data amplified imbalances. This means that NLP models will
not simply just replicate biases in our training data, but rather
arguably make it worse. This creates the question of whether these
models will create more harm than benefits. I believe this work took a
step in the right direction of trying to reduce these amplified biases
and making sure that our models are learning what we would like them to
learn.

## “Liar, Liar Pants on Fire”: A New Benchmark Dataset for Fake News Detection – Wang

### Reading Response

In this paper, Wang presents a new dataset – LIAR – to assist in the
development of models for fake news detection. The dataset was created
by collecting manually labelled short statements from the fact-checking
site politifact.com. The dataset consists of 12,836 short statements
that are labelled for “truthfulness, subject, context/venue, speaker,
state, party and prior history.” Truthfulness ratings range on the scale
of “pants-fire, barely-true, half-true, mostly-true and true” and
subjects range include “economy, healthcare, taxes, federal-budget,
education, jobs, state-budget, candidates-biography, elections and
immigration.”

This dataset has multiple strengths compared to datasets that have been
used in the past with regards to fake-news detection. One is that this
dataset covers multiple domains and topics. Because it is not limited to
one domain the dataset is less restricted to a particular topic. This
dataset also includes helpful metadata – speaker, state, and party –
that will assist models in determining fake news.
