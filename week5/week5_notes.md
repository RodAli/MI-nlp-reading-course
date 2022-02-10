Week 5 - NLP Reading Course Notes
================
2022-02-11

This Rmarkdown includes the notes and reading responses for my course
readings in **INF2010 - Reading Course: Natural Language Processing**.

This week’s readings include:

-   Hvitfeldt, Emil & Julia Silge, 2020, Supervised Machine Learning for
    Text Analysis in R, Chapters 4-6, <https://smltar.com>.
-   Jurafsky, Dan, and James H. Martin, 2020, Speech and Language
    Processing, 3rd ed., Chapters 4 and 5,
    <https://web.stanford.edu/~jurafsky/slp3/>.
-   Tatman, Rachel, 2020, ‘What I Won’t Build’, Widening NLP Workshop
    2020, Keynote address, 5 July,
    <https://slideslive.com/38929585/what-i-wont-build> and
    <http://www.rctatman.com/talks/what-i-wont-build>.
-   Liang Wu and Huan Liu. 2018. Tracing Fake-News Footprints:
    Characterizing Social Media Messages by How They Propagate. In
    Proceedings of the Eleventh ACM International Conference on Web
    Search and Data Mining (WSDM ’18). Association for Computing
    Machinery, New York, NY, USA, 637–645.
    <DOI:https://doi.org/10.1145/3159652.3159677>.

## Supervised Machine Learning for Text Analysis in R

### Chapter 4 - Stemming

**Stem**: a base word

Porter Stemming algorithm is one of the most widely used stemmers in
English.

Hunspell stemmer, is a dictionary-based stemmer that may return more
than one stem for each word!

Stemming reduces the *features space* of text data.

**Lemmatization** uses knowledge about a language’s structure to reduce
words down to their lemmas, the canonical or dictionary forms of words.
Uses more linguistic knowledge of a language than stemming.

### Chapter 5 - Word Embeddings

“Linguists have long worked on vector models for language that can
reduce the number of dimensions representing text data based on how
people use language; the quote that opened this chapter dates to 1957.
These kinds of dense word vectors are often called word embeddings.”

**Word embeddings** are a way to represent text data as vectors of
numbers based on a huge corpus of text, capturing semantic meaning from
word’s context.

Pre-trained word embeddings:

-   GloVe
-   word2vec
-   FastText embeddings

Fairness in word embeddings: (models learn, reproduce, and often amplify
whatever biases exist in training data), but this is literally,
concretely true of word embeddings.

### Chapter 6 - Regression

**Predictive model**: generate the most accurate predictions possible
**Inferential model**: test a hypothesis or draw conclusions about a
population **Descriptive model**: describe the properties of the
observed data

``` r
# Import libraries
library(tidyverse)
library(tidymodels)
library(textrecipes)
library(scotus)
library(LiblineaR)
```

``` r
# Split our scotus dataset into training and testing sets
set.seed(1234)
scotus_split <- scotus_filtered %>%
  mutate(year = as.numeric(year),
         text = str_remove_all(text, "'")) %>%
  initial_split()

scotus_train <- training(scotus_split)
scotus_test <- testing(scotus_split)

# Create recipe. Predict year from text data.
scotus_rec <- recipe(year ~ text, data = scotus_train) %>%
  step_tokenize(text) %>%                         # Tokenize text
  step_tokenfilter(text, max_tokens = 1e3) %>%    # Keep words > 1000 freq
  step_tfidf(text) %>%                            # Calc tf-idf
  step_normalize(all_predictors())                # Normalize tf-idf values

# Create SVM linear model
svm_spec <- svm_linear() %>%
  set_mode("regression") %>%
  set_engine("LiblineaR")

# Create workflow
scotus_wf <- workflow() %>%
  add_recipe(scotus_rec) %>% 
  add_model(svm_spec)

# Fit to train set
scotus_fit <- scotus_wf %>% 
  fit(data = scotus_train)

# View our term's bias
scotus_fit %>%
  pull_workflow_fit() %>%
  tidy() %>%
  arrange(-estimate)
```

K-fold validation can be done as follows:

``` r
set.seed(123)
scotus_folds <- vfold_cv(scotus_train)

svm_rs <- fit_resamples(
  scotus_wf,
  scotus_folds,
  control = control_resamples(save_pred = TRUE)
)
```

Performance metrics for regression models:

