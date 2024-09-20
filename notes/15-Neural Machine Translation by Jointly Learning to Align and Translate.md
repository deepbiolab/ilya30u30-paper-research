# N EURAL  M ACHINE  T RANSLATION BY JOINTLY LEARNING TO ALIGN AND TRANSLATE  

Dzmitry Bahdanau Jacobs University Bremen, Germany  

KyungHyun Cho Yoshua Bengio ∗ Universit´ e de Montr´ eal  

# A BSTRACT  

Neural machine translation is a recently proposed approach to machine transla- tion. Unlike the traditional statistical machine translation, the neural machine translation aims at building a single neural network that can be jointly tuned to maximize the translation performance. The models proposed recently for neu- ral machine translation often belong to a family of encoder–decoders and encode a source sentence into a ﬁxed-length vector from which a decoder generates a translation. In this paper, we conjecture that the use of a ﬁxed-length vector is a bottleneck in improving the performance of this basic encoder–decoder architec- ture, and propose to extend this by allowing a model to automatically (soft-)search for parts of a source sentence that are relevant to predicting a target word, without having to form these parts as a hard segment explicitly. With this new approach, we achieve a translation performance comparable to the existing state-of-the-art phrase-based system on the task of English-to-French translation. Furthermore, qualitative analysis reveals that the (soft-)alignments found by the model agree well with our intuition.  

# 1 I NTRODUCTION  

Neural machine translation  is a newly emerging approach to machine translation, recently proposed by Kalchbrenner and Blunsom (2013), Sutskever  et al.  (2014) and Cho  et al.  (2014b). Unlike the traditional phrase-based translation system (see, e.g., Koehn  et al. , 2003) which consists of many small sub-components that are tuned separately, neural machine translation attempts to build and train a single, large neural network that reads a sentence and outputs a correct translation.  

Most of the proposed neural machine translation models belong to a family of  encoder– decoders  (Sutskever  et al. , 2014; Cho  et al. , 2014a), with an encoder and a decoder for each lan- guage, or involve a language-speciﬁc encoder applied to each sentence whose outputs are then com- pared (Hermann and Blunsom, 2014). An encoder neural network reads and encodes a source sen- tence into a ﬁxed-length vector. A decoder then outputs a translation from the encoded vector. The whole encoder–decoder system, which consists of the encoder and the decoder for a language pair, is jointly trained to maximize the probability of a correct translation given a source sentence.  

A potential issue with this encoder–decoder approach is that a neural network needs to be able to compress all the necessary information of a source sentence into a ﬁxed-length vector. This may make it difﬁcult for the neural network to cope with long sentences, especially those that are longer than the sentences in the training corpus. Cho  et al.  (2014b) showed that indeed the performance of a basic encoder–decoder deteriorates rapidly as the length of an input sentence increases.  

In order to address this issue, we introduce an extension to the encoder–decoder model which learns to align and translate jointly. Each time the proposed model generates a word in a translation, it (soft-)searches for a set of positions in a source sentence where the most relevant information is concentrated. The model then predicts a target word based on the context vectors associated with these source positions and all the previous generated target words.  

The most important distinguishing feature of this approach from the basic encoder–decoder is that it does not attempt to encode a whole input sentence into a single ﬁxed-length vector. Instead, it en- codes the input sentence into a sequence of vectors and chooses a subset of these vectors adaptively while decoding the translation. This frees a neural translation model from having to squash all the information of a source sentence, regardless of its length, into a ﬁxed-length vector. We show this allows a model to cope better with long sentences.  

In this paper, we show that the proposed approach of jointly learning to align and translate achieves signiﬁcantly improved translation performance over the basic encoder–decoder approach. The im- provement is more apparent with longer sentences, but can be observed with sentences of any length. On the task of English-to-French translation, the proposed approach achieves, with a single model, a translation performance comparable, or close, to the conventional phrase-based system. Furthermore, qualitative analysis reveals that the proposed model ﬁnds a linguistically plausible (soft-)alignment between a source sentence and the corresponding target sentence.  

# 2BACKGROUND: NEURAL MACHINE TRANSLATION  

From a probabilistic perspective, translation is equivalent to ﬁnding a target sentence  y  that max- imizes the conditional probability of  y  given a source sentence  x , i.e.,  arg max  $p(\textbf{y}\mid\textbf{x})$  . In y neural machine translation, we ﬁt a parameterized model to maximize the conditional probability of sentence pairs using a parallel training corpus. Once the conditional distribution is learned by a translation model, given a source sentence a corresponding translation can be generated by searching for the sentence that maximizes the conditional probability.  

Recently, a number of papers have proposed the use of neural networks to directly learn this condi- tional distribution (see, e.g., Kalchbrenner and Blunsom, 2013; Cho  et al. , 2014a; Sutskever  et al. , 2014; Cho  et al. , 2014b; Forcada and  Neco, 1997). This neural machine translation approach typ- ically consists of two components, the ﬁrst of which encodes a source sentence  $\mathbf{x}$   and the second decodes to a target sentence  y . For instance, two recurrent neural networks (RNN) were used by (Cho  et al. , 2014a) and (Sutskever  et al. , 2014) to encode a variable-length source sentence into a ﬁxed-length vector and to decode the vector into a variable-length target sentence.  

Despite being a quite new approach, neural machine translation has already shown promising results. Sutskever  et al.  (2014) reported that the neural machine translation based on RNNs with long short- term memory (LSTM) units achieves close to the state-of-the-art performance of the conventional phrase-based machine translation system on an English-to-French translation task.   Adding neural components to existing translation systems, for instance, to score the phrase pairs in the phrase table (Cho  et al. , 2014a) or to re-rank candidate translations (Sutskever  et al. , 2014), has allowed to surpass the previous state-of-the-art performance level.  

# 2.1 RNN E NCODER –D ECODER  

Here, we describe brieﬂy the underlying framework, called  RNN Encoder–Decoder , proposed by Cho  et al.  (2014a) and Sutskever  et al.  (2014) upon which we build a novel architecture that learns to align and translate simultaneously.  

In the Encoder–Decoder framework, an encoder reads the input sentence, a sequence of vectors  $\mathbf{x}=\left(x_{1},\cdot\cdot\cdot,x_{T_{x}}\right)$  , into a vector  $c$  . 2   The most common approach is to use an RNN such that  

