# EncodeDecoder
This repo contains the implementation of an encoder decoder for MNIST images for learned data compression. At the end, I also explored the possibility of generative models using the trained decoder

# Learned Data Compression Using MNIST Dataset

## Introduction

This document provides an overview of the implementation and outcomes of a data compression project using the MNIST dataset. It covers the details of the encoder and decoder models, the handling of the dataset, and the outcomes regarding the balance between bit rate and distortion. I also explored the possibility of generative models once the decoder was trained

## Technical Setup

### Languages and Frameworks
- **TensorFlow Compression (TFC)**
- **Python**

### MNIST Dataset

The dataset I used was MNIST handwritten digits from 0 to 9.

![mnist_ex](https://github.com/basileplus/EncodeDecoder/assets/115778954/e09254f3-1ef4-4d28-889d-77f47db02e50)

### Before training

Before training, when passing the image in the latent space and restoring it with decoder, the result I obtained was :

![recons](https://github.com/basileplus/EncodeDecoder/assets/115778954/f17cc718-2ac7-4d0f-8793-24ea67a553a3)

As we can see, the figure is just noise as the encoder/decoder has not been trained yet

### The selected latent space

The chosen latent space I chose to represent my images is a 50-dimensional space which probability distribution follows Gaussian distributions.

### Noise Addition for Robust Training
**Purpose:** Incorporates noise into the data representation to enable robust quantization and to minimize quantization errors. This kind of quantization is called *dithered quantization*
The probability distribution followed by latent representations are represented in the following figure :

![distrib](https://github.com/basileplus/EncodeDecoder/assets/115778954/371a6f42-c38d-4633-b216-5d2305123ec0)

We thus minimize the Kullback-Leibler divergence to fit the out-of-encoder probability distribution to the one selected (Gaussian + uniform noise) so that the quantizer can be chosen intelligently to reduce distorsion and increase the rate.

### Results after training

After training here are the reconstructed images (out of the decoder)

![recons2](https://github.com/basileplus/EncodeDecoder/assets/115778954/a43045c6-0250-4d6f-88df-5dac174d9283)

## Training Model Architecture

### Encoder (Analysis Network)
- **Convolutional Layers:** Utilizes 2D convolutions with Leaky ReLU activation, configured to reduce dimensions while retaining critical features.
- **Dense Layers:** Transforms the convolutional output into a flat, dense format for further processing.

### Decoder (Synthesis Network)
- **Reconstruction Layers:** Employs dense layers and 2D transposed convolutions to reconstruct the original data dimensions from the encoded format.
- **Activation:** Leaky ReLU is used throughout to maintain non-linearity.

### Training parameters

The training has been done on 15 epochs for a 50-dimensional latent space on the entire training set (60000 samples)

## Discuss of the results


### Evaluation Metrics

**Bit Rate and Distortion:** Metrics used to evaluate the precision and effectiveness of the compression model.

In the loss function,  ``lmbda`` is the parameter which tells the model how much distorsion needs to be prioretized over bits rate. I studied the effect of lambda on both bits rates and distorsion, results are presented in the following figure :

![lamb](https://github.com/basileplus/EncodeDecoder/assets/115778954/8b4886f3-c492-4953-b1f6-9f860b707e10)

As we could imagine increasing distorsion allow us to reach better rates. The same way lower is the distorsion, better can the rate be. 

## Generative AI using the decoder

Once the decoder has been trained, from a random latent vector we can generate an artificial handwritten digits with the decoder : it is a classic architecture for generative AI. We now have a model capable to generate handwritten digits

![gener](https://github.com/basileplus/EncodeDecoder/assets/115778954/7adf088a-831e-45a0-9ee3-64486f9cf47b)

Of course generated images are not perfect, but we could have improve the generation by not taking a completely random vector but vectors from canonical basis

## Further details

Further detail on this work can be found in the (french written) ``report`` pdf of the git.