-   Root Mean Squared Error (RMSE)
-   R-squared

“RSME is a measure of the difference between the predicted and observed
values; if the model fits the data well, RMSE is lower. To compute RMSE,
you take the mean values of the squared difference between the predicted
and observed values, then take the square root.

R-squared is the squared correlation between the predicted and observed
values. When the model fits the data well, the predicted and observed
values are closer together with a higher correlation between them. The
correlation between two variables is bounded between −1 and 1, so the
closer R-squared is to one, the better.”

**Null model** simple, non-informative model that always predicts the
largest class (for classification) or the mean (such as the mean year of
Supreme Court opinions, in our specific regression case).

**Feature Hashing** is dimensionality reduction method with a simple
premise. We begin with a hashing function that we then apply to our
tokens,

“A **hashing function** takes input of variable size and maps it to
output of a fixed size. Hashing functions are commonly used in
cryptography.”

## Speech and Language Processing

### Chapter 4 - Naive Bayes and Sentiment Classification

**Generative classifiers** like naive Bayes build a model of how a class
could generate some input data. Given an observation, they return the
class most likely to have generated the observation

**Discriminative classifiers** like logistic regression instead learn
what features from the input are most useful to discriminate between the
different possible classes.

Naive Bayes classifiers make two simplifying assumptions:

1.  Bag of words assumption: we assume that position does not matter.
2.  Naive Bayes assumption: this is the conditional independence
    assumption that the probabilities are independent given the class
    and hence can be ‘naively’ multiplied.

Metrics for classification:

-   Accuracy
-   Precision
-   Recall
-   F1-score (harmonic mean of precision and recall)

**Macroaveraging** compute the performance for each class, then average
over the classes.

**Microaveraging** collect the decisions for all classes into a single
confusion matrix, and then compute precision and recall from that table.

**Bootstrapping** refers to repeatedly drawing large numbers of smaller
samples with replacement (called bootstrap samples) from an original
larger sample.

Statistical significance tests should be used to determine whether we
can be confident that one version of a classifier is better than
another.

### Chapter 5 - Logistic Regression

**Logistic Regression** can be used to classify an observation into one
of two classes, or into one of many classes (multinomial logistic
regresssion).

The **sigmoid function** takes a real value and maps it to the range
\[0,1\].

A vector where one value is 1 and the rest are 0, is a **one-hot
vector**.

## What I Won’t Build - Tatman, Rachel

#### Notes

No system is inevitable.

All language is shaped by its social context Many demographic factors
are linked to systematic variation in language. Ex: age, social-economic
status, race/ethnicity.

Questions to ask - who benefits from this system - can users choose not
to interact with this system - is it genuinely bettering the world?

## Tracing Fake-News Footprints: Characterizing Social Media Messages by How They Propagate – Wu and Liu.

#### Reading Response

In this article Wu and Liu, explore novel ways of detecting fake-news
and spam. Wu and Liu point out that previous research would “heavily
rely on content features, to tackle these challenges.” Wu and Liu
decided to shift the focus to look at the diffusion of information to
determine fake news. The diffusion of information is how a news article,
social post or a meme propagates through a network of users. Similar
users will tend to share information that caters to their beliefs and
values. And they would share this information with users that tend to
share those same values. By analyzing the trace of information, Wu and
Liu strive to find patterns in the way that fake-news articles are
shared.

Wu and Liu introduce TraceMiner that can classify messages within a
social diffusion network. They evaluate TraceMiner on real social
network data from Twitter to demonstrate its effectiveness. To determine
whether a tweet was fake or not, Wu and Liu gathered data from the
fact-checking website Snopes. Their experiment was to determine whether
TraceMiner would be able to accurately able to predict whether a tweet
was fake or not. They found that “TraceMiner outperforms all the
baselines and its variations” and that “Diffusion-based methods perform
better than content-based methods.”

This line of work was an interesting and novel approach at looking at
determining fake news. I was quite surprised that a model trained on
social diffusion features outperformed content-based models for
determining fake news. I believe this goes to show how to difficult it
is to determine fake news from the morphology of a message.

My question is what are these models “learning”, or how are they
determining whether a message is fake based off the diffusion trace? How
does this model perform on other forms of data – articles, blogs,
comments? Transparency of this model of this model will help determine
what the models strengths are, and what are some potential pitfalls.