$$
h_{t}=f\left(x_{t},h_{t-1}\right)
$$  

and  

$$
c=q\left(\{h_{1},\cdot\cdot\cdot\,,h_{T_{x}}\}\right),
$$  

where  $h_{t}\,\in\,\mathbb{R}^{n}$    is a hidden state at time    $t$  , and  $c$   is a vector generated from the sequence   the hidden states.  f  and  q  are some nonlinear functions. Sutskever  et al.  (2014) used an LSTM as  $f$   and  $q\left(\{h_{1},\cdot\cdot\cdot,{\dot{h_{T}}}\}\right)={\bar{h}}_{T}$  , for instance.  

The decoder is often trained to predict the next word    $y_{t^{\prime}}$   given the context vector    $c$   and all the previously pre cted words    $\left\{y_{1},\cdot\cdot\cdot\ ,y_{t^{\prime}-1}\right\}$  . In other words, the decoder deﬁnes a probability over the translation  y  by decomposing the joint probability into the ordered conditionals:  

$$
p(\mathbf{y})=\prod_{t=1}^{T}p(y_{t}\mid\{y_{1},\cdot\cdot\cdot\,,y_{t-1}\}\,,c),
$$  

where  ${\bf y}=\left(y_{1},\cdot\cdot\cdot,y_{T_{y}}\right)$  . With an RNN, each conditional probability is modeled as  

$$
p(y_{t}\mid\{y_{1},\cdot\cdot\cdot,y_{t-1}\}\,,c)=g(y_{t-1},s_{t},c),
$$  

where  $g$   is a nonlinear, potentially multi-layered, function that outputs the probability of  $y_{t}$  , and    $s_{t}$   is the hidden state of the RNN. It should be noted that other architectures such as a hybrid of an RNN and a de-convolutional neural network can be used (Kalchbrenner and Blunsom, 2013).  

# 3LEARNING TO ALIGN AND TRANSLATE  

In this section, we propose a novel architecture for neural machine translation. The new architecture consists of a bidirectional RNN as an encoder (Sec. 3.2) and a decoder that emulates searching through a source sentence during decoding a translation (Sec. 3.1).  

# 3.1 D ECODER : G ENERAL  D ESCRIPTION  

In a new model architecture, we deﬁne each conditional probability in Eq. (2) as:  

$$
p(y_{i}|y_{1},.\,.\,.\,,y_{i-1},\mathbf{x})=g(y_{i-1},s_{i},c_{i}),
$$  

where  $s_{i}$   is an RNN hidden state for time  $i$  , computed by  

$$
s_{i}=f(s_{i-1},y_{i-1},c_{i}).
$$  

It should be noted that unlike the existing encoder–decoder ap- proach (see Eq. (2)), here the probability is conditioned on a distinct context vector  $c_{i}$   for each target word  $y_{i}$  .  

The context vector    $c_{i}$   depends on a sequence of  annotations  $\left(h_{1},\cdot\cdot\cdot,h_{T_{x}}\right)$   to which an encoder maps the input sentence. Each annotation  $h_{i}$   contains information about the whole input sequence with a strong focus on the parts surrounding the    $i$  -th word of the input sequence. We explain in detail how the annotations are com- puted in the next section.  

The context vector  $c_{i}$   is, then, computed as a weighted sum of these annotations  $h_{i}$  :  

$$
c_{i}=\sum_{j=1}^{T_{x}}\alpha_{i j}h_{j}.
$$  

![](images/2c9345792cb8fda0f8ad4957a7462a2e633dfe54cc34ed1071164229e2832d55.jpg)  
Figure 1: The graphical illus- tration of the proposed model trying to generate the    $t$  -th tar- get word    $y_{t}$   given a source sentence    $(x_{1},x_{2},.\,.\,.\,,x_{T})$  .  

The weight  $\alpha_{i j}$   of each annotation  $h_{j}$   is computed by  

$$
\alpha_{i j}=\frac{\exp{(e_{i j})}}{\sum_{k=1}^{T_{x}}\exp{(e_{i k})}},
$$  

where  

$$
e_{i j}=a(s_{i-1},h_{j})
$$  

is an  alignment model  which scores how well the inputs around position    $j$   and the output at position  $i$   match. The score is based on the RNN hidden state  $s_{i-1}$   (just before emitting  $y_{i}$  , Eq. (4)) and the  $j$  -th annotation  $h_{j}$   of the input sentence.  

We parametrize the alignment model  $a$   as a feedforward neural network which is jointly trained with all the other components of the proposed system. Note that unlike in traditional machine translation, the alignment is not considered to be a latent variable. Instead, the alignment model directly com- putes a soft alignment, which allows the gradient of the cost function to be backpropagated through. This gradient can be used to train the alignment model as well as the whole translation model jointly.  

We can understand the approach of taking a weighted sum of all the annotations as computing an expected annotation , where the expectation is over possible alignments. Let  $\alpha_{i j}$   be a probability that the target word    $y_{i}$   is aligned to, or translated from, a source word    $x_{j}$  . Then, the    $i$  -th context vector  $c_{i}$   is the expected annotation over all the annotations with probabilities    $\alpha_{i j}$  .  

The probability    $\alpha_{i j}$  , or its associated energy  $e_{i j}$  , reﬂects the importance of the annotation    $h_{j}$   with respect to the previous hidden state    $s_{i-1}$   in deciding the next state  $s_{i}$   and generating    $y_{i}$  . Intuitively, this implements a mechanism of attention in the decoder. The decoder decides parts of the source sentence to pay attention to. By letting the decoder have an attention mechanism, we relieve the encoder from the burden of having to encode all information in the source sentence into a ﬁxed- length vector. With this new approach the information can be spread throughout the sequence of annotations, which can be selectively retrieved by the decoder accordingly.  

# 3.2ENCODER: BIDIRECTIONAL RNN FOR ANNOTATING SEQUENCES  

The usual RNN, described in Eq. (1), reads an input sequence  $\mathbf{x}$   in order starting from the ﬁrst symbol  $x_{1}$   to the last one    $x_{T_{x}}$  . However, in the proposed scheme, we would like the annotation of each word to summarize not only the preceding words, but also the following words. Hence, we propose to use a bidirectional RNN (BiRNN, Schuster and Paliwal, 1997), which has been successfully used recently in speech recognition (see, e.g., Graves  et al. , 2013).  

