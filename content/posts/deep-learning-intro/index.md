---
title: "My introduction to Deep Learning"
date: 2023-11-17T09:03:20-08:00
featuredImage: "posts/deep-learning-intro/deep-learning-feature.png"
tags: ["software development", "machine learning", "deep learning"]
description: "My first foray into Deep Learning - a Machine Learning technique used to build AI models like ChatGPT"
---

_This is a compilation of my notes and insights from my participation in week 8 of the [Machine Learning ZoomCamp](https://mlzoomcamp.com/)_

## What is Deep Learning?

{{< figure src="./Deep-learning-visualization-4.gif" class="center" >}}

One of my goals in starting the [Machine Learning ZoomCamp](https://mlzoomcamp.com/) was to deepen my understanding of the fundamentals driving recent advances in Machine Learning and Generative AI. I was particularly excited to begin Week 8, which delved into TensorFlow and Keras, two pivotal libraries in deep learning research and application.

At its heart, deep learning comprises techniques for constructing sophisticated neural networks, which are biologically-inspired machine learning models. These models, developed using deep learning techniques, consist of multiple "layers" that process an input by transmitting it through each layer to discern patterns or features.

## Building a Simple Neural Network Using Deep Learning

{{< figure src="./bee-wasp.png" class="center" >}}

For this weeks [project](https://github.com/Tadwork/MLZoomCampSolutions/blob/main/week8/homework.ipynb), I constructed a basic convolutional neural network (CNN) using Keras. The task was to determine whether an image depicted a bee or a wasp, utilizing [this dataset](https://www.kaggle.com/datasets/jerzydziewierz/bee-vs-wasp) from Kaggle. Remarkably, the model required only a few lines of code to process the 150x150 RGB images in the training set. It included a [2D convolutional layer](https://keras.io/api/layers/convolution_layers/convolution2d/) generating 32 filters of 3x3 pixels, a pooling layer to downsample the output, and then 64-neuron and 1 neuron dense layers, culminating in a binary classification: bee (0) or wasp (1).

During training, each of the 3x3 filters are convolved (slid over) the training images and a score is produced on the strength of the match. Each epoch (run) of the training adjusts the weights with the goal of trying to identify which patterns occur most frequently in a bee and a wasp for use in the final model.

Training it just on the dataset itself wasn't enough though, and the model over-fit to the training data pretty quickly. Applying a set of recommended transformations and augmentations to the training set helped to stabilize the model as can be seen in the below graph showing the accuracy score through multiple epochs of training.

{{< figure src="./after-data-augmentation.png" class="center" title="Comparing accuracy before and after augmentation">}}

While the final accuracy peaked at 79% on the test set, which isn't great, the minimal effort and computational resources required to build this model were impressive.

## Additional Learning

I must highlight Jeremy Howard's excellent deep learning [course at fast.ai](https://course.fast.ai/). I intend to dive further into this resource and the accompanying [book](https://course.fast.ai/Resources/book.html) after I complete the ZoomCamp course.

The [Keras documentation](https://keras.io/) was also notably well-organized and user-friendly. It offers a wealth of information for those seeking to understand the forefront of deep learning, including API documentation, guides, and comprehensive examples.

[Stanford's CS231n](https://cs231n.github.io/) is another valuable resource, frequently cited in the ZoomCamp videos. I look forward to exploring it more in the coming weeks.

Finally, tools like the [Tensor Flow Playground](https://playground.tensorflow.org/) and Google's [Teachable Machine](https://teachablemachine.withgoogle.com/) are excellent for experimenting with deep learning concepts. They have greatly aided in enhancing my understanding of the field.

_11/9/2023 added additional detail to explain the layers in the homework project_