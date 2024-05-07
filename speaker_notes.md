## Title slide

Thanks for handing over.

And thanks again for given us the opportunity to present in the space webinar.

My name is Bernd Doser, and my background is computational chemistry.

However, I have worked for almost 15 years as a scientific software engineer in different fields.

My focus is HPC and machine learning using C++, CUDA, Python, and many others.

Sebastian has presented the scientific background and potential applications,
and I will focus more on the technical design and implementation.


## Outline

First, I will say a few general words about best practice development environment setup.

Then, I will introduce our Spherinator machine-learning model and explain its training process.

Then, I will explain our workflow HiPSter which is used for inferences to display the modeled galaxies and dataset projections.


## FAIR principles

Let me start with a few words about best practice software design.

With respect to the design and implementation we follow the so-called FAIR principles,
that have been established in the scientific community.

What are these principles, and why are they important?

These principles state that the software should be easily findable and accessible,
interact with other software through open standards, and be simple to use and modify.

Please find a good overview in the given reference for more details.


## Development environment

Before diving deeper into our model, a few words on useful frameworks and technical environments.

The slides contain links to the tools.

We have established a development environment using GitHub that covers the entire software lifecycle,
from feature design to deployment, with GitHub Actions providing continuous integration.

For dependency management, we use Poetry, and Dependabot is used to automate version updates.

The models will be converted into the ONNX open source standard, enabling interoperability among ML frameworks and programming languages.

We use Weights & Biases to track and orchestrate the training process.

This development setup complies with the FAIR principles, providing stability while allowing for the necessary flexibility to make steady improvements.


## The model

So, what is Spherinator?

The term "Spherinator" is derived from the spherical latent space manifold.

Let me now explain in more detail.

The Spherinator model is a machine learning model which is built for dimensionality reduction.

This allows to learn data features automatically.

To achieve this, we use a generative model known as a convolutional variational autoencoder.

This is an unsupervised machine-learning approach that enables us to generate new data by identifying patterns in existing data.

Unlike a plain autoencoder, which reconstructs images from a single point in the latent space,
the VAE uses a sample from a learned distribution.

As a result, the neighborhood in the latent space represents similar features.

The rotational invariance is crucial for learning features from galaxy images, as they should not be dependent on image rotation.

To achieve this, the image with the lowest reconstruction loss is selected,
and only that image is then used for the training step.

It is possible to have other manifolds, but the spherical manifold is the most suitable for visualization.


## The Power spherical distribution

The power spherical distribution published in 2020 provides a reliable method to handle spherical latent space.

Here, the direction represents the position in the latent space, and kappa is a measure of the distribution function's width.

The dimension of a circle is 2, a sphere is 3, and hyperspheres have a dimension greater than 3.


## The Loss Function

The loss function is one of the most critical aspects of the model training.

The loss function consists of the reconstruction loss and the Kullback-Leibler divergence.

The reconstruction loss is calculated using a pixel-wise Euclidean distance.

The Kullback-Leibler divergence measures the difference between the power spherical distribution function and a reference distribution function.

In the Euclidean space, the reference function is a centric Gaussian distribution, whereas a uniform distribution is used in the spherical case.

It is crucial to find the right balance between these two parts of the loss function

This is achieved by the factor lambda, which scales down the Kullback-Leibler divergence.


## The training

For the training, **PyTorch Lightning** is used, which decouples research from the engineering code.

This has several advantages:  

It is easier to read, shows better reproducibility, and can scale up using accelerated and distributed hardware.

With the command line interface of PyTorch Lightning, everything is defined in a single file. The classpath and its arguments define the model.

It is also possible to use further models in the arguments, for example, for the encoder and decoder. So it's easy to integrate existing models, for example, Resnet or Visual Transformer as encoder part.

One of the best features is the trainer section: You can switch on the GPU accelerator with a single line. With another line, you can control the number of GPUs used.

Here, it is also possible to add loggers like Weights&Biases, Callbacks, Profiler, and so on.


## Monitoring the training

Another key aspect is monitoring the training.

The AI platform Weights&Biases is a comfortable way to track the training experiments.

In addition, the data can be shared easily with others.

The trained models can be organized and version-controlled in the model registry,
from where they can be used for fine-tuning, staging, or production.

So, searching for and copying the correct model is no longer needed.

Weights&Biases provides also a hyperparameter optimization, and it's free for academic research.


## HiPSter: The Inference

Hipster is a workflow, that our team has developed.

Especially for our project visualization is highly important

We need to visualize the generated images from the latent space with the trained model.

Here, the hierarchical progressive survey, the HiPS format, opens an ideal way to visualize the spherical manifold hierarchically, where we can zoom into the latent space and get more generated images in this region.

With the HiPS file structure, we can use the program Aladin-Lite for the final visualization. 


## HiPSter: The Workflow

On this slide, the HiPSter workflow and the individual tasks are schematically shown.

To generate the HiPS tiles, the decoder is called for all hierarchical points of the spherical latent space.

The encoder part of the model generates the catalog file, which contains the location in the latent space for a given input dataset.

In addition, other images and tables need to be created to provide the necessary information for Aladin-Lite.

The tool we used for visualization mentioned before.


## Summary and Outlook

With that I would like to summerize.

The take-home message is: Spherinator and HiPSter provide a promising solution for classifying large datasets in general.

A key for establishing a stable technical framework is to obey modern software development techniques and following best practices such as the FAIR principles.

This allows continuously improving our project.

Currently, we are evaluating new encoder and decoder models and loss functions to further improve data reconstruction and reflect more image details.

Additionally, we are assessing appropriate workflow frameworks to establish and expand our HiPSter workflow.

We are going to test Geometric deep learning, that would provide a direct way to train 3D structures with rotational invariance.

Finally, as mentioned by Sebastian, you can find the prototype and the source code on Github.


If you have any questions, please don't hesitate to contact us.

Sebastian can assist with scientific inquiries
and I can help with technical or general inquiries regarding modern software development and machine learning.

Thank you for your attention.
