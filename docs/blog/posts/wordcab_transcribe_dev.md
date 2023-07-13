---
draft: true
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
as well. It's creator has put a lot of effort into making it similar to the original implementation, and keeps adding
new features as soon as they are available in the original library.

!!! tip
    This is super valuable for us, as we can easily update the library and benefit from the latest improvements.

The decision to base the API on the FastAPI framework was an essential aspect of the project. This choice ensured that 
the API would be highly performant, easy to use, and perfect for deployment at scale. The Docker-based deployment added 
a layer of convenience, and the batch request feature extended its flexibility for larger tasks.

The API was designed to handle a variety of tasks with ease. A few lines of Python code enable users to transcribe an 
audio file or a YouTube video, complete with options for diarization, and timestamping.

## A Philosophical Perspective: Transcription as Universal Access

Developing Wordcab Transcribe was not just about creating a tool. It was about championing a philosophy – a belief 
in the universality of information access. In our modern age, where data is increasingly important, access to 
information should be a right, not a privilege. Like water flowing freely, quality transcription should be universally 
accessible.

Open-source projects such as Wordcab Transcribe are the vessels that carry this philosophy forward. By democratizing the 
process of transcription, we are not only enhancing access to information but also stimulating innovation and fostering 
collaboration.

Transcription, in essence, is about breaking down barriers. It's about ensuring that auditory content is accessible and 
comprehensible to everyone, irrespective of their language proficiency, hearing abilities, or learning preferences. 

Open-source projects such as Wordcab Transcribe allow us to inch closer to this ideal, paving the way for a more 
inclusive, equitable future.

As a techie, I believe that giving access to that kind of technology to everyone allows them to create new things, or
add extra layers on top of it. This is why I'm so passionate about open-source projects.

## Reflections

Looking back, the journey of creating Wordcab Transcribe has been a fulfilling one. Each line of code written, each feature developed, each bug fixed brought with it a sense of accomplishment. This open-source API is more than just a tool for transcribing audio files. It's a testament to what we can achieve when we leverage technology for the greater good.

As the creator of Wordcab Transcribe, my journey doesn't end here. The roadmap is filled with potential enhancements, new features, and countless opportunities to learn and grow. The goal remains the same: to ensure that quality resources are as universally accessible as water. After all, like water, access to information is a fundamental human necessity.