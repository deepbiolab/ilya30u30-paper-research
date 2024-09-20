# Deep Speech 2: End-to-End Speech Recognition in English and Mandarin  

# Baidu Research – Silicon Valley AI Lab ∗  

Dario Amodei, Rishita Anubhai, Eric Battenberg, Carl Case, Jared Casper, Bryan Catanzaro, Jingdong Chen, Mike Chrzanowski, Adam Coates, Greg Diamos, Erich Elsen, Jesse Engel, Linxi Fan, Christopher Fougner, Tony Han, Awni Hannun, Billy Jun, Patrick LeGresley, Libby Lin, Sharan Narang, Andrew   $\mathrm{Mg}$  , Sherjil Ozair, Ryan Prenger, Jonathan Raiman, Sanjeev Satheesh, David Seetapun, Shubho Sengupta, Yi Wang, Zhiqian Wang, Chong Wang, Bo Xiao, Dani Yogatama, Jun Zhan, Zhenyao Zhu  

# Abstract  

We show that an end-to-end deep learning approach can be used to recognize either English or Mandarin Chinese speech—two vastly different languages. Be- cause it replaces entire pipelines of hand-engineered components with neural net- works, end-to-end learning allows us to handle a diverse variety of speech includ- ing noisy environments, accents and different languages. Key to our approach is our application of HPC techniques, resulting in a   $7\mathbf{x}$   speedup over our previous system [26]. Because of this efﬁciency, experiments that previously took weeks now run in days. This enables us to iterate more quickly to identify superior ar- chitectures and algorithms. As a result, in several cases, our system is competitive with the transcription of human workers when benchmarked on standard datasets. Finally, using a technique called Batch Dispatch with GPUs in the data center, we show that our system can be inexpensively deployed in an online setting, deliver- ing low latency when serving users at scale.  

# 1 Introduction  

Decades worth of hand-engineered domain knowledge has gone into current state-of-the-art auto- matic speech recognition (ASR) pipelines. A simple but powerful alternative solution is to train such ASR models end-to-end, using deep learning to replace most modules with a single model [26]. We present the second generation of our speech system that exempliﬁes the major advantages of end- to-end learning. The Deep Speech 2 ASR pipeline approaches or exceeds the accuracy of Amazon Mechanical Turk human workers on several benchmarks, works in multiple languages with little modiﬁcation, and is deployable in a production setting. It thus represents a signiﬁcant step towards a single ASR system that addresses the entire range of speech recognition contexts handled by hu- mans. Since our system is built on end-to-end deep learning, we can employ a spectrum of deep learning techniques: capturing large training sets, training larger models with high performance computing, and methodically exploring the space of neural network architectures. We show that through these techniques we are able to reduce error rates of our previous end-to-end system [26] in English by up to  $43\%$  , and can also recognize Mandarin speech with high accuracy.  

One of the challenges of speech recognition is the wide range of variability in speech and acoustics. As a result, modern ASR pipelines are made up of numerous components including complex feature extraction, acoustic models, language and pronunciation models, speaker adaptation, etc. Build- ing and tuning these individual components makes developing a new speech recognizer very hard, especially for a new language. Indeed, many parts do not generalize well across environments or languages and it is often necessary to support multiple application-speciﬁc systems in order to pro- vide acceptable accuracy. This state of affairs is different from human speech recognition: people have the innate ability to learn any language during childhood, using general skills to learn language. After learning to read and write, most humans can transcribe speech with robustness to variation in environment, speaker accent and noise, without additional training for the transcription task. To meet the expectations of speech recognition users, we believe that a single engine must learn to be similarly competent; able to handle most applications with only minor modiﬁcations and able to learn new languages from scratch without dramatic changes. Our end-to-end system puts this goal within reach, allowing us to approach or exceed the performance of human workers on several tests in two very different languages: Mandarin and English.  

Since Deep Speech 2 (DS2) is an end-to-end deep learning system, we can achieve performance gains by focusing on three crucial components: the model architecture, large labeled training datasets, and computational scale. This approach has also yielded great advances in other appli- cation areas such as computer vision and natural language. This paper details our contribution to these three areas for speech recognition, including an extensive investigation of model architectures and the effect of data and model size on recognition performance. In particular, we describe numer- ous experiments with neural networks trained with the Connectionist Temporal Classiﬁcation (CTC) loss function [22] to predict speech transcriptions from audio. We consider networks composed of many layers of recurrent connections, convolutional ﬁlters, and nonlinearities, as well as the impact of a speciﬁc instance of Batch Normalization [63] (BatchNorm) applied to RNNs. We not only ﬁnd networks that produce much better predictions than those in previous work [26], but also ﬁnd instances of recurrent models that can be deployed in a production setting with no signiﬁcant loss in accuracy.  

Beyond the search for better model architecture, deep learning systems beneﬁt greatly from large quantities of training data. We detail our data capturing pipeline that has enabled us to create larger datasets than what is typically used to train speech recognition systems. Our English speech system is trained on 11,940 hours of speech, while the Mandarin system is trained on 9,400 hours. We use data synthesis to further augment the data during training.  

Training on large quantities of data usually requires the use of larger models. Indeed, our models have many more parameters than those used in our previous system. Training a single model at these scales requires tens of exaFLOPs 1   that would require 3-6 weeks to execute on a single GPU. This makes model exploration a very time consuming exercise, so we have built a highly optimized training system that uses 8 or 16 GPUs to train one model. In contrast to previous large-scale training approaches that use parameter servers and asynchronous updates [18, 10], we use synchronous SGD, which is easier to debug while testing new ideas, and also converges faster for the same degree of data parallelism. To make the entire system efﬁcient, we describe optimizations for a single GPU as well as improvements to scalability for multiple GPUs. We employ optimization techniques typically found in High Performance Computing to improve scalability. These optimizations include a fast implementation of the CTC loss function on the GPU, and a custom memory allocator. We also use carefully integrated compute nodes and a custom implementation of all-reduce to accelerate inter-GPU communication. Overall the system sustains approximately 50 teraFLOP/second when training on 16 GPUs. This amounts to 3 teraFLOP/second per GPU which is about   $50\%$   of peak theoretical performance. This scalability and efﬁciency cuts training times down to 3 to 5 days, allowing us to iterate more quickly on our models and datasets.  

We benchmark our system on several publicly available test sets and compare the results to our previous end-to-end system [26]. Our goal is to eventually reach human-level performance not only on speciﬁc benchmarks, where it is possible to improve through dataset-speciﬁc tuning, but on a range of benchmarks that reﬂects a diverse set of scenarios. To that end, we have also measured the performance of human workers on each benchmark for comparison. We ﬁnd that our system outperforms humans in some commonly-studied benchmarks and has signiﬁcantly closed the gap in much harder cases. In addition to public benchmarks, we show the performance of our Mandarin system on internal datasets that reﬂect real-world product scenarios.  

Deep learning systems can be challenging to deploy at scale. Large neural networks are compu- tationally expensive to evaluate for each user utterance, and some network architectures are more easily deployed than others. Through model exploration, we ﬁnd high-accuracy, deployable net- work architectures, which we detail here. We also employ a batching scheme suitable for GPU hardware called Batch Dispatch that leads to an efﬁcient, real-time implementation of our Mandarin engine on production servers. Our implementation achieves a 98th percentile compute latency of 67 milliseconds, while the server is loaded with 10 simultaneous audio streams.  

The remainder of the paper is as follows. We begin with a review of related work in deep learning, end-to-end speech recognition, and scalability in Section 2. Section 3 describes the architectural and algorithmic improvements to the model and Section 4 explains how to efﬁciently compute them. We discuss the training data and steps taken to further augment the training set in Section 5. An analysis of results for the DS2 system in English and Mandarin is presented in Section 6. We end with a description of the steps needed to deploy DS2 to real users in Section 7.  

# 2 Related Work  

This work is inspired by previous work in both deep learning and speech recognition. Feed-forward neural network acoustic models were explored more than 20 years ago [7, 50, 19]. Recurrent neu- ral networks and networks with convolution were also used in speech recognition around the same time [51, 67]. More recently DNNs have become a ﬁxture in the ASR pipeline with almost all state of the art speech work containing some form of deep neural network [42, 29, 17, 16, 43, 58]. Convolutional networks have also been found beneﬁcial for acoustic models [1, 53]. Recurrent neural networks, typically LSTMs, are just beginning to be deployed in state-of-the art recogniz- ers [24, 25, 55] and work well together with convolutional layers for the feature extraction [52]. Models with both bidirectional [24] and unidirectional recurrence have been explored as well.  

End-to-end speech recognition is an active area of research, showing compelling results when used to re-score the outputs of a DNN-HMM [23] and standalone [26]. Two methods are currently used to map variable length audio sequences directly to variable length transcriptions. The RNN encoder- decoder paradigm uses an encoder RNN to map the input to a ﬁxed length vector and a decoder network to expand the ﬁxed length vector into a sequence of output predictions [11, 62]. Adding an attentional mechanism to the decoder greatly improves performance of the system, particularly with long inputs or outputs [2]. In speech, the RNN encoder-decoder with attention performs well both in predicting phonemes [12] or graphemes [3, 8].  

The other commonly used technique for mapping variable length audio input to variable length output is the CTC loss function [22] coupled with an RNN to model temporal information. The CTC- RNN model performs well in end-to-end speech recognition with grapheme outputs [23, 27, 26, 40]. The CTC-RNN model has also been shown to work well in predicting phonemes [41, 54], though a lexicon is still needed in this case. Furthermore it has been necessary to pre-train the CTC-RNN network with a DNN cross-entropy network that is fed frame-wise alignments from a GMM-HMM system [54]. In contrast, we train the CTC-RNN networks from scratch without the need of frame- wise alignments for pre-training.  

Exploiting scale in deep learning has been central to the success of the ﬁeld thus far [36, 38]. Train- ing on a single GPU resulted in substantial performance gains [49], which were subsequently scaled linearly to two [36] or more GPUs [15]. We take advantage of work in increasing individual GPU efﬁciency for low-level deep learning primitives [9]. We build on the past work in using model- parallelism [15], data-parallelism [18] or a combination of the two [64, 26] to create a fast and highly scalable system for training deep RNNs in speech recognition.  

Data has also been central to the success of end-to-end speech recognition, with over 7000 hours of labeled speech used in Deep Speech 1 (DS1) [26]. Data augmentation has been highly effective in improving the performance of deep learning in computer vision [39, 56, 14]. This has also been shown to improve speech systems [21, 26]. Techniques used for data augmentation in speech range from simple noise addition [26] to complex perturbations such as simulating changes to the vocal tract length and rate of speech of the speaker [31, 35].  

Existing speech systems can also be used to bootstrap new data collection. In one approach, the authors use one speech engine to align and ﬁlter a thousand hours of read speech [46]. In another approach, a heavy-weight ofﬂine speech recognizer is used to generate transcriptions for tens of thousands of hours of speech [33]. This is then passed through a ﬁlter and used to re-train the recog- nizer, resulting in signiﬁcant performance gains. We draw inspiration from these past approaches in bootstrapping larger datasets and data augmentation to increase the effective amount of labeled data for our system.  

# 3 Model Architecture  

