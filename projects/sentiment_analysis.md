---
layout: page
title: Sentiment Analysis - Docker / AWS implementation
description: >
  Project and deployment documentation
hide_description: false
sitemap: false
---

1. this list will be replaced by the toc
{:toc .large-only}


## The Idea

* Why sentiment analysis was chosen as the idea
  - Used everyday to see what the customers are saying about the product
  - NLP to show experience in AI
  - Then hosting on AWS and docker to show full pipeline development


## The Data

* What the data was and how it was collected
 - Twitter sentiment marked data collected from previous challenge
 - Only using Negative and positive sentiment comments, not using neutral sentiment texts

* Wordcloud showing most positive words and most negative words in the text
  - [Image here]

* Barplot showing frequency of words in both positive and negative texts
  - [Images here]

### Data processing

* Remove stopwords
* Tokenize
* Lamentize
* And other pre-processing techniques (lowercase)


## Fine Tune model

* Why I finetuned a model
* What model was chosen for finetuning
* How the model was finetuned

## Deployment of trained model

* Hosted in docker image locally in a container
  - RESTful API

* Hosted in S3 bucket on AWS
  - RESTful API calls for inference
  - AWS Sagemaker for hosting and endpoint creation

  - Tried AWS comprehend, but finetuning model to custom dataset costs money but is possible


