---
title: "TensorFlow"
category: Users/tk/ML
tags: TensorFlow
created_at: 2018-08-10 13:44:54 +0900
updated_at: 2018-08-10 21:00:15 +0900
published: false
number: 14
---

# TensorFlow
This is a note where to write down my questions and the answers about TensorFlow.
This note is just for myself, so I don't need to think about how it looks good or not.
Just write 'em down here.

# General
* Why do we name variables in Tensorflow?  
    -> The program doen't depend on the names but names are used in the pickled files and TensorBoard.  
source: https://stackoverflow.com/questions/33648167/why-do-we-name-variables-in-tensorflow  
  
* Can we omit arg's name or not?
    -> Yes, we can.
  
* What is `tf.name_scope`?
https://qiita.com/TomokIshii/items/ffe999b3e1a506c396c8

* `tf.name_scope` vs `tf.variable_scope`
https://stackoverflow.com/questions/34215746/difference-between-variable-scope-and-name-scope-in-tensorflow

# CNN  
* `tf.nn.conv2d` vs `tf.layers.conv2d`?
    ->For convolution, they are the same.  
        Parameters are slightly different.  
source: https://stackoverflow.com/questions/42785026/tf-nn-conv2d-vs-tf-layers-conv2d  
    -> `tf.nn.conv2d` has less args than `tf.layers.conv2d`
        cannot set activation function in `tf.nn.conv2d`
    ->**seems like better to use `tf.layers.conv2d`**
  
* Why are kernel size and strides like `[1, 2, 2, 1]` in `tf.nn.conv2d`?  
    ->Computes a 2-D convolution given 4-D input and filter tensors.  
  
    Given an input tensor of shape [batch, in_height, in_width, in_channels] and a filter / kernel tensor of shape [filter_height, filter_width, in_channels, out_channels], this op performs the following:  
  
1. Flattens the filter to a 2-D matrix with shape [filter_height * filter_width * in_channels, output_channels].
1. Extracts image patches from the input tensor to form a virtual tensor of shape [batch, out_height, out_width, filter_height * filter_width * in_channels].
1. For each patch, right-multiplies the filter matrix and the image patch vector.

detail: https://www.tensorflow.org/api_guides/python/nn#Convolution

* What is the difference between `tf.layers.conv2d` and `tf.layers.Conv2D`?