A simple multi-layer model with a single recurrent layer cannot exploit thousands of hours of la- belled speech. In order to learn from datasets this large, we increase the model capacity via depth. We explore architectures with up to 11 layers including many bidirectional recurrent layers and con- volutional layers. These models have nearly 8 times the amount of computation per data example as the models in Deep Speech 1 making fast optimization and computation critical. In order to optimize these models successfully, we use Batch Normalization for RNNs and a novel optimization curricu- lum we call SortaGrad. We also exploit long  strides  between RNN inputs to reduce computation per example by a factor of 3. This is helpful for both training and evaluation, though requires some modiﬁcations in order to work well with CTC. Finally, though many of our research results make use of bidirectional recurrent layers, we ﬁnd that excellent models exist using only unidirectional recurrent layers—a feature that makes such models much easier to deploy. Taken together these features allow us to tractably optimize deep RNNs and improve performance by more than   $40\%$   in both English and Mandarin error rates over the smaller baseline models.  

# 3.1 Preliminaries  

Figure 1 shows the architecture of the DS2 system which at its core is similar to the previous DS1 system [26]: a recurrent neural network (RNN) trained to ingest speech spectrograms and generate text transcriptions.  

Let a single utterance  $x^{(i)}$  and label  $y^{(i)}$  be sampled from a training set  $\begin{array}{r l}{\mathcal{X}}&{{}=}\end{array}$   $\{(x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),.\,.\,.\}$  . Each utterance,    $x^{(i)}$  , is a time-series of length    $T^{{\bar{(i)}}}$    where every time-slice is a vector of audio features,  $x_{t}^{(i)},t=0,\ldots,T^{(i)}-1$  . We use a spectrogram of power normalized audio clips as the features to the system, so  $x_{t,p}^{(i)}$    denotes the power of the  $p^{\flat}$  ’th frequency bin in the audio frame at time  $t$  . The goal of the RNN is to convert an input sequence    $x^{(i)}$    into a ﬁnal transcription  $y^{(i)}$  . For notational convenience, we drop the superscripts and use    $x$   to denote a chosen utterance and  $y$   the corresponding label.  

The outputs of the network are the graphemes of each language. At each output time-step  $t$  , the RNN makes a prediction over char  $p(\bar{\ell}_{t}|x)$  , where  $\ell_{t}$   is either a character in the alphabet or the blank symbol. In English we have  $\ell_{t}\in\{\mathbf{a}$   ∈{ a, b, c,  . . .  ,  z ,  space ,  apostrophe ,  blank } , where we have added the  apostrophe  as well as a  space  symbol to denote word boundaries. For the Mandarin system the network outputs simpliﬁed Chinese characters. We describe this in more detail in Section 3.9.  

The RNN model is composed of several layers of hidden units. The architectures we experiment with consist of one or more convolutional layers, followed by one or more recurrent layers, followed by one or more fully connected layers.  

The hidden representation at layer    $l$   is given by  $h^{l}$    with the convention that    $h^{0}$    represents the input  $x$  . The bottom of the network is one or more convolutions over the time dimension of the input. For a context window of size    $c$  , the  $i$  -th activation at time-step    $t$   of the convolutional layer is given by  

$$
h_{t,i}^{l}=f(w_{i}^{l}\circ h_{t-c:t+c}^{l-1})
$$  

where  $\bigcirc$  denotes the element-w e product between the    $i$  -th ﬁlter and the context window of the previous layers activations, and  $f$   denotes a unary nonlinear function. We use the clipped rectiﬁed- linear (ReLU) function    $\sigma(x)=\bar{\operatorname*{min}\{\operatorname*{max}\{x,0\}}},20\}$    s our nonlinearity. In some layers, usually the ﬁrst, we sub-sample by striding the convolution by  s  frames. The goal is to shorten the number of time-steps for the recurrent layers above.  

Following the convolutional layers are one or more bidirectional recurrent layers [57]. The forward   →      $\overleftarrow{h}^{l}$    in time and backward in time recurrent layer activations are computed as  

$$
\begin{array}{r}{\vec{h}_{\,t}^{\,l}=g(h_{t}^{l-1},\,\vec{h}_{\,t-1}^{\,l})}\\ {\overleftarrow{h}_{\,t}^{\,l}=g(h_{t}^{l-1},\,\overleftarrow{h}_{\,t+1}^{\,l})}\end{array}
$$  

![](images/06c1756769cdf5478c3d46f1c4a5b3d48a2079fc72700dcf01f1a18e75936c9c.jpg)  
Figure 1:  Architecture of the DS2 system used to train on both English and Mandarin speech. We explore variants of this architecture by varying the number of convolutional layers from 1 to 3 and the number of recurrent or GRU layers from 1 to 7.  

The two sets  ctivations are summed to form the output activations for the layer  $h^{l}=\overrightarrow{h}^{l}+\overleftarrow{h}^{l}$    . The function  $g(\cdot)$   can be the standard recurrent operation  

$$
\overrightarrow{h}_{\,\,t}^{l}=f(W^{l}h_{t}^{l-1}+\,\overrightarrow{U}^{l}\,\overrightarrow{h}_{\,\,t-1}^{l}+b^{l})
$$  

where  $W^{l}$    is the input-hidden weight matrix,    $\vec{U}^{l}$    is the recurrent weight matrix and  $b^{l}$    is a bias term. In this case the input-hidden weights are shared for both directions of the recurrence. The function  $g(\cdot)$   can also represent more complex recurrence operations such as the Long Short-Term Memory (LSTM) units [30] and the gated recurrent units (GRU) [11].  

After the bidirectional recurrent layers we apply one or more fully connected layers with  

$$
h_{t}^{l}=f(W^{l}h_{t}^{l-1}+b^{l})
$$  

The output layer  $L$   is a softmax computing a probability distribution over characters given by  

$$
p(\ell_{t}=k|x)=\frac{\exp(w_{k}^{L}\cdot h_{t}^{L-1})}{\sum_{j}\exp(w_{j}^{L}\cdot h_{t}^{L-1})}
$$  

The model is trained using the CTC loss function [22]. Given an input-output pair    $(x,y)$   and the current parameters of the network  $\theta$  , we c ss function  $\mathcal{L}(x,y;\theta)$   and its derivative with respect to the parameters of the network  ∇  $\nabla_{\boldsymbol{\theta}}\mathbf{\bar{L}}(x,y;\boldsymbol{\theta})$  L . This derivative is then used to update the network parameters through the backpropagation through time algorithm.  

In the following subsections we describe the architectural and algorithmic improvements made rel- ative to DS1 [26]. Unless otherwise stated these improvements are language agnostic. We report results on an English speaker held out development set which is an internal dataset containing 2048 utterances of primarily read speech. All models are trained on datasets described in Section 5. We report Word Error Rate (WER) for the English system and Character Error Rate (CER) for the Mandarin system. In both cases we integrate a language model in a beam search decoding step as described in Section 3.8.  

![](images/48143317f23f3a7431195417e3d3acb236bcc3dc26164e397761f89b39ec69ea.jpg)  
Table 1:  Comparison of WER on a training and development set for various depths of RNN, with and without BatchNorm. The number of parameters is kept constant as the depth increases, thus the number of hidden units per layer decreases. All networks have 38 million parameters. The architecture “M RNN, N total” implies 1 layer of 1D convolution at the input, M consecutive bidirectional RNN layers, and the rest as fully-connected layers with N total layers in the network.  

# 3.2 Batch Normalization for Deep RNNs  

To efﬁciently scale our model as we scale the training set, we increase the depth of the networks by adding more hidden layers, rather than making each layer larger. Previous work has examined doing so by increasing the number of consecutive bidirectional recurrent layers [24]. We explore Batch Normalization (BatchNorm) as a technique to accelerate training for such networks [63] since they often suffer from optimization issues.  

Recent research has shown that BatchNorm improves the speed of convergence of recurrent nets, without showing any improvement in generalization performance [37]. In contrast, we demonstrate that when applied to very deep networks of simple RNNs on large data sets, batch normalization substantially improves ﬁnal generalization error while greatly accelerating training.  

In a typical feed-forward layer containing an afﬁne  tion follow linearity    $f(\cdot)$  , we insert a BatchNorm transformation by applying  $f(\mathcal{B}(W h))$   instead of  $f(W h+b)$  , where  

$$
\mathcal{B}(x)=\gamma\frac{x-\mathrm{E}[x]}{\left(\mathrm{Var}[x]+\epsilon\right)^{1/2}}+\beta.
$$  

The terms  $\mathrm{E}$   and  Var  are the empirical mean and variance over a minibatch. The bias    $b$   of the layer is dropped since its effect is cancelled by mean removal. The learnable parameters    $\gamma$   and  $\beta$  allow the layer to scale and shift each hidden unit as desired. The constant    $\epsilon$   is small and positive, and is included only for numerical stability. In our convolutional layers the mean and variance are estimated over all the temporal output units for a given convolutional ﬁlter on a minibatch. The BatchNorm transformation reduces  internal covariate shift  by insulating a given layer from potentially uninteresting changes in the mean and variance of the layer’s input.  

We consider two methods of extending BatchNorm to bidirectional RNNs [37]. A natural extension is to insert a BatchNorm transformation immediately before every non-linearity. Equation 3 then becomes  

$$
\overrightarrow{h}_{\,t}^{\prime}=f(\mathcal{B}(W^{l}h_{t}^{l-1}+\overrightarrow{U}^{l}{\,\overrightarrow{h}_{\,t-1}^{\prime}})).
$$  

In this case the mean and variance statistics are accumulated over a single time-step of the minibatch. The sequential dependence between time-steps prevents averaging over all time-steps. We ﬁnd that this technique does not lead to improvements in optimization. We also tried accumulating an average over successive time-steps, so later time-steps are normalized over all present and previous time- steps. This also proved ineffective and greatly complicated backpropagation.  

We ﬁnd that  sequence-wise  normalization [37] overcomes these issues. The recurrent computation is given by  

$$
\overrightarrow{h}_{\,\,t}^{\prime}=f(\mathcal{B}(W^{l}h_{t}^{l-1})+\overrightarrow{U}^{l}\,\overrightarrow{h}_{\,\,t-1}^{\prime}).
$$  

For each hidden unit, we compute the mean and variance statistics over all items in the minibatch over the length of the sequence. Figure 2 shows that deep networks converge faster with sequence- wise normalization. Table 1 shows that the performance improvement from sequence-wise normal- ization increases with the depth of the network, with a   $12\%$   performance difference for the deepest network. When comparing depth, in order to control for model size we hold constant the total  

![](images/b6a7968cc4002e8c7a05f4211f64dcd22c09d4f0bd82e4c3cbc539319dd2acb3.jpg)  
Figure 2:  Training curves of two models trained with and without BatchNorm. We start the plot after the ﬁrst epoch of training as the curve is more difﬁcult to interpret due to the SortaGrad curriculum method mentioned in Section 3.3  

![](images/9de49d17648b2bb8b0ae15fa83c05c00e9e2cc561cddb107875d2cf6354e2517.jpg)  

number of parameters and still see strong performance gains. We would expect to see even larger improvements from depth if we held constant the number of activations per layer and added lay- ers. We also ﬁnd that BatchNorm harms generalization error for the shallowest network just as it converges slower for shallower networks.  

