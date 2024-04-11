# Introduccion
## Inteligencia Artificial
Study of computational models that allow machines to perceive, reason and act with great flexibility

## Machine Learning
Any computer program that improves its performance at some task through experience

There are 3 main ingredients:
1. Representation
2. Performance
3. Optimization

### ML Problem
$$f* = arg min_{f \in H} L(f(x)) $$

Aproximated as

$$f*_{Tr} = arg min_{f \in H} \frac{1}{N} \sum_{x_i \in Tr}^N L(f(x_i))$$

# K-Nearest Neighbors
- Case based reasoning
- Supervised approach
- Can be used for prediction and classification

1. Achieves amazing reults with large datasets
2. Critical issue is to use a suitable distance metric
3. A good metric is related to quentifying semantic distance
4. Supervised ML is related to learning suitable metrics

## Implementations
- Majority vote
- Weighted majority vote: Weight vote is proportional to the inverse of the distance

## Distance metric
Eucledian distance isnt always the best choice

Finding suitable metrics is one of the mains tasks of a ML technique

Deep Learning is an excellent tool to build semantic distance spaces

## Hashing
Local Sensitive Hashing can approximate NN

# Distributed representations
Knoledge of some object is distributed in several dimentions

# Deep Learning
Two main ingredients
1. Feature Learning
2. Hierarchical compositional representation

## Feature Learning
### Handcrafted Feature Extractor
- Bag of Words
- SIFT Based Representation

### Trainable Feature Extractor

## Hierarchical Compositional Representation
This architecture provides an efficient learning framework, where lower level layers are shared by higher level layers
Examples:
- Sounds > Phonemes > Syllables > Words
- Pixels > Edges > Parts > Objects

# Convolutional Neural Networks
Convolution provides a mechanism to search for a given feature by sliding a filter through the spatial span of the input

# Intro to Keras
Keras is a high level neural network library


# Activatio Functions
## ReLU
$$max(x,0)$$

## Soft Max
$$\frac{e^z_i}{\sum_i e^{z_i}}$$

- Is differentiable
- Pushes the highes value up and the rest down
- Good output fo rlog likelihood optimizations

## Leaky ReLU
$$max(ax,x)$$

- Does not saturate
- Faster convergence than sigmoid
- Efficient
- Not good for classification
- Non zero centered

## ELU
$$x, x > 0$$
$$a(e^x-1), otherwise$$

- Requieres calculation of e(x)
- All benefits of ReLU and LeakyReLU

# Loss function and regularization
## Negative log-likelihood
- COmmon loss function used in ML
- Well suited for continous variables
- Larger penalty to larger erros

## Cross entropy
Overhead of transmiting a signal by a pdf p(x) using an ecoding that considers a pdf q(x)

## Multi-Label vs Multi-Class
- Multi-Label: Se puede pertenecer a más de una clase
- Multi-Class: Se elige una clase entre todas
