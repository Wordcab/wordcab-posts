---
draft: false
date: 2023-03-31
categories:
    - asr
    - openai
    - docker
    - machine learning
    - nvidia
authors:
    - chainyo
title: Wordcab Transcribe
description: An open-source ASR solution using Whisper, Docker and FastAPI
---

# Wordcab Transcribe - An open-source ASR solution using Whisper, Docker and FastAPI

**Automatic Speech Recognition** (ASR) has become an essential tool for developers and businesses. 
With Wordcab Transcribe, you can **leverage ASR in your projects without relying on expensive third-party platforms**.

We've implemented an open-source ASR solution using Docker, FastAPI, and the faster-whisper library, which is a fast 
implementation of the transcription model from **OpenAI Whisper**. 

This project utilizes **CTranslate2** under the hood to **speed up the processing of audio files** while requiring less 
than 5GB of VRAM on the GPU with the *large-v2* Whisper model.

*In this blog post, we'll present the Wordcab Transcribe project and show you how to use it in your own applications.*

<!-- more -->

## Why Wordcab Transcribe?

Wordcab Transcribe offers several advantages over closed ASR platforms:

* 🤗 **Open-source**: Our project is open-source and based on open-source libraries, allowing you to customize and extend it as needed.
* ⚡ **Fast**: The faster-whisper library and CTranslate2 make audio processing incredibly fast compared to other implementations.
* 🐳 **Easy to deploy**: You can deploy the project on your workstation or in the cloud using Docker.
* 🔥 **Batch requests**: You can transcribe multiple audio files at once because batch requests are implemented in the API.
* 💸 **Cost-effective**: As an open-source solution, you won't have to pay for costly ASR platforms.
* 🫶 **Easy-to-use API**: With just a few lines of code, you can use the API to transcribe audio files or even YouTube videos.

All the code is available on [GitHub](https://github.com/Wordcab/wordcab-transcribe).

!!! note
    The project is still in its early stages, so it may not be suitable for production use. 
    However, we are working on improving it and adding new features.

    If you have any suggestions or feedback, please let us know in the [GitHub issues](https://github.com/Wordcab/wordcab-transcribe/issues).

## Building and running the project

We use Docker to allow you to deploy the project on your workstation or in the cloud. It also avoids the hassle of installing
all the dependencies on your workstation and matching the versions of the CUDA and cuDNN libraries.

### Prerequisites

To build and run the project, there are some prerequisites:

* **Docker**: You'll need to install Docker on your workstation. You can follow the instructions [here](https://docs.docker.com/get-docker/).
* **NVIDIA GPU**: You'll need an NVIDIA GPU with at least 5GB of VRAM to run the project.

!!! tip
    The project uses the `large-v2` model from OpenAI Whisper, which requires at least 2.5GB of VRAM, and with a batch size of 1,
    it's another 2GB of VRAM. **So, you'll need at least 5GB of VRAM to run the project.**

    The project was tested on a workstation with an NVIDIA RTX 3090 GPU and 24GB of VRAM and it worked fine with a batch size of 4.

* **NVIDIA Container Toolkit**: You'll need to install the NVIDIA Container Toolkit on your workstation. You can follow the instructions [here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker).

### Building the Docker image

To build the Docker image, run the following command:

<!-- termynal -->

```

docker build -t wordcab-transcribe:latest .

```

Depending on your internet connection, it may take a few minutes to download the NVIDIA base image.

### Running the Docker container

To run the Docker container, run the following command:

<!-- termynal -->

```

docker run -d --name wordcab-transcribe \
    --gpus all \
    --ipc=host \
    --shm-size 64g \
    --ulimit memlock=1 \
    --ulimit stack=67108864 \
    -p 5001:5001 \
    --restart unless-stopped \
    wordcab-transcribe:latest

```

- `--gpus all` - Use all the GPUs on the workstation. Example using a specific GPU: `--gpus "device=1"`.
- `--shm-size 64g` - Increase the shared memory size to 64GB. This allow PyTorch to use more memory, which can be useful when using large models and multiple GPUs.
- `--ipc=host` - Share the host's IPC namespace. This allows the container to benefit from the host's shared memory.
- `--ulimit memlock=1` - Allow the container to lock memory on the GPU. This is recommended by NVIDIA for GPU containers.
- `--ulimit stack=67108864` - Set the maximum stack size to 64MB. This can be useful for preventing stack overflow errors that may occur if the container tries to allocate more stack memory than is available.

The container will start and you'll be able to use the API once the models are downloaded and loaded.

### Updating configuration

You can update the configuration by editing the `.env` file.

The configuration file is used to set the following parameters:

```bash
PROJECT_NAME="Wordcab Transcribe"
VERSION="0.1.0"
DESCRIPTION="ASR FastAPI server using faster-whisper and pyannote-audio."
API_PREFIX="/api/v1"
DEBUG=True
BATCH_SIZE=4
MAX_WAIT=0.1
WHISPER_MODEL="large-v2"
EMBEDDING_MODEL="speechbrain/spkrec-ecapa-voxceleb"
```

To customize your deployment, you can edit the `.env` file and rebuild the Docker image.

- BATCH_SIZE - The maximum number of audio files to process in parallel. The default value is 4.
- MAX_WAIT - The maximum number of seconds to wait to process the audio files in the queue. 
The default value is 0.1, which means that after 100ms, the audio files in the queue will be processed even if the 
batch size is not reached.
- WHISPER_MODEL - The name of the Whisper model to use. The default value is `large-v2`.
- EMBEDDING_MODEL - The name of the speech embedding model to use. The default value is `speechbrain/spkrec-ecapa-voxceleb`.


## API Endpoints

We provide two endpoints to transcribe audio:

1. `/api/v1/audio` - Accepts an audio file as input.
2. `/api/v1/youtube` - Accepts a YouTube video URL as input.

### Transcribing an audio file

To transcribe an audio file, use the `/api/v1/audio` endpoint. Here's an example using Python and the `requests` library:

```python
import requests

filepath = "sample_1.mp3"
url = "http://localhost:5001/api/v1/audio"

with open(filepath, "rb") as f:
    files = {"file": (filepath, f)}
    response = requests.post(url, files=files)

print(response.json())
```

Alternatively, you can use `curl` in the terminal:

<!-- termynal -->

```

curl -X 'POST' \
    'http://localhost:5001/api/v1/youtube' \
    -H 'accept: application/json' \
    -H 'Content-Type: multipart/form-data' \
    -F 'file=@/path/to/audio/file.wav'

```

### Transcribing a YouTube video

To transcribe a YouTube video, use the `/api/v1/youtube` endpoint. Here's an example using Python and the `requests` library:

```python
import requests

video_url = "https://youtu.be/dQw4w9WgXcQ"
url = f"http://localhost:5001/api/v1/youtube?url={video_url}"
response = requests.post(url)

print(response.json())
```

Or with `curl`:

<!-- termynal -->

```

curl -X 'POST' \
    'http://localhost:5001/api/v1/youtube?url=https://youtu.be/dQw4w9WgXcQ'

```

---

Wordcab Transcribe is a powerful, open-source ASR solution built with Docker, FastAPI, and the faster-whisper library.

By implementing this project in your applications, you can leverage the benefits of ASR without the costs and restrictions of commercial platforms.

With its **easy-to-use API**, **blazing-fast performance**, and **low VRAM usage**, Wordcab Transcribe is an excellent 
choice for developers and businesses seeking a reliable, cost-effective ASR solution. 

Give it a try, and let the open-source community help you unlock the potential of ASR in your projects.