A BiRNN consists of forward and backward RNN’s. The forward RNN   →  reads the input sequence as it is ordered (from    $x_{1}$   to  $x_{T_{x}}$  ) and calculates a sequence of  forward hidden states    $(\stackrel{\rightarrow}{h}_{1}^{\bullet},\cdot\cdot\cdot,\stackrel{\leftarrow}{h}_{T_{x}})$   · · · . The backward RNN    $\leftarrows$   reads the sequence in the reverse order (from    $x_{T_{x}}$   to    $x_{1}$  ), resulting in a sequence of  backward hidden states    $(\overleftarrow{h}_{1},\cdot\cdot\cdot\ ,\overleftarrow{h}_{T_{x}})$   · · · ) .  

We obtain an annotation for each word    $x_{j}$   by concatenating the forward hidden state    $\overrightarrow{h}_{\,j}$   and the backward one    $\mathbf{\xi}_{\overline{{h}}_{\ j}}$  , i.e.,  $h_{j}=\left[\overline{{h}}_{j}^{\top};\overleftarrow{h}_{j}^{\top}\right]^{\top}$  i ⊤ . In this way, the annotation    $h_{j}$   contains the summaries of both the preceding words and the following words. Due to the tendency of RNNs to better represent recent inputs, the annotation    $h_{j}$   will be focused on the words around  $x_{j}$  . This sequence of annotations is used by the decoder and the alignment model later to compute the context vector (Eqs. (5)–(6)).  

See Fig. 1 for the graphical illustration of the proposed model.  

# 4 E XPERIMENT  S ETTINGS  

We evaluate the proposed approach on the task of English-to-French translation. We use the bilin- gual, parallel corpora provided by ACL WMT ’14.   As a comparison, we also report the perfor- mance of an RNN Encoder–Decoder which was proposed recently by Cho  et al.  (2014a). We use the same training procedures and the same dataset for both models.  

# 4.1 D ATASET  

WMT ’14 contains the following English-French parallel corpora: Europarl (61M words), news commentary (5.5M), UN (421M) and two crawled corpora of 90M and 272.5M words respectively, totaling 850M words. Following the procedure described in Cho  et al.  (2014a), we reduce the size of the combined corpus to have 348M words using the data selection method by Axelrod  et al.  (2011). We do not use any monolingual data other than the mentioned parallel corpora, although it may be possible to use a much larger monolingual corpus to pretrain an encoder. We concatenate news-test-  

![](images/4bd9fc68bb3d070112761595bbd121185fdb085b22443f8d2a35247f612aae65.jpg)  
Figure 2: The BLEU scores of the generated translations on the test set with respect to the lengths of the sen- tences. The results are on the full test set which in- cludes sentences having un- known words to the models.  

2012 and news-test-2013 to make a development (validation) set, and evaluate the models on the test set (news-test-2014) from WMT ’14, which consists of 3003 sentences not present in the training data.  

After a usual tokenization 6 , we use a shortlist of 30,000 most frequent words in each language to train our models. Any word not included in the shortlist is mapped to a special token ( [ UNK ] ). We do not apply any other special preprocessing, such as lowercasing or stemming, to the data.  

# 4.2 M ODELS  

We train two types of models. The ﬁrst one is an RNN Encoder–Decoder (RNNencdec, Cho  et al. , 2014a), and the other is the proposed model, to which we refer as RNNsearch. We train each model twice: ﬁrst with the sentences of length up to 30 words (RNNencdec-30, RNNsearch-30) and then with the sentences of length up to 50 word (RNNencdec-50, RNNsearch-50).  

The encoder and decoder of the RNNencdec have 1000 hidden units each.   The encoder of the RNNsearch consists of forward and backward recurrent neural networks (RNN) each having 1000 hidden units. Its decoder has 1000 hidden units. In both cases, we use a multilayer network with a single maxout (Goodfellow  et al. , 2013) hidden layer to compute the conditional probability of each target word (Pascanu  et al. , 2014).  

We use a minibatch stochastic gradient descent (SGD) algorithm together with Adadelta (Zeiler, 2012) to train each model. Each SGD update direction is computed using a minibatch of 80 sen- tences. We trained each model for approximately 5 days.  

Once a model is trained, we use a beam search to ﬁnd a translation that approximately maximizes the conditional probability (see, e.g., Graves, 2012; Boulanger-Lewandowski  et al. , 2013). Sutskever et al.  (2014) used this approach to generate translations from their neural machine translation model.  

For more details on the architectures of the models and training procedure used in the experiments, see Appendices A and B.  

# 5 R ESULTS  

# 5.1 Q UANTITATIVE  R ESULTS  

In Table 1, we list the translation performances measured in BLEU score. It is clear from the table that in all the cases, the proposed RNNsearch outperforms the conventional RNNencdec. More importantly, the performance of the RNNsearch is as high as that of the conventional phrase-based translation system (Moses), when only the sentences consisting of known words are considered. This is a signiﬁcant achievement, considering that Moses uses a separate monolingual corpus (418M words) in addition to the parallel corpora we used to train the RNNsearch and RNNencdec.  

![](images/fcb6fc42095ee639b0c5f89a6f2a7d3da8d9e47b8e94452f90ff34f94f8411be.jpg)  
Figure 3: Four sample alignments found by RNNsearch-50. The  $\mathbf{X}$  -axis and y-axis of each plot correspond to the words in the source sentence (English) and the generated translation (French), respectively. Each pixel shows the weight  $\alpha_{i j}$   of the annotation of the    $j$  -th source word for the  $i$  -th target word (see Eq. (6)), in grayscale ( 0 : black,  1 : white). (a) an arbitrary sentence. (b–d) three randomly selected samples among the sentences without any unknown words and of length between 10 and 20 words from the test set.  