The BatchNorm approach works well in training, but is difﬁcult to implement for a deployed ASR system, since it is often necessary to evaluate a single utterance in deployment rather than a batch. We ﬁnd that normalizing each neuron to its mean and variance over just the sequence degrades performance. Instead, we store a running average of the mean and variance for the neuron collected during training, and use these for evaluation in deployment [63]. Using this technique, we can evaluate a single utterance at a time with better results than evaluating with a large batch.  

# 3.3 SortaGrad  

Training on examples of varying length pose some algorithmic challenges. One possible solution is truncating backpropagation through time [68], so that all examples have the same sequence length during training [52]. However, this can inhibit the ability to learn longer term dependencies. Other works have found that presenting examples in order of difﬁculty can accelerate online learning [6, 70]. A common theme in many sequence learning problems including machine translation and speech recognition is that longer examples tend to be more challenging [11].  

The CTC cost function that we use implicitly depends on the length of the utterance,  

$$
\mathcal{L}(x,y;\theta)=-\log\sum_{\ell\in\mathrm{All}(x,y)}\prod_{t}^{T}p_{\mathrm{ct}}(\ell_{t}|x;\theta).
$$  

where   $\operatorname{Align}(x,y)$   is the set of all possible alignments of the characters of the transcription    $y$   to frames of input    $x$   under the CTC operator. In equation 9, the inner term is a product over time-steps of the sequence, which shrinks with the length of the sequence since    $p_{\mathtt{c t c}}(\ell_{t}|x;\theta)<1$  . This moti- vates a curriculum learning strategy we title SortaGrad. SortaGrad uses the length of the utterance as a heuristic for difﬁculty, since long utterances have higher cost than short utterances.  

![](images/e9a7144f806951b94ab6cf59ef2ab6a6c0a47bbb6e0f0728f0e65dc2cfcd4605.jpg)  

In the ﬁrst training epoch, we iterate through the training set in increasing order of the length of the longest utterance in the minibatch. After the ﬁrst epoch, training reverts back to a random order over minibatches. Table 2 shows a comparison of training cost with and without SortaGrad on the 9 layer model with 7 recurrent layers. This effect is particularly pronounced for networks without BatchNorm, since they are numerically less stable. In some sense the two techniques substitute for one another, though we still ﬁnd gains when applying SortaGrad and BatchNorm together. Even with BatchNorm we ﬁnd that this curriculum improves numerical stability and sensitivity to small changes in training. Numerical instability can arise from different transcendental function imple- mentations in the CPU and the GPU, especially when computing the CTC cost. This curriculum gives comparable results for both implementations.  

We suspect that these beneﬁts occur primarily because long utterances tend to have larger gradients, yet we use a ﬁxed learning rate independent of utterance length. Furthermore, longer utterances are more likely to cause the internal state of the RNNs to explode at an early stage in training.  

# 3.4 Comparison of simple RNNs and GRUs  

The models we have shown so far are  simple  RNNs that have bidirectional recurrent layers with the recurrence for both the forward in time and backward in time directions modeled by Equation 3. Current research in speech and language processing has shown that having a more complex re- currence can allow the network to remember state over more time-steps while making them more computationally expensive to train [52, 8, 62, 2]. Two commonly used recurrent architectures are the Long Short-Term Memory (LSTM) units [30] and the Gated Recurrent Units (GRU) [11], though many other variations exist. A recent comprehensive study of thousands of variations of LSTM and GRU architectures showed that a GRU is comparable to an LSTM with a properly initialized forget gate bias, and their best variants are competitive with each other [32]. We decided to examine GRUs because experiments on smaller data sets showed the GRU and LSTM reach similar accuracy for the same number of parameters, but the GRUs were faster to train and less likely to diverge.  

The GRUs we use are computed by  

$$
\begin{array}{r l}&{z_{t}=\sigma\big(W_{z}x_{t}+U_{z}h_{t-1}+b_{z}\big)}\\ &{r_{t}=\sigma\big(W_{r}x_{t}+U_{r}h_{t-1}+b_{r}\big)}\\ &{\tilde{h}_{t}=f\big(W_{h}x_{t}+r_{t}\circ U_{h}h_{t-1}+b_{h}\big)}\\ &{h_{t}=(1-z_{t})h_{t-1}+z_{t}\tilde{h}_{t}}\end{array}
$$  

where    $\sigma(\cdot)$   is the sigmoid function,    $z$   and    $r$   represent the  update  and  reset  gates respectively, and we drop the layer superscripts for simplicity. We differ slightly from the standard GRU in that we multiply the hidden state  $h_{t-1}$   by  $U_{h}$   prior to scaling by the reset gate. This allows for all operations on  $h_{t-1}$   to be computed in a single matrix multiplication. The output nonlinearity  $f(\cdot)$   is typically the hyperbolic tangent function  tanh . However, we ﬁnd similar performance for  tanh  and clipped- ReLU nonlinearities and choose to use the clipped-ReLU for simplicity and uniformity with the rest of the network.  

Both GRU and simple RNN architectures beneﬁt from batch normalization and show strong re- sults with deep networks. However, Table 3 shows that for a ﬁxed number of parameters, the GRU architectures achieve better WER for all network depths. This is clear evidence of the long term dependencies inherent in the speech recognition task present both within individual words and be-  

![Table 4:  Comparison of WER for various arrangements of convolutional layers. In all cases, the convolutions are followed by 7 recurrent layers and 1 fully connected layer. For 2D-invariant convolutions the ﬁrst dimen- sion is frequency and the second dimension is time. All models have BatchNorm, SortaGrad, and 35 million parameters. ](images/7b719d2702197e2a6b45c7df9f02cfac01dd06ad50f1a6e34a07f83acdcf8e94.jpg)  

tween words. As we discuss in Section 3.8, even simple RNNs are able to implicitly learn a language model due to the large amount of training data. Interestingly, the GRU networks with 5 or more re- current layers do not signiﬁcantly improve performance. We attribute this to the thinning from 1728 hidden units per layer for 1 recurrent layer to 768 hidden units per layer for 7 recurrent layers, to keep the total number of parameters constant.  

The GRU networks outperform the simple RNNs in Table 3. However, in later results (Section 6) we ﬁnd that as we scale up the model size, for a ﬁxed computational budget the simple RNN networks perform slightly better. Given this, most of the remaining experiments use the simple RNN layers rather than the GRUs.  

# 3.5 Frequency Convolutions  

Temporal convolution is commonly used in speech recognition to efﬁciently model temporal trans- lation invariance for variable length utterances. This type of convolution was ﬁrst proposed for neural networks in speech more than 25 years ago [67]. Many neural network speech models have a ﬁrst layer that processes input frames with some context window [16, 66]. This can be viewed as a temporal convolution with a stride of one.  

Additionally, sub-sampling is essential to make recurrent neural networks computationally tractable with high sample-rate audio. The DS1 system accomplished this through the use of a spectrogram as input and temporal convolution in the ﬁrst layer with a stride parameter to reduce the number of time-steps [26].  

Convolutions in frequency and time domains, when applied to the spectral input features prior to any other processing, can slightly improve ASR performance [1, 53, 60]. Convolution in frequency attempts to model spectral variance due to speaker variability more concisely than what is pos- sible with large fully connected networks. Since spectral ordering of features is removed by fully- connected and recurrent layers, frequency convolutions work better as the ﬁrst layers of the network.  

We experiment with adding between one and three layers of convolution. These are both in the time- and-frequency domain (2D invariance) and in the time-only domain (1D invariance). In all cases we use a  same  convolution, preserving the number of input features in both frequency and time. In some cases, we specify a stride across either dimension which reduces the size of the output. We do not explicitly control for the number of parameters, since convolutional layers add a small fraction of parameters to our networks. All networks shown in Table 4 have about 35 million parameters.  

We report results on two datasets—a development set of 2048 utterances (“Regular Dev”) and a much noisier dataset of 2048 utterances (“Noisy Dev”) randomly sampled from the CHiME 2015 development datasets [4]. We ﬁnd that multiple layers of 1D-invariant convolutions provides a very small beneﬁt. The 2D-invariant convolutions improve results substantially on noisy data, while providing a small beneﬁt on clean data. The change from one layer of 1D-invariant convolution to three layers of 2D-invariant convolution improves WER by   $23.9\%$   on the noisy development set.  

![Table 5:  Comparison of WER with different amounts of striding for unigram and bigram outputs on a model with 1 layer of 1D-invariant convolution, 7 recurrent layers, and 1 fully connected layer. All models have BatchNorm, SortaGrad, and 35 million parameters. The models are compared on a development set with and without the use of a 5-gram language model. ](images/370e6e89739f1fb3597d3b4826654407954bd7f17c38d5667cfaf888e2cd66fe.jpg)  

# 3.6 Striding  

In the convolutional layers, we apply a longer stride and wider context to speed up training as fewer time-steps are required to model a given utterance. Downsampling the input sound (through FFT and convolutional striding) reduces the number of time-steps and computation required in the following layers, but at the expense of reduced performance.  

In our Mandarin models, we employ striding in the straightforward way. However, in English, striding can reduce accuracy simply because the output of our network requires at least one time- step per output character, and the number of characters in English speech per time-step is high enough to cause problems when striding 2 . To overcome this, we can enrich the English alphabet with symbols representing alternate labellings like whole words, syllables or non-overlapping    $n$  - grams. In practice, we use non-overlapping bi-graphemes or bigrams, since these are simple to construct, unlike syllables, and there are few of them compared to alternatives such as whole words. We transform unigram labels into bigram labels through a simple isomorphism.  

Non-overlapping bigrams shorten the length of the output transcription and thus allow for a decrease in the length of the unrolled RNN. The sentence  the cat sat  with non-overlapping bigrams is seg- mented as    $[t h,e,s p a c e,c a,t,s p a c e,s a,t]$  . Notice that for words with odd number of characters, the last character becomes an unigram and  space  is treated as an unigram as well. This isomorphism ensures that the same words are always composed of the same bigram and unigram tokens. The output set of bigrams consists of all bigrams that occur in the training set.  

In Table 5 we show results for both the bigram and unigram systems for various levels of striding, with or without a language model. We observe that bigrams allow for larger strides without any sacriﬁce in in the word error rate. This allows us to reduce the number of time-steps of the unrolled RNN beneﬁting both computation and memory usage.  

# 3.7 Row Convolution and Unidirectional Models  

Bidirectional RNN models are challenging to deploy in an online, low-latency setting, because they are built to operate on an entire sample, and so it is not possible to perform the transcription process as the utterance streams from the user. We have found an unidirectional architecture that performs as well as our bidirectional models. This allows us to use unidirectional, forward-only RNN layers in our deployment system.  

To accomplish this, we employ a special layer that we call row convolution, shown in Figure 3. The intuition behind this layer is that we only need a small portion of future information to make an accurate prediction at the current time-step. Suppose at time-step    $t$  , we use a future context of    $\tau$  steps. We now ha a feature matrix  $h_{t:t+\tau}=[h_{t},h_{t+1},...,h_{t+\tau}]$   of size    $d\times(\tau+1)$  . We deﬁne  parameter matrix  W  of the same size as  $h_{t:t+\tau}$  . The activations  $r_{t}$   for the new layer at time-step  t are  

![](images/8cfb344a2806035e323e6591efd5d40a2a2c7702a0c73a36b21fba36d5dec810.jpg)  
Figure 3:  Row convolution architecture with future context size of 2  

