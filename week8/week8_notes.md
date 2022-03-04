## Deep Learning with R

### Chapter 4 - Fundamentals of Machine Learning

Machine Learning has four broad categories:

1. **Supervised learning** - learning to map input data to known targets, given a set of examples. Examples include: classification and regression.
2. **Unsupervised learning** - finding interesting transformations of the input data without the help of any targets. Examples include: dimensionality reduction and clustering.
3. **Self-supervised learning** - is supervised learning without human - annotated labels - you can think of it as supervised learning without any humans in the loop. Examples: autoencoders and temporally supervised learning.
4. **Reinforcement learning** - an agent receives information about its environment and learns to choose actions that will maximize some reward.

*Why do we split data into training, validation and test set?*

After a few epochs, models will begin to overfit to the data. The goal is to achieve models that genaralize - that perform well on never before seen data - and overfitting is the central obstacle.

- Training data - used to train the model
- Validation data - used to evaluate your model
- Testing data - used to test your model a final time

**K-fold validation**
Split your data into K partitions of equal size. For each partition i, train the model on the remaining K-1 partitions, and evaluate the model on partition i. Your final score will be the averages of the K scores obtained.

**Iterated K-fold validation with shuffling**
Apply K-fold validation P times, shuffling the data every time bfore splitting it K ways. The final score is the average of the cores obtained at each run of K-fold validation. You end up training and evaluating P\*K models.

Data preprocessing for neural networks:

1. **Vectorization** - transforming input data into tensors of floating-point data.
2. **Value normalization**
3. **Handling missing values**
4. **Feature engineering** - is the process of using your own *knowledge* about the data and the model to make the algorithm work better by applying transformations to the data.

Example of feature engineering for reading the time of a clock:
- raw data, pixel grid of clock
- clock hands coordinates (x1,y1) and (x2,y2)
- angles of clock hands (45 degrees, 0 degrees)

The fundamental issue in machine learning is the tension between optimization and generalization.

**Optimization** refers to the process of adjusting a model to get the best performance possible on the training data.

**Generalization** referes to how well the trained model performs on data it has never seen before.

How to prevent overfittin:

1. Reducing the learnable parameters of the model - which is determined by the number of layers and the number of units per layer.
2. Adding weight regularization
3. Adding **dropout**

**Dropout**, applied to a layer, consists of randomly dropping out (setting to 0) a number of output features of the layer during training. The **dropout rate** is the fraction of the features that are zeroes out; it's usually between 0.2 and 0.5.

**Occam's razor** - given two explanations for something, the explanation that most likely to be correct is the simplest one - the one that makes fewer assumptions.

Universal Workflow of machine learning:

1. Defining the problem and assembling a dataset
2. Choosing a measure of success - accuracy, precision, recall, customer-retention rate?
3. Deciding on an evaluation protocol - maintaining a hold-out validation set, doing k-fold cross-validation and iterated k-fold validation
4. Preparing your data
5. Developing a model that does better than a baseline
6. Scaling up: developing a model that overfits
7. Regularizing your model and tuning your hyperparameters


## Chapter 6 - Deep learning for text and sequences

The two fundamental deep-learning algorithms for sequence processing are **recurrent neural networks** and **1D convnets**.

**Tokens**, the different units into which you can break down text (words, characters, or n-grams) are called tokens, and breaking text into such tokens is called **tokenization**.

**One-hot encoding** consists of associating a unique integer index with every word and then turning this integer index i into a binary vector of size N (this size of the vocabulary); the vector is all zeros except for the ith entry, which is 1.

**One-hot hashing** instead of assigning an index to each word and keeping a reference of these indices in a dictionary, you can hash words into vectors of fixed size.

**Word embeddings** or word vectors, are low-dimensional floating-point vectors that are dense as opposed to sparse. 

**Feedforward networks** are densely connected networks that have no memory. Each input shown to them is process independently, with no state kept in between inputs. 

**Recurrent Neural Network** (RNN) maintains a state containing information relative to what it has seen so far. An RNN is a type of neural network that has an internal loop. 


## Climbing towards NLU: On Meaning, Form, and Understanding in the Age of Data – Bender and Koller

Bender and Koller, in Climbing towards NLU: On Meaning Form, and Understanding in the Age of Data, explore the language that is used in language modeling research. They found that often language models were described as “understanding” or “learning meaning.” Although the field has had many successes, Bender and Koller argue that we need to distinguish between form and meaning. And that language models trained on form have no a priori way of learning meaning.

The authors identify that one consequence of using overzealous language in academic writing is that it will trickle down into media and the popular press. This language creates AI hype and misconception on what language models are doing. One example is “BERT is a system by which Google’s algorithm uses pattern recognition to better understand how human beings communicate so that it can return more relevant results for users.” 

The authors explain that one reason for this ambiguous language is that “we do not fully understand what exactly it is about language the large LMs (linear models) come to implicitly represent.” Language models are able to identify various patterns and probabilistic relationships between the form of text but in no way has the ability to infer meaning. The authors define form as “any observable realization of language.” They define meaning to be “the relation between the form and something external to language.” 

Bender and Koller use a number of thought experiments in order to support their point. One of which was the Octopus Test, where an octopus acts as a middleman between the cable communication of two individuals. No matter how long the octopus listens to the cable communications of the two individuals, is not able to derive meaning or the communicative intent of the two individuals. 

Lastly, the authors examine how human children learn. They found in child development research that children don’t learn language through “passive exposure”, such as television or radio. Rather joint attention and intersubjectivity is crucial for learning language. It requires “the ability to be aware of what another human is attending to and guess what they are intending to communicate.” In conclusion, “human children do not learn meaning from form alone and we should expect machines to do so either.”


## Fake News Detection Using Machine Learning Ensemble Methods – Ahmad et al.

In this article, Ahmad et al. use ensemble methods to detect fake news articles. The ensemble techniques used included random forests, a voting classifier (random forests, logistic regression and kNN), bagging classifier, and a boosting classifier (AdaBoost and XGBoost). The datasets used were the ISOT Fake News Dataset and two datasets that are publicly available from Kaggle. The conclusion of the study was that “the ensemble learner XGBoost performed better in comparison to other learning models on all performance metrics.” The performance metrics that were used in this study were Accuracy, Recall, Precision, and F1-Score.
