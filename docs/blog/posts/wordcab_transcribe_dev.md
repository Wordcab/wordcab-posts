---
draft: false
date: 2023-07-13
categories:
    - asr
    - faster-whisper
    - docker
authors:
    - chainyo
title: Wordcab Transcribe
description: An open-source ASR solution using Whisper, Docker and FastAPI
---

# The development of Wordcab Transcribe

As a machine learning engineer and open-source enthusiast, I've always been driven by the desire to create solutions 
that bridge the gap between technological capability and universal accessibility. 

Out of this pursuit, Wordcab Transcribe was born - a FastAPI based API for transcribing audio files using Faster-Whisper 
and NVIDIA NeMo.

This journey of creating an open-source, production-ready transcription service has been both challenging and rewarding.

<!-- more -->

## Building Wordcab Transcribe

The creation of Wordcab Transcribe was inspired by the need for a fast, cost-effective, and universally accessible 
transcription service.

The objective was clear: leverage state-of-the-art **Automatic Speech Recognition (ASR)** tools to develop an API that 
is not just efficient and accurate but also scalable and customizable.

To make this vision a reality, the project was built around [Faster-Whisper](https://github.com/guillaumekln/faster-whisper) 
and NVIDIA [NeMo](https://github.com/NVIDIA/NeMo). These tools provided the backbone for the swift and efficient ASR 
capabilities of the API. The speed, facilitated by the `faster-whisper` library and `CTranslate2`, set Wordcab Transcribe 
apart from many ASR implementations on the market.

Faster-whisper is easily installable in a Python project and provides a simple interface for transcribing audio files.
It is based on the [Whisper](https://github.com/openai/whisper) library and model, and new custom models can be added
as well. Its creator has put a lot of effort into making it similar to the original implementation, and keeps adding
new features as soon as they are available in the original library.

!!! tip
    This is super valuable for us, as we can easily update the library and benefit from the latest improvements.

The decision to base the API on the FastAPI framework was an essential aspect of the project. This choice ensured that 
the API would be highly performant, easy to use, and perfect for deployment at scale. The Docker-based deployment added 
a layer of convenience, and the batch request feature extended its flexibility for larger tasks.

The API was designed to handle a variety of tasks with ease. A few lines of Python code enable users to transcribe an 
audio file or a YouTube video, complete with options for diarization, and timestamping.

It's funny to see that super efficient and qualitative tools are available for free and require a little bit of 
engineering to be used together. This is the beauty of open-source.

## A Philosophical Perspective

Developing Wordcab Transcribe was not just about creating a useful ASR tool. It was about following through on a 
longstanding philosophy at Wordcab: because so many downstream processes depend on accurate transcription, production-ready 
ASR should be a freely available commodity.

Accurate transcription leads to more accurate _translation_, _summarization_, and information _extraction_. While paid services 
make sense ([we offer ASR ourselves](https://wordcab.com/)) given the amount of time and maintenance required for product 
ASR, there should be quality self-hosted options that even beginner developers can build upon.

As a techie, I believe that giving access to that kind of technology to everyone allows them to create projects that even 
surprise us. This is why I'm so **passionate about open-source** projects.

## Reflections

Each line of code written, each feature developed, each bug fixed brought a sense of accomplishment.

It's super hard to develop an universal tool that achieves close performance for all languages, especially when you
don't speak or read more than three languages. But we are always polishing the tool and trying to improve it also for
low resource languages.

Our roadmap is filled with further optimizations, new features, and countless opportunities to learn and grow. But the 
goal remains the same: to **ensure that production-ready ASR resources are universally accessible**.