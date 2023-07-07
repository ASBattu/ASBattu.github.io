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

# Data Processing

  - Data pre-processing steps included cleaning the input text by removing HTTP/HTTPS links, converting it to lowercase, eliminating punctuation, numbers, stopwords and reducing excessive whitespace. 
  - The data per class was balanced for training the model.
  - These steps ensure that the data is standardized and free from irrelevant elements.

## Stemming vs Lemmatization

<p align="center" width="100%">
    <img width="70%" src="https://i.gyazo.com/7fe9cedadc978a8eaaaa89b979580a25.png">
</p>
Stemming vs Lemmatization [Img Source](https://www.turing.com/kb/stemming-vs-lemmatization-in-python)
{:.figure}

  - Stemming simplifies words by removing suffixes, while lemmatization transforms words to their base form.
    - Stemming is faster and applies simple rules, but the resulting stems may not be the actual words.
    - Lemmatization considers word meawning and context, producing accurate base forms.

<hr style="border:2px solid gray">

# Fine-Tuning BERT language model

* Why did I fine-tune a model?
  - The decision to fine-tune a model was made because training a model from scratch with the available data was not feasible due to limited data and the time it would have taken. However, various model configurations were explored during the process to reach this decision.

* What model was chosen for fine-tuning?
  - The chosen model for fine-tuning was the base **BERT (Bidirectional Encoder Representations from Transformers)** model, as it was freely available (vs GPT model) and considerably suitable for the task at hand.

* How was the model fine-tuned?
  - The model was fine-tuned by **tokenizing** the text with a maximum length of 18 tokens (value found experimentally). It was then compiled with Adam optimizer, using **sparse categorical cross-entropy loss**. The model underwent a total of **5 epochs** during the fine-tuning training process.

## Tokenization in NLP

<p align="center" width="100%">
    <img width="70%" src="https://i.gyazo.com/02ab0eb4eacbae6c552b133326ec62d0.png">
</p>
Tokenization [Img Source](https://medium.com/@ajay_khanna/tokenization-techniques-in-natural-language-processing-67bb22088c75)
{:.figure}

- Tokenization is a fundamental process in NLP that breaks text into smaller units called tokens. These tokens can be words, characters, or subwords, depending on the tokenization strategy used.

- The **BERT Tokenizer** used in this project is specifically designed for the BERT model. It employs a technique called **WordPiece tokenization**, which splits words into subwords. This approach allows the tokenizer to handle out-of-vocabulary (OOV) words and capture the more fine-grained linguistic information present in the text.

## Deployment of trained model

* Hosted in docker image locally in a container
  - RESTful API

* Hosted in S3 bucket on AWS
  - RESTful API calls for inference
  - AWS Sagemaker for hosting and endpoint creation

  - Tried AWS comprehend, but finetuning model to custom dataset costs money but is possible


