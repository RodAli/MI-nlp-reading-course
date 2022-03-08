## Speech and Language Processing

### Chapter 8 - Sequence Labeling for Parts of Speech and Named Entities

**Parts of Speech (POS)**: are useful clues into sentence structure and meaning

**Adjective**: noun modifiers describing properties

**Adverb**: verb modifiers of time, place, manner

**Noun**: words for persons, places, things, etc.

**Verb**: words for actions and processes

**Proper Noun**: name of a person, organization, place, etc.

**Interjection**: exclamation, greeeting, yes/no response, etc.

**Adposition (preposition/postposition)**: marks a noun's spacial, temporal, or other relation

**Auxiliary**: helping verb marking tense, aspect, mood, etc.

**Coordinating conjunction**: joins two phrase/clauses

**Determiner**: marks noun phrase properties

**Numeral**: represents a number, quantity, measurement, etc.

**Particle**: a preposition-like form used together with a verb

**Pronoun**: a shorthand for referring to an entity or event

**Subordinating Conjunction**: joins a main clause with a subordinate clause such as a sentential complement

**Punctuation**

**Symbols**

**Other**


#### Hidden Markov Models (HMMs)

**Markov chain** is a model that tells us something about the probabilities of sequences of random variables, state, each of which can take on values from some set.

**Markov Assumption**: that when predicting the future, the past doesn't matter, only the present. P(qi = a | q1 ... qi-1) = P(qi = a | qi-1)

**Hidden Markov Model (HMM)** allows us to talk about both *observed* events (like words that we see in the input) and *hidden* events (like part-of-speech tags) that we think of as causal factos in our probabilistic model.

### Chapter 9 - Deep Learning Architectures for Sequence Processing

Language is inherently a temporal phenomenon. Spoken language is a sequence of acoustic events over time, and we comprehend and produce both spoken and written language as a continous input stream.

A **Recurrent Neural Network (RNN)** is any network that contains a cycle within its network connections, meaning that the value of some unit is directly, or indirectly, dependent on its own earlier outputs as an input.

**Long short-term Memory (LSTM)** is the most commonly used extension to RNNs. They divide the context management problem into two sub-problems: removing information no longer needed from the context, and adding infromation likely to be needed for later decision making.

**Transformers** is an approach to sequence processing that eliminates recurrent connections and returns to architectures reminiscent of the fully connected networks.

## What Can We Do to Improve Peer Review in NLP? – Rogers and Augenstein

In What Can We Do to Improve Peer Review in NLP?, Rogers and Augenstein point out some of the problems of peer review in the field of NLP. They state that peer review does not guarantee quality control and it fails to identify high-impact papers. Furthermore, scientific merit has no correct answer in the peer-review process. Because the peer review process is slow, time-consuming and cognitively intense, reviewers tend to rely on heuristics which introduces biases. Among these biases, Rogers and Augenstein identify writing style, results not surpassing state of the art, a topic not being “trendy”, work not on English, and work from well-known labs.

Rogers and Augenstein recommend several strategies to improve the peer-review process. One of which is better review matching for different types of papers. Having reviewers with expertise in a particular discipline can properly assess the quality of papers being submitted. Another is stating the primary focus and editorial priorities pre-submission. This would help authors and reviewers be transparent on the expectations and goals of a conference. 