One of the motivations behind the proposed approach was the use of a ﬁxed-length context vector in the basic encoder–decoder approach. We conjectured that this limitation may make the basic encoder–decoder approach to underperform with long sentences. In Fig. 2, we see that the perfor- mance of RNNencdec dramatically drops as the length of the sentences increases. On the other hand, both RNNsearch-30 and RNNsearch-50 are more robust to the length of the sentences. RNNsearch- 50, especially, shows no performance deterioration even with sentences of length 50 or more. This superiority of the proposed model over the basic encoder–decoder is further conﬁrmed by the fact that the RNNsearch-30 even outperforms RNNencdec-50 (see Table 1).  

![](images/532658ccc124763cafc64638476de81e53225d190bc7d45b69a2987e059f2058.jpg)  

Table 1: BLEU scores of the trained models com- puted on the test set. The second and third columns show respectively the scores on all the sentences and, on the sentences without any unknown word in them- selves and in the reference translations. Note that RNNsearch-  $.50^{\star}$  was trained much longer until the performance on the development set stopped improv- ing. ( ◦ ) We disallowed the models to generate [UNK] tokens when only the sentences having no unknown words were evaluated (last column).  

# 5.2QUALITATIVE ANALYSIS  

# 5.2.1 A LIGNMENT  

The proposed approach provides an intuitive way to inspect the (soft-)alignment between the words in a generated translation and those in a source sentence. This is done by visualizing the annotation weights    $\alpha_{i j}$   from Eq. (6), as in Fig. 3. Each row of a matrix in each plot indicates the weights associated with the annotations. From this we see which positions in the source sentence were considered more important when generating the target word.  

We can see from the alignments in Fig. 3 that the alignment of words between English and French is largely monotonic. We see strong weights along the diagonal of each matrix. However, we also observe a number of non-trivial, non-monotonic alignments. Adjectives and nouns are typically ordered differently between French and English, and we see an example in Fig. 3 (a). From this ﬁgure, we see that the model correctly translates a phrase [European Economic Area] into [zone economique europ´ een]. The RNNsearch was able to correctly align [zone] with [Area], jumping over the two words ([European] and [Economic]), and then looked one word back at a time to complete the whole phrase [zone ´ economique europ´ eenne].  

The strength of the soft-alignment, opposed to a hard-alignment, is evident, for instance, from Fig. 3 (d). Consider the source phrase [the man] which was translated into [l’ homme]. Any hard alignment will map [the] to [l’] and [man] to [homme]. This is not helpful for translation, as one must consider the word following [the] to determine whether it should be translated into [le], [la],

 [les] or [l’]. Our soft-alignment solves this issue naturally by letting the model look at both [the] and

 [man], and in this example, we see that the model was able to correctly translate [the] into [l’]. We observe similar behaviors in all the presented cases in Fig. 3. An additional beneﬁt of the soft align- ment is that it naturally deals with source and target phrases of different lengths, without requiring a counter-intuitive way of mapping some words to or from nowhere ([NULL]) (see, e.g., Chapters 4 and 5 of Koehn, 2010).  

# 5.2.2LONG SENTENCES  

As clearly visible from Fig. 2 the proposed model (RNNsearch) is much better than the conventional model (RNNencdec) at translating long sentences. This is likely due to the fact that the RNNsearch does not require encoding a long sentence into a ﬁxed-length vector perfectly, but only accurately encoding the parts of the input sentence that surround a particular word.  

As an example, consider this source sentence from the test set:  

An admitting privilege is the right of a doctor to admit a patient to a hospital or a medical centre to carry out a diagnosis or a procedure, based on his status as a health care worker at a hospital.  

The RNNencdec-50 translated this sentence into:  

