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
  - The data collected process involved gathering and merging sentiment-tagged tweets. The tweets had already been tagged with either positive or negative sentiment labels.
  - The project did **not** consider neutral sentiment texts, and it was a deliberate choice made to showcase the feasibility of me developing the project. However, this requirement can be modified if necessary for future cases.

<hr style="border:2px solid gray">
## Positive Wordcloud
<img align="center" src="https://i.gyazo.com/445f47201753aec3ab38f379cb95d28d.png" alt="drawing" width="850" height="200" style="display:block">
Wordcloud showing the most commonly used positive words present in the text
{:.figure}

<hr style="border:2px solid gray">
## Negative Wordcloud
<img align="center" src="https://i.gyazo.com/c00c750add41cf10010383849d55e59e.png" alt="drawing" width="850" height="200" style="display:block">
Wordcloud showing the most commonly used negative words present in the text
{:.figure}

<hr style="border:2px solid gray">
## Commonly used Positive Words frequency

<p align="center" width="100%">
    <img width="50%" src="https://i.gyazo.com/1991df7e89b6c736d95e02cd09f28bb1.png">
</p>
Barplot showing the frequency of the commonly used positive words present in the text
{:.figure}

<hr style="border:2px solid gray">
## Commonly used Negative Words frequency


<p align="center" width="100%">
    <img width="50%" src="https://i.gyazo.com/b86c0187f89421e04e5581c8770aecd6.png">
</p>
Barplot showing the frequency of the commonly used negative words present in the text
{:.figure}
<hr style="border:2px solid gray">

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


