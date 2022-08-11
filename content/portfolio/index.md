---
title: "Portfolio"
date: 2022-07-23T15:55:13+02:00
draft: false
description: "Portfolio"

lightgallery: true

math:
  enable: true
toc:
  auto: false
---

Below is the list of projects I worked on. I believe that your work is as good as you can show it, that's why I try to put each project in a demo-able format. For now I am using [streamlit](streamlit.io) as a tool to demo the models i build. 

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