Un privil\` ege d’admission est le droit d’un m´ edecin de reconnaˆ ıtre un patient \` l’hˆ opital ou un centre m´ edical d’un diagnostic ou de prendre un diagnostic en fonction de son ´ etat de sant´ e.  

The RNNencdec-50 correctly translated the source sentence until [a medical center]. However, from there on (underlined), it deviated from the original meaning of the source sentence. For instance, it replaced [based on his status as a health care worker at a hospital] in the source sentence with [en fonction de son ´ etat de sant´ e] (“based on his state of health”).  

On the other hand, the RNNsearch-50 generated the following correct translation, preserving the whole meaning of the input sentence without omitting any details:  

Un privil\` ege d’admission est le droit d’un m´ edecin d’admettre un patient \` a un hˆ opital ou un centre m´ edical pour effectuer un diagnostic ou une proc´ edure, selon son statut de travailleur des soins de sant´ e \` a l’hˆ opital.  

Let us consider another sentence from the test set:  

This kind of experience is part of Disney’s efforts to ”extend the lifetime of its series and build new relationships with audiences via digital platforms that are becoming ever more important,” he added.  

The translation by the RNNencdec-50 is  

Ce type d’exp´ erience fait partie des initiatives du Disney pour ”prolonger la dur´ ee de vie de ses nouvelles et de d´ evelopper des liens avec les lecteurs num´ eriques qui deviennent plus complexes.  

As with the previous example, the RNNencdec began deviating from the actual meaning of the source sentence after generating approximately 30 words (see the underlined phrase). After that point, the quality of the translation deteriorates, with basic mistakes such as the lack of a closing quotation mark.  

Again, the RNNsearch-50 was able to translate this long sentence correctly:  

Ce genre d’exp´ erience fait partie des efforts de Disney pour ”prolonger la dur´ ee de vie de ses s´ eries et cr´ eer de nouvelles relations avec des publics via des plateformes num´ eriques de plus en plus importantes”, a-t-il ajout´ e.  

In conjunction with the quantitative results presented already, these qualitative observations con- ﬁrm our hypotheses that the RNNsearch architecture enables far more reliable translation of long sentences than the standard RNNencdec model.  

In Appendix C, we provide a few more sample translations of long source sentences generated by the RNNencdec-50, RNNsearch-50 and Google Translate along with the reference translations.  

# 6 R ELATED  W ORK  

# 6.1LEARNING TO ALIGN  

A similar approach of aligning an output symbol with an input symbol was proposed recently by Graves (2013) in the context of handwriting synthesis. Handwriting synthesis is a task where the model is asked to generate handwriting of a given sequence of characters. In his work, he used a mixture of Gaussian kernels to compute the weights of the annotations, where the location, width and mixture coefﬁcient of each kernel was predicted from an alignment model. More speciﬁcally, his alignment was restricted to predict the location such that the location increases monotonically.  

The main difference from our approach is that, in (Graves, 2013), the modes of the weights of the annotations only move in one direction. In the context of machine translation, this is a severe limi- tation, as (long-distance) reordering is often needed to generate a grammatically correct translation (for instance, English-to-German).  

Our approach, on the other hand, requires computing the annotation weight of every word in the source sentence for each word in the translation. This drawback is not severe with the task of translation in which most of input and output sentences are only 15–40 words. However, this may limit the applicability of the proposed scheme to other tasks.  

# 6.2 N EURAL  N ETWORKS FOR  M ACHINE  T RANSLATION  

Since Bengio  et al.  (2003) introduced a neural probabilistic language model which uses a neural net- work to model the conditional probability of a word given a ﬁxed number of the preceding words, neural networks have widely been used in machine translation. However, the role of neural net- works has been largely limited to simply providing a single feature to an existing statistical machine translation system or to re-rank a list of candidate translations provided by an existing system.  

For instance, Schwenk (2012) proposed using a feedforward neural network to compute the score of a pair of source and target phrases and to use the score as an additional feature in the phrase-based statistical machine translation system. More recently, Kalchbrenner and Blunsom (2013) and Devlin et al.  (2014) reported the successful use of the neural networks as a sub-component of the existing translation system. Traditionally, a neural network trained as a target-side language model has been used to rescore or rerank a list of candidate translations (see, e.g., Schwenk  et al. , 2006).  

Although the above approaches were shown to improve the translation performance over the state- of-the-art machine translation systems, we are more interested in a more ambitious objective of designing a completely new translation system based on neural networks. The neural machine trans- lation approach we consider in this paper is therefore a radical departure from these earlier works. Rather than using a neural network as a part of the existing system, our model works on its own and generates a translation from a source sentence directly.  

# 7 C ONCLUSION  

The conventional approach to neural machine translation, called an encoder–decoder approach, en- codes a whole input sentence into a ﬁxed-length vector from which a translation will be decoded. We conjectured that the use of a ﬁxed-length context vector is problematic for translating long sen- tences, based on a recent empirical study reported by Cho  et al.  (2014b) and Pouget-Abadie  et al. (2014).  

In this paper, we proposed a novel architecture that addresses this issue. We extended the basic encoder–decoder by letting a model (soft-)search for a set of input words, or their annotations com- puted by an encoder, when generating each target word. This frees the model from having to encode a whole source sentence into a ﬁxed-length vector, and also lets the model focus only on information relevant to the generation of the next target word. This has a major positive impact on the ability of the neural machine translation system to yield good results on longer sentences. Unlike with the traditional machine translation systems, all of the pieces of the translation system, including the alignment mechanism, are jointly trained towards a better log-probability of producing correct translations.  

We tested the proposed model, called RNNsearch, on the task of English-to-French translation. The experiment revealed that the proposed RNNsearch outperforms the conventional encoder–decoder model (RNNencdec) signiﬁcantly, regardless of the sentence length and that it is much more ro- bust to the length of a source sentence. From the qualitative analysis where we investigated the (soft-)alignment generated by the RNNsearch, we were able to conclude that the model can cor- rectly align each target word with the relevant words, or their annotations, in the source sentence as it generated a correct translation.  

Perhaps more importantly, the proposed approach achieved a translation performance comparable to the existing phrase-based statistical machine translation. It is a striking result, considering that the proposed architecture, or the whole family of neural machine translation, has only been proposed as recently as this year. We believe the architecture proposed here is a promising step toward better machine translation and a better understanding of natural languages in general.  

One of challenges left for the future is to better handle unknown, or rare words. This will be required for the model to be more widely used and to match the performance of current state-of-the-art machine translation systems in all contexts.  

# A CKNOWLEDGMENTS  

The authors would like to thank the developers of Theano (Bergstra  et al. , 2010; Bastien  et al. , 2012). We acknowledge the support of the following agencies for research funding and computing support: NSERC, Calcul Qu´ ebec, Compute Canada, the Canada Research Chairs and CIFAR. Bah- danau thanks the support from Planet Intelligent Systems GmbH. We also thank Felix Hill, Bart van Merri´ enboer, Jean Pouget-Abadie, Coline Devin and Tae-Ho Kim.  

# R EFERENCES  

Axelrod, A., He, X., and Gao, J. (2011). Domain adaptation via pseudo in-domain data selection. In  Proceedings of the ACL Conference on Empirical Methods in Natural Language Processing (EMNLP) , pages 355–362. Association for Computational Linguistics. Bastien, F., Lamblin, P., Pascanu, R., Bergstra, J., Goodfellow, I. J., Bergeron, A., Bouchard, N., and Bengio, Y. (2012). Theano: new features and speed improvements. Deep Learning and Unsupervised Feature Learning NIPS 2012 Workshop. Bengio, Y., Simard, P., and Frasconi, P. (1994). Learning long-term dependencies with gradient descent is difﬁcult.  IEEE Transactions on Neural Networks ,  5 (2), 157–166. Bengio, Y., Ducharme, R., Vincent, P., and Janvin, C. (2003). A neural probabilistic language model. J. Mach. Learn. Res. ,  3 , 1137–1155. Bergstra, J., Breuleux, O., Bastien, F., Lamblin, P., Pascanu, R., Desjardins, G., Turian, J., Warde- Farley, D., and Bengio, Y. (2010). Theano: a CPU and GPU math expression compiler. In Proceedings of the Python for Scientiﬁc Computing Conference (SciPy) . Oral Presentation. Boulanger-Lewandowski, N., Bengio, Y., and Vincent, P. (2013). Audio chord recognition with recurrent neural networks. In  ISMIR . Cho, K., van Merrienboer, B., Gulcehre, C., Bougares, F., Schwenk, H., and Bengio, Y. (2014a). Learning phrase representations using RNN encoder-decoder for statistical machine translation. In  Proceedings of the Empiricial Methods in Natural Language Processing (EMNLP 2014) . to appear. Cho, K., van Merri¨ enboer, B., Bahdanau, D., and Bengio, Y. (2014b). On the properties of neural machine translation: Encoder–Decoder approaches. In  Eighth Workshop on Syntax, Semantics and Structure in Statistical Translation . to appear. Devlin, J., Zbib, R., Huang, Z., Lamar, T., Schwartz, R., and Makhoul, J. (2014). Fast and robust neural network joint models for statistical machine translation. In  Association for Computational Linguistics . Forcada, M. L. and  Neco, R. P. (1997). Recursive hetero-associative memories for translation. In J. Mira, R. Moreno-D´ ıaz, and J. Cabestany, editors,  Biological and Artiﬁcial Computation: From Neuroscience to Technology , volume 1240 of  Lecture Notes in Computer Science , pages 453–462. Springer Berlin Heidelberg. Goodfellow, I., Warde-Farley, D., Mirza, M., Courville, A., and Bengio, Y. (2013). Maxout net- works. In  Proceedings of The 30th International Conference on Machine Learning , pages 1319– 1327. Graves, A. (2012). Sequence transduction with recurrent neural networks. In  Proceedings of the 29th International Conference on Machine Learning (ICML 2012) . Graves, A. (2013). Generating sequences with recurrent neural networks. arXiv: 1308.0850 [cs.NE] . Graves, A., Jaitly, N., and Mohamed, A.-R. (2013). Hybrid speech recognition with deep bidirec- tional LSTM. In  Automatic Speech Recognition and Understanding (ASRU), 2013 IEEE Work- shop on , pages 273–278.  

Hermann, K. and Blunsom, P. (2014). Multilingual distributed representations without word align- ment. In  Proceedings of the Second International Conference on Learning Representations (ICLR 2014) . Hochreiter, S. (1991). Untersuchungen zu dynamischen neuronalen Netzen. Diploma thesis, Institut f¨ ur Informatik, Lehrstuhl Prof. Brauer, Technische Universit¨ at M¨ unchen. Hochreiter, S. and Schmidhuber, J. (1997). Long short-term memory.  Neural Computation ,  9 (8), 1735–1780. Kalchbrenner, N. and Blunsom, P. (2013). Recurrent continuous translation models. In  Proceedings of the ACL Conference on Empirical Methods in Natural Language Processing (EMNLP) , pages 1700–1709. Association for Computational Linguistics. Koehn, P. (2010).  Statistical Machine Translation . Cambridge University Press, New York, NY, USA. Koehn, P., Och, F. J., and Marcu, D. (2003). Statistical phrase-based translation. In  Proceedings of the 2003 Conference of the North American Chapter of the Association for Computational Linguistics on Human Language Technology - Volume 1 , NAACL ’03, pages 48–54, Stroudsburg, PA, USA. Association for Computational Linguistics. Pascanu, R., Mikolov, T., and Bengio, Y. (2013a). On the difﬁculty of training recurrent neural networks. In  ICML’2013 . Pascanu, R., Mikolov, T., and Bengio, Y. (2013b). On the difﬁculty of training recurrent neural networks. In  Proceedings of the 30th International Conference on Machine Learning (ICML 2013) . Pascanu, R., Gulcehre, C., Cho, K., and Bengio, Y. (2014). How to construct deep recurrent neural networks. In  Proceedings of the Second International Conference on Learning Representations (ICLR 2014) . Pouget-Abadie, J., Bahdanau, D., van Merri¨ enboer, B., Cho, K., and Bengio, Y. (2014). Overcoming the curse of sentence length for neural machine translation using automatic segmentation. In Eighth Workshop on Syntax, Semantics and Structure in Statistical Translation . to appear. Schuster, M. and Paliwal, K. K. (1997). Bidirectional recurrent neural networks.  Signal Processing, IEEE Transactions on ,  45 (11), 2673–2681. Schwenk, H. (2012). Continuous space translation models for phrase-based statistical machine translation. In M. Kay and C. Boitet, editors,  Proceedings of the 24th International Conference on Computational Linguistics (COLIN) , pages 1071–1080. Indian Institute of Technology Bombay. Schwenk, H., Dchelotte, D., and Gauvain, J.-L. (2006). Continuous space language models for statistical machine translation. In  Proceedings of the COLING/ACL on Main conference poster sessions , pages 723–730. Association for Computational Linguistics. Sutskever, I., Vinyals, O., and Le, Q. (2014). Sequence to sequence learning with neural networks. In  Advances in Neural Information Processing Systems (NIPS 2014) . Zeiler, M. D. (2012). ADADELTA: An adaptive learning rate method. arXiv: 1212.5701 [cs.LG] .  

# A M ODEL  A RCHITECTURE  

# A.1 A RCHITECTURAL  C HOICES  

The proposed scheme in Section 3 is a general framework where one can freely deﬁne, for instance, the activation functions    $f$   of recurrent neural networks (RNN) and the alignment model  $a$  . Here, we describe the choices we made for the experiments in this paper.  

# A.1.1 R ECURRENT  N EURAL  N ETWORK  

For the activation function  $f$   of an RNN, we use the gated hidden unit recently proposed by Cho et al.  (2014a). The gated hidden unit is an alternative to the conventional  simple  units such as an element-wise  tanh . This gated unit is similar to a long short-term memory (LSTM) unit proposed earlier by Hochreiter and Schmidhuber (1997), sharing with it the ability to better model and learn long-term dependencies. This is made possible by having computation paths in the unfolded RNN for which the product of derivatives is close to 1. These paths allow gradients to ﬂow backward easily without suffering too much from the vanishing effect (Hochreiter, 1991; Bengio  et al. , 1994; Pascanu  et al. , 2013a). It is therefore possible to use LSTM units instead of the gated hidden unit described here, as was done in a similar context by Sutskever  et al.  (2014).  

The new state  $s_{i}$   of the RNN employing  $n$   gated hidden units 8   is computed by  

$$
s_{i}=f{\bigl(}s_{i-1},y_{i-1},c_{i}{\bigr)}=(1-z_{i})\circ s_{i-1}+z_{i}\circ\tilde{s}_{i},
$$  

where  $\bigcirc$  is an element- se multiplication, and  $z_{i}$   is the output of the update gates (see below). The proposed updated state  $\tilde{s}_{i}$   is computed by i  

$$
\tilde{s}_{i}=\operatorname{tanh}\left(W e(y_{i-1})+U\left[r_{i}\circ s_{i-1}\right]+C c_{i}\right),
$$  

where    $e(y_{i-1})\,\in\,\mathbb{R}^{m}$    is an  $m$  mensional embedding o  word    $y_{i-1}$  nd  $r_{i}$   is the output of the reset gates (see below). When  $y_{i}$   is represented as a  1 -of-  $\mathit{K}$   vector,  $e(y_{i})$   is simply a column of an embedding matrix  $E\in\mathbb{R}^{m\times K}$  . Whenever possible, we omit bias terms to make the equations less cluttered.  

The update gates  $z_{i}$   allow each hidden unit to maintain its previous activation, and the reset gates    $r_{i}$  control how much and what information from the previous state should be reset. We compute them by  

$$
\begin{array}{r}{z_{i}=\sigma\left(W_{z}e(y_{i-1})+U_{z}s_{i-1}+C_{z}c_{i}\right),}\\ {r_{i}=\sigma\left(W_{r}e(y_{i-1}\right)+U_{r}s_{i-1}+C_{r}c_{i}\right),}\end{array}
$$  

where  $\sigma\left(\cdot\right)$   is a logistic sigmoid function.  

At each step of the decoder, we compute the output probability (Eq. (4)) as a multi-layered func- tion (Pascanu  et al. , 2014). We use a single hidden layer of maxout units (Goodfellow  et al. , 2013) and normalize the output probabilities (one for each word) with a softmax function (see Eq. (6)).  

# A.1.2 A LIGNMENT  M ODEL  

The alignment model should be design cons ring that the model needs to be evaluated  $T_{x}\times T_{y}$  times for each sentence pair of lengths  $T_{x}$   and  $T_{y}$  . In order to reduce computation, we use a single- layer multilayer perceptron such that  

$$
a(s_{i-1},h_{j})=v_{a}^{\top}\operatorname{tanh}\left(W_{a}s_{i-1}+U_{a}h_{j}\right),
$$  

where    $W_{a}\,\in\,\mathbb{R}^{n\times n},U_{a}\,\in\,\mathbb{R}^{n\times2n}$    and    $v_{a}\,\in\,\mathbb{R}^{n}$    are the weight matrices. Since    $U_{a}h_{j}$   does not depend on  i , we can pre-compute it in advance to minimize the computational cost.  

# A.2 D ETAILED  D ESCRIPTION OF THE  M ODEL  

# A.2.1 E NCODER  

In this section, we describe in detail the architecture of the proposed model (RNNsearch) used in the experiments (see Sec. 4–5). From here on, we omit all bias terms in order to increase readability.  

The model takes a source sentence of 1-of-K coded word vectors as input  

$$
\mathbf{x}=(x_{1},\allowbreak\cdot\,\cdot\,\cdot,x_{T_{x}}),\;x_{i}\in\mathbb{R}^{K_{x}}
$$  

and outputs a translated sentence of 1-of-K coded word vectors  

$$
\mathbf{y}=(y_{1},.\,.\,.\,,y_{T_{y}}),\;y_{i}\in\mathbb{R}^{K_{y}},
$$  

where    $K_{x}$   and    $K_{y}$   are the vocabulary sizes of source and target languages, respectively.    $T_{x}$   and  $T_{y}$  respectively denote the lengths of source and target sentences.  

First, the forward states of the bidirectional recurrent neural network (BiRNN) are computed:  

$$
\overrightarrow{h}_{\,i}=\left\{\begin{array}{l l}{(1-\overrightarrow{z}_{\,i})\circ\overrightarrow{h}_{\,i-1}+\overrightarrow{z}_{\,i}\circ\overrightarrow{h}_{\,i}}&{,\mathrm{if~}i>0}\\ {0}&{,\mathrm{if~}i=0}\end{array}\right.
$$  

where  

$$
\begin{array}{r l}&{\vec{\underline{{h}}}_{\,i}=\operatorname{tanh}\left(\vec{W}\overline{{E}}x_{i}+\vec{U}\left[\vec{r}_{\,i}\circ\vec{h}_{\,i-1}\right]\right)}\\ &{\vec{z}_{\,i}=\!\sigma\left(\vec{W}_{z}\overline{{E}}x_{i}+\vec{U}_{z}\,\vec{h}_{\,i-1}\right)}\\ &{\vec{r}_{\,i}=\!\sigma\left(\vec{W}_{r}\overline{{E}}x_{i}+\vec{U}_{r}\,\vec{h}_{\,i-1}\right).}\end{array}
$$  

$\overline{{E}}\,\in\,\mathbb{R}^{m\times K_{x}}$    is e wo d embedding matrix.    $\overrightarrow{W},\overrightarrow{W}_{z},\overrightarrow{W}_{r}\,\in\,\mathbb{R}^{n\times m},\,\overrightarrow{U},\overrightarrow{U}_{z},\overrightarrow{U}_{r}\,\in\,\mathbb{R}^{n\times n}$   ∈  ∈   are weight matrices.  m  and  n  are the word embedding dimensionality and the number of hidden units, respectively.  $\sigma(\cdot)$   is as usual a logistic sigmoid function.  

The backward states  $(\overleftarrow{h}_{1},\cdot\cdot\cdot\ ,\overleftarrow{h}_{T_{x}})$   · · ·  are computed similarly. We share the word embedding matrix  $\overline{E}$   between the forward and backward RNNs, unlike the weight matrices.  

We concatenate the forward and backward states to to obtain the annotations    $\left({{h}_{1}},{{h}_{2}},\cdot\cdot\cdot,{{h}_{{T}_{x}}}\right)$  , where  

$$
\begin{array}{r}{h_{i}=\left[\begin{array}{c}{\overrightarrow{h}_{\,i}}\\ {\overleftarrow{h}_{\,i}}\end{array}\right]}\end{array}
$$  

A.2.2 D ECODER  

The hidden state  $s_{i}$   of the decoder given the annotations from the encoder is computed by  

$$
\begin{array}{r}{s_{i}=\!\left(1-z_{i}\right)\circ s_{i-1}+z_{i}\circ\tilde{s}_{i},}\end{array}
$$  

where  

$$
\begin{array}{r l}&{\tilde{s}_{i}=\operatorname{tanh}\left(W E y_{i-1}+U\left[r_{i}\circ s_{i-1}\right]+C c_{i}\right)}\\ &{z_{i}=\!\sigma\left(W_{z}E y_{i-1}+U_{z}s_{i-1}+C_{z}c_{i}\right)}\\ &{r_{i}=\!\sigma\left(W_{r}E y_{i-1}+U_{r}s_{i-1}+C_{r}c_{i}\right)}\end{array}
$$  

$E$   is  matrix for the target l guage  $W,W_{z},W_{r}\in\mathbb{R}^{n\times m},U,U_{z},U_{r}\in\mathbb{R}^{n\times n}.$  , and  $C,C_{z},C_{r}\,\in\,\mathbb{R}^{n\times2\breve{n}}$   ∈   are weights. Again,  m  and  n  are the word e bedding dimensio and the number of hidden units, respectively. The initial hidden state  $s_{0}$   is computed by  $s_{0}~=$   $\operatorname{tanh}{\left(W_{s}\overleftarrow{h}_{1}\right)}$    ,  where  $W_{s}\in\mathbb{R}^{n\times n}$  .  

The context vector  $c_{i}$   are recomputed at each step by the alignment model:  

$$
c_{i}=\sum_{j=1}^{T_{x}}\alpha_{i j}h_{j},
$$  

![](images/a37886c2c199ce715bebb70f2c531820f08b09456b25adc593c4667ea9588402.jpg)  

Table 2: Learning statistics and relevant information. Each update corresponds to updating the parameters once using a single minibatch. One epoch is one pass through the training set. NLL is the average conditional log-probabilities of the sentences in either the training set or the development set. Note that the lengths of the sentences differ.  

where  

$$
\begin{array}{r}{\alpha_{i j}=\!\!\frac{\exp\big(\boldsymbol{e}_{i j}\big)}{\sum_{k=1}^{T_{x}}\exp\big(\boldsymbol{e}_{i k}\big)}\!\!\!\!\!}\\ {e_{i j}=\!\!v_{a}^{\top}\operatorname{tanh}\big(W_{a}s_{i-1}+U_{a}h_{j}\big)\,,}\end{array}
$$  

$h_{j}$   $j$  -th annotation in the source sentence (see Eq. (7)).    $v_{a}\,\in\,\mathbb{R}^{n^{\prime}},W_{a}\,\in\,\mathbb{R}^{n^{\prime}\times n}$    and  $U_{a}\,\in\,\bar{\mathbb{R}^{n^{\prime}\times2n}}$   ∈   are weight m ces. Note that the model becomes RNN Encoder–Decoder (Cho et al. , 2014a), if we ﬁx    $c_{i}$   to    $\overrightarrow{h}_{\:T_{x}}$  .  

With the decoder state  $s_{i-1}$  , the context    $c_{i}$   and the last generated word  $y_{i-1}$  , we deﬁne the probability of a target word  $y_{i}$   as  

$$
p(y_{i}|s_{i},y_{i-1},c_{i})\propto\exp\left(y_{i}^{\top}W_{o}t_{i}\right),
$$  

where  

$$
t_{i}=\left[\operatorname*{max}\left\{\tilde{t}_{i,2j-1},\tilde{t}_{i,2j}\right\}\right]_{j=1,\dots,l}^{\top}
$$  

and    $\tilde{t}_{i,k}$   is the    $k$  -th element of a vector  $\tilde{t}_{i}$   which is computed by  

$$
\tilde{t}_{i}=\!\!U_{o}s_{i-1}+V_{o}E y_{i-1}+C_{o}c_{i}.
$$  

$W_{o}\in\mathbb{R}^{K_{y}\times l},U_{o}\in\mathbb{R}^{2l\times n},V_{o}\in\mathbb{R}^{2l\times m}$    and    $C_{o}\in\mathbb{R}^{2l\times2n}$    are weight matrices. This can be under- stood as having a deep output (Pascanu  et al. , 2014) with a single maxout hidden layer (Goodfellow et al. , 2013).  

# A.2.3 M ODEL  S IZE  

For all the models used in this paper, the size of a hidden layer    $n$   is 1000, the word embedding dimensionality    $m$   is 620 and the size of the maxout hidden layer in the deep output    $l$   is 500. The number of hidden units in the alignment model    $n^{\prime}$    is 1000.  

# B T RAINING  P ROCEDURE  

# B.1 P ARAMETER  I NITIALIZATION  

We initialized the recu t we t matrices  $U,U_{z},U_{r},\overleftarrow{U},\overleftarrow{U}_{z},\overleftarrow{U}_{r},\overrightarrow{U},\overrightarrow{U}_{z}$   and    $\vec{U}_{r}$   as random or- thogonal matrices. For  $W_{a}$   and  $U_{a}$  , we initialized them by sampling each element from the Gaussian distribution of mean  0  and variance    $0.001^{2}$  . All the elements of    $V_{a}$   and all the bias vectors were ini- tialized to zero. Any other weight matrix was initialized by sampling from the Gaussian distribution of mean  0  and variance    $0.01^{2}$  .  

# B.2 T RAINING  

We used the stochastic gradient descent (SGD) algorithm. Adadelta (Zeiler, 2012) was used to automatically adapt the learning rate of each parameter   $\epsilon=10^{-6}$    and    $\rho\,=\,0.95)$  ). We explicitly normalized the    $L_{2}$  -norm of the gradient of the cost function each time to be at most a predeﬁned threshold of  1 , when the norm was larger than the threshold (Pascanu  et al. , 2013b). Each SGD update direction was computed with a minibatch of 80 sentences.  

At each update our implementation requires time proportional to the length of the longest sentence in a minibatch. Hence, to minimize the waste of computation, before every 20-th update, we retrieved 1600 sentence pairs, sorted them according to the lengths and split them into 20 minibatches. The training data was shufﬂed once before training and was traversed sequentially in this manner.  

In Tables 2 we present the statistics related to training all the models used in the experiments.  

![C T RANSLATIONS OF  L ONG  S ENTENCES ](images/b56e44d7eb064786785bb122925b4dd87a4fc2653434820fcc090b4239c8dafd.jpg)  
Table 3: The translations generated by RNNenc-50 and RNNsearch-50 from long source sentences (30 words or more) selected from the test set. For each source sentence, we also show the gold- standard translation. The translations by Google Translate were made on 27 August 2014.  