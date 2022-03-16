## Speech and Language Processing

### Machine Translation and Encoder-Decoder Models

**Computer-aided translation** (CAT) - using machine translation systems to produce a draft translation that is revised in a post-editing phase by a human translator.

Universal aspects of language:

- nouns
- verbs
- ways to ask questions
- ways to issue commands
- linguistic systems to indicate agreement or disagreement

**Encoder-decoder** networks, or **sequence-to-sequence** networks, are models capable of generating contextually appropriate arbitrary length, output sequences. 

For Machine Translation, the training data typically consists of sets of sentences and their translation.

Translations are evaluated along two dimensions:

1. **Adequacy**: how well the translation captures the exact meaning of the source sentence. Sometimes called **faithfulness** or **fidelity**.
2. **Fluency**: how fluent the translation is in the target language (is it grammatical, clear, readable, natural).

**character F-score (chrF)** - used for automatic machine translation evaluation.

## The Illustrated Transformer

http://jalammar.github.io/illustrated-transformer/

By: Jay Alammar

Transformer is made up of an **encoding** and **decoding** component.

The encoding component is a stack of encoders and the decoder component a stack of decoders.

Each encoder has a self-attention layer and a feed forward neural network.

A decoder has the same layers as an encoder including attention layer that helps the decoder focus on relevant parts of the input sentence.


Steps in self-attention:

1. Create the embeddings of each word
2. Calculate a score that tells us how much focus to to place on other parts of the input sentence as we encode a word at a certain position
3. Divide the scores by 8 (the square root of the dimension of the key vectors used in the paper)
4. Pass into a softmax operation
5. Multiply each value vector by the softmax score
6. Sum up the weighted value vectors


## Stanford CS224N: NLP with Deep Learning - Transformers and Self-Attention

- Constant path length between any two positions
- Unbounded memory
- Trivial to parallelize (per layer)
- Models self-similarity
- Relative attention provides expressive timing, equivariance, and extends naturally to graphs


## Transformer: A Novel Neural Network Architecture for Language Understanding

By: Jakob Uszkoreit

> "Neural networks, in particular recurrent neural networks (RNNs), are now at the core of the leading approaches to language understanding tasks such as language modeling, machine translation and question answering."

Recurrent Neural Networks (RNNs) process language sequentially one word at a time. This also makes it more difficult to take advantage of Tensoir Processing Units (TPUs) and GPUs.

The transformer applies a self-attention mechanism which directly models relationships between all words in a sentence.


## AI Powered Misinformation and Manipulation at Scale

By: Nitech Dhanjani

GPT-3 is a model that is able to create syntetic text given an initial prompt text.

More powerful models have been created inspired by GPT-3 such as LaMDA and EleutherAI.

There will be an arms race of researchers trying to detect whether a piece of text was authored by a language model, and developers adapting language models to evade detection by those tools.

There is the risk of manipulators utilizing GPT-3 to target individuals, groups and promote campaigns on social media.

An impresivie feature of GPT-3 is to generate source code. This is because it was trained on the text of the internet and much that text was documentation of computer code.