$$
r_{t,i}=\sum_{j=1}^{\tau+1}W_{i,j}h_{t+j-1,i},\;\mathrm{for}\;1\leq i\leq d.
$$  

Since the convolution-like operation in Eq. 11 is row oriented for both    $W$   and  $h_{t:t+\tau}$  , we call this layer row convolution.  

We place the row convolution layer above all recurrent layers. This has two advantages. First, this allows us to stream all computation below the row convolution layer on a ﬁner granularity given little future context is needed. Second, this results in better CER compared to the best bidirectional model for Mandarin. We conjecture that the recurrent layers have learned good feature representations, so the row convolution layer simply gathers the appropriate information to feed to the classiﬁer. Results for a unidirectional Mandarin speech system with row convolution and a comparison to a bidirectional model are given in Section 7 on deployment.  

# 3.8 Language Model  

We train our RNN Models over millions of unique utterances, which enables the network to learn a powerful implicit language model. Our best models are quite adept at spelling, without any external language constraints. Further, in our development datasets we ﬁnd many cases where our models can implicitly disambiguate homophones—for example, “he expects the Japanese agent  to  sell it for  two hundred seventy ﬁve thousand dollars”. Nevertheless, the labeled training data is small compared to the size of unlabeled text corpora that are available. Thus we ﬁnd that WER improves when we supplement our system with a language model trained from external text.  

We use an  $n$  -gram language model since they scale well to large amounts of unlabeled text [26]. For English, our language model is a Kneser-Ney smoothed 5-gram model with pruning that is trained using the KenLM toolkit [28] on cleaned text from the Common Crawl Repository 3 . The vocabulary is the most frequently used 400,000 words from 250 million lines of text, which produces a language model with about 850 million  $n$  -grams. For Mandarin, the language model is a Kneser- Ney smoothed character level 5-gram model with pruning that is trained on an internal text corpus of 8 billion lines of text. This produces a language model with about 2 billion  $n$  -grams.  

During inference we search for the transcription  $y$   that maximizes    $Q(y)$   shown in Equation 12. This is a linear combination of  log  probabilities from the CTC trained network and language model, along with a word insertion term [26].  

$$
Q(y)=\log(p_{\mathsf{c t c}}(y|x))+\alpha\log(p_{\mathsf{l m}}(y))+\beta\,\mathbf{word\_count}(y)
$$  

The weight    $\alpha$   controls the relative contributions of the language model and the CTC network. The weight  $\beta$   encourages more words in the transcription. These parameters are tuned on a development set. We use a beam search to ﬁnd the optimal transcription [27].  

![Table 6:  Comparison of WER for English and CER for Mandarin with and without a language model. These are simple RNN models with only one layer of 1D invariant convolution. ](images/95c1f88c6e779579f872959df308171aedcf2bd402cb17dacb6c99bc4e2ef7fa.jpg)  

Table 6 shows that an external language model helps both English and Mandarin speech systems. The relative improvement given by the language model drops from   $48\%$   to  $36\%$   in English and  $27\%$  to  $23\%$   in Mandarin, as we go from a model with 5 layers and 1 recurrent layer to a model with 9 layers and 7 recurrent layers. We hypothesize that the network builds a stronger implicit language model with more recurrent layers.  

The relative performance improvement from a language model is higher in English than in Mandarin. We attribute this to the fact that a Chinese character represents a larger block of information than an English character. For example, if we output directly to syllables or words in English, the model would make fewer spelling mistakes and the language model would likely help less.  

# 3.9 Adaptation to Mandarin  

The techniques that we have described so far can be used to build an end-to-end Mandarin speech recognition system that outputs Chinese characters directly. This precludes the need to construct a pronunciation model, which is often a fairly involved component for porting speech systems to other languages [59]. Direct output to characters also precludes the need to explicitly model language speciﬁc pronunciation features. For example we do not need to model Mandarin tones explicitly, as some speech systems must do [59, 45].  

The only architectural changes we make to our networks are due to the characteristics of the Chinese character set. Firstly, the output layer of the network outputs about 6000 characters, which includes the Roman alphabet, since hybrid Chinese-English transcripts are common. We incur an out of vocabulary error at evaluation time if a character is not contained in this set. This is not a major concern, as our test set has only  $0.74\%$   out of vocab characters.  

We use a character level language model in Mandarin as words are not usually segmented in text. The word insertion term of Equation 12 becomes a character insertion term. In addition, we ﬁnd that the performance of the beam search during decoding levels off at a smaller beam size. This allows us to use a beam size of 200 with a negligible degradation in CER. In Section 6.2, we show that our Mandarin speech models show roughly the same improvements to architectural changes as our English speech models.  

# 4 System Optimizations  

Our networks have tens of millions of parameters, and the training algorithm takes tens of single- precision exaFLOPs to converge. Since our ability to evaluate hypotheses about our data and mod- els depends on the ability to train models quickly, we built a highly optimized training system. This system has two main components—a deep learning library written in  $\mathrm{C++}$  , along with a high- performance linear algebra library written in both CUDA and  $\mathrm{C++}$  . Our optimized software, running on dense compute nodes with 8 Titan X GPUs per node, allows us to sustain 24 single-precision teraFLOP/second when training a single model on one node. This is    $45\%$   of the theoretical peak computational throughput of each node. We also can scale to multiple nodes, as outlined in the next subsection.  

# 4.1 Scalability and Data-Parallelism  

We use the standard technique of data-parallelism to train on multiple GPUs using synchronous SGD. Our most common conﬁguration uses a minibatch of  512  on  8  GPUs. Our training pipeline binds one process to each GPU. These processes then exchange gradient matrices during the back- propagation by using all-reduce, which exchanges a matrix between multiple processes and sums the result so that at the end, each process has a copy of the sum of all matrices from all processes.  

We ﬁnd synchronous SGD useful because it is reproducible and deterministic. We have found that the appearance of non-determinism in our system often signals a serious bug, and so having reproducibility as a goal has greatly facilitates debugging. In contrast, asynchronous methods such as asynchronous SGD with parameter servers as found in Dean et al. [18] typically do not provide reproducibility and are therefore more difﬁcult to debug. Synchronous SGD is simple to understand and implement. It scales well as we add multiple nodes to the training process.  

![](images/a1bf148e28963dcca04a75e440d987f8e26bd4fcb08382b9aa06a3e8fe52201e.jpg)  
Figure 4:  Scaling comparison of two networks—a 5 layer model with 3 recurrent layers containing 2560 hidden units in each layer and a 9 layer model with 7 recurrent layers containing 1760 hidden units in each layer. The times shown are to train 1 epoch. The 5 layer model trains faster because it uses larger matrices and is more computationally efﬁcient.  

Figure 4 shows that time taken to train one epoch halves as we double the number of GPUs that we train on, thus achieving near-linear weak scaling. We keep the minibatch per GPU constant at 64 during this experiment, effectively doubling the minibatch as we double the number of GPUs. Although we have the ability to scale to large minibatches, we typically use either 8 or 16 GPUs during training with a minibatch of 512 or 1024, in order to converge to the best result.  

Since all-reduce is critical to the scalability of our training, we wrote our own implementation of the ring algorithm [48, 65] for higher performance and better stability. Our implementation avoids extraneous copies between CPU and GPU, and is fundamental to our scalability. We conﬁgure OpenMPI with the  smcuda  transport that can send and receive buffers residing in the memory of two different GPUs by using GPUDirect. When two GPUs are in the same PCI root complex, this avoids any unnecessary copies to CPU memory. This also takes advantage of tree-structured interconnects by running multiple segments of the ring concurrently between neighboring devices. We built our implementation using MPI send and receive, along with CUDA kernels for the element- wise operations.  

Table 7 compares the performance of our all-reduce implementation with that provided by OpenMPI version 1.8.5. We report the time spent in all-reduce for a full training run that ran for one epoch on our English dataset using a 5 layer, 3 recurrent layer architecture with  2560  hidden units for all layers. In this table, we use a minibatch of 64 per GPU, expanding the algorithmic minibatch as we scale to more GPUs. We see that our implementation is considerably faster than OpenMPI’s when the communication is within a node (8 GPUs or less). As we increase the number of GPUs and increase the amount of inter-node communication, the gap shrinks, although our implementation is still 2-4X faster.  

All of our training runs use either 8 or 16 GPUs, and in this regime, our all-reduce implementation results in    $2.5\times$   faster training for the full training run, compared to using OpenMPI directly. Opti- mizing all-reduce has thus resulted in important productivity beneﬁts for our experiments, and has made our simple synchronous SGD approach scalable.  

![Table 7:  Comparison of two different all-reduce implementations. All times are in seconds. Performance gain is the ratio of OpenMPI all-reduce time to our all-reduce time. ](images/5dd44d92ac41200bab15dd4530a0f7a6d24e65785f9433bc2168a943059d2903.jpg)  

![Table 8:  Comparison of time spent in seconds in computing the CTC loss function and gradient in one epoch for two different implementations. Speedup is the ratio of CPU CTC time to GPU CTC time. ](images/c499bbe7251efcad09c8492807e6c4997bc4b847db1eb4a54b86bcecf5eaac60.jpg)  

# 4.2 GPU implementation of CTC loss function  

Calculating the CTC loss function is more complicated than performing forward and back prop- agation on our RNN architectures. Originally, we transferred activations from the GPUs to the CPU, where we calculated the loss function using an OpenMP parallelized implementation of CTC. However, this implementation limited our scalability rather signiﬁcantly, for two reasons. Firstly, it became computationally more signiﬁcant as we improved efﬁciency and scalability of the RNN itself. Secondly, transferring large activation matrices between CPU and GPU required us to spend interconnect bandwidth for CTC, rather than on transferring gradient matrices to allow us to scale using data parallelism to more processors.  

To overcome this, we wrote a GPU implementation of the CTC loss function. Our parallel imple- mentation relies on a slight refactoring to simplify the dependences in the CTC calculation, as well as the use of optimized parallel sort implementations from ModernGPU [5]. We give more details of this parallelization in the Appendix.  

Table 8 compares the performance of two CTC implementations. The GPU implementation saves us 95 minutes per epoch in English, and 25 minutes in Mandarin. This reduces overall training time by  $10.20\%$  , which is also an important productivity beneﬁt for our experiments.  

# 4.3 Memory allocation  

Our system makes frequent use of dynamic memory allocations to GPU and CPU memory, mainly to store activation data for variable length utterances, and for intermediate results. Individual al- locations can be very large; over 1 GB for the longest utterances. For these very large allocations we found that CUDA’s memory allocator and even  std::malloc  introduced signiﬁcant overhead into our application—over a 2x slowdown from using  std::malloc  in some cases. This is because both  cudaMalloc  and  std::malloc  forward very large allocations to the operating system or GPU driver to update the system page tables. This is a good optimization for systems running multiple applications, all sharing memory resources, but editing page tables is pure overhead for our system where nodes are dedicated entirely to running a single model. To get around this limitation, we wrote our own memory allocator for both CPU and GPU allocations. Our implementation follows the approach of the last level shared allocator in jemalloc: all allocations are carved out of contigu- ous memory blocks using the buddy algorithm [34]. To avoid fragmentation, we preallocate all of GPU memory at the start of training and subdivide individual allocations from this block. Simi- larly, we set the CPU memory block size that we forward to  mmap  to be substantially larger than std::malloc , at 12GB.  

