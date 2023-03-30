---
draft: false
date: 2023-03-30
categories:
    - docker
    - machine learning
    - nvidia
authors:
    - chainyo
title: Keep your workstation clean - Docker
description: Optimize Docker Storage for Machine Learning
---

# Keep your workstation clean - Docker
## Optimize Docker Storage for Machine Learning

When working with Machine Learning, especially with large images like NVIDIA ones for training models on GPUs,
it is important to manage your workstation storage efficiently.

**Docker is a great tool for containerization, providing a consistent environment for deploying applications.**

However, as you create and run containers, unused files and storage may accumulate on your system.

In this post, we'll cover how to use Docker commands to prevent unused files and storage from cluttering your workstation.

<!-- more -->

## 1. Remove unused containers

Stopped containers can take up valuable storage space on your workstation. To remove them, use the following command:

<!-- termynal -->

```

docker container prune

```

This command will prompt you to confirm the deletion of stopped containers. Enter `y` to proceed.
Be careful, as this action is irreversible.

## 2. Remove unused images

Unused images can also occupy significant storage space. I always have multiple **NVIDIA** images on my workstation because
of the different versions of **CUDA** and **cuDNN** that I use for different projects.

To remove unused images, use the following command:

<!-- termynal -->

```

docker image prune

```

Bye-bye, unused `cuda11.0-cudnn8-devel-ubuntu18.04` image! 😢

But hey, now we are all using **CUDA 12.1** and **PyTorch 2.0**, right? 

Right? 😅

## 3. Remove unused networks and volumes

Unused networks and volumes can also take up storage space on your workstation.

It's less the case when working with ML training, but if you have built some APIs on top of your ML models, you may have
some unused networks and volumes. It's always a good idea to clean them up from time to time.

To remove unused networks and volumes, use the following commands:

<!-- termynal -->

```

docker network prune
docker volume prune

```

These commands will prompt you to confirm the deletion. Enter `y` to proceed. 
Remember that this action is irreversible, so only run this command if you're sure you no longer need these resources.

## 4. Remove the build cache

One of the most common causes of unused files and storage is the build cache. I discovered how much space the build cache
can take up when I was working on a project that required me to build a lot of Docker images.

Thankfully, Docker provides a command to remove the build cache. To do so, use the following command:

<!-- termynal -->

```

docker builder prune

```

The build cache is used to accelerate the Docker image building process, but most of the time you don't need to keep it
when you are going to build a new image for a new project.

I saved 25GB of storage space on my workstation by removing the build cache of only one project. 😱

Imagine how much space you can save if you remove the build cache after months of working on different projects!

---

By using these Docker commands, you can efficiently manage your workstation's storage, preventing unused files from
accumulating and optimizing space for machine learning tasks.

Always be cautious when running these commands, as they will delete unused containers, images, networks, volumes, and build cache.
Only run them if you are certain you no longer need the resources. With a clean workstation, you'll be better equipped to train your machine learning models using large images and Docker.
