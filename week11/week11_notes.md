## How GPT3 Works - Visualizations and Animations

#### By: Jay Alammar

Link: https://jalammar.github.io/how-gpt3-works-visualizations-animations/

A trained language model generates text.

We can optionally pass in some text as input, which influences the output.

GPT3 contains 175 billion parameters.

High level steps:

1. Convert the word to a vector (list of numbers) representing the word
2. Compute prediction
3. Conver resulting vector to word


## Attention is all you need

#### By: Vaswani et al

Link: https://arxiv.org/pdf/1706.03762.pdf


## Open Sourcing BERT: State-of-the-Art Pre-training for Natural Language Processing

#### By: Jacob Devlin and Ming-Wei Change

Link: https://ai.googleblog.com/2018/11/open-sourcing-bert-state-of-art-pre.html

One of the biggest challenges in NLP is the shortage of training data.

Researchers have therefore developed a variety of techniques for training general purpose language representation models using unannotated text on the web.

**Bidirectional Encoder Representations from Transformers** (BERT): deeply bidirectional, unsupervised language representation, pre-trained using only a plain text corpus.

Pre-trained representations can be:

1. Context-free
2. Contextual
	- Unidirectional
	- bidirectional

Context-free models include *word2vec*, *GloVe* generate a single word embedding represntation for each word in the vocabulary.

Context models will represent words based on the context words around it. Bidirectional will represent words by the word that come before AND after.

## BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding

#### By: Devlin et al.

Link: https://arxiv.org/abs/1810.04805

Related work on pre-training general language representations:

1. Unsupervised feature-based approaches
	- ELMo
2. Unsupervised fine-tuning approaches
	- OpenAI GPT
3. Transfer learning from supervised data


Steps in BERT implementation:

1. Pre-training
	- the model is trained on unlabeled data over different pre-training tasks
2. Fine-tuning
	- BERT model is first initialized with the pre-trained parameters, and all of the parameters are fine-tuned using labeled data


## The Annotated Transformer

Link: https://nlp.seas.harvard.edu/2018/04/03/attention.html

"An attention function can be described as mapping a query and a set of key-value pairs to an output, where the query, keys, values, and output are all vectors."


