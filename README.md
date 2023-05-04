Download Link: https://assignmentchef.com/product/solved-cs7643-ps2-and-hw2
<br>
In this homework, we will learn different applications in deep learning with respect to image gradients, including saliency maps, GradCAM, fooling images, class visualizations, and style transfer. This homework is divided into two major following parts:

– Understand network visualization and implement saliency maps, GradCAM, fooling images, class visualizations.– Understand and implement style transfer.

Note that this homework is designed referring to [assignment 3 from Standford CS231n course][2].

Download the starter code [here]({{site.baseurl}}/assets/f17cs7643_hw2_starter.zip).

## Setup

Suppose you have already installed the dependencies in the homework 1. Before you start this one, here are some preparation work you need to do.

First install the dependency:

“`pip install future“`

Then download the imagenet_val_25 dataset

“`cd cs7643/datasetsbash get_imagenet_val.sh“`

We will use **PyTorch** to finish the problems in this homework, which has been tested with python2.7 on Linux and Mac.

Throughout this homework, we will use SqueezeNet which should easily support you to perform all the experiments on a CPU machine. You are encouraged to use a larger model to finish the rest of the experiments if GPU resouces are not a problem for you, but please highlight the backbone network you use in your implementation if you do it.

Switching a backbone network is quite easy in PyTorch. You can refer to [torchvision model zoos][6] for more information.

* [Iandola et al, “SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and &lt; 0.5MB model size”, arXiv 2016][7]

## Part 1

Part 1 is integrated with Facebook’s new visualization and interpretation library [Captum][10]! We will use Captum throughout the sections to see how much simpler it is to visualize model outputs.

Open notebook `NetworkVisualization-Pytorch.ipynb`. We will explore the use of *image gradients* for generating new images, by studying and implementing key components in four papers:

1. [Karen Simonyan, Andrea Vedaldi, and Andrew Zisserman. “Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps”, ICLR Workshop 2014.][3]2. [Ramprasaath R. Selvaraju, Michael Cogswell, Abhishek Das, Ramakrishna Vedantam, Devi Parikh, Dhruv Batra. “Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization”, IJCV 2019.][9]3. [Szegedy et al, “Intriguing properties of neural networks”, ICLR 2014][4]4. [Yosinski et al, “Understanding Neural Networks Through Deep Visualization”, ICML 2015 Deep Learning Workshop][5]

You will need to first read these papers, and then we will guide you to understand them deeper with some problems.

### Q1.1: Saliency Maps (11 points)

You need to implement `compute_saliency_maps` function referring to the [paper][3]. Perform a forward and backward pass through the model to compute the gradient of the correct class score with respect to each input image. You first want to compute the loss over the correct scores, and then compute the gradients with a backward pass.

### Q1.2: Guided GradCam (11 points)

You need to implement `Guided GradCam` in three stages (GuidedBackprop, GradCam, and GuidedGradCam) as described in the [paper][9].

### Q1.3: Layerwise Captum (3 points)

You need to explore and use Captum as we have already done in the previous sections.

### Q1.3: Fooling Image (10 points)

You need to generate a fooling image in `make_fool_image` referring to the [paper][4]. You should perform gradient ascent on the score of the target class, stopping when the model is fooled.

### Q1.4 Class Visualization (10 points)

You need to implement `create_class_visualization` function refering to the [paper][5]. By starting with a random noise image and performing gradient ascent on a target class, we can generate an image that the network will recognize as the target class.

**Deliverables**

Submit the notebook you finished with all the generated outputs.

### Part 2

Another task closely related to image gradient is style transfer. This has become a cool application in deep learning with computer vision. In this notebook we will study and implement the style transfer technique from:

* [“Image Style Transfer Using Convolutional Neural Networks” (Gatys et al., CVPR 2015)][8].

The general idea is to take two images (a content image and a style image), and produce a new image that reflects the content of one but the artistic “style” of the other. We will do this by first formulating a loss function that matches the content and style of each respective image in the feature space of a deep network, and then performing gradient descent on the pixels of the image itself.

Open notebook `StyleTransfer-Pytorch.ipynb`. Implement the loss functions for this task and the training update code.

### Q2.1 Implement Content Loss (3 points)

Content loss measures how much the feature map of the generated image differs from the feature map of the source image. Implement the `content_loss` function and pass the `content_loss_test`.

### Q2.2 Implement Style Loss (6 points)

First, compute the Gram matrix which represents the correlations between the responses of each filter, by implementing the function `gram_matrix` and pass `gram_matrix_test`. Then implement `style_loss` function and pass the `style_loss_test`. Each of the function worth 3 points.

### Q2.3 Implement Total Variation Loss (3 points)

Implement total variation regularization loss in `tv_loss`, which is the sum of the squares of differences in the pixel values for all pairs of pixels that are next to each other (horizontally or vertically). You need to both pass `tv_loss_test` and provide an efficient vectorized implementation to receive the full credit.

### Q2.4 Finish Style Transfer (6 points)

Read the `style_transfer` function and figure out what are all the parameters, inputs, solvers, etc. The update rule in the following block is hold out for you to finish. What you need to implement is the update rule with by forwarding it to criterion functions and perform the backward update.

You need to generate the pretty pictures outputs which are similar to the given examples in the following block to receive full credits.

### Q2.5 Feature Inversion (2 points)

Suppose you implement things correctly, what you have done can do another cool thing. In an attempt to understand the types of features that convolutional networks learn to recognize, the following paper attempts to reconstruct an image from its feature representation. We can easily implement this idea using image gradients from the pretrained network, which is exactly what we did above (but with two different feature representations).

* Aravindh Mahendran, Andrea Vedaldi, “Understanding Deep Image Representations by Inverting them”, CVPR 2015

Just run this block and generate the outputs. If you previous implementation is correct, you will get the full credits.

**Deliverables**

Submit the notebook you finished with all the generated outputs.

For each of the loss function in part 2, **you will need to pass the unit test to receive full credits, otherwise it will be 0.** For the final output you will be expected to generate the images similar to the output to receive the full credits.