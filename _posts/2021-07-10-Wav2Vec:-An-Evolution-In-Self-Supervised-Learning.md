---
title: "Wav2Vec part 1: Wav2Vec and VQwav2vec"
last_modified_at: 2021-07-10T16:20:02-05:00
categories: paper_review
tags:
    - audio
    - self_supervised_learning
---

## What is Wav2Vec?

Wav2vec is a series of papers from Facebook AI Research that develop methods for self-supervised learning of audio representations. The aim of wav2vec is to learn leverage an unsupervised pretraining step 
to learn better representations that can be used for downstream tasks. 
Wav2Vec achieves impressive results, often beating the state of the art with less labeled training data. 

So far there are four wav2vec focused papers from facebook. 

1. WAV2VEC: UNSUPERVISED PRE-TRAINING FOR SPEECH RECOGNITION (wav2vec 1.0)
2. VQ-WAV2VEC: SELF-SUPERVISED LEARNING OF DISCRETE SPEECH REPRESENTATIONS (VQwav2vec)
3. wav2vec 2.0: A Framework for Self-Supervised Learning of Speech Representations (wav2vec 2.0)
4. Unsupervised Speech Representation (wav2vec -U)

These papers explore different methods for self-supervised learning of speech. The first Wav2Vec paper used a simple method to achieve state of the art results. 
Subsequently, VQ-Wav2Vec and Wav2Vec 2.0 applied quantitization to allow advanced NLP architectures such as BERT to be applied to the speech domain. These advances allowed for incredible results, including good Word Error Rate with only 10 minutes of labeled training data. Finally, the most recent paper proposes a fully unsupervised approach that amazingly can achieve Automatic Speech Recognition *without any training data*. 

## Wav2Vec 

The first Wav2Vec paper applies a simple architecture to learn useful representations of speech audio. The aim is to train a neural network to take a raw audio signal and transform it to an expressive vector representation that can be used for downstream taks. Wav2vec contrasts with previous approaches to Automatic Speech Recognition which often relied on engineered features. For example, DeepSpeech 2 converted an audio signal into Mel Frequency Cepstral Coefficient features which were then fed into a bi-directional LSTM. By learning speech representations end to end, wav2vec can allow for more flexible modeling of a speech input.

### Analogy to Word2vec

As the name suggests, Wav2Vec draws inspiration from Word2vec, one of the original self-supervised algorithms to learn word embeddings for natural language tasks. The intuition, behind Word2Vec, Wav2Vec, and other self-supervised learning approaches is generally the same. In these algorithms the proximity or coccurence of items in a sequence (words or audio sequences) is used as to learn their meaning. For example, in Word2Vec, words that occur close together in a sentence are assumed to be more closely related to each other. 

Despite the eponymous relationship to Word2Vec, Wav2Vec is a more direct extension of Contrastive Predictive Coding (CPC). Wav2Vec leverages the CPC paradigm with only a few architectural differences. 

## VQwav2vec




## Conclusion