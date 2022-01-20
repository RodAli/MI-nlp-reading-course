Week 2 - NLP Reading Course Notes
================
2022-01-21

This Rmarkdown includes the notes and reading responses for my course
readings in **INF2010 - Reading Course: Natural Language Processing**.

This week’s readings include:

-   Silge, Julia & David Robinson, 2020, Text Mining with R, Chapters
    5-7: <https://www.tidytextmining.com>.
-   C. D. Manning and H. Schütze, Foundations of statistical natural
    language processing. Cambridge, Mass: MIT Press, 1999. Chapters 16.
-   Bolukbasi, Tolga, Kai-Wei Chang, James Y. Zou, Venkatesh Saligrama
    and Adam T. Kalai, 2016, ‘Man is to Computer Programmer as Woman is
    to Homemaker? Debiasing Word Embeddings’, Advances in Neural
    Information Processing Systems, 29 (NIPS 2016),
    <http://papers.nips.cc/paper/6228-man-is-to-computer-programmer-as-woman-is-to-homemaker-d>.
-   Chang, Kai-Wei, Vinod Prabhakaran, and Vicente Ordonez, 2019, ‘Bias
    and Fairness in Natural Language Processing’, Proceedings of the
    2019 Conference on Empirical Methods in Natural Language Processing
    and the 9th International Joint Conference on Natural Language
    Processing (EMNLP-IJCNLP): Tutorial Abstracts,
    <https://aclweb.org/anthology/D19-2004/>.

``` r
# Import libraries
library(tm)
library(topicmodels)
library(tidyverse)
library(tidytext)
library(gutenbergr)
library(stringr)
```

## Text Mining with R

### Chapter 5 - Converting to and from non-tidy formats

**Document-Term Matrix (DTM)** is a matrix where:

-   each row represents one document

-   each column represents one term

-   each value contains the number of appearances of that term in that
    document

`tidy()` turns a document-term matrix into a tidy data frame.

`cast()` turns a tidy one-term-per-row data frame into a document-term
matrix.

``` r
# Import a document-term matrix
data("AssociatedPress", package = "topicmodels")
AssociatedPress
```

    ## <<DocumentTermMatrix (documents: 2246, terms: 10473)>>
    ## Non-/sparse entries: 302031/23220327
    ## Sparsity           : 99%
    ## Maximal term length: 18
    ## Weighting          : term frequency (tf)

``` r
# Convert our document-term matrix into tidy format
ap_tidy <- tidy(AssociatedPress)
ap_tidy
```

    ## # A tibble: 302,031 × 3
    ##    document term       count
    ##       <int> <chr>      <dbl>
    ##  1        1 adding         1
    ##  2        1 adult          2
    ##  3        1 ago            1
    ##  4        1 alcohol        1
    ##  5        1 allegedly      1
    ##  6        1 allen          1
    ##  7        1 apparently     2
    ##  8        1 appeared       1
    ##  9        1 arrested       1
    ## 10        1 assault        1
    ## # … with 302,021 more rows

``` r
# We can convert our tidy format back into a DTM format
ap_tidy %>% 
  cast_dtm(document, term, count)
```

    ## <<DocumentTermMatrix (documents: 2246, terms: 10473)>>
    ## Non-/sparse entries: 302031/23220327
    ## Sparsity           : 99%
    ## Maximal term length: 18
    ## Weighting          : term frequency (tf)

### Chapter 6 - Topic Modeling

**Latent Dirichlet Allocation** is an algorithm for topic modeling. We
can use it using the function `LDA()` function. It follows two
principles:

-   Every document is a mixture of topics
-   Every topic is a mixture of words

``` r
# Run LDA with 2 topics, on the Associate Press dataset
data("AssociatedPress")
ap_lda <- LDA(AssociatedPress, k = 2, control = list(seed = 1234))
ap_lda
```

    ## A LDA_VEM topic model with 2 topics.

**Beta** tells us the per-topic-per-word probabilities.

``` r
# Tidy our LDA results using beta
ap_topics <- tidy(ap_lda, matrix = "beta")
ap_topics
```

    ## # A tibble: 20,946 × 3
    ##    topic term           beta
    ##    <int> <chr>         <dbl>
    ##  1     1 aaron      1.69e-12
    ##  2     2 aaron      3.90e- 5
    ##  3     1 abandon    2.65e- 5
    ##  4     2 abandon    3.99e- 5
    ##  5     1 abandoned  1.39e- 4
    ##  6     2 abandoned  5.88e- 5
    ##  7     1 abandoning 2.45e-33
    ##  8     2 abandoning 2.34e- 5
    ##  9     1 abbott     2.13e- 6
    ## 10     2 abbott     2.97e- 5
    ## # … with 20,936 more rows

