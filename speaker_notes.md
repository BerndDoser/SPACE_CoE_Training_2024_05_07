## Title slide

Thank you for allowing us to showcase our prototype in the space webinar series.

During his talk, Sebastian presented the scientific background and potential applications, while my focus was on the technical design and implementation.

My scientific background is in quantum chemistry.

I am not an astronomer.

I have worked as a software engineer for around 20 years, covering topics such as HPC, machine learning, C++, CUDA, and so on.


## Outline

In my talk, I will introduce our development setup that respects the FAIR principle and uses the best techniques for the software lifecycle process.

Next, I will introduce the Spherinator model and explain its training process.

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

For the reconstruction loss is calculated by using a pixel-wise Euclidean distance.

Whereas, the Kullback-Leibler divergence is used to measure the difference between the power spherical distribution function and a reference distribution function.

In the Euclidean space the refernce function is a centric Gaussian distribution, whereas in the spherical case a uniform distribution is used.

It is crucial to find the right balance between these two parts of the loss function, and to achieve this, a factor called lambda is introduced.


## The training


## Monitoring the training
