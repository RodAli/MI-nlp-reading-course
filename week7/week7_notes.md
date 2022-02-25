## Deep Learning in R

### Chapter 1 - What is deep learning?

**Artificial Intelligence**: the effort to automate intellectual tasks normally performed by humans

**Symbolic AI**: that human-level artificial intelligence could be achieved by having programmers handcraft a sufficiently large set of explicit rules for manipulating knowledge. It was dominant until the paradigm shift from 1950s to late 1980s.


The central problem in machine learning and deep learning is to meaningfully transform data: in other words, to learn useful representations of the input data at hand—representations that get us closer to the expected output.

**Learning**, in the context of machine learning, describes an automatic search process for better representations.

Kernel methods are a group of classification algorithms, the best known of which is the support vector machine (SVM)

SVMs aim at solving classification problems by finding good decision boundaries between two sets of points belonging to two different categories.


### Chapter 2 - The mathematical building blocks of neural networks

**Tensors** are a generalization of vectors and matrices to an arbitrary number of dimensions (note that in the context of tensors, a dimension is often called an axis).

The dimension of a tensor is often called an axis.

A tensor is defined by three attributes:

1. Number of axes (rank) - For instance, a 3D tensor has three axes, and a matrix has two axes.
2. Shape - This is an integer vector that describes how many dimensions the tensor has along each axis.
3. Data type - This is the type of data contained in the tensor.

The **derivative** tells you the direction and magnitude of change. For every "smooth" and continous function f(x), there exists a derivitive f'(x). It is differentiable.

A **gradient** is the derivative of a tensor operation. It is the generalization of the concept to functions that take multidimensional inputs (tensors).

Stochastic: randomly determined. 

Each iteration over the training data is called an **Epoch**.

Learning means finding a combination of model parameters that minimizes a loss function for a given set of training data samples and their corresponding targets.

The **loss** is the quantity you’ll attempt to minimize during training, so it should represent a measure of success for the task you’re trying to solve.

The **optimizer** specifies the exact way in which the gradient of the loss will be used to update parameters: for instance, it could be the RMSProp optimizer, SGD with momentum, and so on.


### Chapter 3 - Getting started with neural networks

A neural networks is comprised of:

1. Layers - which are combined into a network (or model)
2. Input data - and corresponding targets (class labels, target variable)
3. The loss function - which defines the feedback signal for learning
4. The optimizer - which determines how learning proceeds

"Different layers are appropriate for different tensor formats and different types of data processing. For instance, simple vector data, stored in 2D tensors of shape (samples, features), is often processed by densely connected layers, also called fully connected or dense layers (the layer_dense function in Keras). Sequence data, stored in 3D tensors of shape (samples, timesteps, features), is typically processed by recurrent layers such as layer_lstm. Image data, stored in 4D tensors, is usually processed by 2D convolution layers (layer_conv_2d)."

Crossentropy is a quantity from the field of Information Theory that measures the distance between probability distributions or, in this case, between the ground-truth distribution and your predictions.

## Fake News Detection on Social Media: A Data Mining Perspective

In this work, Shu et al. explore the research in addressing the problem of fake news. They note is that an increasing number of people are receiving their news online. Furthermore, more Americans are using social media as a source of news compared to television. Shu et al. explain that fake news is becoming more and more prevalent and that it poses a risk to the population’s well-being.

First, Shu et al. define Fake news. They identify that there is no consistent definition of fake news across the literature. After looking through other work, they decide to use the definition of "Fake news is a news article that is intentionally and verifiably false." This definition has two key features: authenticity and intent. Authenticity is the content of the article and what is being said. The content can be verified if it is false, partially true, or true. The intent is the reason or purpose of the author when writing the article. This is more difficult to determine from reading the news article. 

Shu et al. created a classifier using modern classification techniques. Furthermore, they recommend lines for future research in the lines of fake news detection. One of which is a data-oriented strategy, to determine fake news early. A system that can give "early alerts" during the dissemination period of a social media post. 

Although I think this may be a viable strategy, we should be careful when implementing this system in practice. We should ask ourselves how the fake news classifier is determining fake news and if it is accurate. If your system is giving many alerts on false positives this approach can easily backfire.


## False Information on Web and Social Media: A Survey

In this work, Kumar and Shah do a comprehensive review of the literature on false information, fake news in the context of the web and social media. They mention that the web has brought along real-time citizen journalism which has brought given the power for any individual to create articles and content. A consequence that came out of this is the prevalence of false information on the web. With the increasing adoption of consuming news from the web, now more than ever, the study of false information is important. In this article Kumar and Shah explore the *mechanisms*, *impact*, *characteristics*, and *detection* of false information.

A lot of content is covered in this article. One topic that stood out is the formation of echo chambers. Kumar and Shah brought up that due to the promotion of “personalized content” and learning about the reader on social media platforms, there has been an increase in groups of users that share information that reinforces their existing worldviews and beliefs. It is in these echo chambers that false information tends to be created and disseminated. Information that is shared by like-minded peers, will not be as critically evaluated as information that is shared by those whom you differ. This is due to confirmation bias, our tendency to more easily accept evidence that confirms our pre-existing notions and beliefs.

It seems that these echo chambers are phenomena that should be studied further if we would like to make headway into mitigating the harmful effects of false information.