![](images/1160d520a6e8528b9c57128977b73a6a28283062f9f63881fced48655798091f.jpg)  

Table 9:  Summary of the datasets used to train DS2 in English. The Wall Street Journal (WSJ), Switchboard and Fisher [13] corpora are all published by the Linguistic Data Consortium. The LibriSpeech dataset [46] is available free on-line. The other datasets are internal Baidu corpora.  

Most of the memory required for training deep recurrent networks is used to store activations through each layer for use by back propagation, not to store the parameters of the network. For example, storing the weights for a 70M parameter network with 9 layers requires approximately  $280~\mathrm{MB}$   of memory, but storing the activations for a batch of 64, seven-second utterances requires   $1.5~\mathrm{GB}$   of memory. TitanX GPUs include 12GB of GDDR5 RAM, and sometimes very deep networks can exceed the GPU memory capacity when processing long utterances. This can happen unpredictably, especially when the distribution of utterance lengths includes outliers, and it is desirable to avoid a catastrophic failure when this occurs. When a requested memory allocation exceeds available GPU memory, we allocate page-locked GPU-memory-mapped CPU memory using  cudaMallocHost  in- stead. This memory can be accessed directly by the GPU by forwarding individual memory trans- actions over PCIe at reduced bandwidth, and it allows a model to continue to make progress even after encountering an outlier.  

The combination of fast memory allocation with a fallback mechanism that allows us to slightly overﬂow available GPU memory in exceptional cases makes the system signiﬁcantly simpler, more robust, and more efﬁcient.  

# 5 Training Data  

Large-scale deep learning systems require an abundance of labeled training data. We have collected an extensive training dataset for both English and Mandarin speech models, in addition to augment- ing our training with publicly available datasets. In English we use 11,940 hours of labeled speech data containing 8 million utterances summarized in Table 9. For the Mandarin system we use 9,400 hours of labeled audio containing 11 million utterances. The Mandarin speech data consists of in- ternal Baidu corpora, representing a mix of read speech and spontaneous speech, in both standard Mandarin and accented Mandarin.  

# 5.1 Dataset Construction  

Some of the internal English (3,600 hours) and Mandarin (1,400 hours) datasets were created from raw data captured as long audio clips with noisy transcriptions. The length of these clips ranged from several minutes to more than hour, making it impractical to unroll them in time in the RNN during training. To solve this problem, we developed an alignment, segmentation and ﬁltering pipeline that can generate a training set with shorter utterances and few erroneous transcriptions.  

The ﬁrst step in the pipeline is to use an existing bidirectional RNN model trained with CTC to align the transcription to the frames of audio. For a given audio-transcript pair,    $(x,y)$  , we ﬁnd the alignment that maximizes  

$$
\ell^{*}=\operatorname*{arg\,max}_{\ell\in\mathrm{Allg}(x,y)}\prod_{t}^{T}p_{\sf c t c}(\ell_{t}|x;\theta).
$$  

This is essentially a Viterbi alignment found using a RNN model trained with CTC. Since Equation 9 integrates over the alignment, the CTC loss function is never explicitly asked to produce an accurate alignment. In principle, CTC could choose to emit all the characters of the transcription after some ﬁxed delay and this can happen with unidirectional RNNs [54]. However, we found that CTC produces an accurate alignment when trained with a bidirectional RNN.  

Following the alignment is a segmentation step that splices the audio and the corresponding aligned transcription whenever it encounters a long series of consecutive  blank  labels occurs, since this usually denotes a stretch of silence. By tuning the number of consecutive  blank s, we can tune the length of the utterances generated. For the English speech data, we also require a  space  token to be within the stretch of  blank s in order to segment only on word boundaries. We tune the segmentation to generate utterances that are on average 7 seconds long.  

The ﬁnal step in the pipeline removes erroneous examples that arise from a failed alignment. We crowd source the ground truth transcriptions for several thousand examples. The word level edit distance between the ground truth and the aligned transcription is used to produce a  good  or  bad label. The threshold for the word level edit distance is chosen such that the resulting WER of the good  portion of the development set is less than  $5\%$  . We then train a linear classiﬁer to accurately predict bad examples given the input features generated from the speech recognizer. We ﬁnd the following features useful: the raw CTC cost, the CTC cost normalized by the sequence length, the CTC cost normalized by the transcript length, the ratio of the sequence length to the transcript length, the number of words in the transcription and the number of characters in the transcription. For the English dataset, we ﬁnd that the ﬁltering pipeline reduces the WER from  $17\%$   to  $5\%$   while retaining more than  $50\%$   of the examples.  

# 5.2 Data Augmentation  

We augment our training data by adding noise to increase the effective size of our training data and to improve our robustness to noisy speech [26]. Although the training data contains some intrinsic noise, we can increase the quantity and variety of noise through augmentation. Too much noise augmentation tends to make optimization difﬁcult and can lead to worse results, and too little noise augmentation makes the system less robust to low signal-to-noise speech. We ﬁnd that a good balance is to add noise to   $40\%$   of the utterances that are chosen at random. The noise source consists of several thousand hours of randomly selected audio clips combined to produce hundreds of hours of noise.  

# 5.3 Scaling Data  

Our English and Mandarin corpora are substantially larger than those commonly reported in speech recognition literature. In Table 10, we show the effect of increasing the amount of labeled training data on WER. This is done by randomly sampling the full dataset before training. For each dataset, the model was trained for up to 20 epochs though usually early-stopped based on the error on a held out development set. We note that the WER decreases with a power law for both the regular and noisy development sets. The WER decreases by  ${\sim}40\%$   re for each factor of 10 increase in training set size. We also observe a consistent gap in WER ( ∼ 60% relative) between the regular and noisy datasets, implying that more data beneﬁts both cases equally.  

This implies that a speech system will continue to improve with more labeled training data. We hypothesize that equally as important as increasing raw number of hours is increasing the number of speech  contexts  that are captured in the dataset. A context can be any property that makes speech unique including different speakers, background noise, environment, and microphone hardware. While we do not have the labels needed to validate this claim, we suspect that measuring WER as a function of speakers in the dataset would lead to much larger relative gains than simple random sampling.  

# 6 Results  

To better assess the real-world applicability of our speech system, we evaluate on a wide range of test sets. We use several publicly available benchmarks and several test sets collected internally. Together these test sets represent a wide range of challenging speech environments including low signal-to-noise ratios (noisy and far-ﬁeld), accented, read, spontaneous and conversational speech.  

![](images/4c68437b0e40be215a3f904b29eeda3d98f89627672b2c4f79ecf5973c97b273.jpg)  

All models are trained for 20 epochs on either the full English dataset, described in Table 9, or the full Mandarin dataset described in Section 5. We use stochastic gradient descent with Nesterov momentum [61] along with a minibatch of 512 utterances. If the norm of the gradient exceeds a threshold of 400, it is rescaled to 400 [47]. The model which performs the best on a held-out  during training is chosen for evaluation. The learning rate is chosen from    $[1\times$   $10^{-4},\mathsf{\dot{6}}\times10^{-4}]$   ×  to yield fastest convergence and annealed by a constant factor of 1.2 after each epoch. We use a momentum of 0.99 for all models.  

The language models used are those described in Section 3.8. The decoding parameters from Equa- tion 12 are tuned on a held-out development set. We use a beam size of 500 for the English decoder and a beam size of 200 for the Mandarin decoder.  

# 6.1 English  

The best DS2 model has 11 layers with 3 layers of 2D convolution, 7 bidirectional recurrent layers, a fully-connected output layer along with Batch Normalization. The ﬁrst layer outputs to bigrams with a temporal stride of 3. By comparison the DS1 model has 5 layers with a single bidirectional recurrent layer and it outputs to unigrams with a temporal stride of 2 in the ﬁrst layer. We report results on several test sets for both the DS2 and DS1 model. We do not tune or adapt either model to any of the speech conditions in the test sets. Language model decoding parameters are set once on a held-out development set.  

To put the performance of our system in context, we benchmark most of our results against human workers, since speech recognition is an audio perception and language understanding problem that humans excel at. We obtain a measure of human level performance by paying workers from Amazon Mechanical Turk to hand-transcribe all of our test sets. Two workers transcribe the same audio clip, that is typically about 5 seconds long, and we use the better of the two transcriptions for the ﬁnal WER calculation. They are free to listen to the audio clip as many times as they like. These workers are mostly based in the United States, and on average spend about 27 seconds per transcription. The hand-transcribed results are compared to the existing ground truth to produce a WER. While the existing ground truth transcriptions do have some label error, this is rarely more than  $1\%$  . This implies that disagreement between the ground truth transcripts and the human level transcripts is a good heuristic for human level performance.  

# 6.1.1 Model Size  

Our English speech training set is substantially larger than the size of commonly used speech datasets. Furthermore, the data is augmented with noise synthesis. To get the best generalization error, we expect that the model size must increase to fully exploit the patterns in the data. In Sec- tion 3.2 we explored the effect of model depth while ﬁxing the number of parameters. In contrast, here we show the effect of varying model size on the performance of the speech system. We only vary the size of each layer, while keeping the depth and other architectural parameters constant. We evaluate the models on the same Regular and Noisy development sets that we use in Section 3.5.  

The models in Table 11 differ from those in Table 3 in that we increase the the stride to 3 and output to bigrams. Because we increase the model size to as many as 100 million parameters, we ﬁnd that an increase in stride is necessary for fast computation and memory constraints. However, in this regime we note that the performance advantage of the GRU networks appears to diminish over the  

![](images/722317ad8546f2477e5267e34f342bbda1c0b5ad19bc792de382dcbe11c09899.jpg)  

Table 11:  Comparing the effect of model size on the WER of the English speech system on both the regular and noisy development sets. We vary the number of hidden units in all but the convolutional layers. The GRU model has 3 layers of bidirectional GRUs with 1 layer of 2D-invariant convolution. The RNN model has 7 layers of bidirectional simple recurrence with 3 layers of 2D-invariant convolution. Both models output bigrams with a temporal stride of 3. All models contain approximately 35 million parameters and are trained with BatchNorm and SortaGrad.  

![](images/90863fbe6287b4400003d0630dafc47b08857b221f0e210f8c450635a8b85221.jpg)  

simple RNN. In fact, for the 100 million parameter networks the simple RNN performs better than the GRU network and is faster to train despite the 2 extra layers of convolution.  

Table 11 shows that the performance of the system improves consistently up to 100 million parame- ters. All further English DS2 results are reported with this same 100 million parameter RNN model since it achieves the lowest generalization errors.  

Table 12 shows that the 100 million parameter RNN model (DS2) gives a   $43.4\%$   relative improve- ment over the 5-layer model with 1 recurrent layer (DS1) on an internal Baidu dataset of 3,300 utterances that contains a wide variety of speech including challenging accents, low signal-to-noise ratios from far-ﬁeld or background noise, spontaneous and conversational speech.  

# 6.1.2 Read Speech  

Read speech with high signal-to-noise ratio is arguably the easiest large vocabulary for a continuous speech recognition task. We benchmark our system on two test sets from the Wall Street Journal (WSJ) corpus of read news articles. These are available in the LDC catalog as LDC94S13B and LDC93S6B. We also take advantage of the recently developed LibriSpeech corpus constructed using audio books from the LibriVox project [46].  

