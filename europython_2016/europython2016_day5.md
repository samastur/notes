## EuroPython 2016 - Friday, 22.7.

### A Gentle Introduction to Neural Networks - Tariq Rashid
* not a talk about libraries; just introduction to topic itself
* start with two questions: locate people in this photo and add these numbers
  * first easy for us and not computers and the other is the opposite
  * neural networks are about helping us to solve first kind of problems with computers
* simple predicting machine (question->think->answer)
  * imagine conversion from kilometers to miles is difficult; we come up with a model where one is some factor multiplied with other and try some factors
  * we can compare it with known result => tweak it and try again => ..again..
  * this is how in principle neural networks work
* if we don know how something works
  * think of a model with adjustable parameters
  * use the error to refine parameters
* second example: measuring caterpillars and ladybirds
  * get two clusters of values on length/width graph
  * we can generate a separating line with previous approach that would separate caterpillars from ladybirds
  * classifying things is kind of like predicting things
* we can apply a factor (learning rate) when moving so that we can get better without moving too enthusiastically
* boolean logic (IF I have eaten vegetables AND I am still hungry THEN I can have ice cream)
  * example: having two inputs into model
  * simple classifiers limited, because they cannot learn data that has XOR relationship (no line correctly separates two classes)
  * answer: in simple example two have two separating lines => more than one node
* some problems cannot be solved with simple linear classifiers and need multiple node solution
* nature knows to do complicated tasks with resources that seem limited to us (1100 neurons in snail, 0.4g brain of a pigeon)
* idea: maybe our function should pass information only when signal passes certain threshold
* artificial neuron: multiple inputs => sum inputs|threshold function => output (feeding signal for node in next layer or output)
* use matrix multiplication
  * dot product of weights and incoming signals
  * used also because it can be calculated very effectivelly with computers
* what happens when we get a result with an error?
  * we know what it is when it comes out, but what is in the network?
  * what we do: use inuitive heuristic to split it
    * distribute it evenly or maybe proportionally to the strength of links
    * error back-propagation: error(hidden) = wt(hidden output) * error(output)
* but how do we actually update the weights
  * we will not be able to untangle in mathematically clean way
  * perfect is the enemy of good
  * look at immediate surrounding and decide based on that (problem: can find local minimum/maximum); often works pretty well
  * as slope gets smaller, you might want to make smaller steps to not overstep it
* Python Class and Functions: Neural Network Class composed of initialise (set size, initial weights), train (do the learning), query (query for answers)
* useful libraries: numpy (matrix maths), scipy, matplotlib, notebook
* examples of how it works (training...)


### An Introduction to Deep Learning - Geoff French
* focus mainly on image processing and principles instead of maths behind
* will discuss Theano lib/framework (and some other tools)
* gh:Britefury/deep-learning-tutorial-pydata2016 (notebooks are viewable on Github)
  * if you run it, it will try to download 500MB
* speakerdeck.com/britefury
* ImageNet is an academic image classification set (1mil images in about 1000 classes)
  * ground truth prepared manually through Amazon Mechanical Turk
* ImageNet Top-5 challenge: you score if ground truth class is one your top 5 predictions
  * in 2012 best approaches used hand-crafted approach with 25% error rate
  * in last years the rate with deep learning came down first to 15% and now 5%-7%
* neural network software comes in two flavours: neural network toolkits and expression compilers
  * expression compilers are somewhat lower level - Theano is that (write NumPy style expressions)
  * more information on deeplearning.net
* what is neural network (see previous talk)
* Theano performs symbolic differentiation for you (calculating cost and gradient descent) - other toolkits like Torch and Tensorflow do this for you as well
* randomly split the training set into mini-batches of about 100 samples and train on a mini-batches at the same time
  * run this until you get result you want
* dense layer: each unit is connected to every unit of previous layer
  * 2 hidden layers, both 256 units after 300 iterations get below 2% error
  * fully connected networks have weakness: no translation invariance; learned features are position dependent (e.g. important WHERE ball in image is)
  * for more general imagery requires a training set large enough to see all features in all possible positions...
* Convolutional networks is how we address that
  * convolution: slide a convolution kernel over an image
  * multiply image pixels by kernel pixels and sum
  * convolutions are often used for feature detections (Gabor filters)
  * each unit only connected to units in its neighbourhood
  * the values of weights form a convolution kernel -> for practical computer vision more than one kernel must be used to extract a variety of features
  * maths is still the same
  * down sampling: in a typical networks for computer vision we need to shrink resolution with max-poooling "layer" and striding (pick sample, skip a few, pick sample... - often faster and can work as well as max-pooling)
* Lasagne
  * is a neural network library built on top of Theano
  * provides API for constructing a network
  * quite a thin layer on top of Theano so understanding it is helpful
  * on the plus side customising it is simple(r)
* for inspiration could look at architecture developed by other like Inception by Google and <something> from Microsoft
* CHECK SLIDES AND VIDEO (missed few bits)
* DropOout - normally necessary for training (turned off at predict time)
  * over-fitting is a well-known problem in machine learning, affects neural networks particularly
  * a model over-fits when it is very good at correctly predicting samples in training set but fails with other items
  * during training randomly choose units to drop out
* when training goes wrong (as often will)
  * loss becomes NaN (ensure you track the loss after each epoch and correct)
  * classification error rate equivalent of random guess (not learning)
  * learns to predict constant value; optimises constant value for best loss (a constant value is a local minimum that the network won get out of - they cheat like crazy)
* not sufficient for more complex problems
  * often problem to get sufficient amount of training data
* using a pretrained network like Oxford VGG-19 (?) is often a good idea
* Transfer learning - what if we don't have enough training data? re-use part (often most) of a pre-trained network for a new task
* deep learning networks are easily fooled


### Deep Learning with Python & TensorFlow - Ian Lewis
* neural network are good for finding a way to solve the problem
  * also good at finding regressions
* cool demo of problems and how they work (url??)
* simulation of how brains works, but from a practical point of view we are doing matrix operations
* tensor is a generalized version of vectors and matrices (n-dimensional vector)
* we found NN methods that allow us to use more data usefully and improve performance
  * only became possible recently
  * The Inception model (GoogleNet, 2015)
* DNN = a large matrix ops; a few GPUs >> CPU (still takes hours/days to train)
  * you need distributed training
* at Google use of deep learning is growing exponentially
* Tensorflow
  *  open source library, generic purpose
  * used by many production ML projects (inside of Google)
  * flexible intuitive construction
  * automatic differentiation
* Core concepts
  * Graph: a TF computation represented as dataflow graph
  * Operation: computation performed
  * Tensor: data flowing through
* Jupyter notebook with example use (tutorial)
* TensorBoard can read logs created by TF and look at accuracy, values of loss functions and see how network is performing
  * can show also a graph representation of NN itself
* TF was built with distributed training in mind from beginning
  * supports different times of parallelism (split model, share data = model parallelism; also data parallelism by spliting data)
  * each approach has a different set of advantages and disadvantages
* why is data parallelism important?
  * testing takes time and as you add nodes you essentially bottleneck network
  * need dedicated network
* at Google releasing Cloud Machine Learning (Cloud ML)
  * fully managed, distributed training and prediction for custom TF
  * exposing dedicated hardware they are using (design by them)
* http://bit.ly/tensorflow-workshop
* https://www.tensorflow.org