**Gamma** tells us the per-document-per-topic probabilities.

``` r
# Tidy our LDA results using gamma
ap_documents <- tidy(ap_lda, matrix = "gamma")
ap_documents
```

    ## # A tibble: 4,492 × 3
    ##    document topic    gamma
    ##       <int> <int>    <dbl>
    ##  1        1     1 0.248   
    ##  2        2     1 0.362   
    ##  3        3     1 0.527   
    ##  4        4     1 0.357   
    ##  5        5     1 0.181   
    ##  6        6     1 0.000588
    ##  7        7     1 0.773   
    ##  8        8     1 0.00445 
    ##  9        9     1 0.967   
    ## 10       10     1 0.147   
    ## # … with 4,482 more rows

### The Great Library Heist Example

``` r
# The books we will be using
titles <- c("Twenty Thousand Leagues under the Sea", 
            "Pride and Prejudice", 
            "Great Expectations")

# Get our book data
books <- gutenberg_works(title %in% titles) %>% 
  gutenberg_download(meta_fields = "title", mirror = "http://mirrors.xmission.com/gutenberg/")
books
```

    ## # A tibble: 46,731 × 3
    ##    gutenberg_id text                                     title                  
    ##           <int> <chr>                                    <chr>                  
    ##  1          164 "This etext was done by a number of ano… Twenty Thousand League…
    ##  2          164 "Gutenberg Project, to whom we owe a gr… Twenty Thousand League…
    ##  3          164 "whom we dedicate this book."            Twenty Thousand League…
    ##  4          164 ""                                       Twenty Thousand League…
    ##  5          164 ""                                       Twenty Thousand League…
    ##  6          164 ""                                       Twenty Thousand League…
    ##  7          164 ""                                       Twenty Thousand League…
    ##  8          164 ""                                       Twenty Thousand League…
    ##  9          164 ""                                       Twenty Thousand League…
    ## 10          164 "TWENTY THOUSAND LEAGUES UNDER THE SEA"  Twenty Thousand League…
    ## # … with 46,721 more rows

``` r
# Create document column that includes 'title of book' and 'chapter'
books_by_chapter <- books %>% 
  group_by(title) %>% 
  mutate(chapter = cumsum(str_detect(text, regex("^chapter ", ignore_case = TRUE)))) %>% 
  ungroup() %>% 
  filter(chapter > 0) %>% 
  unite(document, title, chapter)

# Split into tokens and count words
books_word_counts <- books_by_chapter %>% 
  unnest_tokens(word, text) %>% 
  anti_join(stop_words, by="word") %>% 
  count(document, word, sort = TRUE) %>% 
  ungroup()
books_word_counts
```

    ## # A tibble: 90,252 × 3
    ##    document              word        n
    ##    <chr>                 <chr>   <int>
    ##  1 Great Expectations_57 joe        88
    ##  2 Great Expectations_7  joe        70
    ##  3 Great Expectations_17 biddy      63
    ##  4 Great Expectations_27 joe        58
    ##  5 Great Expectations_38 estella    58
    ##  6 Great Expectations_2  joe        56
    ##  7 Great Expectations_23 pocket     53
    ##  8 Great Expectations_15 joe        50
    ##  9 Great Expectations_18 joe        50
    ## 10 Great Expectations_9  joe        44
    ## # … with 90,242 more rows

``` r
# Create a document term matrix (each chapter is a document)
chapters_dtm <- books_word_counts %>% 
  cast_dtm(document, word, n)

# Run LDA topic modeling on our document term matrix
chapters_lda <- LDA(chapters_dtm, k = 3, control = list(seed = 1234))

# Get our topic-per-word beta values
chapters_topics <- tidy(chapters_lda, matrix = "beta")

# Lets look at the top 5 beta per topic
top_beta <- chapters_topics %>% 
  group_by(topic) %>% 
  slice_max(beta, n = 5) %>% 
  ungroup() %>% 
  arrange(topic, -beta)
top_beta
```

    ## # A tibble: 15 × 3
    ##    topic term         beta
    ##    <int> <chr>       <dbl>
    ##  1     1 joe       0.0124 
    ##  2     1 miss      0.00656
    ##  3     1 time      0.00649
    ##  4     1 pip       0.00585
    ##  5     1 looked    0.00580
    ##  6     2 captain   0.0153 
    ##  7     2 nautilus  0.0131 
    ##  8     2 sea       0.00880
    ##  9     2 nemo      0.00873
    ## 10     2 ned       0.00805
    ## 11     3 elizabeth 0.0154 
    ## 12     3 darcy     0.00964
    ## 13     3 miss      0.00779
    ## 14     3 bennet    0.00763
    ## 15     3 jane      0.00714

