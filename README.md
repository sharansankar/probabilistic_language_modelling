# RNN_language_modelling
## Summary 
Generating text using language modelling of different corpora

## Introduction 
Language modelling is inherent in several applications from speech recognition to natural language understanding. In its essence, language modelling is simply detecting which words are likely to come after another by performing analysis on a given training set. 

Given language modelling however, is it possible to generate text that imitates a given training set. For example, is it possible for a computer to generate a Edgar Allan Poe poem? Or a Shakesperian sonnet? Having read these works, I can definitely see a pattern in the language each author uses. Is it possible however, for a computer to detect these patterns embedded in the text? 

## Background Knowledge 
### Language Modelling 
Language models work by generating a probability of each word occurring in a sentence. For example, given a sentence composed of words (W): 
<p align="center">
W1 W2 W3 W4 W5 
</p>
We can compute the probability of the sentence using the chain rule: 
<p align="center">
P(W1,W2,W3,W4,W5)=P(W1)P(W2|W1)P(W3|W2,W1)P(W4|W3,W2,W1)P(W5|W4,W3,W2,W1)
</p>

This can be generalized to the equation: 

![Generalized Equation](docs/equation.png)
(source: https://web.stanford.edu/class/cs124/lec/languagemodeling.pdf) 

However while this method works for relatively small sentences, it can be computationally expensive when performing modelling on large texts. To work around this, previous models such as N-gram proababilistic models only performed probabilities of the next word based on the chain rule applied to the previous N words. However this would fail to accurately model the word dependencies that spanned far back in the sentence. 

### Recurrent Neural Networks 
Recurrent neural networks work much like feedforward neural networks, however RNNs contain a feedback connection. This allows the network to contain a memory of the previous outputs, which can then affect the next of the RNN. This works especially well in language modelling, the output of a word is dependent on the previous words in the sentence. 

A really good visualization of an RNN is the image below: 
![RNN Architecture](docs/rnn_structure.jpg) 

(source: http://www.wildml.com/2015/09/recurrent-neural-networks-tutorial-part-1-introduction-to-rnns/) 

In this case:
- Xt: input word at time t 
- St: all the save states (previous words) at time t 
- Ot: output of RNN at time t
- U,W,V: weights of the input, saved states and output respectively 

RNN are better language models, for they are able to look further back than the previous N-gram models. In this Recurrent Neural Network, I have created a neural network with a single hidden layer and a feedback connection from the output of the hidden layer back into itself. 

## Project Implementation
### Preprocessing 
For language modelling, I have chosen two initial sets of text which will be used as training text. One contains all every single sonnet created by shakespeare, while the other is a set of every poem created by Edgar Allan Poe. Both are favourite authors of mine. 

In the preprocessing phase, each line of text is turned into a vector where each word is represented as an index value of a large dictionary.In the process, any punctuation or numbers are stripped from the sentences. 

### Training
Training of the neural network can be broken down into three parts: 
1. Forward propagation
2. Backpropagation-through-time 
3. Stochastic Gradient Descent

#### Forward Propagation 
Forward propagation for a given input vector has the following structure: 
1. Initialize hidden states S to 0: this represents the memory of the cell. Given that we are at the start of our input vector (time =0), there has been no previous input and thus our memory is none (set to 0). 
2. At each time step: 
  a. we update our hidden memory using the equation: s[t] = tanh(U*x[t] + W*s[t-1]) 
  b. we output our probability gradient of the next word using equation: O(t) = softmax(V*s[t]) 

The * represents the dot product between our hidden and input vector and its corresponding weight.

#### Backpropagation-Through-Time
To determine the loss of our neural network, we calculate the loss of the RNN output at each time step. To do this we calculate the cross entropy loss of the neural net output at time t and the corresponding expected output at the same timestep: 
![cross entropy](docs/cross_entropy.png)

#### Stochastic Gradient Descent 




