# Ilya30u30 Paper Research

## Purpose

This repo, inspired by the influential reading list recommended by Ilya Sutskever to John Carmack in 2020, curates and reproduces the essential AI and deep learning papers that shaped the field. Dubbed as the "ilya30u30," this collection represents the 27 papers that provide a comprehensive foundation and advanced insights into neural networks, generative models, optimization, and more. Each paper is  researched and reproduced, offering detailed notes and code implementations aimed at deepening the understanding of key concepts for researchers, students, and practitioners alike. Through reproduction, I aim to deepen my understanding of the implementation intricacies, validate the findings, and explore potential improvements. By sharing this work, I hope to provide valuable insights and foster further exploration in the deep learning community.



## How to Read

The collection is organized to guide readers from foundational theories to advanced topics. Each entry includes a brief summary and a link to the original paper or resource. The suggested reading order at the end can help you navigate through the materials effectively.

First, use the [Reading Order](#reading-order-1) section to understand the knowledge dependencies between the 27 papers. Then, explore the [Reproduction Research](#reproduction-research-1) section to delve deeper into the detailed studies, including reproductions and summaries of each paper.



## Table of Contents

### [Reading Order](#reading-order-1)

### [Reproduction Research](#reproduction-research-1)

### [Full Paper List](#full-paper-list-1)

### [References](#references-1)
---



## Reading Order

### Foundational Theory and Introduction to Neural Networks

1. **[CS231n Convolutional Neural Networks for Visual Recognition](#27-cs231n-convolutional-neural-networks-for-visual-recognition)**
   - A comprehensive course from Stanford University that covers the basics of CNNs, architectures, and training techniques—a great starting point for learning visual recognition.
2. **[The Unreasonable Effectiveness of Recurrent Neural Networks](#4-the-unreasonable-effectiveness-of-recurrent-neural-networks)**
   - Emphasizes the powerful capabilities of RNNs in handling sequential data, further solidifying your understanding of RNNs.
3. **[Understanding LSTM Networks](#5-understanding-lstm-networks)**
   - Introduces LSTMs and their advantages in handling long-term dependencies, forming a foundation for understanding RNNs.
4. **[Recurrent Neural Network Regularization](#6-recurrent-neural-network-regularization)**
   - Learn how to optimize RNNs and LSTMs by reducing overfitting through regularization techniques.
5. **[ImageNet Classification with Deep Convolutional Neural Networks](#9-imagenet-classification-with-deep-convolutional-neural-networks)**
   - Understand the basics of Convolutional Neural Networks (CNNs) and their applications in image recognition.
6. **[Deep Residual Learning for Image Recognition](#12-deep-residual-learning-for-image-recognition)**
   - Learn how ResNet addresses training challenges in deep networks and enhances accuracy.
7. **[Multi-Scale Context Aggregation by Dilated Convolutions](#13-multi-scale-context-aggregation-by-dilated-convolutions)**
   - Delve into using dilated convolutions in semantic segmentation tasks to achieve multi-scale context aggregation.

### Machine Translation and Natural Language Processing

1. **[Neural Machine Translation by Jointly Learning to Align and Translate](#15-neural-machine-translation-by-jointly-learning-to-align-and-translate)**
   - Study the foundational models and alignment mechanisms in neural machine translation.
2. **[Attention Is All You Need](#1-attention-is-all-you-need)** and **[The Annotated Transformer](#2-the-annotated-transformer)**
   - Deeply understand the Transformer model's implementation details and code through practical explanations.
3. **[Pointer Networks](#8-pointer-networks)**
   - Learn about a new network structure that addresses variable-length output sequences, suitable for tasks like sorting and combinatorial optimization.
4. **[Scaling Laws for Neural Language Models](#23-scaling-laws-for-neural-language-models)**
   - Explore factors affecting the performance of large language models and their scaling laws.

### Deep Learning and Optimization

1. **[GPipe: Easy Scaling with Micro-Batch Pipeline Parallelism](#11-gpipe-easy-scaling-with-micro-batch-pipeline-parallelism)**
   - Learn how to scale neural network capacity through micro-batch pipeline parallelism and understand parallel training techniques for large models.
2. **[Deep Speech 2: End-to-End Speech Recognition in English and Mandarin](#22-deep-speech-2-end-to-end-speech-recognition-in-english-and-mandarin)**
   - Understand how to build and optimize end-to-end speech recognition systems, especially for handling different languages.

### Graph Structures and Relational Reasoning

1. **[Neural Message Passing for Quantum Chemistry](#14-neural-message-passing-for-quantum-chemistry)**
   - Study the application of graph neural networks in quantum chemistry, a key to understanding supervised learning on graph data structures.
2. **[A Simple Neural Network Module for Relational Reasoning](#17-a-simple-neural-network-module-for-relational-reasoning)**
   - Learn how to enhance existing neural networks with relational reasoning modules for unstructured input.
3. **[Relational Recurrent Neural Networks](#19-relational-recurrent-neural-networks)**
   - Explore new memory modules that perform relational reasoning in sequential data, combining memory and reasoning.

### Generative Models and Complexity

1. **[Variational Lossy Autoencoder](#18-variational-lossy-autoencoder)**
   - Explore the combination of autoregressive models and variational autoencoders to achieve complex generative tasks.
2. **[The First Law of Complexodynamics](#3-the-first-law-of-complexodynamics)**
   - Understand the evolution of complexity in physical systems and its relation to Kolmogorov complexity.
3. **[Kolmogorov Complexity and Algorithmic Randomness](#26-kolmogorov-complexity-and-algorithmic-randomness)**
   - Learn Kolmogorov complexity theory and its applications in algorithmic randomness, laying the foundation for unsupervised learning.
4. **[Quantifying the Rise and Fall of Complexity in Closed Systems: The Coffee Automaton](#20-quantifying-the-rise-and-fall-of-complexity-in-closed-systems-the-coffee-automaton)**
   - Delve into the quantitative measurement of complexity in closed systems and understand the trend of complexity over time.

### Advanced Topics

1. **[Neural Turing Machines](#21-neural-turing-machines)**
   - Combine neural networks with the concept of Turing machines to expand your understanding of neural network architectures.
2. **[Identity Mappings in Deep Residual Networks](#16-identity-mappings-in-deep-residual-networks)**
   - Further study the internal mechanisms of ResNet and the importance of identity mappings in information propagation.
3. **[Order Matters: Sequence to Sequence for Sets](#10-order-matters-sequence-to-sequence-for-sets)**
   - Explore how to handle cases where inputs and outputs are not necessarily ordered sequences.
4. **[Keeping Neural Networks Simple by Minimizing the Description Length of the Weights](#7-keeping-neural-networks-simple-by-minimizing-the-description-length-of-the-weights)**
   - Learn how the Minimum Description Length principle applies to simplifying neural networks and balancing weight information content.
5. **[A Tutorial Introduction to the Minimum Description Length Principle](#24-a-tutorial-introduction-to-the-minimum-description-length-principle)**
   - Deepen your understanding of the MDL principle and its applications in model selection and data compression.
6. **[Machine Super Intelligence](#25-machine-super-intelligence)**
   - Explore the concepts and research progress in machine superintelligence, understanding the definition of intelligence and its quantifiable standards.


---

## Reproduction Research

> **Note**: The reproduction of the code and detailed notes for each paper are currently a work in progress. These sections will be updated in the `main` branch once finalized. Until then, you can find the basic information about each paper and follow our progress through ongoing updates.


| Field                                  | Name                                                                 | Summary                                                                                                                      |    Note    |    Reproduction    |
|----------------------------------------|----------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|:-----------:|:------------------:|
| Foundational Theory and Neural Networks | [CS231n Convolutional Neural Networks for Visual Recognition](#27-cs231n-convolutional-neural-networks-for-visual-recognition) | A comprehensive course from Stanford University that covers the basics of CNNs, architectures, and training techniques—a great starting point for learning visual recognition. | [note](notes/27-CS231n%20Convolutional%20Neural%20Networks%20for%20Visual%20Recognition.md) | [code](codes/27-cs231n-convolutional-neural-networks-for-visual-recognition) |
| Foundational Theory and Neural Networks | [The Unreasonable Effectiveness of Recurrent Neural Networks](#4-the-unreasonable-effectiveness-of-recurrent-neural-networks) | Emphasizes the powerful capabilities of RNNs in handling sequential data, further solidifying your understanding of RNNs. | [note](notes/06-Recurrent%20Neural%20Networks%20Regularization.md) | [code](codes/4-the-unreasonable-effectiveness-of-recurrent-neural-networks) |
| Foundational Theory and Neural Networks | [Understanding LSTM Networks](#5-understanding-lstm-networks)        | Introduces LSTMs and their advantages in handling long-term dependencies, forming a foundation for understanding RNNs. | [note](notes/05-Understanding%20LSTM%20Networks.md) | [code](codes/5-understanding-lstm-networks) |
| Foundational Theory and Neural Networks | [Recurrent Neural Network Regularization](#6-recurrent-neural-network-regularization) | Learn how to optimize RNNs and LSTMs by reducing overfitting through regularization techniques. | [note](notes/06-Recurrent%20Neural%20Networks%20Regularization.md) | [code](codes/6-recurrent-neural-network-regularization) |
| Foundational Theory and Neural Networks | [ImageNet Classification with Deep Convolutional Neural Networks](#9-imagenet-classification-with-deep-convolutional-neural-networks) | Understand the basics of Convolutional Neural Networks (CNNs) and their applications in image recognition. | [note](notes/09-ImageNet%20Classification%20with%20Deep%20Convolutional%20Neural%20Networks.md) | [code](codes/9-imagenet-classification-with-deep-convolutional-neural-networks) |
| Foundational Theory and Neural Networks | [Deep Residual Learning for Image Recognition](#12-deep-residual-learning-for-image-recognition) | Learn how ResNet addresses training challenges in deep networks and enhances accuracy. | [note](notes/12-Deep%20Residual%20Learning%20for%20Image%20Recognition.md) | [code](codes/12-deep-residual-learning-for-image-recognition) |
| Foundational Theory and Neural Networks | [Multi-Scale Context Aggregation by Dilated Convolutions](#13-multi-scale-context-aggregation-by-dilated-convolutions) | Delve into using dilated convolutions in semantic segmentation tasks to achieve multi-scale context aggregation. | [note](notes/13-Multi-Scale%20Context%20Aggregation%20by%20Dilated%20Convolutions.md) | [code](codes/13-multi-scale-context-aggregation-by-dilated-convolutions) |
| Machine Translation and NLP            | [Neural Machine Translation by Jointly Learning to Align and Translate](#15-neural-machine-translation-by-jointly-learning-to-align-and-translate) | Study the foundational models and alignment mechanisms in neural machine translation. | [note](notes/15-Neural%20Machine%20Translation%20by%20Jointly%20Learning%20to%20Align%20and%20Translate.md) | [code](codes/15-neural-machine-translation-by-jointly-learning-to-align-and-translate) |
| Machine Translation and NLP            | [Attention Is All You Need](#1-attention-is-all-you-need)            | No explanation needed—the seminal Transformer paper that is a must-read.                                                    | [note](notes/01-Attention%20Is%20All%20You%20Need.md) ✅                       | [code](codes/1-attention-is-all-you-need)               |
| Machine Translation and NLP            | [The Annotated Transformer](#2-the-annotated-transformer)            | Deeply understand the Transformer model's implementation details and code through practical explanations. | [note](notes/02-The%20Annotated%20Transformer.md) | [code](codes/2-the-annotated-transformer) |
| Machine Translation and NLP            | [Pointer Networks](#8-pointer-networks)                              | Learn about a new network structure that addresses variable-length output sequences, suitable for tasks like sorting and combinatorial optimization. | [note](notes/08-Pointer%20Networks.md) | [code](codes/8-pointer-networks) |
| Machine Translation and NLP            | [Scaling Laws for Neural Language Models](#23-scaling-laws-for-neural-language-models) | Explore factors affecting the performance of large language models and their scaling laws. | [note](notes/23-Scaling%20Laws%20for%20Neural%20Language%20Model.md) | [code](codes/23-scaling-laws-for-neural-language-models) |
| Deep Learning and Optimization         | [GPipe: Easy Scaling with Micro-Batch Pipeline Parallelism](#11-gpipe-easy-scaling-with-micro-batch-pipeline-parallelism) | Learn how to scale neural network capacity through micro-batch pipeline parallelism and understand parallel training techniques for large models. | [note](notes/11-GPipe%20Easy%20Scaling%20with%20Micro-Batch%20Pipeline%20Parallelism.md) | [code](codes/11-gpipe-easy-scaling-with-micro-batch-pipeline-parallelism) |
| Deep Learning and Optimization         | [Deep Speech 2: End-to-End Speech Recognition in English and Mandarin](#22-deep-speech-2-end-to-end-speech-recognition-in-english-and-mandarin) | Understand how to build and optimize end-to-end speech recognition systems, especially for handling different languages. | [note](notes/22-Deep%20Speech%202%20End-to-End%20Speech%20Recognition%20in%20English%20and%20Mandarin.md) | [code](codes/22-deep-speech-2-end-to-end-speech-recognition-in-english-and-mandarin) |
| Graph Structures and Relational Reasoning | [Neural Message Passing for Quantum Chemistry](#14-neural-message-passing-for-quantum-chemistry) | Study the application of graph neural networks in quantum chemistry, a key to understanding supervised learning on graph data structures. | [note](notes/14-Neural%20Message%20Passing%20for%20Quantum%20Chemistry.md) | [code](codes/14-neural-message-passing-for-quantum-chemistry) |
| Graph Structures and Relational Reasoning | [A Simple Neural Network Module for Relational Reasoning](#17-a-simple-neural-network-module-for-relational-reasoning) | Learn how to enhance existing neural networks with relational reasoning modules for unstructured input. | [note](notes/17-A%20simple%20neural%20network%20module%20for%20relational%20reasoning.md) | [code](codes/17-a-simple-neural-network-module-for-relational-reasoning) |
| Graph Structures and Relational Reasoning | [Relational Recurrent Neural Networks](#19-relational-recurrent-neural-networks) | Explore new memory modules that perform relational reasoning in sequential data, combining memory and reasoning. | [note](notes/19-Relational%20recurrent%20neural%20networks.md) | [code](codes/19-relational-recurrent-neural-networks) |
| Generative Models and Complexity       | [Variational Lossy Autoencoder](#18-variational-lossy-autoencoder)   | Explore the combination of autoregressive models and variational autoencoders to achieve complex generative tasks. | [note](notes/18-Variational%20Lossy%20Autoencoder.md) | [code](codes/18-variational-lossy-autoencoder) |
| Generative Models and Complexity       | [The First Law of Complexodynamics](#3-the-first-law-of-complexodynamics) | Understand the evolution of complexity in physical systems and its relation to Kolmogorov complexity. | [note](notes/03-The%20First%20Law%20of%20Complexodynamics.md) | [code](codes/3-the-first-law-of-complexodynamics) |
| Generative Models and Complexity       | [Kolmogorov Complexity and Algorithmic Randomness](#26-kolmogorov-complexity-and-algorithmic-randomness) | Learn Kolmogorov complexity theory and its applications in algorithmic randomness, laying the foundation for unsupervised learning. | [note](notes/26-Algorithmic%20Statistics.md) | [code](codes/26-kolmogorov-complexity-and-algorithmic-randomness) |
| Generative Models and Complexity       | [Quantifying the Rise and Fall of Complexity in Closed Systems: The Coffee Automaton](#20-quantifying-the-rise-and-fall-of-complexity-in-closed-systems-the-coffee-automaton) | Delve into the quantitative measurement of complexity in closed systems and understand the trend of complexity over time. | [note](notes/20-Quantifying%20the%20Rise%20and%20Fall%20of%20Complexity%20in%20Closed%20Systems%20The%20Coffee%20Automaton.md) | [code](codes/20-quantifying-the-rise-and-fall-of-complexity-in-closed-systems-the-coffee-automaton) |
| Advanced Topics                       | [Neural Turing Machines](#21-neural-turing-machines)                 | Combine neural networks with the concept of Turing machines to expand your understanding of neural network architectures. | [note](notes/21-Neural%20Turing%20Machines.md) | [code](codes/21-neural-turing-machines) |
| Advanced Topics                       | [Identity Mappings in Deep Residual Networks](#16-identity-mappings-in-deep-residual-networks) | Further study the internal mechanisms of ResNet and the importance of identity mappings in information propagation. | [note](notes/16-Identity%20Mappings%20in%20Deep%20Residual%20Networks.md) | [code](codes/16-identity-mappings-in-deep-residual-networks) |
| Advanced Topics                       | [Order Matters: Sequence to Sequence for Sets](#10-order-matters-sequence-to-sequence-for-sets) | Explore how to handle cases where inputs and outputs are not necessarily ordered sequences. | [note](notes/10-Order%20Matters%20Sequence%20to%20Sequence%20for%20Sets.md) | [code](codes/10-order-matters-sequence-to-sequence-for-sets) |
| Advanced Topics                       | [Keeping Neural Networks Simple by Minimizing the Description Length of the Weights](#7-keeping-neural-networks-simple-by-minimizing-the-description-length-of-the-weights) | Learn how the Minimum Description Length principle applies to simplifying neural networks and balancing weight information content. | [note](notes/07-Keeping%20Neural%20Networks%20Simple%20by%20Minimizing%20the%20Description%20Length%20of%20the%20Weights.md) | [code](codes/7-keeping-neural-networks-simple-by-minimizing-the-description-length-of-the-weights) |
| Advanced Topics                       | [A Tutorial Introduction to the Minimum Description Length Principle](#24-a-tutorial-introduction-to-the-minimum-description-length-principle) | Deepen your understanding of the MDL principle and its applications in model selection and data compression. | [note](notes/24-A%20Tutorial%20Introduction%20to%20the%20Minimum%20Description%20Length%20Principle.md) | [code](codes/24-a-tutorial-introduction-to-the-minimum-description-length-principle) |
| Advanced Topics                       | [Machine Super Intelligence](#25-machine-super-intelligence)         | Explore the concepts and research progress in machine superintelligence, understanding the definition of intelligence and its quantifiable standards. | [note](notes/25-Machine_Super_Intelligence.md) | [code](codes/25-machine-super-intelligence) |


---


## Full Paper List

### 1) Attention Is All You Need

No explanation needed—the seminal Transformer paper that is a must-read.

Link: https://arxiv.org/pdf/1706.03762

---

### 2) The Annotated Transformer

This article is a blog post written in 2018 by researchers including Alexander Rush, an associate professor at Cornell University. It provides a line-by-line explanation of the Transformer and includes a complete Python implementation. This helps readers understand the theory while deepening their knowledge through practice.

Article: https://nlp.seas.harvard.edu/2018/04/03/attention.html

Code: https://github.com/harvardnlp/annotated-transformer/

---

### 3) The First Law of Complexodynamics

This is an article titled "The First Law of Complexodynamics" by Scott Aaronson, discussing why the "complexity" or "interestingness" of physical systems seems to increase over time, reach a maximum, and then decrease, while entropy increases monotonically. Aaronson attempts to explain this phenomenon using Kolmogorov complexity and related concepts, pointing out several challenges and possible solutions in this field.

Article: https://scottaaronson.blog/?p=762

---

### 4) The Unreasonable Effectiveness of Recurrent Neural Networks

This article, written by Andrej Karpathy in 2015, emphasizes the effectiveness of Recurrent Neural Networks (RNNs). It explores the powerful capabilities of RNNs in handling sequential data.

Link: https://karpathy.github.io/2015/05/21/rnn-effectiveness/

---

### 5) Understanding LSTM Networks

Written in 2015 by Christopher Olah, this article introduces Long Short-Term Memory (LSTM) networks, a special kind of RNN capable of handling long-term dependencies. LSTMs have achieved great success in fields like speech recognition, language modeling, translation, and image captioning.

Link: https://colah.github.io/posts/2015-08-Understanding-LSTMs/

---

### 6) Recurrent Neural Network Regularization

Authored by Ilya Sutskever in 2015, this paper proposes a simple regularization technique for Recurrent Neural Networks (RNNs) with Long Short-Term Memory (LSTM) units. The paper demonstrates how to correctly apply dropout to LSTM networks, significantly reducing overfitting across various tasks, including language modeling, speech recognition, image caption generation, and machine translation.

Link: https://arxiv.org/pdf/1409.2329.pdf

---

### 7) Keeping Neural Networks Simple by Minimizing the Description Length of the Weights

This paper discusses how the generalization ability of supervised neural networks improves when the weights contain less information than the output vectors of the training cases. By penalizing the information content of the weights during learning, the weights are kept simple. This can be achieved by adding Gaussian noise to control the information content of the weights.

Link: https://www.cs.toronto.edu/~hinton/absps/colt93.pdf

---

### 8) Pointer Networks

This paper introduces a new neural network architecture designed to learn the conditional probability of output sequences composed of discrete tokens that represent positions in an input sequence. Existing models like sequence-to-sequence and Neural Turing Machines struggle with problems where the target output dictionary size depends on the input length, such as sorting variable-length sequences and various combinatorial optimization problems.

Link: https://arxiv.org/pdf/1506.03134

---

### 9) ImageNet Classification with Deep Convolutional Neural Networks

Authored by Geoffrey Hinton, Ilya Sutskever, and others, this groundbreaking paper introduced AlexNet, revolutionizing image recognition and kickstarting the deep learning revolution. They trained a large, deep convolutional neural network to classify the 1.2 million high-resolution images in the ImageNet LSVRC-2010 contest into 1000 different classes.

Link: https://proceedings.neurips.cc/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf

---

### 10) Order Matters: Sequence to Sequence for Sets

The paper explores how the order in which data is organized affects the learning of underlying patterns. The authors investigate an extension of the sequence-to-sequence (seq2seq) framework to go beyond sequences and handle input sets in a principled way. They also propose a loss function to address the lack of structure in output sets by exploring different data sequences during training.

Link: https://arxiv.org/pdf/1511.06391

---

### 11) GPipe: Easy Scaling with Micro-Batch Pipeline Parallelism

This paper introduces GPipe, a model parallelism library that allows scaling the capacity of large neural networks through micro-batch pipeline parallelism. The authors demonstrate its application in image classification and multilingual neural machine translation tasks.

Link: https://arxiv.org/pdf/1811.06965

---

### 12) Deep Residual Learning for Image Recognition

Authored by Kaiming He, this 2016 CVPR Best Paper describes the Deep Residual Learning framework, significantly reducing the difficulty of training deeper neural networks and improving accuracy.

Link: https://arxiv.org/pdf/1512.03385

---

### 13) Multi-Scale Context Aggregation by Dilated Convolutions

The authors develop a novel convolutional network module tailored for dense prediction tasks like semantic segmentation. This module uses dilated convolutions to effectively aggregate multi-scale contextual information without reducing image resolution.

Link: https://arxiv.org/pdf/1511.07122

---

### 14) Neural Message Passing for Quantum Chemistry

The paper summarizes and organizes existing neural network models for graph-structured data that the authors believe are most promising. It proposes a general framework for supervised learning on graphs called Message Passing Neural Networks (MPNNs).

Link: https://arxiv.org/pdf/1704.01212

---

### 15) Neural Machine Translation by Jointly Learning to Align and Translate

Published in 2014 by Bahdanau et al., this paper is one of the pioneering works in neural machine translation. It introduces a novel model architecture and training method that allows neural networks to automatically search for parts of a source sentence that are relevant to predicting a target word.

Link: https://arxiv.org/pdf/1409.0473

---

### 16) Identity Mappings in Deep Residual Networks

This paper, also authored by Kaiming He, further analyzes the propagation mechanisms behind residual blocks. The authors propose a new residual unit that simplifies the training process and improves model generalization.

Link: https://arxiv.org/pdf/1603.05027

---

### 17) A Simple Neural Network Module for Relational Reasoning

To explore relational reasoning further and test whether this capability can be easily added to existing systems, DeepMind researchers developed a simple, plug-and-play module called the Relation Network (RN). This module can be inserted into existing neural network architectures to equip them with the ability to reason about relationships between entities.

Link: https://arxiv.org/pdf/1706.01427

---

### 18) Variational Lossy Autoencoder

This paper successfully combines autoregressive models with Variational Autoencoders (VAEs) to achieve generative tasks. It addresses the issue where VAEs tend to ignore some latent representations during training and introduces the Variational Lossy Autoencoder (VLAE).

Link: https://arxiv.org/pdf/1611.02731

---

### 19) Relational Recurrent Neural Networks

This paper from DeepMind and University College London introduces the Relational Memory Core (RMC), capable of performing relational reasoning in sequential information. It achieves state-of-the-art performance on the WikiText-103, Project Gutenberg, and GigaWord datasets.

Link: https://arxiv.org/pdf/1806.01822

---

### 20) Quantifying the Rise and Fall of Complexity in Closed Systems: The Coffee Automaton

This paper attempts to measure the pattern where the "complexity" or "interestingness" of closed systems increases over time, reaches a maximum, and then decreases, unlike entropy, which increases monotonically. The authors use a simple two-dimensional cellular automaton model to simulate the mixing of two liquids ("coffee" and "cream") and propose "structural complexity" as an approximate measure of Kolmogorov complexity.

Link: https://arxiv.org/pdf/1405.6903

Further Reading: [Beauty and Structural Complexity (in Chinese)](https://zhuanlan.zhihu.com/p/87743946)

---

### 21) Neural Turing Machines

Neural Turing Machines (NTMs) are a deep learning algorithm that combines neural networks and the concept of Turing machines. The paper enhances the capabilities of neural networks by coupling them to external memory resources, with which they can interact using attention mechanisms.

Link: https://arxiv.org/pdf/1410.5401

---

### 22) Deep Speech 2: End-to-End Speech Recognition in English and Mandarin

Published by Baidu Research's Silicon Valley AI Lab, the authors demonstrate an end-to-end deep learning approach that can recognize English and Mandarin speech. They replace hand-engineered components with neural networks, handling various speech scenarios, including noisy environments and different accents.

Link: https://arxiv.org/pdf/1512.02595.pdf

---

### 23) Scaling Laws for Neural Language Models

A classic paper from OpenAI, the authors explore the factors that affect language model performance in terms of cross-entropy loss. They find that model size, dataset size, and training compute affect the loss and can be largely traded off against each other.

Link: https://arxiv.org/pdf/2001.08361

---

### 24) A Tutorial Introduction to the Minimum Description Length Principle

This paper provides a tutorial introduction to the Minimum Description Length (MDL) principle, a method for model selection and data compression.

Link: https://arxiv.org/pdf/math/0406077

---

### 25) Machine Super Intelligence

Authored by DeepMind co-founder and chief scientist Shane Legg, this 2008 doctoral thesis is considered one of the earliest academic papers to systematically explore Artificial General Intelligence (AGI). It lays the foundation for subsequent research in the field.

Link: https://www.vetta.org/documents/Machine_Super_Intelligence.pdf

---

### 26) Kolmogorov Complexity and Algorithmic Randomness

Published by the American Mathematical Society, this book by A. Shen, V. A. Uspenskii, and N. K. Vereshchagin introduces Kolmogorov complexity theory and its applications in algorithmic randomness, providing a theoretical foundation for understanding computational complexity and randomness.

Link: https://www.lirmm.fr/~ashen/kolmbook-eng-scan.pdf

---

### 27) CS231n Convolutional Neural Networks for Visual Recognition

CS231n is a machine learning course at Stanford University, focusing on using Convolutional Neural Networks for visual recognition. It comprehensively covers CNN architectures, training techniques, and the latest research findings.

Link: https://cs231n.github.io/

---

## References

Ref: Exclusive Q&A: John Carmack’s ‘Different Path’ to Artificial General Intelligence

>"So I asked Ilya Sutskever, OpenAI’s chief scientist, for a reading list. He gave me a list of like 40 research papers and said, ‘If you really learn all of these, you’ll know 90% of what matters today.’ And I did. I plowed through all those things and it all started sorting out in my head."

Ref: https://x.com/ID_AA_Carmack/status/1622673143469858816

>I rather expected @ilyasut to have made a public post by now after all the discussion of the AI reading list he gave me. A canonical list of references from a leading figure would be appreciated by many. I would be curious myself about what he would add from the last three years.