``` r
# Lets visualize the top 5 beta per topic
top_beta %>% 
  mutate(term = reorder_within(term, beta, topic)) %>% # Order within each facet
  ggplot(aes(beta, term, fill = factor(topic))) +
  geom_col(show.legend = FALSE) +
  facet_wrap(~ topic, scales = "free") +
  scale_y_reordered()
```

![](week2_notes_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

We can see that these words are clustered roughly to our books.

Topic (1) is most likely *Great Expectations*, with the words “joe”,
“miss” and “pip”.

Topic (2) is most likely *Twenty Thousand Leagues Under the Sea*, with
the words “sea”, “captain” and “nautilus”.

Topic (3) is most likely *Pride and Prejudice*, with the words
“elizabeth”, “darcy” and “miss”.

``` r
# Get our per-document-per-topic gamma probabilities
# and separate document column back into title and chapter columns
chapters_gamma <- tidy(chapters_lda, matrix = "gamma") %>% 
  separate(document, c("title", "chapter"), sep = "_", convert = TRUE) %>% 
  arrange(title, chapter, topic)
chapters_gamma
```

    ## # A tibble: 498 × 4
    ##    title              chapter topic     gamma
    ##    <chr>                <int> <int>     <dbl>
    ##  1 Great Expectations       1     1 1.00     
    ##  2 Great Expectations       1     2 0.0000522
    ##  3 Great Expectations       1     3 0.0000522
    ##  4 Great Expectations       2     1 1.00     
    ##  5 Great Expectations       2     2 0.0000272
    ##  6 Great Expectations       2     3 0.0000272
    ##  7 Great Expectations       3     1 1.00     
    ##  8 Great Expectations       3     2 0.0000512
    ##  9 Great Expectations       3     3 0.0000512
    ## 10 Great Expectations       4     1 1.00     
    ## # … with 488 more rows

``` r
# Visualize our gamma values per book
chapters_gamma %>% 
  mutate(title = reorder(title, gamma * topic)) %>% 
  ggplot(aes(factor(topic), gamma)) +
  geom_boxplot() +
  facet_wrap(~ title) +
  labs(x = "topic", y = expression(gamma))
```

![](week2_notes_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

We can see above that our gamma values fairly consistent for determining
which document belongs to what book. Again, Topic (1) is most likely
*Great Expectations*, Topic (2) *Twenty Thousand Leagues under the Sea*
and Topic (3) is *Pride and Prejudice*.

## Foundations of Statistical Natural Language Processing. Chapter 16 - Text Categorization

Classification or categorization is the task of assigning objects from a
universe to two or more classes or categories.

The goal in text categorization is to classify the topic or theme of a
document.

#### Decision Trees

Decision trees are usually built by first growing a large and then
**pruning** it back to a reasonable size.

The pruning step is necessary because very large trees **overfit** the
training set.

**Overfitting** occurs when classifiers make decision based on
accidental properties of the training set.

One splitting criterion that is used in decision trees is **Information
Gain**. Information Gain is defined as the difference of the entropy of
the mother node and the weighted sum of the entropies of the child
nodes. Information gain is intuitively appealing because it can be
interpreted as measuring the **reduction of uncertainty**. If we make
the split that maximizes information gain we are reducing uncertainty.

The greatest advantage to decision trees is that they are transparent.
This is helpful in explaining the model to researchers and those who are
interested in the inner workings of the model.

#### Perceptrons

*Gradient Descent* is a class of iterative learning algorithms. In
gradient descent, we attempt to optimize a criterion function, such as
squared error or likelihood. In each step, we computer the derivative of
the function and change the parameters of the model in the direction of
the steepest gradient.

Our algorithm may converge to a *local optimum*, which is a location in
the weight space that are locally optimal, but inferior to the *global
optimum*.

If the perceptron learning algorithm is run on a problem that is not
linearly separable, the linear separator will sometimes move back and
forth erratically while the algorithm tries in vain to find a perfectly
separating plane.

Perceptrons have not been used much in be NLP cause most problems are
not linearly separable and the perceptron NLP learning algorithm does
not find a good approximate separator in such cases.

#### K-Nearest Neighbor Classification

To classify a new object, the intuition is to look at the k most similar
objects, find the majority class from the k objects and make that class
to be our prediction. The complexity of KNN is finding a good measure of
similarity.

## Man is to Computer Programmer as Woman is to Homemaker? Debiasing Word Embeddings

### Reading Response

Bolukbasi et al.’s Man is to Computer Programmer as Woman is to
Homemaker? Debiasing Word Embeddings was a very interesting read and
raises many ethical issues related to Natural Language Processing (NLP).
Bolukbasi et al. found that NLP algorithms were learning sexist biases
from the language they were trained on. This creates many implications
on whether deployed NLP applications are further reinforcing and
propagating gender stereotypes and biases into our culture.

Word embeddings is a common representation of words used in many NLP
tasks. A Word embedding represents a word as a n-dimensional vector to
approximate semantic meaning. Representing words as vectors provides
many useful advantages, one of which is being able to manipulate word
embeddings using vector operations. Using vector subtraction, you can
create word analogies, such as “man is to woman as king is to X” can be
represented by the following operation: <man> - <woman> = <king> - <X>,
where X = “queen”. Bolukbasi and al. looked at word embeddings in
word2vec, a popular and publicly available embedding application trained
on the Google News corpus. They found that these word embeddings contain
sexist and offensive representations, for example <he> - <doctor> =
<she> - <nurse>. And furthermore, there was a lack of research in
recognizing and addressing these biases in word embeddings.

The approach Bolukbasi et al. take to solve this issue is by developing
a debiasing algorithm that manipulates “gender-neutral” words to have
equal distance to “gendered” word pairs, such as man-woman. They do this
by identifying a gender subspace and by equalizing gender neutral words
to zero, or by “softening” their gender bias. To determine gendered
words and biased embeddings they employed U.S. based Amazon Mechanical
Turk crowd-workers to assess these biases. Bolukbasi et al. conclude
that their debiasing algorithm is effective in reducing gender biases in
word embeddings. Before applying their debiasing algorithm they found
analogies such as <he> - <doctor> = <she> - <X>, where X = nurse, and
after applying their debiasing algorithm they instead found more
acceptable words such as X = physician.

I think this study was an interesting first step towards creating NLP
algorithms that are more equitable and just. Bolukbasi et al. make a
strong argument that although human language contains gender biases,
leaving word embeddings biased runs the risk of further propagating
gender stereotypes and biases. They suggest that “in a small way
debiased word embeddings can hopefully contribute to reducing gender
bias in society.”

Further research can look more clearly at what determines whether a bias
is unacceptable or not? Additionally, who determines what biases are
unacceptable or harmful? This line of research is drawn predominately
from a population of U.S. crowd-workers. The debiased word embeddings
may be effective in reducing gender bias in U.S. language but how well
do these debiased word embeddings scale to other nations, cultures, and
backgrounds? Further line of work should generate biases that exist from
a wider range of nationalities, backgrounds, and ethnicities if it wants
to properly address issues of diversity and representation.

## Bias and Fairness in Natural Language Processing Presentation Notes

Link:
<http://web.cs.ucla.edu/~kwchang/documents/slides/emnlp19-fairNLP-part1.pdf>

Types of biases:

-   Reporting Bias
-   Selection Bias
-   Overgeneralization
-   Sampling error
-   Correspondence bias
-   In-group bias

“Human data perpetuates human biases. As ML learns from human data, the
result is a bias network effect. Bias Laundering.”

Is all bias good, bad, neutral?

-   Bias in statistics and machine learning
-   Cognitive biases
-   Algorithmic bias

“Although neural networks might be said to write their own programs,
they do so towards goals set by humans, using data collected for human
purposes. If the data is skewed, even by accident, the computers will
amplify injustice.” — The Guardian

“Word embeddings get things normatively wrong precisely because they get
things descriptively right!”

Allocative Harm – when a system allocates or withholds a certain
opportunity or resource

Associative Harm – when systems reinforce the subordination of some
groups along the lines of identity

Case where false positive is better than the false negative - Privacy in
images

-   False positive: when an image that doesn’t need to be blurred get
    blurred
-   False negative: something that needs to be blurred is not blurred

Case where false negative is better than the false positive - Spam
filtering

-   False negative: email that is SPAM is not caught, so you see it in
    your inbox
-   False positive: valid email being marked as SPAM and is removed from
    your inbox

Concluding points:

-   Be mindful of various sorts of biases in the NLP models and data
-   Explore “debiasing” technique, but be cautious
-   Identify the biases that matter for your problem and test for those
    biases
-   Consider this an iterative process, rather than something with a
    “done” state
-   Be transparent about your model and its performance
