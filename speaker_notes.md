## Title slide

Thank you for allowing us to showcase our prototype in the space webinar series.

Sebastian has presented the scientific background and potential applications, while I will focus more on the technical design and implementation.

My scientific background is in quantum chemistry.

However, I have been working for almost 20 years as a software engineer, covering topics in HPC and machine learning using C++, CUDA, Python, and many others.


## Outline
I will introduce our development setup, which respects the FAIR principles, and try to use the best available techniques for software lifecycle development.

Then, I will introduce the Spherinator model and explain its training process.

Afterward, the trained model is used in the so-called HiPSter workflow to construct inferences to display the modeled galaxies and dataset projections.


## Development environment


## The model

The Spherinator model is built on dimensionality reduction, which allows it to learn data features automatically.

To achieve this, we use a generative model known as a convolutional variational autoencoder.

This is an unsupervised approach that enables us to generate new data by identifying patterns in existing data.

Unlike the plain autoencoder, which reconstructs images from a single point in the latent space, the VAE uses a sample from a learned distribution.

As a result, the neighborhood in the latent space represents similar features.
The rotational invariance is crucial for learning features from galaxy images, as they should not be dependent on image rotation.

Thus, the image with the lowest reconstruction loss is selected, and only that image is used for the training step.

The term "Spherinator" is derived from the spherical latent space manifold.

It is possible to have other manifolds, but the spherical manifold is the most suitable for visualizing later.


## The Power spherical distribution

The power spherical distribution published in 2020 provides a reliable method to handle spherical latent space.

Here, the direction represents the position in the latent space, and kappa is a measure of the distribution function's width.

The dimension of a circle is 2, a sphere is 3, and hyperspheres have a dimension greater than 3.


## The Loss Function

The loss function consists of the reconstruction loss and the Kullback-Leibler divergence.

The reconstruction loss is calculated using a pixel-wise Euclidean distance.

The Kullback-Leibler divergence measures the difference between the power spherical distribution function and a reference distribution function.

In the Euclidean space, the reference function is a centric Gaussian distribution, whereas a uniform distribution is used in the spherical case.

It is crucial to find the right balance between these two parts of the loss function

This is achieved by the factor lambda, which scales down the Kullback-Leibler divergence.


## The training

For the training, **PyTorch Lightning** is used, which decouples research from the engineering code. It is easier to read, shows better reproducibility, and can scale up using accelerated and distributed hardware.

With the command line interface of PyTorch Lightning, everything is defined in a single file. The classpath and its arguments define the model.

It is also possible to use further models in the arguments, for example, for the encoder and decoder. So it's easy to integrate existing models, for example, Resnet or Visual Transformer as encoder part.

The most excellent section is the trainer part: You can switch on the GPU accelerator with a single line. With another line, you can control the number of GPUs used.

Here, it is also possible to add loggers like Weights&Biases, Callbacks, Profiler, and so on.


## Monitoring the training


## HiPSter: The Inference


## HiPSter: The Workflow


## Summary and Outlook


