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


# The Idea

* Why sentiment analysis was chosen as an idea?
  - Sentiment analysis was chosen because it allows us to gather valuable insights about customer opinion on a product, which is a crucial aspect of business decision-making.
  - Implementing sentiment analysis demonstrates my expertise in **Natural Language Processing (NLP)** and showcases my profeciency in AI.
  - The project involves hosting the sentiment analysis system on **AWS** (Amazon Web Services) OR utilizing **Docker** for containerization, showcasing my skills in full pipeline development from deployment to production.


# The Data

* What the data was and how it was collected?
  - The data used for this project was collected from **Twitter**. Specifically, the data consisted of sentiment-labeled tweets obtained from previous challenges related to the ongoing **["SemEval"](https://semeval.github.io/)** series of evaluation and computational semantic analysis.



 - Twitter sentiment marked data collected from previous challenge. The data was collected from the ongoing "SemEval" series of evaluation and computational semantic analysis systems and data was collected from this website and merged together.
 - The data was already compiled and sentiment tagged tweets from previous years.
 Only using Negative and positive sentiment comments, not using neutral sentiment texts, as I just want to showcase that the project is possible and this requirement can be changed if needed.



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


