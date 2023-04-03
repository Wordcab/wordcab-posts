---
draft: false
date: 2023-04-03
categories:
    - docker
    - nvidia
    - machine learning
authors:
    - chainyo
title: NVIDIA Docker Images for Deep Learning on GPUs
description: A step-by-step guide to flexing your GPU muscles with NVIDIA Docker Images!
---

# NVIDIA Docker Images for Deep Learning on GPUs
## A step-by-step guide to flexing your GPU muscles with NVIDIA Docker Images!

Are you ready to conquer the high seas of deep learning with the help of NVIDIA Docker Images?

Well, you've come to the right place! In this treasure trove of a blog post, we'll guide you through the uncharted waters of the installation process, present a standard Dockerfile, and show you some nifty commands to launch your training like a seasoned sailor. 

So hoist the Jolly Roger, and let's set sail! Hopefully after reading this, you won't sink like a stone. 🛟

<!-- more -->

## Installation

In this section, we'll cover the installation process for NVIDIA Docker Images.

!!! tip
    It's highly recommended to run all this on Linux systems. You are free to try it on Windows or Mac, but you may encounter some issues.

### Install Docker

Before you can start flexing your GPU muscles, you'll need to have Docker installed on your system. 

If you haven't already, head over to the Docker website and follow the instructions to install Docker Desktop for your OS. 

Remember, X marks the spot! 🗺️

### Install NVIDIA Container Toolkit

To ensure a smooth sail on the GPU ocean, you need to install the NVIDIA Container Toolkit. 

This allows Docker to use NVIDIA GPUs and simplifies running GPU-accelerated applications in containers. 

To get started, follow the [instructions](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker) on the NVIDIA Container Toolkit documentation page.

## NVIDIA Docker Images

Now that you've installed Docker and the NVIDIA Container Toolkit, you're ready to set sail with NVIDIA Docker Images.

NVIDIA Docker Images are identified as `nvidia/<image>:<tag>`.

For example `nvidia/cuda:11.7.0-devel-ubuntu22.04` is the image for CUDA 11.7.0 on Ubuntu 22.04.

## Standard Dockerfile

Now that you've got your trusty compass (Docker) and treasure map (NVIDIA Container Toolkit), it's time to create a Dockerfile for your deep learning adventure! 

Here's a simple example to get you started:

```
# Use an official NVIDIA CUDA image as a parent image
FROM nvidia/cuda:11.7.0-devel-ubuntu22.04

# Install required packages for your project
RUN apt-get update && apt-get install -y \
    git \
    curl \
    software-properties-common

# Install Python 3.10 and pip
RUN add-apt-repository ppa:deadsnakes/ppa \
    && apt install -y python3.10 \
    && rm -rf /var/lib/apt/lists/* \
    && curl -sS https://bootstrap.pypa.io/get-pip.py | python3.10

# Install Python dependencies
COPY requirements.txt /requirements.txt
RUN python3.10 -m pip install -r requirements.txt

# Install PyTorch that matches your CUDA version
RUN python3.10 -m pip install --upgrade torch==1.13.1+cu117 --extra-index-url https://download.pytorch.org/whl/cu117

# Set the working directory to /app and copy the current directory contents into the container at /app
WORKDIR /app
COPY . /app

# Run main.py when the container launches
CMD ["python3.10", "main.py"]
```

If you are familiar with Docker, you'll notice that this is a standard Dockerfile.

The only thing here is that we doing an extra step to install PyTorch that matches your CUDA version.

It's important to be sure we won't run into deep water trouble later on.

## Launching your training

With your Dockerfile ready, it's time to build the image and launch your training. Here's how to run the show:

1. Build the image

    ```
    docker build -t my-deep-learning-image .
    ```

This command tells Docker to build an image using the Dockerfile in the current directory (denoted by the period) and tag it as my-deep-learning-image.

2. Run the image

    ```
    ddocker run --gpus all \
        -v /path/to/your/host/training/data:/workspace/data \
        -v /path/to/your/host/model/output:/workspace/output \
        --shm-size 1g \
        --restart unless-stopped \
        my-deep-learning-image
    ```

This command does a few things:

* `--gpus all` tells Docker to use all available NVIDIA GPUs.
* The `-v` flag is used twice to mount two volumes:
    * `/path/to/your/host/training/data` is the host path containing your training data, mapped to `/workspace/data` inside the container.
    * `/path/to/your/host/model/output` is the host path where the trained models will be saved, mapped to `/workspace/output` inside the container.
* `--shm-size 1g` sets the shared memory size to 1GB. This is required for PyTorch to work properly.
* `--restart unless-stopped` tells Docker to restart the container if it stops for any reason.
* `my-deep-learning-image` is the image tag that we built in the previous step.

And that's it! With these commands, you've successfully launched your deep learning training using NVIDIA Docker images, utilizing the full power of your GPUs. 

!!! success
    Now, sit back, grab a bottle of grog 🍹, and watch your model sail through the epochs like a well-trained pirate crew 🏴‍☠️.

## Updating your training code

If you need to update your training code, you can do so by following these steps:

1. Make the changes to your training code.
2. Rebuild the image with the new changes by running the `docker build` command again.
3. Restart the container with the new image by running the `docker run` command again.

The container build process will be much faster than the first time, since Docker will only rebuild the layers that have changed.

---

Arr, mateys! You've now learned how to harness the power of NVIDIA Docker images for deep learning on GPUs. 

By following this guide, you've installed the necessary tools, created a standard Dockerfile, and launched your GPU-accelerated training using some handy-dandy commands. 

Now you can sail the high seas of deep learning with confidence, knowing that you have the power of NVIDIA GPUs and Docker at your disposal. 

And always remember: keep your sense of humor, for even the stormiest seas can be conquered with a hearty laugh and the right tools.

Happy sailing! ⛵