Table 13 shows that the DS2 system outperforms humans in 3 out of the 4 test sets and is competitive on the fourth. Given this result, we suspect that there is little room for a generic speech system to further improve on clean read speech without further domain adaptation.  

![Table 13:  Comparison of WER for two speech systems and human level performance on read speech. ](images/068a8902ac3d25c29ed388c13b419f8b37a1b3729a353b877be591e58341b8e3.jpg)  

![Table 14:  Comparing WER of the DS1 system to the DS2 system on accented speech. ](images/b4d1c18cbcd748f037b4c18607638de46854abf29c98635eb441ffc555e7c694.jpg)  

![](images/e2a1bae4a31713996d2cfd71507bfc9c78720977ed17807a9d2dd773d4effef9.jpg)  

# 6.1.3 Accented Speech  

Our source for accented speech is the publicly available VoxForge ( http://www.voxforge.org ) dataset, which has clean speech read from speakers with many different accents. We group these accents into four categories. The American-Canadian and Indian groups are self-explanatory. The Commonwealth accent denotes speakers with British, Irish, South African, Australian and New Zealand accents. The European group contains speakers with accents from countries in Europe that do not have English as a ﬁrst language. We construct a test set from the VoxForge data with 1024 examples from each accent group for a total of 4096 examples.  

Performance on these test sets is to some extent a measure of the breadth and quality of our training data. Table 14 shows that our performance improved on all the accents when we include more accented training data and use an architecture that can effectively train on that data. However human level performance is still notably better than that of DS2 for all but the Indian accent.  

# 6.1.4 Noisy Speech  

We test our performance on noisy speech using the publicly available test sets from the recently completed third CHiME challenge [4]. This dataset has 1320 utterances from the WSJ test set read in various noisy environments, including a bus, a cafe, a street and a pedestrian area. The CHiME set also includes 1320 utterances with simulated noise from the same environments as well as the control set containing the same utterances delivered by the same speakers in a noise-free environment. Differences between results on the control set and the noisy sets provide a measure of the network’s ability to handle a variety of real and synthetic noise conditions. The CHiME audio has 6 channels and using all of them can provide substantial performance improvements [69]. We use a  single  channel for all our results, since multi-channel audio is not pervasive on most devices. Table 15 shows that DS2 substantially improves upon DS1, however DS2 is worse than human level performance on noisy data. The relative gap between DS2 and human level performance is larger when the data comes from a real noisy environment instead of synthetically adding noise to clean speech.  

# 6.2 Mandarin  

In Table 16 we compare several architectures trained on the Mandarin Chinese speech, on a develop- ment set of 2000 utterances as well as a test set of 1882 examples of noisy speech. This development set was also used to tune the decoding parameters We see that the deepest model with 2D-invariant convolution and BatchNorm outperforms the shallow RNN by   $48\%$   relative, thus continuing the trend that we saw with the English system—multiple layers of bidirectional recurrence improves performance substantially.  

![Table 16:  Comparison of the improvements in DeepSpeech with architectural improvements. The development and test sets are Baidu internal corpora. All the models in the table have about 80 million parameters each ](images/6560dcd157ba8f4318a6b018b03c2dcae0fbf031001329850760e932545df2db.jpg)  

We ﬁnd that our best Mandarin Chinese speech system transcribes short voice-query like utterances better than a typical Mandarin Chinese speaker. To benchmark against humans we ran a test with 100 randomly selected utterances and had a group of 5 humans label all of them together. The group of humans had an error rate of   $4.0\%$   as compared to the speech systems performance of  $3.7\%$  . We also compared a single human transcriber to the speech system on 250 randomly selected utterances. In this case the speech system performs much better:   $9.7\%$   for the human compared to  $5.7\%$   for the speech model.  

# 7 Deployment  

Real-world applications usually require a speech system to transcribe in real time or with relatively low latency. The system used in Section 6.1 is not well-designed for this task, for several reasons. First, since the RNN has several bidirectional layers, transcribing the ﬁrst part of an utterance re- quires the entire utterance to be presented to the RNN. Second, since we use a wide beam when decoding with a language model, beam search can be expensive, particularly in Mandarin where the number of possible next characters is very large (around 6000). Third, as described in Section 3, we normalize power across an entire utterance, which again requires the entire utterance to be available in advance.  

We solve the power normalization problem by using some statistics from our training set to perform an adaptive normalization of speech inputs during online transcription. We can solve the other problems by modifying our network and decoding procedure to produce a model that performs almost as well while having much lower latency. We focus on our Mandarin system since some aspects of that system are more challenging to deploy (e.g. the large character set), but the same techniques could also be applied in English.  

In this section, latency refers to the computational latency of our speech system as measured from the end of an utterance until the transcription is produced. This latency does not include data trans- mission over the internet, and does not measure latency from the beginning of an utterance until the ﬁrst transcription is produced. We focus on latency from end of utterance to transcription because it is important to applications using speech recognition.  

# 7.1 Batch Dispatch  

In order to deploy our relatively large deep neural networks at low latency, we have paid special at- tention to efﬁciency during deployment. Most internet applications process requests individually as they arrive in the data center. This makes for a straightforward implementation where each request can be managed by one thread. However, processing requests individually is inefﬁcient computa- tionally, for two main reasons. Firstly, when processing requests individually, the processor must load all the weights of the network for each request. This lowers the arithmetic intensity of the work- load, and tends to make the computation memory bandwidth bound, as it is difﬁcult to effectively use on-chip caches when requests are presented individually. Secondly, the amount of parallelism that can be exploited to classify one request is limited, making it difﬁcult to exploit SIMD or multi- core parallelism. RNNs are especially challenging to deploy because evaluating RNNs sample by  

![](images/ea8c6daaa4eb83494794aed16d96321adb2bc4fa5f1d13f0e26b239ea9ba1a8f.jpg)  
Figure 5:  Probability that a request is processed in a batch of given size  

sample relies on sequential matrix vector multiplications, which are bandwidth bound and difﬁcult to parallelize.  

To overcome these issues, we built a batching scheduler called Batch Dispatch that assembles streams of data from user requests into batches before performing forward propagation on these batches. In this case, there is a tradeoff between increased batch size, and consequently improved efﬁciency, and increased latency. The more we buffer user requests to assemble a large batch, the longer users must wait for their results. This places constraints on the amount of batching we can perform.  

We use an eager batching scheme that processes each batch as soon as the previous batch is com- pleted, regardless of how much work is ready by that point. This scheduling algorithm has proved to be the best at reducing end-user latency, despite the fact that it is less efﬁcient computationally, since it does not attempt to maximize batch size.  

Figure 5 shows the probability that a request is processed in a batch of given size for our production system running on a single NVIDIA Quadro K1200 GPU, with 10-30 concurrent user requests. As expected, batching works best when the server is heavily loaded: as load increases, the distribution shifts to favor processing requests in larger batches. However, even with a light load of only 10 concurrent user requests, our system performs more than half the work in batches with at least 2 samples.  

![](images/473088636231f0ca6e7706abe49aa6c1e06b740aef0e02e82eb9f4640f0313f8.jpg)  
Figure 6:  Median and 98 percentile latencies as a function of server load  

We see in Figure 6, that our system achieves a median latency of   $44~\mathrm{ms}$  , and a 98 percentile latency of   $70\,\mathrm{ms}$   when loaded with 10 concurrent streams. As the load increases on the server, the batching scheduler shifts work to more efﬁcient batches, which keeps latency low. This shows that Batch Dispatch makes it possible to deploy these large models at high throughput and low latency.  

![](images/85f4155b0497ad6c665de4c09f00e812e30815ab4b97d884620029048d72b100.jpg)  
igure 7:  Comparison of ker  compute    $A x=b$   where  $A$   is  with dimension    $2560\times2560$  , and x  is a matrix with dimension  $2560\times$   ×  Batch size, where Batch size  ∈  $\in[1,10]$  . All matrices are in half-precision format.  

# 7.2 Deployment Optimized Matrix Multiply Kernels  

We have found that deploying our models using half-precision (16-bit) ﬂoating-point arithmetic does not measurably change recognition accuracy. Because deployment does not require any updates to the network weights, it is far less sensitive to numerical precision than training. Using half-precision arithmetic saves memory space and bandwidth, which is especially useful for deployment, since RNN evaluation is dominated by the cost of caching and streaming the weight matrices.  

As seen in Section 7.1, the batch size during deployment is much smaller than in training. We found that standard BLAS libraries are inefﬁcient at this batch size. To overcome this, we wrote our own half-precision matrix-matrix multiply kernel. For 10 simultaneous streams over 90 percent of batches are for  $N\leq4$  , a regime where the matrix multiply will be bandwidth bound. We store the  $A$  matrix transposed to maximize bandwidth by using the widest possible vector loads while avoiding transposition after loading. Each warp computes four rows of output for all  $N$   output columns. Note that for  $N\leq4$   $B$  matrix ﬁts entirely in the L1 cache. T s schem achieves 90 percent of peak bandwidth for  $N\leq4$   ≤  but starts to lose efﬁciency for larger  N  as the  B  matrix stops ﬁtting into the L1 cache. Nonetheless, it continues to provide improved performance over existing libraries up to  $N=10$  .  

Figure 7 shows that our deployment kernel sustains a higher computational throughput than those from Nervana Systems [44] on the K1200 GPU, across the entire range of batch sizes that we use in deployment. Both our kernels and the Nervana kernels are signiﬁcantly faster than NVIDIA CUBLAS version 7.0, more details are found here [20].  

# 7.3 Beam Search  

Performing the beam search involves repeated lookups in the  $n$  -gram language model, most of which translate to uncached reads from memory. The direct implementation of beam search means that each time-step dispatches one lookup per character for each beam. In Mandarin, this results in over 1M lookups per  $40\mathrm{ms}$   stride of speech data, which is too slow for deployment. To deal with this problem, we use a heuristic to further prune the beam search. Rather than considering all characters as viable additions to the beam, we only consider the fewest number of characters whose cumulative probability is at least  $p$  . In practice, we have found that  $p=0.99$   works well. Additionally, we limit ourselves to no more than 40 characters. This speeds up the Mandarin language model lookup time by a factor of   $150\mathbf{x}$  , and has a negligible effect on the CER (  $\mathrm{{}\;0.1-0.3\%}$   relative).  

# 7.4 Results  

We can deploy our system at low latency and high throughput without sacriﬁcing much accuracy. On a held-out set of 2000 utterances, our research system achieves 5.81 character error rate whereas the deployed system achieves 6.10 character error rate. This is only a  $5\%$   relative degradation for the deployed system. In order to accomplish this, we employ a neural network architecture with low deployment latency, reduce the precision of our network to 16-bit, built a batching scheduler to more efﬁciently evaluate RNNs, and ﬁnd a simple heuristic to reduce beam search cost. The model has ﬁve forward-only recurrent layers with 2560 hidden units, one row convolution layer (Section 3.7) with  $\tau=19$  , and one fully-connected layer with 2560 hidden units. These techniques allow us to deploy Deep Speech at low cost to interactive applications.  

# 8 Conclusion  

End-to-end deep learning presents the exciting opportunity to improve speech recognition systems continually with increases in data and computation. Indeed, our results show that, compared to the previous incarnation, Deep Speech has signiﬁcantly closed the gap in transcription performance with human workers by leveraging more data and larger models. Further, since the approach is highly generic, we’ve shown that it can quickly be applied to new languages. Creating high-performing recognizers for two very different languages, English and Mandarin, required essentially no expert knowledge of the languages. Finally, we have also shown that this approach can be efﬁciently deployed by batching user requests together on a GPU server, paving the way to deliver end-to-end Deep Learning technologies to users.  

To achieve these results, we have explored various network architectures, ﬁnding several effective techniques: enhancements to numerical optimization through SortaGrad and Batch Normalization, evaluation of RNNs with larger strides with bigram outputs for English, searching through both bidirectional and unidirectional models. This exploration was powered by a well optimized, High Performance Computing inspired training system that allows us to train new, full-scale models on our large datasets in just a few days.  

Overall, we believe our results conﬁrm and exemplify the value of end-to-end Deep Learning meth- ods for speech recognition in several settings. In those cases where our system is not already com- parable to humans, the difference has fallen rapidly, largely because of application-agnostic Deep Learning techniques. We believe these techniques will continue to scale, and thus conclude that the vision of a single speech system that outperforms humans in most scenarios is imminently achiev- able.  

# Acknowledgments  

We are grateful to Baidu’s speech technology group for help with data preparation and useful con- versations. We would like to thank Scott Gray, Amir Khosrowshahi and all of Nervana Systems for their excellent matrix multiply routines and useful discussions. We would also like to thank Natalia Gimelshein of NVIDIA for useful discussions and thoughts on implementing our fast deployment matrix multiply.  

# References  

[1] O. Abdel-Hamid, A.-r. Mohamed, H. Jang, and G. Penn. Applying convolutional neural networks con- cepts to hybrid nn-hmm model for speech recognition. In  ICASSP , 2012.

 [2] D. Bahdanau, K. Cho, and Y. Bengio. Neural machine translation by jointly learning to align and translate. In  ICLR , 2015.

 [3] D. Bahdanau, J. Chorowski, D. Serdyuk, P. Brakel, and Y. Bengio. End-to-end attention-based large vocabulary speech recognition. abs/1508.04395, 2015. http://arxiv.org/abs/1508.04395.

 [4] J. Barker, E. Marxer, Ricard Vincent, and S. Watanabe. The third ’CHiME’ speech separation and recogni- tion challenge: Dataset, task and baselines. 2015. Submitted to IEEE 2015 Automatic Speech Recognition and Understanding Workshop (ASRU).

 [5] S. Baxter. Modern GPU.  https://nvlabs.github.io/moderngpu/ .

 [6] Y. Bengio, J. Louradour, R. Collobert, and J. Weston. Curriculum learning. In  International Conference on Machine Learning , 2009.

 [7] H. Bourlard and N. Morgan.  Connectionist Speech Recognition: A Hybrid Approach . Kluwer Academic Publishers, Norwell, MA, 1993.  

[8] W. Chan, N. Jaitly, Q. Le, and O. Vinyals. Listen, attend, and spell. abs/1508.01211, 2015. http://arxiv.org/abs/1508.01211.

 [9] S. Chetlur, C. Woolley, P. Vandermersch, J. Cohen, J. Tran, B. Catanzaro, and E. Shelhamer. cuDNN: Efﬁcient primitives for deep learning.

 [10] T. Chilimbi, Y. Suzue, J. Apacible, and K. Kalyanaraman. Project adam: Building an efﬁcient and scalable deep learning training system. In  USENIX Symposium on Operating Systems Design and Implementation , 2014.

 [11] K. Cho, B. Van Merrienboer, C. Gulcehre, D. Bahdanau, F. Bougares, H. Schwenk, and Y. Bengio. Learn- ing phrase representations using rnn encoder-decoder for statistical machine translation. In  EMNLP , 2014.

 [12] J. Chorowski, D. Bahdanau, K. Cho, and Y. Bengio. End-to-end continuous speech recognition using attention-based recurrent nn: First results. abs/1412.1602, 2015. http://arxiv.org/abs/1412.1602.

 [13] C. Cieri, D. Miller, and K. Walker. The Fisher corpus: a resource for the next generations of speech-to- text. In  LREC , volume 4, pages 69–71, 2004.

 [14] A. Coates, B. Carpenter, C. Case, S. Satheesh, B. Suresh, T. Wang, D. J. Wu, and A. Y. Ng. Text detection and character recognition in scene images with unsupervised feature learning. In  International Conference on Document Analysis and Recognition , 2011.

 [15] A. Coates, B. Huval, T. Wang, D. J. Wu, A. Y. Ng, and B. Catanzaro. Deep learning with COTS HPC. In International Conference on Machine Learning , 2013.

 [16] G. Dahl, D. Yu, and L. Deng. Large vocabulary continuous speech recognition with context-dependent DBN-HMMs. In  Proc. ICASSP , 2011.

 [17] G. Dahl, D. Yu, L. Deng, and A. Acero. Context-dependent pre-trained deep neural networks for large vocabulary speech recognition.  IEEE Transactions on Audio, Speech, and Language Processing , 2011.

 [18] J. Dean, G. S. Corrado, R. Monga, K. Chen, M. Devin, Q. Le, M. Mao, M. Ranzato, A. Senior, P. Tucker, K. Yang, and A. Ng. Large scale distributed deep networks. In  Advances in Neural Information Processing Systems 25 , 2012.

 [19] D. Ellis and N. Morgan. Size matters: An empirical study of neural network training for large vocabulary continuous speech recognition. In  ICASSP , volume 2, pages 1013–1016. IEEE, 1999.

 [20] E. Elsen. Optimizing RNN performance.  http://svail.github.io/rnn_perf . Accessed: 2015-11-24.

 [21] M. J. F. Gales, A. Ragni, H. Aldamarki, and C. Gautier. Support vector machines for noise robust ASR. In  ASRU , pages 205–2010, 2009.

 [22] A. Graves, S. Fernández, F. Gomez, and J. Schmidhuber. Connectionist temporal classiﬁcation: Labelling unsegmented sequence data with recurrent neural networks. In  ICML , pages 369–376. ACM, 2006.

 [23] A. Graves and N. Jaitly. Towards end-to-end speech recognition with recurrent neural networks. In  ICML , 2014.

 [24] A. Graves, A.-r. Mohamed, and G. Hinton. Speech recognition with deep recurrent neural networks. In ICASSP , 2013.

 [25] H. H. Sak, A. Senior, and F. Beaufays. Long short-term memory recurrent neural network architectures for large scale acoustic modeling. In  Interspeech , 2014.

 [26] A. Hannun, C. Case, J. Casper, B. Catanzaro, G. Diamos, E. Elsen, R. Prenger, S. Satheesh, S. Sengupta, A. Coates, and A. Y. Ng. Deep speech: Scaling up end-to-end speech recognition. 1412.5567, 2014. http://arxiv.org/abs/1412.5567.

 [27] A. Y. Hannun, A. L. Maas, D. Jurafsky, and A. Y. Ng. First-pass large vocabulary continuous speech recognition using bi-directional recurrent DNNs. abs/1408.2873, 2014. http://arxiv.org/abs/1408.2873.

 [28] K. Heaﬁeld, I. Pouzyrevsky, J. H. Clark, and P. Koehn. Scalable modiﬁed Kneser-Ney language model estimation. In  Proceedings of the 51st Annual Meeting of the Association for Computational Linguistics , Soﬁa, Bulgaria, 8 2013.

 [29] G. Hinton, L. Deng, D. Yu, G. Dahl, A. Mohamed, N. Jaitly, A. Senior, V. Vanhoucke, P. Nguyen, T. Sainath, and B. Kingsbury. Deep neural networks for acoustic modeling in speech recognition.  IEEE Signal Processing Magazine , 29(November):82–97, 2012.

 [30] S. Hochreiter and J. Schmidhuber. Long short-term memory.  Neural Computation , 9(8):1735—1780, 1997.

 [31] N. Jaitly and G. Hinton. Vocal tract length perturbation (VTLP) improves speech recognition. In  ICML Workshop on Deep Learning for Audio, Speech, and Language Processing , 2013.

 [32] R. Jozefowicz, W. Zaremba, and I. Sutskever. An empirical exploration of recurrent network architectures. In  ICML , 2015.  

[33] O. Kapralova, J. Alex, E. Weinstein, P. Moreno, and O. Siohan. A big data approach to acoustic model training corpus selection. In  Interspeech , 2014.

 [34] K. C. Knowlton. A fast storage allocator.  Commun. ACM , 8(10):623–624, Oct. 1965.

 [35] T. Ko, V. Peddinti, D. Povey, and S. Khudanpur. Audio augmentation for speech recognition. In  Inter- speech , 2015.

 [36] A. Krizhevsky, I. Sutskever, and G. Hinton. Imagenet classiﬁcation with deep convolutional neural net- works. In  Advances in Neural Information Processing Systems 25 , pages 1106–1114, 2012.

 [37] C. Laurent, G. Pereyra, P. Brakel, Y. Zhang, and Y. Bengio. Batch normalized recurrent neural networks. abs/1510.01378, 2015. http://arxiv.org/abs/1510.01378.

 [38] Q. Le, M. Ranzato, R. Monga, M. Devin, K. Chen, G. Corrado, J. Dean, and A. Ng. Building high-level features using large scale unsupervised learning. In  International Conference on Machine Learning , 2012.

 [39] Y. LeCun, F. J. Huang, and L. Bottou. Learning methods for generic object recognition with invariance to pose and lighting. In  Computer Vision and Pattern Recognition , volume 2, pages 97–104, 2004.

 [40] A. Maas, Z. Xie, D. Jurafsky, and A. Ng. Lexicon-free conversational speech recognition with neural networks. In  NAACL , 2015.

 [41] Y. Miao, M. Gowayyed, and F. Metz. EESEN: End-to-end speech recognition using deep rnn models and wfst-based decoding. In  ASRU , 2015.

 [42] A. Mohamed, G. Dahl, and G. Hinton. Acoustic modeling using deep belief networks.  IEEE Transactions on Audio, Speech, and Language Processing , (99), 2011.

 [43] A. S. N. Jaitly, P. Nguyen and V. Vanhoucke. Application of pretrained deep neural networks to large vocabulary speech recognition. In  Interspeech , 2012.

 [44] Nervana Systems. Nervana GPU.  https://github.com/NervanaSystems/nervanagpu . Accessed: 2015-11-06.

 [45] J. Niu, L. Xie, L. Jia, and N. Hu. Context-dependent deep neural networks for commercial mandarin speech recognition applications. In  APSIPA , 2013.

 [46] V. Panayotov, G. Chen, D. Povey, and S. Khudanpur. Librispeech: an asr corpus based on public domain audio books. In  ICASSP , 2015.

 [47] R. Pascanu, T. Mikolov, and Y. Bengio. On the difﬁculty of training recurrent neural networks. abs/1211.5063, 2012. http://arxiv.org/abs/1211.5063.

 [48] P. Patarasuk and X. Yuan. Bandwidth optimal all-reduce algorithms for clusters of workstations.  J. Parallel Distrib. Comput. , 69(2):117–124, Feb. 2009.

 [49] R. Raina, A. Madhavan, and A. Ng. Large-scale deep unsupervised learning using graphics processors. In  26th International Conference on Machine Learning , 2009.

 [50] S. Renals, N. Morgan, H. Bourlard, M. Cohen, and H. Franco. Connectionist probability estimators in HMM speech recognition.  IEEE Transactions on Speech and Audio Processing , 2(1):161–174, 1994.

 [51] T. Robinson, M. Hochberg, and S. Renals. The use of recurrent neural networks in continuous speech recognition. pages 253–258, 1996.

 [52] T. Sainath, O. Vinyals, A. Senior, and H. Sak. Convolutional, long short-term memory, fully connected deep neural networks. In  ICASSP , 2015.

 [53] T. N. Sainath, A. rahman Mohamed, B. Kingsbury, and B. Ramabhadran. Deep convolutional neural networks for LVCSR. In  ICASSP , 2013.

 [54] H. Sak, A. Senior, K. Rao, and F. Beaufays. Fast and accurate recurrent neural network acoustic models for speech recognition. abs/1507.06947, 2015. http://arxiv.org/abs/1507.06947.

 [55] H. Sak, O. Vinyals, G. Heigold, A. Senior, E. McDermott, R. Monga, and M. Mao. Sequence discrimina- tive distributed training of long shortterm memory recurrent neural networks. In  Interspeech , 2014.

 [56] B. Sapp, A. Saxena, and A. Ng. A fast data collection and augmentation procedure for object recognition. In  AAAI Twenty-Third Conference on Artiﬁcial Intelligence , 2008.

 [57] M. Schuster and K. K. Paliwal. Bidirectional recurrent neural networks.  IEEE Transactions on Signal Processing , 45(11):2673–2681, 1997.

 [58] F. Seide, G. Li, and D. Yu. Conversational speech transcription using context-dependent deep neural networks. In  Interspeech , pages 437–440, 2011.

 [59] J. Shan, G. Wu, Z. Hu, X. Tang, M. Jansche, and P. Moreno. Search by voice in mandarin chinese. In Interspeech , 2010.

 [60] H. Soltau, G. Saon, and T. Sainath. Joint training of convolutional and non-convolutional neural networks. In  ICASSP , 2014.  

[61] I. Sutskever, J. Martens, G. Dahl, and G. Hinton. On the importance of momentum and initialization in  

[62] I. Sutskever, O. Vinyals, and Q. V. Le. Sequence to sequence learning with neural networks. 2014. http://arxiv.org/abs/1409.3215.

 [63] C. Szegedy and S. Ioffe. Batch normalization: Accelerating deep network training by reducing internal covariate shift. abs/1502.03167, 2015. http://arxiv.org/abs/1502.03167.

 [64] C. Szegedy, W. Liu, Y. Jia, P. Sermanet, S. Reed, D. Anguelov, D. Erhan, V. Vanhoucke, and A. Rabi- novich. Going deeper with convolutions. 2014.

 [65] R. Thakur and R. Rabenseifner. Optimization of collective communication operations in mpich.  Interna- tional Journal of High Performance Computing Applications , 19:49–66, 2005.

 [66] K. Vesely, A. Ghoshal, L. Burget, and D. Povey. Sequence-discriminative training of deep neural net- works. In  Interspeech , 2013.

 [67] A. Waibel, T. Hanazawa, G. Hinton, K. Shikano, and K. Lang. Phoneme recognition using time-delay neural networks,â  A I acoustics speech and signal processing.  IEEE Transactions on Acoustics, Speech and Signal Processing , 37(3):328–339, 1989.

 [68] R. Williams and J. Peng. An efﬁcient gradient-based algorithm for online training of recurrent network trajectories.  Neural computation , 2:490–501, 1990.

 [69] T. Yoshioka, N. Ito, M. Delcroix, A. Ogawa, K. Kinoshita, M. F. C. Yu, W. J. Fabian, M. Espi, T. Higuchi, S. Araki, and T. Nakatani. The ntt chime-3 system: Advances in speech enhancement and recognition for mobile multi-microphone devices. In  IEEE ASRU , 2015.  

[70] W. Zaremba and I. Sutskever. Learning to execute. abs/1410.4615, 2014. http://arxiv.org/abs/1410.4615.  

# A Scalability improvements  

In this section, we discuss some of our scalability improvements in more detail.  

# A.1 Node and cluster architecture  

The software stack runs on a compute dense node built from  2  Intel CPUs and  8  NVIDIA Titan X GPUs, with peak single-precision computational throughput of  53  teraFLOP/second. Each node also has  384  GB of CPU memory and an  8  TB storage volume built from two 4 TB hard disks in RAID-0 conﬁguration. We use the CPU memory to cache our input data so that we are not directly exposed to the low bandwidth and high latency of spinning disks. We replicate our English and Mandarin datasets on each node’s local hard disk. This allows us to use our network only for weight updates and avoids having to rely on centralized ﬁle servers.  

![](images/a4ede73f3940a8390d6adfc73e5944ac879da2bd4294a22a09d0cb3f15c65848.jpg)  
Figure 8:  Schematic of our training node where PLX indicates a PCI switch and the dotted box includes all devices that are connected by the same PCI root complex.  

Figure 8 shows a schematic diagram of one our nodes, where all devices connected by the same PCI root complex are encapsulated in a dotted box. We have tried to maximize the number of GPUs within the root complex for faster communication between GPUs using GPUDirect. This allows us to use an efﬁcient communication mechanism to transfer gradient matrices between GPUs.  

All the nodes in our cluster are connected through Fourteen Data Rate (FDR) Inﬁniband which is primarily used for gradient transfer during back-propagation.  

# A.2 GPU Implementation of CTC Loss Function  

The CTC loss function that we use to train our models has two passes: forward and backward, and the gradient computation involves element-wise addition of two matrices,  $\alpha$   and    $\beta$  , generated during the forward and backward passes respectively. Finally, we sum the gradients using the character in the utterance label as the key, to generate one gradient per character. These gradients are then back- propagated through the network. The input to the CTC loss function are probabilities calculated by the softmax function which can be very small, so we compute in  log  probability space for better numerical stability.  

The forward pass of the CTC algorithm calculates the    $\alpha$   matrix, which has  S  rows and    $\top$   columns, where    ${\mathsf{S}}=2({\mathsf{L}}+1)$  . The variable  L  is the number of characters in the label and    $\intercal$   is the number of time-steps in the utterance. Our CPU-based implementation of the CTC algorithm assigns one thread to each utterance label in a minibatch, performing the CTC calculation for the utterances in parallel. Each thread calculates the relevant entries of the matrix sequentially. This is inefﬁcient for two reasons.  

Firstly, since the remainder of our network is computed on the GPU, the output of the softmax function has to be copied to the CPU for CTC calculation. The gradient matrices from the CTC function then has to be copied back to the GPU for backpropagation. For languages like Mandarin with large character sets, these matrices have hundreds of millions of entries, making this copy expensive. Furthermore, we need as much interconnect bandwidth as possible for synchronizing the gradient updates with data parallelism, so this copy incurs a substantial opportunity cost.  

Secondly, although entries in each column of the  $\alpha$   matrix can be computed in parallel, the number of entries to calculate in each column depends both on the column and the number of repeated characters in the utterance label. Due to this complexity, the CPU implementation does not use SIMD parallelism optimally, making the computation inefﬁcient.  

We wrote a GPU-based implementation of CTC in order to overcome these two problems. The key insight behind our implementation is that we can compute all elements in each column of the    $\alpha$  matrix, rather than just the valid entries. If we do so, Figure 9 shows that invalid elements either contain a ﬁnite garbage value   $(\mathbb{G})$  , or    $-\infty$  ( I ), when w  a special summation function that adds probabilities in  log  space that discards inputs that are  −∞ . This summation is shown in Figure 9 where arrows incident on a circle are inputs and the result is stored in the circle. However, when we compute the ﬁnal gradient by element-wise summing  $\alpha$   and  $\beta$  , all ﬁnite garbage values will be added with a corresponding  $-\infty$  value from the other matrix, which results in  $-\infty$  , effectively ignoring the garbage value and computing the correct result. One important observation is that this element-wise sum of    $\alpha$   and  $\beta$   is a simple sum and does not use our summation function.  

To compute the gradient, we take each column of the matrix generated from element-wise addition of  $\alpha$   and    $\beta$   matrices, and do a key-value reduction using the character as key, using the ModernGPU library [5]. This means elements of the column corresponding to the same character will sum up their values. In the example sh  Fi  the blank character,  $\mathcal{B}$  , is the only eated character and at some columns, say for  t  $t=1$   of  t  $t=2$  , both valid elements ( ) and  −∞ correspond to it. Since our summation function in  log  space effectively ignores the  −∞ elements, only the valid elements are combined in the reduction.  

In our GPU implementation, we map each utterance in the minibatch to a CUDA  thread block . Since there are no dependencies between the elements of a column, all of them can be computed in parallel by the threads in a threadblock. There are dependencies between columns, since the column corresponding to time-step    $t+1$   cannot be computed before the column corresponding to time-step  $t$  . The reverse happens when computing the    $\beta$   matrix, when column corresponding to time-step  $t$   cannot be computed before the column corresponding to time-step    $t+1$  . Thus, in both cases, columns are processed sequentially by the thread block.  

Mapping the forward and backward passes to corresponding CUDA kernels is straightforward since there are no data dependencies between elements of a column. The kernel that does the backward pass also computes the gradient. However, since the gradients must be summed up based on the label  

![](images/68f1a82fe0ebcb764b5fd64cd55a9c0d2882727ed68d88b2c34309033df57e64.jpg)  
Figure 9:  Forward and backward pass for GPU implementation of CTC. Gray circles contain valid values, circle with  I  contain  $-\infty$  and circle with  $\mathtt{G}$   contain garbage values that are ﬁnite.  $\mathcal{B}$   stand for the blank character that the CTC algorithm adds to the input utterance label. Column labels on top show different time-steps going from  1  to    $\intercal$  .  

values, with each character as key, we must deal with data dependencies due to repeated characters in an utterance label. For languages with small character sets like English, this happens with high probability. Even if there are no repeated characters, the CTC algorithm adds  $\mathsf{L}+\mathrm{1}$   blank characters to the utterance label. We solve this problem by performing a key-value sort, where the keys are the characters in the utterance label, and the values are the indices of each character in the utterance. After sorting, all occurrences of a given character are arranged in contiguous segments. We only need to do the sort once for each utterance. The indices generated by the sort are then used to sequentially sum up the gradients for each character. This sum is done once per column and in parallel over all characters in the utterance. Amortizing the cost of key-value sort over    $\intercal$   columns is a key insight that makes the gradient calculation fast.  

Our GPU implementation uses fast  shared memory  and registers to achieve high performance when performing this task. Both forward and backward kernels store the    $\alpha$   matrix in  shared memory . Since  shared memory  is a limited resource, it is not possible to store the entire    $\beta$   matrix. However, as we go backward in time, we only need to keep one column of the  $\beta$   matrix as we compute the gradient, adding element-wise the column of the    $\beta$   matrix with the corresponding column of the  $\alpha$   matrix. Due to on-chip memory space constraints, we read the output of the softmax function directly from off-chip  global memory .  

Due to inaccuracies in ﬂoating-point arithmetic, especially in transcendental functions, our GPU and CPU implementation are not bit-wise identical. This is not an impediment in practice, since both implementations train models equally well when coupled with the technique of sorting utterances by length mentioned in Section 3.3.  