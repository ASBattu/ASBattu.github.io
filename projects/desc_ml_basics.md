---
layout: page
title: Machine Learning Tensorflow
description: >
  Notes and topics inspired by the Tensorflow tutorials on Machine Learning
hide_description: false
sitemap: false
---

1. this list will be replaced by the toc
{:toc .large-only}



## Image Classification - Machine Learning
### Libraries used and quick description
- [Numpy](https://numpy.org/doc/stable/) - Scientific computing in python, numerical calculations.
- [PIL](https://pillow.readthedocs.io/en/stable/) - Python Imaging Library for image processing capabilities in Python.
- [Tensorflow](https://www.tensorflow.org/) - End-to-End open source machine learning platform.
- [Keras](https://keras.io/) - An API built on top of Tensorflow, acts as a framework for NN.


### Importing directory of images
- Using **tf.keras.utils.image_dataset_from_directory**, generate a Dataset that yields batches of images from subdirectories together with its labels. Using this, data is split into training and validation subsets, custom sized batches and input image size is set.
- Batches created are a tensor of the shape (p, q, r, a). This is a batch of 'p' images of shape "'q' x 'r' x 'a'".
- To load the images from disk without having I/O blocking problems, **Dataset.cache** (keeps images in memory after they're loaded off disk during the first epoch) and **Dataset.prefetch** (Overlaps data preprocessing and model execution while training) are implemented.


- RGB channel values for the images are in the range of [0,255]. These values are then **standardized** or **normalized** between the range of [0,1] using **tf.keras.layers.Rescaling** (Or divide numpy image array by 255)

### Sequential Keras Model
- A **Sequential** model is appropriate for a plain stack of layers where each layer has exactly one input tensor and one output tensor. Simplest version of a Sequential Model contains **Neurons** connected to each other. An input layer, hidden layer(s) and output layer. 
- A **Dense** is a simple layer of neurons in which each neuron receives input from all the neurons of the previous layer.
- A **Convolutional** layer implies a convolution is applied to the input data to filter the information and produce a feature map. This filter, also called a kernel, convolves with the image and creates an activation map.
- A **MaxPooling2D** layer is similar to a convolutional layer, but instad of doing convolution operation, we select the max values in the receptive fields of the input, saving the indices and producing a summarized output volume.

- By **stacking convolutional and Maxpooling layers on top of each other**, a convolutional block is formed.

- All convolutional layers use **ReLU** activation function to introduce non-linearity into the NN. (Rectified Linear Unit)
- The final layer uses a **Softmax** activation function, usually used for multi-class classification problems where class membership is required on more than two class labels.
- Finally, **the model is compiled using 'Adam' optimizer**. An optimizer is an algorithm used to change the attributes of the neural network such as weights and learning rates to reduce the losses. Optimizerse are used to solve optimization problems by minimizing the function.
- Model is trained (fit) on the data for a fixed number of **epochs** (iterations on a dataset)

### Overfitting
- When there is a small number of training examples, the model sometimes learns from the noises or unwanted details from training examples. It means that the model will have a difficult time generalizing on a new dataset.

### Data augmentation
- To counter overfitting, Data augmentation takes the approach of generating additional training data from the existing examples by augmenting/editing them using random transformations that yield close-enough images.

### Dropout
- Another technique to reduce overfitting by introducing dropout regularization to the network.
- It works by randomly dropping (by setting the activation to zero) a number of output units from the layer during the training process.