---
title: "Portfolio"
date: 2021-07-23T15:55:13+02:00
draft: false
description: "Portfolio"

lightgallery: true

math:
  enable: true
toc:
  auto: false
---

Below is the list of projects I worked on. I believe that your work is as good as you can show it, that's why I try to put each project in a demo-able format. For now I am using [streamlit](https://streamlit.io) as a tool to demo the models i build. 

# Twitter Sentiment Analysis [DEMO](https://twitter-sentiment.portfolio.wassimseifeddine.com/)

## Description
This is the simple model where you can analyze the sentiment of a single tweet by writing it in a text box, or you can also analyze a sentiment of hashtag.

The approach I used is utilizing transfer learning to fine-tune an existing Transformer based model ( specifically the [`distilbert-base-uncased`](https://huggingface.co/distilbert-base-uncased)). I fine-tuned the model on the [Sentiment140](http://help.sentiment140.com/) dataset.

## Tech Stack
- [Flair](https://github.com/flairNLP/flair) for NLP finetuning and data preprocessing.
- [Huggingface](https://huggingface.co/) for using the `pipeline` feature
- [Tweepy](https://www.tweepy.org/) for fetching the tweets from the twitter.
- [Streamlit](https://streamlit.io/) for building the demo app.

---

# Quantized Neural Network for Object Detection 

## Description
This work was done as part of a project with a self driving car company. The aim was to take a pre-trained model and try to run it **as fast** as possible and **as light** as possible to be used for real-time object detection

The model we used is a [MobileNetV2](https://arxiv.org/abs/1801.04381) pre-trained model on [ImageNet](https://www.image-net.org/) with SSDLite object detector. The model was trained with [Fp-32](https://en.wikipedia.org/wiki/Single-precision_floating-point_format) data format.

We applied several [model compression](https://arxiv.org/abs/1710.09282) techniques to reduce the size of the model and monitor it's performance. Some of the techniques we used are:

- [Quantization](https://arxiv.org/abs/2103.13630)
- [Pruning](https://arxiv.org/abs/2101.09671)
- Fused Convolution
- Knowledge Distillation

We got interesting results with the model. The model can reliably detect objects in images with same accuracy as the Fp-32 version while going as low as [Int-8](https://www.ibm.com/docs/en/informix-servers/12.10?topic=types-int8) data format.

**Disclaimer**: This work is not entirely my own. I was part of a team that worked on it.
## Tech Stack
- [PyTorch](https://pytorch.org) for building the model.
- [Tensorboard](https://www.tensorflow.org/tensorboard) for monitoring the model.
- [OpenCV](https://opencv.org) for image processing.

---


# Legal case classification

## Description
This project was two folds, first we had to build a model that would classify a legal case based on the description entered into several categories. The aim was to provide a tool that will help lawyers to classify the cases faster and easier. Second, we had to extract the entities from the case description that were relevant to the classification. 

Here's a small sketch of how the system should behave.

![Image](/case_classification_sketch.png "Defamation Classification with element extraction")


This project was extremely interesting as we faced several technical challenges that I never encountered before.
Some of the challenges:

- The data we got was very messy. It was a digitized version of PDF documents
- Each case in our training data was several hundred words long which made it difficult that we used some NLP models.

## Tech Stack
- [Doc2Vec](https://radimrehurek.com/gensim/models/doc2vec.html) for building the baseline model.
- [PyTorch](https://pytorch.org) for building the model.
- [Huggingface](https://huggingface.co/) for using the text tools it provides
- [Weights & Biases](https://wandb.ai) for monitoring the model training.
- [Jupyter Notebooks](https://jupyter.org/) for showcasing the results of the EDA
---