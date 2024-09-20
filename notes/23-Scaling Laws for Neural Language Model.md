# Scaling Laws for Neural Language Models  

Sam McCandlish  

Johns Hopkins University, OpenAI OpenAI  

sam@openai.com  

![](images/6c2932d31235b2fa902670ac088ffdef978d1834587063cd03e2fa373a5c4a06.jpg)  

# Abstract  

We study empirical scaling laws for language model performance on the cross-entropy loss. The loss scales as a power-law with model size, dataset size, and the amount of compute used for training, with some trends spanning more than seven orders of magnitude. Other architectural details such as network width or depth have minimal effects within a wide range. Simple equations govern the dependence of overﬁtting on model/dataset size and the dependence of training speed on model size. These relationships allow us to determine the optimal allocation of a ﬁxed compute budget. Larger models are signiﬁcantly more sample- efﬁcient, such that optimally compute-efﬁcient training involves training very large models on a relatively modest amount of data and stopping signiﬁcantly before convergence.  

# Contents  

1 Introduction 2 2 Background and Methods 6 3 Empirical Results and Basic Power Laws 7 4 Charting the Inﬁnite Data Limit and Overﬁtting 10 5 Scaling Laws with Model Size and Training Time 12 6 Optimal Allocation of the Compute Budget 14 7 Related Work 18 8 Discussion 18 Appendices 20 A Summary of Power Laws 20 B Empirical Model of Compute-Efﬁcient Frontier 20 C Caveats 22 D Supplemental Figures 23 1 Introduction  

Language provides a natural domain for the study of artiﬁcial intelligence, as the vast majority of reason- ing tasks can be efﬁciently expressed and evaluated in language, and the world’s text provides a wealth of data for unsupervised learning via generative modeling. Deep learning has recently seen rapid progress in lan- guage modeling, with state of the art models [RNSS18, DCLT18,  $\mathrm{YD\bar{Y}^{+}}19$  ,  $\mathrm{LOC}^{\dot{+}}19$  ,  $\mathrm{RSR}^{+}\bar{19}]$   approaching human-level performance on many speciﬁc tasks   $\mathrm{[WPV^{+}19]}$  , including the composition of coherent multi- paragraph prompted text samples   $[\mathrm{RWC^{+}}19]$  .  

One might expect language modeling performance to depend on model architecture, the size of neural models, the computing power used to train them, and the data available for this training process. In this work we will empirically investigate the dependence of language modeling loss on all of these factors, focusing on the Transformer architecture   $[\mathrm{V}\mathrm{S}\bar{\mathrm{P}}^{+}17$  ,  $\mathrm{LSP^{+}18}]$  . The high ceiling and low ﬂoor for performance on language tasks allows us to study trends over more than seven orders of magnitude in scale.  

Throughout we will observe precise power-law scalings for performance as a function of training time, con- text length, dataset size, model size, and compute budget.  

# 1.1 Summary  

Our key ﬁndings for Transformer language models are are as follows:  

![](images/5b5a4af363575fe93818f5b8e1c74ea3bf8ca64ac770961a8e342dbda6a0097f.jpg)  
Figure 1 Language modeling performance improves smoothly as we increase the model size, datasetset size, and amount of compute 2   used for training. For optimal performance all three factors must be scaled up in tandem. Empirical performance has a power-law relationship with each individual factor when not bottlenecked by the other two.  

Performance depends strongly on scale, weakly on model shape: Model performance depends most strongly on scale, which consists of three factors: the number of model parameters  $N$   (excluding embed- dings), the size of the dataset    $D$  , and the amount of compute  $C$   used for training. Within reasonable limits, performance depends very weakly on other architectural hyperparameters such as depth vs. width. (Section 3)  

Smooth power laws: Performance has a power-law relationship with each of the three scale factors  $N,D,C$   when not bottlenecked by the other two, with trends spanning more than six orders of magnitude (see Figure 1). We observe no signs of deviation from these trends on the upper end, though performance must ﬂatten out eventually before reaching zero loss. (Section 3)  

Universality of overﬁtting: Performance improves predictably as long as we scale up  $N$   and  $D$   in tandem, but enters a regime of diminishing returns if either    $N$   or    $D$   is held ﬁxed while the other increases. The performance penalty depends predictably on the ratio    $N^{0.74}/D$  , meaning that every time we increase the model size  $8\mathbf{x}$  , we only need to increase the data by roughly   $5\mathbf{x}$   to avoid a penalty. (Section 4)  

Universality of training: Training curves follow predictable power-laws whose parameters are roughly independent of the model size. By extrapolating the early part of a training curve, we can roughly predict the loss that would be achieved if we trained for much longer. (Section 5)  

Transfer improves with test performance: When we evaluate models on text with a different distribution than they were trained on, the results are strongly correlated to those on the training validation set with a roughly constant offset in the loss – in other words, transfer to a different distribution incurs a constant penalty but otherwise improves roughly in line with performance on the training set. (Section 3.2.2)  

Sample efﬁciency: Large models are more sample-efﬁcient than small models, reaching the same level of performance with fewer optimization steps (Figure 2) and using fewer data points (Figure 4).  

Convergence is inefﬁcient: When working within a ﬁxed compute budget    $C$   but without any other restric- tions on the model size    $N$   or available data  $D$  , we attain optimal performance by training  very large models and stopping  signiﬁcantly short of convergence  (see Figure 3). Maximally compute-efﬁcient training would therefore be far more sample efﬁcient than one might expect based on training small models to convergence, with data requirements growing very slowly as    $\bar{D\sim C^{0.27}}$    with training compute. (Section 6)  

Optimal batch size: The ideal batch size for training these models is roughly a power of the loss only, and continues to be determinable by measuring the gradient noise scale [MKAT18]; it is roughly 1-2 million tokens at convergence for the largest models we can train. (Section 5.1)  

Taken together, these results show that language modeling performance improves smoothly and predictably as we appropriately scale up model size, data, and compute. We expect that larger language models will perform better and be more sample efﬁcient than current models.  

![](images/25b39e498a786bcbe00094f21ce6da3636891215487d2658c8b89b5f9ecac761.jpg)  
Figure 2 We show a series of language model training runs, with models ranging in size from  $10^{3}$    to  $10^{9}$  parameters (excluding embeddings).  

![](images/c3b86f3060e0e672b4baf562aad2aa7e97d4ca9635d4f1ba810b99772ed06d77.jpg)  
Figure 3 As more compute becomes available, we can choose how much to allocate towards training larger models, using larger batches, and training for more steps. We illustrate this for a billion-fold increase in compute. For optimally compute-efﬁcient training, most of the increase should go towards increased model size. A relatively small increase in data is needed to avoid reuse. Of the increase in data, most can be used to increase parallelism through larger batch sizes, with only a very small increase in serial training time required.  

# 1.2 Summary of Scaling Laws  

The test loss of a Transformer trained to auto regressive ly model language can be predicted using a power-law when performance is limited by only either the number of non-embedding parameters    $N$  , the dataset size    $D$  , or the optimally allocated compute budget  $C_{\mathrm{min}}$   (see Figure 1):  

1. For models with a limited number of parameters, trained to convergence on sufﬁciently large datasets:  

$$
L(N)=\left(N_{\mathrm{c}}/N\right)^{\alpha_{N}};\ \ \alpha_{N}\sim0.076,\ \ \ N_{\mathrm{c}}\sim8.8\times10^{13}\ (\mathrm{non-emeddim~parameters})
$$  

2. For large models trained with a limited dataset with early stopping:  

$$
L(D)=\left(D_{\mathrm{c}}/D\right)^{\alpha_{D}};\,\,\,\alpha_{D}\sim0.095,\quad D_{\mathrm{c}}\sim5.4\times10^{13}\,(\mathrm{totens})
$$  

3. When training with a limited amount of compute, a sufﬁciently large dataset, an optimally-sized model, and a sufﬁciently small batch size (making optimal 3   use of compute):  

$$
L(C_{\mathrm{min}})=\left(C_{\mathrm{c}}^{\mathrm{min}}/C_{\mathrm{min}}\right)^{\alpha_{C}^{\mathrm{min}}};\ \ \alpha_{C}^{\mathrm{min}}\sim0.050,\ \ \ C_{\mathrm{c}}^{\mathrm{min}}\sim3.1\times10^{8}\ (\mathrm{PF-days})
$$  

3 We also observe an empirical power-law trend with the training compute    $C$   (Figure 1) while training at ﬁxed batch size, but it is the trend with  $C_{\mathrm{min}}$   that should be used to make predictions. They are related by equation (5.5).  

![](images/39643f8abc0cf577bd8494cb7804661796c4c3a38a3c533bc287afd188f50b6e.jpg)  
Figure 4 Left : The early-stopped test loss    $L(N,D)$   varies predictably with the dataset size    $D$   and model size  $N$   according to Equation (1.5).  Right : After an initial transient period, learning curves for all model sizes    $N$   can be ﬁt with Equation (1.6), which is parameterized in terms of    $S_{\mathrm{min}}$  , the number of steps when training at large batch size (details in Section 5.1).  

These relations hold across eight orders of magnitude in  $C_{\mathrm{min}}$  , six orders of magnitude in    $N$  , and over two orders of magnitude in    $D$  . They depend very weakly on model shape and other Transformer hyperparameters (depth, width, number of self-attention heads), with speciﬁc numerical values associated with the Webtext2 training set   $[\mathrm{RWC^{+}19}]$  . The power laws    $\alpha_{\mathrm{N}},\alpha_{\mathrm{D}},\bar{\alpha_{C}^{\mathrm{min}}}$  specify the degree of performance improvement expected as we scale up  $N,\,D$  , or  $C_{\mathrm{min}}$  ; for example, doubling the number of parameters yields a loss that is smaller by a factor    $2^{-\alpha_{N}}\:=\:0.95$  . The precise numerical values of    $N_{\mathrm{{c}}},\bar{C_{\mathrm{{c}}}^{\mathrm{{min}}}}$  ,  and    $D_{\mathrm{c}}$   depend on the vocabulary size and tokenization and hence do not have a fundamental meaning.  

The critical batch size, which determines the speed/efﬁciency tradeoff for data parallelism ([MKAT18]), also roughly obeys a power law in    $L$  :  

$$
B_{\mathrm{crit}}\left(L\right)=\frac{B_{*}}{L^{1/\alpha_{B}}},\qquad B_{*}\sim2\cdot10^{8}\;\mathrm{totons},\;\;\alpha_{B}\sim0.21
$$  

Equation (1.1) and (1.2) together suggest that as we increase the model size, we should increase the dataset size sublinearly according to    $D\propto N^{\frac{\alpha_{N}}{\alpha_{D}}}\sim N^{0.74}$   ∼ . In fact, w ﬁnd  t there is a single equation combining (1.1) and (1.2) that governs the simultaneous dependence on  N  and  D  and governs the degree of overﬁtting:  

$$
L(N,D)=\left[\left({\frac{N_{c}}{N}}\right)^{\frac{\alpha_{N}}{\alpha_{D}}}+{\frac{D_{c}}{D}}\right]^{\alpha_{D}}
$$  

with ﬁts pictured on the left in ﬁgure 4. We conjecture that this functional form may also parameterize the trained log-likelihood for other generative modeling tasks.  

When training a given model for a ﬁnite number of parameter update steps    $S$   in the inﬁnite data limit, after an initial transient period, the learning curves can be accurately ﬁt by (see the right of ﬁgure 4)  

$$
L(N,S)=\left(\frac{N_{c}}{N}\right)^{\alpha_{N}}+\left(\frac{S_{c}}{S_{\mathrm{min}}(S)}\right)^{\alpha_{S}}
$$  

where    $S_{c}\approx2.1\times10^{3}$    and  $\alpha_{S}\approx0.76$  , and    $S_{\mathrm{min}}(S)$   is the minimum possible number of optimization steps (parameter updates) estimated using Equation (5.4).  

When training within a ﬁxed compute budget    $C$  , but with no other constraints, Equation (1.6) leads to the prediction that the optimal model size  $N$  , optimal batch size    $B$  , optimal number of steps    $S$  , and dataset size  $D$   should grow as  

$$
N\propto C^{\alpha_{C}^{\mathrm{min}}/\alpha_{N}},\quad B\propto C^{\alpha_{C}^{\mathrm{min}}/\alpha_{B}},\quad S\propto C^{\alpha_{C}^{\mathrm{min}}/\alpha_{S}},\quad D=B\cdot S
$$  

with  

$$
\alpha_{C}^{\mathrm{min}}=1/\left(1/\alpha_{S}+1/\alpha_{B}+1/\alpha_{N}\right)
$$  

which closely matche the empirically optimal results    $N\,\propto\,C_{\mathrm{min}}^{0.73}$  ,    $B\,\propto\,C_{\mathrm{min}}^{0.24}$  , and    $S\,\propto\,C_{\mathrm{min}}^{0.03}$    . As the computational budget  C  increases, it should be spent primarily on larger models, without dramatic increases in training time or dataset size (see Figure 3). This also implies that as models grow larger, they become increasingly sample efﬁcient. In practice, researchers typically train smaller models for longer than would be maximally compute-efﬁcient because of hardware constraints. Optimal performance depends on total compute as a power law (see Equation (1.3)).  

We provide some basic theoretical motivation for Equation (1.5), an analysis of learning curve ﬁts and their implications for training time, and a breakdown of our results per token. We also make some brief compar- isons to LSTMs and recurrent Transformers   $[\mathrm{DGeV^{+}}18]$  .  

# 1.3 Notation  

We use the following notation:  

•    $L$   – the cross entropy loss in nats. Typically it will be averaged over the tokens in a context, but in some cases we report the loss for speciﬁc tokens within the context. •    $N$   – the number of model parameters,  excluding all vocabulary and positional embeddings •    $C\approx6N B S-$   an estimate of the total non-embedding training compute, where    $B$   is the batch size, and  $S$   is the number of training steps (ie parameter updates). We quote numerical values in PF-days, where one PF  $\mathrm{-day}=10^{15}\times\bar{24}\times\bar{3600}=8.64\times1\bar{0}^{19}$    ﬂoating point operations. •    $D$   – the dataset size in tokens •    $B_{\mathrm{crit}}$   – the critical batch size [MKAT18], deﬁned and discussed in Section 5.1. Training at the critical batch size provides a roughly optimal compromise between time and compute efﬁciency. •    $C_{\mathrm{min}}$   – an estimate of the minimum amount of non-embedding compute to reach a given value of the loss. This is the training compute that would be used if the model were trained at a batch size much less than the critical batch size. •    $S_{\mathrm{min}}$   – an estimate of the minimal number of training steps needed to reach a given value of the loss. This is also the number of training steps that would be used if the model were trained at a batch size much greater than the critical batch size. •    $\alpha_{X}$  onents for the scaling of the loss as  $L(X)\propto1/X^{\alpha_{X}}$    where    $X$   can be any of  $N,D,C,S,B,C^{\mathrm{min}}$  .  

# 2 Background and Methods  

We train language models on WebText2, an extended version of the WebText   $[\mathrm{RWC^{+}19}]$   dataset, tokenized using byte-pair encoding [SHB15] with a vocabulary size    $n_{\mathrm{vacab}}\,=\,50257$  . We optimize the autoregres- sive log-likelihood (i.e. cross-entropy loss) averaged over a 1024-token context, which is also our principal performance metric. We record the loss on the WebText2 test distribution and on a selection of other text distributions. We primarily train decoder-only   $[\mathrm{L}S\mathbf{P}^{+}18\$  , RNSS18] Transformer   $[\mathrm{V}\mathrm{S}\mathbf{P}^{+}17]$   models, though we also train LSTM models and Universal Transformers   $[\mathrm{DGeV^{+}}18]$   for comparison.  

# 2.1 Parameter and Compute Scaling of Transformers  

We parameterize the Transformer architecture using hyperparameters  $n_{\mathrm{layer}}$   (number of layers),    $d_{\mathrm{model}}$   (di- mension of the residual stream),    $d_{\mathrm{ff}}$  (dimension of the intermediate feed-forward layer),    $d_{\mathrm{{att}}}$   (dimension of the attention output), and  $n_{\mathrm{hads}}$   (number of attention heads per layer). We include    $n_{\mathrm{{ctx}}}$   tokens in the input context, with  $n_{\mathrm{ctx}}=1024$   except where otherwise noted.  

We use  $N$   to denote the model size, which we deﬁne as the number of  non-embedding  parameters  

$$
\begin{array}{r l}{\lefteqn{N\approx2d_{\mathrm{model}}n_{\mathrm{layer}}\left(2d_{\mathrm{attn}}+d_{\mathrm{ff}}\right)}}\\ &{=12n_{\mathrm{layer}}d_{\mathrm{model}}^{2}\quad\mathrm{~with~the~standard}\quad d_{\mathrm{attn}}=d_{\mathrm{ff}}/4=d_{\mathrm{model}}}\end{array}
$$  

where we have excluded biases and other sub-leading terms. Our models also have  $n_{\mathrm{vacab}}d_{\mathrm{model}}$   parameters in an embedding matrix, and use    $n_{\mathrm{ctx}}d_{\mathrm{model}}$   parameters for positional embeddings, but we do not include these when discussing the ‘model size’  $N$  ; we will see that this produces signiﬁcantly cleaner scaling laws.  

Evaluating a forward pass of the Transformer involves roughly  

$$
C_{\mathrm{forward}}\approx2N+2n_{\mathrm{layer}}n_{\mathrm{ctx}}d_{\mathrm{model}}
$$  

add-multiply operations, where the factor of two comes from the multiply-accumulate operation used in matrix multiplication. A more detailed per-operation parameter and compute count is included in Table 1.  

![Table 1 Parameter counts and compute (forward pass) estimates for a Transformer model. Sub-leading terms such as nonlinearities, biases, and layer normalization are omitted. ](images/bb57801c485ecd242ba2ee1b19d7f49b75d6b4b81805305cb9d3cdee85dc43cc.jpg)  

For contexts and models with    $d_{\mathrm{model}}\,>\,n_{\mathrm{ctx}}/12$  , the context-dependent computational cost per token is a relatively small fraction of the total compute. Since we primarily study models where    $d_{\mathrm{model}}\gg n_{\mathrm{ctx}}/12$  , we do not include context-dependent terms in our training compute estimate. Accounting for the backwards pass (approximately twice the compute as the forwards pass), we then deﬁne the estimated non-embedding compute as  $C\approx6N$   ﬂoating point operators per training token.  

# 2.2 Training Procedures  

Unless otherwise noted, we train models with the Adam optimizer [KB14] for a ﬁxed  $2.5\times10^{5}$    steps with a batch size of  512  sequences of  1024  tokens. Due to memory constraints, our largest models (more than 1B parameters) were trained with Adafactor [SS18]. We experimented with a variety of learning rates and schedules, as discussed in Appendix D.6. We found that results at convergence were largely independent of learning rate schedule. Unless otherwise noted, all training runs included in our data used a learning rate schedule with a 3000 step linear warmup followed by a cosine decay to zero.  

# 2.3 Datasets  

We train our models on an extended version of the WebText dataset described in   $[\mathrm{RWC^{+}}19]$  . The original WebText dataset was a web scrape of outbound links from Reddit through December 2017 which received at least 3 karma. In the second version, WebText2, we added outbound Reddit links from the period of January to October 2018, also with a minimum of 3 karma. The karma threshold served as a heuristic for whether people found the link interesting or useful. The text of the new links was extracted with the Newspaper3k python library. In total, the dataset consists of  $20.3\mathbf{M}$   documents containing 96 G nd    $1.6\bar{2}\times\bar{1}0^{10}$  words (as deﬁned by  wc ). We then apply the reversible tokenizer described in [RWC  $[\mathrm{RWC^{+}}19]$  19], which yields  $2.29\times10^{10}$    tokens. We reserve  $6.6\times\mathbf{\dot{10}}^{8}$  hese tokens for use as a test set, and we also test on similarly- prepared samples of Books Corpus [ZKZ  $[\mathrm{ZKZ}^{+}15]$  15], Common Crawl [Fou], English Wikipedia, and a collection of publicly-available Internet Books.  

# 3 Empirical Results and Basic Power Laws  

To characterize language model scaling we train a wide variety of models, varying a number of factors including:  

•  Model size (ranging in size from 768 to 1.5 billion non-embedding parameters) •  Dataset size (ranging from 22 million to 23 billion tokens) •  Shape (including depth, width, attention heads, and feed-forward dimension) •  Context length (1024 for most runs, though we also experiment with shorter contexts) •  Batch size (  $2^{19}$    for most runs, but we also vary it to measure the critical batch size)  

![](images/add2d255a8f8d7eaa8cc72caadfc09cbd85f9c51e4728c61cd3b68606c612d7b.jpg)  
Figure 5 Performance depends very mildly on model shape when the total number of non-embedding parameters    $N$   is held ﬁxed. The loss varies only a few percent over a wide range of shapes. Small differences in parameter counts are compensated for by using the ﬁt to  $L(N)$   as a baseline. Aspect ratio in particular can vary by a factor of 40 while only slightly impacting performance; an    $\left(n_{\mathrm{layer}},d_{\mathrm{model}}\right)=\left(6,4288\right)$   reaches a loss within   $3\%$   of the  (48 ,  1600)  model used in   $[\mathrm{RWC^{+}}19]$  .  

![](images/f75941eefe52357f831f7aa688d257a102371a1c205de63f9ef8ba307fde6bf0.jpg)  
Figure 6 Left:  When we include embedding parameters, performance appears to depend strongly on the number of layers in addition to the number of parameters.  Right:  When we exclude embedding parameters, the performance of models with different depths converge to a single trend. Only models with fewer than 2 layers or with extreme depth-to-width ratios deviate signiﬁcantly from the trend.  

In this section we will display data along with empirically-motivated ﬁts, deferring theoretical analysis to later sections.  

# 3.1 Approximate Transformer Shape and Hyperparameter Independence  

Transformer performance depends very weakly on the shape parameters    $n_{\mathrm{layer}},n_{\mathrm{hadrons}}$  , and    $d_{\mathrm{ff}}$  when we hold the total non-embedding parameter count    $N$   ﬁxed. To establish these results we trained models with ﬁxed size while varying a single hyperparameter. This was simplest for the case of    $n_{\mathrm{hads}}$  . When varying    $n_{\mathrm{layer}}$  , we simultaneously varied    $d_{\mathrm{model}}$   while keep  $N\approx12n_{\mathrm{layer}}d_{\mathrm{model}}^{2}$    ﬁxed. Similarly, to vary    $d_{\mathrm{ff}}$  at ﬁxed model size we also simultaneously varied the  $d_{\mathrm{model}}$   parameter, as required by the parameter counts in Table 1. Independence of  $n_{\mathrm{llayers}}$   would follow if deeper Transformers effectively behave as ensembles of shallower models, as has been suggested for ResNets [VWB16]. The results are shown in Figure 5.  

# 3.2 Performance with Non-Embedding Parameter Count    $N$  

In Figure 6 we display the performance of a wide variety of models, ranging from small models with shape  $(n_{\mathrm{layer}},d_{\mathrm{model}})~=~(2,128)$   through billion-parameter models, ranging in shape from  (6 ,  4288)  through (207 ,  768) . Here we have trained to near convergence on the full WebText2 dataset and observe no over- ﬁtting (except possibly for the very largest models).  

As shown in Figure 1, we ﬁnd a steady trend with non-embedding parameter count  $N$  , which can be ﬁt to the ﬁrst term of Equation (1.5), so that  

$$
L(N)\approx\left({\frac{N_{c}}{N}}\right)^{\alpha_{N}}
$$  

![](images/184dfce6752425a6c325dedcede8857339325f85f60de4f2fccbd795361c14da.jpg)  
Figure 7  

To observe these trends it is crucial to study performance as a function of    $N$  ; if we instead use the total parameter count (including the embedding parameters) the trend is somewhat obscured (see Figure 6). This suggests that the embedding matrix can be made smaller without impacting performance, as has been seen in recent work   $[\mathrm{LCG^{+}}19]$  .  

Although these models have been trained on the WebText2 dataset, their test loss on a variety of other datasets is also a power-law in  $N$   with nearly identical power, as shown in Figure 8.  

# 3.2.1 Comparing to LSTMs and Universal Transformers  

In Figure 7 we compare LSTM and Transformer performance as a function of non-embedding parameter count    $N$  . The LSTMs were trained with the same dataset and context length. We see from these ﬁgures that the LSTMs perform as well as Transformers for tokens appearing early in the context, but cannot match the Transformer performance for later tokens. We present power-law relationships between performance and context position Appendix D.5, where increasingly large powers for larger models suggest improved ability to quickly recognize patterns.  

We also compare the performance of standard Transformers to recurrent Transformers   $[\mathrm{DGeV^{+}}18]$   in Figure 17 in the appendix. These models re-use parameters, and so perform slightly better as a function of    $N$  , at the cost of additional compute per-parameter.  

# 3.2.2 Generalization Among Data Distributions  

We have also tested our models on a set of additional text data distributions. The test loss on these datasets as a function of model size is shown in Figure 8; in all cases the models were trained only on the WebText2 dataset. We see that the loss on these other data distributions improves smoothly with model size, in direct parallel with the improvement on WebText2. We ﬁnd that generalization depends almost exclusively on the in-distribution validation loss, and does not depend on the duration of training or proximity to convergence. We also observe no dependence on model depth (see Appendix D.8).  

# 3.3 Performance with Dataset Size and Compute  

We display empirical trends for the test loss as a function of dataset size  $D$   (in tokens) and training compute  $C$   in Figure 1.  

For the trend with  $D$   we trained a model with    $(n_{\mathrm{layer}},n_{\mathrm{e m bd}})=(36,1280)$   on ﬁxed subsets of the WebText2 dataset. We stopped training once the test loss ceased to decrease. We see that the resulting test losses can be ﬁt with simple power-law  

$$
L(D)\approx\left(\frac{D_{c}}{D}\right)^{\alpha_{D}}
$$  

in the dataset size. The data and ﬁt appear in Figure 1.  

The total amount of non-embedding compute used during training can be estimated as  $C=6N B S$  , where  $B$   is the batch size,    $S$   is the number of parameter updates, and the factor of  6  accounts for the forward and backward passes. Thus for a given value of    $C$   we can scan over all models with various    $N$   to ﬁnd the model  

![](images/645f0ffbd045b23bb5274bd9a62c661b1c9abee0fe288347ab326f6760d6c27d.jpg)  
Figure 8 Left:  Generalization performance to other data distributions improves smoothly with model size, with only a small and very slowly growing offset from the WebText2 training distribution.  Right:  Gener- alization performance depends only on training distribution performance, and not on the phase of training. We compare generalization of converged models (points) to that of a single large model (dashed curves) as it trains.  

with the best performance on step    $\begin{array}{r}{S=.\frac{C}{6B S}}\end{array}$    . Note that in these results  the batch size    $B$   remains ﬁxed for all models , which means that these empirical results are not truly optimal. We will account for this in later sections using an adjusted    $C_{\mathrm{min}}$   to produce cleaner trends.  

The result appears as the heavy black line on the left-hand plot in Figure 1. It can be ﬁt with  

$$
L(C)\approx\left(\frac{C_{c}}{C}\right)^{\alpha_{C}}
$$  

The ﬁgure also includes images of individual learning curves to clarify when individual models are optimal. We will study the optimal allocation of compute more closely later on. The data strongly suggests that sample efﬁciency improves with model size, and we also illustrate this directly in Figure 19 in the appendix.  

# 4 Charting the Inﬁnite Data Limit and Overﬁtting  

In Section 3 we found a number of basic scaling laws for language modeling performance. Here we will study the performance of a model of size    $N$   trained on a dataset with    $D$   tokens while varying    $N$   and    $D$  simultaneously. We will empirically demonstrate that the optimally trained test loss accords with the scaling law of Equation (1.5). This provides guidance on how much data we would need to train models of increasing size while keeping overﬁtting under control.  

# 4.1 Proposed    $L(N,D)$   Equation  

We have chosen the parameter iz ation (1.5) (repeated here for convenience):  

$$
L(N,D)=\left[\left({\frac{N_{c}}{N}}\right)^{\frac{\alpha_{N}}{\alpha_{D}}}+{\frac{D_{c}}{D}}\right]^{\alpha_{D}}
$$  

using three principles:  

1. Changes in vocabulary size or tokenization are expected to rescale the loss by an overall factor. The parameter iz ation of    $L(N,D)$   (and all models of the loss) must naturally allow for such a rescaling. 2. Fixing  $D$  nding    $N\to\infty$  , the ove ss should approach  $L(D)$  . Conversely, ﬁxing    $N$   and sending  →∞ the loss must approach  $L(N)$  . 3.    $L(N,D)$   should be analytic at    $D=\infty$  , so that it has a series expansion in  $1/D$   with integer powers. Theoretical support for this principle is signiﬁcantly weaker than for the ﬁrst two.  

Our choice of    $L(N,D)$   satisﬁes the ﬁrst requirement because we can rescale    $N_{c},D_{c}$   with changes in the vocabulary. This also implies that the values of    $N_{c},D_{c}$   have no fundamental meaning.  

![](images/487fdcec1080774d74c9f0fec2ec2c83f62aaf44df3d79c91585b82dc3f65b36.jpg)  
Figure 9 The early-stopped test loss    $L(N,D)$   depends predictably on the dataset size    $D$   and model size  $N$  according to Equation (1.5).  Left : For large    $D$  , performance is a straight power law in  $N$  . For a smaller ﬁxed  $D$  , performance stops improving as    $N$   increases and the model begins to overﬁt. (The reverse is also true, see Figure 4.)  Right : The extent of overﬁtting depends predominantly on the ratio    $N^{\frac{\alpha_{N}}{\alpha_{D}}}/D$  , as predicted in equation (4.3). The line is our ﬁt to that equation.  

Since we stop training early when the test loss ceases to improve and optimize all models in the same way, we expect that larger models should always perform better than smaller models. But with ﬁxed ﬁnite  $D$  , we also do not expect any model to be capable of approaching the best possible loss (ie the entropy of text). Similarly, a model with ﬁxed size will be capacity-limited. These considerations motivate our second principle. Note that knowledge of    $L(N)$   at inﬁnite  $D$   and  $L(D)$   at inﬁnite  $N$   fully determines all the parameters in  $L(N,D)$  .  

The third principle is more speculative. There is a simple and general reason one might expect overﬁtting to scale    $\propto\,1/D$   at very large    $D$  . Overﬁ  should be related to the variance or the signal-to-noise ratio of the dataset [AS17], and this scales as  $1/\bar{D}$  . This expectation should hold for any smooth loss function, e we expect to be able to expand the loss about the  $D\to\infty$  limit. However, this argument assumes that  $1/D$   corrections dominate over other sources of variance, such as the ﬁnite batch size and other limits on the efﬁcacy of optimization. Without empirical conﬁrmation, we would not be very conﬁdent of its applicability.  

Our third principle explains the asymmetry between the roles of    $N$   and    $D$   in Equation (1.5). Very similar symmetric expressions 4   are possible, but they would not have a    $1/D$   expansion with integer powers, and would require the introduction of an additional parameter.  

In any case, we will see that our equation for    $L(N,D)$   ﬁts the data well, which is the most important justiﬁ- cation for our  $L(N,D)$   ansatz.  

# 4.2 Results  

We regularize all our models with   $10\%$   dropout, and by tracking test loss and stopping once it is no longer decreasing. The results are displayed in Figure 9, including a ﬁt to the four parameters    $\alpha_{N},\alpha_{D},N_{c},D_{c}$   in Equation (1.5):  

![Table 2 Fits to  $L(N,D)$  ](images/6aabc47874bf2f2c8a97a01cde3bb82de4b3ef94abc0618617382b90411e93f8.jpg)  

We obtain an excellent ﬁt, with the exception of the runs where the dataset has been reduced by a factor of 1024 , to about    $2\times10^{7}$    tokens. With such a small dataset, an epoch consists of only 40 parameter updates. Perhaps such a tiny dataset represents a different regime for language modeling, as overﬁtting happens very early in training (see Figure 16). Also note that the parameters differ very slightly from those obtained in Section 3, as here we are ﬁtting the full    $L(N,D)$   rather than just  $L(N,\infty)$   or  $L(\infty,D)$  .  

To chart the borderlands of the inﬁnite data limit, we can directly study the extent of overﬁtting. For all but the largest models, we see no sign of overﬁtting when training with the full 22B token WebText2 dataset, so we can take it as representative of    $D=\infty$  . Thus we can compare ﬁnite    $D$   to the inﬁnite data limit by  

![](images/8a203bdb7cc4f44a6e8423b5e22f2d28e1f785c9ff980bb94657d31cfde9236b.jpg)  
Figure 10 The critical batch size  $B_{\mathrm{crit}}$   follows a power law in the loss as performance increase, and does not depend directly on the model size. We ﬁnd that the critical batch size approximately doubles for every  $13\%$   decrease in loss.  $B_{\mathrm{crit}}$   is measured empirically from the data shown in Figure 18, but it is also roughly predicted by the gradient noise scale, as in [MKAT18].  

deﬁning  

$$
\delta L(N,D)\equiv\frac{L(N,D)}{L(N,\infty)}-1
$$  

and studying it as a function of    $N,D$  . In fact, we see empirically that  $\delta L$   depends only a speciﬁc combination of    $N$   and    $D$  , as shown in Figure 16. This follows from the scaling law of Equation (1.5), which implies  

$$
\delta L\approx\left(1+\left({\frac{N}{N_{c}}}\right)^{\frac{\alpha_{N}}{\alpha_{D}}}{\frac{D_{c}}{D}}\right)^{\alpha_{D}}-1
$$  

Note that at large    $D$   this formula also has a series expansion in powers of  $1/D$  .  

We estimate that the variation in the loss with different random seeds is roughly  0 . 02 , which means that to avoid overﬁtting when training to within that threshold of convergence we require  

$$
D\gtrsim(5\times10^{3})\,N^{0.74}
$$  

With this relation, models smaller than    $10^{9}$    parameters can be trained with minimal overﬁtting on the 22B token WebText2 dataset, but our largest models will encounter some mild overﬁtting. More generally, this relation shows that dataset size may grow sub-linearly in model size while avoiding overﬁtting. Note however that this does not typically represent maximally compute-efﬁcient training. We should also emphasize that we have not optimized regularization (eg the dropout probability) while varying dataset and model size.  

# 5 Scaling Laws with Model Size and Training Time  

In this section we will demonstrate that a simple scaling law provides a good description for the loss as a function of model size  $N$   and training time. First we will explain how to use the results of [MKAT18] to deﬁne a universal training step  $S_{\mathrm{min}}$  , which accounts for the fact that most of our models have not been trained at an optimal batch size. Then we will demonstrate that we can ﬁt the model size and training time dependence of the loss using Equation (1.6). Later we will use these results to predict the optimal allocation of training compute between model size and training time, and then conﬁrm that prediction.  

# 5.1 Adjustment for Training at  $B_{\mathrm{crit}}(L)$  

A simple empirical theory for the batch size dependence of training was developed in [MKAT18] (see also  $[\mathrm{SLA}^{+}18$  ,   $\mathrm{ZLN^{+}19]}$  ). It was argued that there is a critical batch size    $B_{\mathrm{crit}}$   for training; for    $B$   up to    $B_{\mathrm{crit}}$  the batch size can be increased with very minimal degradation in compute-efﬁciency, whereas for  $B>B_{\mathrm{crit}}$  increases in  $B$   result in diminishing returns. It was also argued that the gradient noise scale provides a simple prediction for    $B_{\mathrm{crit}}$  , and that neither depends directly on model size except through the value of the loss that has been attained. These results can be used to predict how training time and compute will vary with the batch size. To utilize both training time and compute as effectively as possible, it is best to train with a batch size    $B\approx B_{\mathrm{crit}}$  . Training at    $B\gg B_{\mathrm{crit}}$   minimizes the number of training steps, while    $B\ll B_{\mathrm{crit}}$   minimizes the use of compute.  

More speciﬁcally, it was demonstrated that for a wide variety of neural network tasks, the number of training steps  $S$   and the number of data examples processed    $E=B S$   satisfy the simple relation  

$$
\left(\frac{S}{S_{\mathrm{min}}}-1\right)\left(\frac{E}{E_{\mathrm{min}}}-1\right)=1
$$  

when training to any ﬁxed value of the loss    $L$  . Here  $S_{\mathrm{min}}$   is the minimum number of steps necessary to reach  $L$  , while  $E_{\mathrm{min}}$   is the minimum number of data examples that must be processed.  

We demonstrate the relation (5.1) for Transformers in Figure 18 in the appendix. This relation deﬁnes the critical batch size  

$$
B_{\mathrm{crit}}(L)\equiv\frac{E_{\mathrm{min}}}{S_{\mathrm{min}}}
$$  

which is a function of the target value of the loss. Training at the critical batch size makes a roughly optimal time/compute tradeoff, requiring  $2S_{\mathrm{min}}$   training steps and processing  $E=2E_{\mathrm{min}}$   data examples.  

In Figure 10 we have plotted the critical batch size and gradient noise scale 5   as a function of training loss for two different models. We see that    $B_{\mathrm{crit}}(L)$   is independent of model size, and only depends on the loss    $L$  . So the predictions of [MKAT18] continue to hold for Transformer language models. The critical batch size can be ﬁt with a power-law in the loss  

$$
B_{\mathrm{crit}}(L)\approx\frac{B_{*}}{L^{1/\alpha_{B}}}
$$  

where  $B_{*}\approx2\times10^{8}$    and    $\alpha_{B}\approx0.21$  .  

We have chosen this parameter iz ation for    $B_{\mathrm{crit}}(L)$   because as the loss approaches its minimum value    $L_{\mathrm{min}}$  , the gradient noise scale is expected to diverge, and we expect    $B_{\mathrm{crit}}$   to track this noise scale. We do not know  $L_{\mathrm{min}}$  , as we see no sign that our models are approaching it, but  $L_{\mathrm{min}}>0$   since the entropy of natural language is non-zero. Since apparently  $L_{\mathrm{min}}$   is much smaller than the values of  $L$   we have achieved, we used a parameter iz ation where    $B_{\mathrm{crit}}$   diverges as    $L\rightarrow0$  .  

We will use  $B_{\mathrm{crit}}(L)$   to estimate the relation between the number of training steps  $S$   while training at batch size    $B=2^{19}$    tokens and the number of training steps while training at  $B\gg B_{\mathrm{crit}}$  . This is simply  

$$
S_{\mathrm{min}}(S)\equiv{\frac{S}{1+B_{\mathrm{crit}}(L)/B}}\qquad\mathrm{(minimum~steps,~at}\;B\gg B_{\mathrm{crit}})
$$  

for any given target value  $L$   for the loss. This also deﬁnes a critical value of the compute needed to train to  $L$  with a model of size  $N$   if we were to train at    $B\ll B_{\mathrm{crit}}(L)$  . This is  

$$
C_{\mathrm{min}}(C)\equiv{\frac{C}{1+B/B_{\mathrm{crit}}(L)}}\qquad\mathrm{(minimum\;compounde,\;at\;}B\ll B_{\mathrm{crit}})
$$  

where  $C=6N B S$   estimates the (non-embedding) compute used at batch size  $B$  .  

# 5.2 Results for    $L(N,S_{\mathrm{min}})$   and Performance with Model Size and Compute  

Now we will use    $S_{\mathrm{min}}$   deﬁned in Equation (5.4) to obtain a simple and universal ﬁt for the dependence of the loss on model size and training time in the inﬁnite data limit. We will ﬁt the stable, Adam-optimized training runs using Equation (1.6), repeated here for convenience:  

$$
L(N,S_{\operatorname*{min}})=\left(\frac{N_{c}}{N}\right)^{\alpha_{N}}+\left(\frac{S_{c}}{S_{\operatorname*{min}}}\right)^{\alpha_{S}}
$$  

for the loss. We include all training steps after the warmup period of the learning rate schedule, and ﬁnd a ﬁt to the data with the parameters:  

![](images/7df60fde9eb3accdc30bc08390db84ac9bdf5e77a70beead7431149078e85d7a.jpg)  
Figure 11 When we hold either total compute or number of training steps ﬁxed, performance follows  $L(N,S)$   from Equation (5.6). Each value of compute budget has an associated optimal model size that maximizes performance. Mediocre ﬁts at small  $S$   are unsurprising, as the power-law equation for the learning curves breaks down very early in training.  

![Table 3 Fits to    $L(N,S)$  ](images/094d5a4c05571be2c9af4abbbd8c61e569e280914ab0f8e5c638902e656ee2ba.jpg)  

With these parameters, we obtain the learning curve ﬁts in Figure 4. Though the ﬁts are imperfect, we believe they are quite compelling given the simplicity of Equation (5.6).  

The data and ﬁts can be visualized in a different and more interesting way, as shown in Figure 11. There we study the test loss as a function of model size while ﬁxing either the total non-embedding compute    $C$   used in training, or the number of steps    $S$  . For the ﬁts we use Equation (5.5) and (5.4) along with the parameters above and Equation (5.6).  

The power-law dependence of the loss on    $S_{\mathrm{min}}$   reﬂects the interplay of optimizer dynamics and the loss landscape. Since the ﬁts are best late in training, when the loss may be approximately quadratic, the power- law should provide information about the spectrum of the Hessian of the loss. Its universality suggests that the Hessian eigenvalue density is roughly independent of model size.  

# 5.3 Lower Bound on Early Stopping Step  

The results for  $L(N,S_{\mathrm{min}})$   can be used to derive a lower-bound (and rough estimate) of the step at which early stopping should occur when training is data limited. It is motivated by the idea that ﬁnite and inﬁnite  $D$  learning curves for a given model will be very similar until we  $S_{\mathrm{min}}\approx S_{\mathrm{stop}}$  . Thus ove ng should be proportional to the correction from simply ending training at  $S_{\mathrm{stop}}$  . This will underestimate  $S_{\mathrm{stop}}$  , because in reality the test loss will decrease more slowly when we have a ﬁnite    $D$  , and therefore we will require more training steps to reach the optimal test loss at ﬁnite    $D$  . This line of reasoning leads to the inequality  

$$
S_{\mathrm{stop}}(N,D)\gtrsim\frac{S_{c}}{\left[L(N,D)-L(N,\infty)\right]^{1/\alpha_{S}}}
$$  

where    $L(N,\infty)$   is the converged loss, evaluated with inﬁnite available data. This inequality and its  parison to the empirical data is displayed in Figure 16 in the appendix. In that ﬁgure, the values of  $S_{\mathrm{stop}}$  and  $L(N,D)$   are empiri gh    $S_{\mathrm{stop}}$   is a to mimic training at    $B\,\gg\,B_{\mathrm{crit}})$  , while    $L(N,\infty)$   is computed from the ﬁt to  $L(N,D)$   evaluated at  $D=\infty$   ∞ .  

# 6 Optimal Allocation of the Compute Budget  

We displayed the  empirical  trend of performance as a function of the computation used during training in the top-right of Figure 1. However, this result involved training at a ﬁxed batch size  $B$  , whereas we know  

![](images/2b3b255ade04504ae23755e6a28f64dc38663d8d4b2fd8adf935d1429c5ac117.jpg)  
Figure 12 Left:  Given a ﬁxed compute budget, a particular model size is optimal, though somewhat larger or smaller models can be trained with minimal additional compute.  Right:  Models larger than the compute- efﬁcient size require fewer steps to train, allowing for potentially faster training if sufﬁcient additional paral- lelism is possible. Note that this equation should not be trusted for very large models, as it is only valid in the power-law region of the learning curve, after initial transient effects.  

![](images/e97e690ed8b0f01bd9ffebf726e7a0d5f8ea283d34390a42eabf9af93b3852da.jpg)  
Figure 13 When adjusting performance to simulate training far below the critical batch size, we ﬁnd a somewhat altered power law for    $L(C_{\mathrm{min}})$   when compared with the fully empirical results. The conspicuous lump at    $10^{-5}$    PF-days marks the transition from 1-layer to 2-layer networks; we exclude 1-layer networks in the power-law ﬁts. It is the    $L(C_{\mathrm{min}})$   trend that we expect to provide a reliable extrapolation for larger compute.  

that in fact we could train more efﬁciently 6   by training at the batch size    $B_{\mathrm{crit}}$   discussed in Section 5.1. Large and small values of the loss could have been achieved with fewer samples or fewer steps, respectively, and correcting for this inefﬁciency by standardizing to the critical batch size results in cleaner and more predictable trends.  

In this section we will adjust for this oversight. More importantly, we will use the results of Section 5 to determine the optimal  allocation  of compute between model size  $N$   and the quantity of data processed during training, namely  $2B_{\mathrm{crit}}S_{\mathrm{min}}$  . We will determine this allocation both empirically and theoretically, by using the equation for  $L(N,S_{\mathrm{min}})$  , and we will demonstrate that these methods agree.  

# 6.1 Optimal Performance and Allocations  

Let us ﬁrst study the loss as a function of the optimally allocated compute from Equation (5.5). The result is plotted in Figure 13, along with a power-law ﬁt. We see that as compared to the compute plot of Figure 1, the new ﬁt with  $C_{\mathrm{min}}$   is somewhat improved.  

Given  $L(C_{\mathrm{min}})$  , it is natural to ask for the optimal model size  $N(C_{\mathrm{min}})$   that provides the minimal loss with a given quantity of training compute. The optimal model size is shown in Figure 14. We observe that  $N(C_{\mathrm{min}})$  

![](images/cb02b876d467af4cd6a7b7df9812d4da0762d73a80c65c6ebe4d0145b44ecd1f.jpg)  
Figure 14 Left:  Each value of the compute budget    $C_{\mathrm{min}}$   has an associated optimal model size  $N$  . Optimal model size grows very rapidly with    $C_{\mathrm{min}}$  , increasing by 5x for each  $10\mathbf{x}$   increase in compute. The number of data examples processed makes up the remainder of the increase, growing relatively modestly by only  $2\mathbf{X}$  . Right:  The batch-adjusted number of optimization steps also grows very slowly, if at all, meaning that most of the growth in data examples processed can be used for increased batch sizes.  

can be ﬁt very well with a power-law  

$$
N(C_{\mathrm{min}})\propto(C_{\mathrm{min}})^{0.73}.
$$  

In Figure 12, we show the effect of training models of sub-optimal sizes (see Appendix B.4).  

By deﬁnition  $C_{\mathrm{min}}\equiv6N B_{\mathrm{crit}}S$  e can use  $N(C_{\mathrm{min}})$   to e results. In particular, since prior ﬁts show  $B\propto L^{-4.8}$   ∝   and  $L\propto C_{\mathrm{min}}^{-0.05}$   ∝ , we can conclude that  $B_{\mathrm{crit}}\propto C_{\mathrm{min}}^{0.24}$   ∝ . This leads us to conclude that the optimal number of steps will only grow very slowly with compute, as  

$$
S_{\mathrm{min}}\propto(C_{\mathrm{min}})^{0.03},
$$  

matching the empirical results in Figure 14. In fact the measured exponent is sufﬁciently small that our results may even be consistent with an exponent of zero.  

Thus we conclude that as we scale up language modeling with an optimal allocation of computation, we ld predominantly increase the model size    $N$  , while simultaneously scaling up the batch size via    $B\,\propto$   $B_{\mathrm{crit}}$   with negligible increase in the number of serial steps. Since compute-efﬁcient training uses relatively few optimization steps, additional work on speeding up early training dynamics may be warranted.  

# 6.2 Predictions from    $L(N,S_{\mathrm{min}})$  

The results for    $L(C_{\mathrm{min}})$   and the allocations can be predicted from the    $L(N,S_{\mathrm{min}})$   equation obtained in Section 5. Given our equation for  $L(N,S_{\mathrm{min}})$  , we can substitute  $\begin{array}{r}{S_{\mathrm{min}}\,=\,\frac{C_{\mathrm{min}}}{6N B}}\end{array}$    and then ﬁnd the minimum of the loss as a function of  $N$  , while ﬁxing the training compute. We carry out this procedure in detail in Appendix B, where we also provide some additional predictions.  

For the loss as a function of training compute, we predict that  

$$
L(C_{\mathrm{min}})=\left(\frac{C_{c}^{\mathrm{min}}}{C_{\mathrm{min}}}\right)^{\alpha_{C}^{\mathrm{min}}}
$$  

where  

$$
\alpha_{C}^{\mathrm{min}}\equiv\frac{1}{1/\alpha_{S}+1/\alpha_{B}+1/\alpha_{N}}\approx0.054
$$  

in excellent agreement with the exponent of Figure 13. We also predict that  

$$
N(C_{\mathrm{min}})\propto(C_{\mathrm{min}})^{\alpha_{C}^{\mathrm{min}}/\alpha_{N}}\approx(C_{\mathrm{min}})^{0.71}
$$  

which also matches the scaling of Figure 14 to within a few percent. Our scaling laws provide a predictive framework for the performance of language modeling.  

![](images/f19f30256e2cf5108f8b5d76726655087e1053e837ff7f29f7cd6d4ec6e376fd.jpg)  
Figure 15 Far beyond the model sizes we study empirically, we ﬁnd a contradiction between our equations for  $L(C_{\mathrm{min}})$   and    $L(D)$   due to the slow growth of data needed for compute-efﬁcient training. The intersection marks the point before which we expect our predictions to break down. The location of this point is highly sensitive to the precise exponents from our power-law ﬁts.  

# 6.3 Contradictions and a Conjecture  

We observe no signs of deviation from straight power-law trends at large values of compute, data, or model size. Our trends must eventually level off, though, since natural language has non-zero entropy.  

Indeed, the trends for compute-efﬁcient training described in this section already contain an apparent contra- diction. At scales several orders of magnitude above those documented here, the performance predicted by the  $L(C_{\mathrm{min}})$   scaling law decreases below what should be possible given the slow growth in training data with compute. This implies that our scaling laws must break down before this point, but we conjecture that the intersection point has a deeper meaning: it provides an estimate of the point at which Transformer language models reach maximal performance.  

Since the amount of data used by compute-efﬁcient training grows slowly with the compute budget, the performance predicted by  $L(C_{\mathrm{min}})$   eventually hits a lower bound set by the  $L(D)$   power law (see Figure 15). Let us work this out in more detail.  

To keep overﬁtting under control, the results of Section 4 imply that we should scale the dataset size as  

$$
D\propto N^{0.74}\propto C_{\mathrm{min}}^{0.54}
$$  

where we have used the compute-efﬁcient  $N(C_{\mathrm{min}})$   from Figure 14.  

Let us compare this to the data requirements of compute-efﬁcient training. If we train at the critical batch size (i.e.    $C=2C_{\mathrm{min}})$  ) and never re-use data during training, we ﬁnd that data usage grows with compute as  

$$
D(C_{\mathrm{min}})=\frac{2C_{\mathrm{min}}}{6N(C_{\mathrm{min}})}\approx\left(4\times10^{10}\:\mathrm{totons}\right)\left(C_{\mathrm{min}}/\mathrm{PF-Day}\right)^{0.26}
$$  

This is the maximum rate at which the dataset size can productively grow with compute, since it means that we are only training for a single epoch. But it grows the dataset much more slowly than in Equation (6.6). It appears to imply that compute-efﬁcient training will eventually run into a problem with overﬁtting, even if the training process never re-uses any data!  

According to Figure 1, we expect that when we are bottlenecked by the dataset size (ie by overﬁtting), the loss should scale as    $L(D)\propto\dot{D}^{-0.095}$  . This implies that the loss would scale with compute as    $L(D(C_{\mathrm{min}}))\propto$   $C_{\mathrm{min}}^{-0.03}$  once we are data-limited. Once again, we have a contradiction, as this will eventually intersect with our prediction for    $L(C_{\mathrm{min}})$   from Figure 13, where we found a scaling    $L(C_{\mathrm{min}})\propto C_{\mathrm{min}}^{-0.050}$  .  

The intersection point of    $L(D(C_{\mathrm{min}}))$   and    $L(C_{\mathrm{min}})$   occurs at  

$$
C^{*}\sim10^{4}\;\mathrm{PF-Day}\quad N^{*}\sim10^{12}\;\mathrm{parameters},\quad D^{*}\sim10^{12}\;\mathrm{kg}\mathrm{Cs},\quad L^{*}\sim1.7\;\mathrm{nats}/\mathrm{token}
$$  

though the numerical values are highly uncertain, varying by an order or magnitude in either direction de- pending on the precise values of the exponents from the power-law ﬁts. The most obvious interpretation is that our scaling laws break down at or before we reach this point, which is still many orders of magnitude away in both compute and model size.  

One might also conjecture that this intersection point has a deeper meaning. If we cannot increase the model size beyond  $N^{*}$  without qualitatively different data requirements, perhaps this means that once we reach  $C_{\mathrm{min}}^{*}$    and  $N^{*}$  , we have extracted all of the reliable information available in natural language data. In this interpretation,    $L^{*}$  would provide a rough estimate for the entropy-per-token 7   of natural language. In this scenario, we would expect the loss trend to level off at or before  $L^{*}$  .  

We can guess at the functional form of    $L(C_{\mathrm{min}})$   as it levels off by considering a version of our training dataset with added noise. For example, we could append a random string of tokens to each context shown to the model to artiﬁcially boost the loss by a constant additive factor. Then, the distance from the noise ﬂoor    $L-L_{\mathrm{noise}}$   would be a more meaningful performance metric, with even a small decrease in this distance potentially representing a signiﬁcant boost in qualitative performance. Since the artiﬁcial noise would affect all of our trends equally, the critical point of 6.8 would not change (aside from the absolute value of    $L^{*}$  ), and may be meaningful even if it occurs after the leveling off.  

# 7 Related Work  

Power laws can arise from a wide variety of sources [THK18]. Power-law scalings with model and dataset size in density estimation   $[\mathrm{Was}06]$   and in random forest models [Bia12] may be connected with our results. These models suggest that power-law exponents may have a very rough interpretation as the inverse of the number of relevant features in the data.  

Some early [BB01, Goo01] work found power-law scalings between performance and dataset size. More recent work   $\mathrm{[HNA^{+}17}$  , HAD19] also investigated scaling between model size and data size; their work is perhaps the closest to ours in the literature 8 . Note, however, that   $[\mathrm{HNA^{+}17}]$   found super-linear scaling of dataset size with model size, whereas we ﬁnd a sub-linear scaling. There are some parallels between our ﬁndings on optimal allocation of compute and [Kom19], including power-law learning curves. EfﬁcientNets [TL19] also appear to obey an approximate power-law relation between accuracy and model size. Very recent work [RRBS19b] studies scaling with both dataset size and model size for a variety of datasets, and ﬁts an ansatz similar to ours.  

EfﬁcientNet [TL19] advocates scaling depth and width exponentially (with different coefﬁcients) for optimal performance of image models, resulting in a power-law scaling of width as a function of depth. We ﬁnd that for language models this power should be roughly one when scaling up (as width/depth should remain ﬁxed). But more importantly, we ﬁnd that the precise architectural hyperparameters are unimportant compared to the overall scale of the language model. In [VWB16] it was argued that deep models can function as ensembles of shallower models, which could potentially explain this ﬁnding. Earlier work [ZK16] has compared width and depth, and found that wide ResNets can outperform deep ResNets on image classiﬁcation. Some studies ﬁx computation per data example, which tends to scale in proportion to the number of model parameters, whereas we investigate scaling with both model size and the quantity of training computation.  

Various works [AS17, BHMM18] have investigated generalization in highly over parameterized models, ﬁnd- ing a “jamming transition”   $[\mathrm{GJS^{+}19}]$   when the model size reaches the dataset size (this may require training many orders of magnitude beyond typical practice, and in particular does not use early stopping). We do not observe such a transition, and ﬁnd that the necessary training data scales sublinearly in the model size. Expansions in the model size, particularly at large width [JGH18,  $\mathrm{LXS^{+}19]}$  ], may provide a useful framework for thinking about some of our scaling relations. Our results on optimization, such as the shape of learning curves, can likely be explained using a noisy quadratic model, which can provide quite accurate predictions  $\mathrm{[ZLN^{+}19]}$   in realistic settings. Making this connection quantitative will require a characterization of the Hessian spectrum [Pap18, GKX19, GARD18].  

# 8 Discussion  

We have observed consistent scalings of language model log-likelihood loss with non-embedding parameter count    $N$  , dataset size    $D$  , and optimized training computation    $C_{\mathrm{min}}$  , as encapsulated in Equations (1.5) and (1.6). Conversely, we ﬁnd very weak dependence on many architectural and optimization hyperparameters. Since scalings with  $N,D,C_{\mathrm{min}}$   are power-laws, there are diminishing returns with increasing scale.  

We were able to precisely model the dependence of the loss on  $N$   and  $D$  , and alternatively on    $N$   and    $S$  , when these parameters are varied simultaneously. We used these relations to derive the compute scaling, magnitude of overﬁtting, early stopping step, and data requirements when training large language models. So our scaling relations go beyond mere observation to provide a predictive framework. One might interpret these relations as analogues of the ideal gas law, which relates the macroscopic properties of a gas in a universal way, independent of most of the details of its microscopic consituents.  

It is natural to conjecture that the scaling relations will apply to other generative modeling tasks with a maximum likelihood loss, and perhaps in other settings as well. To this purpose, it will be interesting to test these relations on other domains, such as images, audio, and video models, and perhaps also for random network distillation. At this point we do not know which of our results depend on the structure of natural language data, and which are universal. It would also be exciting to ﬁnd a theoretical framework from which the scaling relations can be derived: a ‘statistical mechanics’ underlying the ‘thermodynamics’ we have observed. Such a theory might make it possible to derive other more precise predictions, and provide a systematic understanding of the limitations of the scaling laws.  

In the domain of natural language, it will be important to investigate whether continued improvement on the loss translates into improvement on relevant language tasks. Smooth quantitative change can mask major qualitative improvements: “more is different”. For example, the smooth aggregate growth of the economy provides no indication of the speciﬁc technological developments that underwrite it. Similarly, the smooth improvements in language model loss may hide seemingly qualitative changes in capability.  

Our results strongly suggest that larger models will continue to perform better, and will also be much more sample efﬁcient than has been previously appreciated. Big models may be more important than big data. In this context, further investigation into model parallelism is warranted. Deep models can be trained using pipelining   $[\mathrm{HCC^{+}18}]$  , which splits parameters depth-wise between devices, but eventually requires increased batch sizes as more devices are used. Wide networks on the other hand are more amenable to parallelization

  $[\mathrm{SCP^{+}}18]$  , since large layers can be split between multiple workers with less serial dependency. Sparsity

 [CGRS19, GRK17] or branching (e.g. [KSH12]) may allow for even faster training of large networks through increased model parallelism. And using methods like [WRH17, WYL19], which grow networks as they train, it might be possible to remain on the compute-efﬁcient frontier for an entire training run.  

# Acknowledgements  

We would like to thank Shan Carter, Paul Christiano, Jack Clark, Ajeya Cotra, Ethan Dyer, Jason Eisner, Danny Hernandez, Jacob Hilton, Brice Menard, Chris Olah, and Ilya Sutskever for discussions and for feed- back on drafts of this work.  

# Appendices  

# A Summary of Power Laws  

For easier reference, we provide a summary below of the key trends described throughout the paper.  

![Table 4 ](images/465d255f45ebc5c4028968136c0bebde7d4dd3f728925fd0531ec43d011de0f7.jpg)  

The empirical ﬁtted values for these trends are:  

![Table 5 ](images/ce48a2a5c5741e46af9b16fa967472b8dc43e7c3ca388d86ea170f7d5b225733.jpg)  

The optimal parameters for compute efﬁcient training are given by:  

![Table 6 ](images/e55f9d9ede2c662e3fdc663cc47df746a20c82d96922c68b97476d67996b84b0.jpg)  

# B Empirical Model of Compute-Efﬁcient Frontier  

Throughout this appendix all values of  $C,S$  ,  and  $\alpha_{C}$   are adjusted for training at the critical batch size  $B_{\mathrm{crit}}$  . We have left off the ‘adj’ label to avoid cluttering the notation.  

# B.1 Deﬁning Equations  

The power-law ﬁt to the learning curves implies a simple prescription for compute-efﬁcient training. In this appendix, we will derive the optimal performance, model size, and number of training steps as a function of the compute budget. We start with the Equation (1.6), repeated here for convenience:  

$$
L\left(N,S\right)=\left(\frac{N_{c}}{N}\right)^{\alpha_{N}}+\left(\frac{S_{c}}{S}\right)^{\alpha_{S}}.
$$  

Here,    $S$   represents the number of parameter updates when training  at the critical batch size  [MKAT18], which was deﬁned in Equation   $(5.2\Bar{)}^{9}$  :  

$$
B\left(L\right)=\frac{B_{*}}{L^{1/\alpha_{B}}}.
$$  

We would like to determine optimal training parameters for a ﬁxed compute budget, so we replace    $S\,=$   $C/\left(6N B\left(L\right)\right)$  , where    $C$   is the number of FLOPs used in the training run:  

$$
L\left(N,C\right)=\left(\frac{N_{c}}{N}\right)^{\alpha_{N}}+\left(6B_{*}S_{c}\frac{N}{L^{1/\alpha_{B}}C}\right)^{\alpha_{S}}.
$$  

Now, we set    $\partial_{N}L\big|_{C}=0$   to ﬁnd the condition for optimality:  

$$
\begin{array}{c}{0=\displaystyle\frac{\partial L}{\partial N}|_{C}}\\ {=-\displaystyle\frac{\alpha_{N}}{N}\left(\displaystyle\frac{N_{c}}{N}\right)^{\alpha_{N}}+\displaystyle\frac{\alpha_{S}}{N}\left(6B_{*}S_{c}\displaystyle\frac{N}{L^{1/\alpha_{B}}C}\right)^{\alpha_{S}}\left(1-5\displaystyle\frac{N}{L}\displaystyle\frac{\partial L}{\partial N}\Big|_{C}\right)}\\ {\implies\displaystyle\frac{\alpha_{N}}{\alpha_{S}}\left(\displaystyle\frac{N_{c}}{N}\right)^{\alpha_{N}}=\left(6B_{*}S_{c}\displaystyle\frac{N}{L^{1/\alpha_{B}}C}\right)^{\alpha_{S}}}\end{array}
$$  

Equation (B.3) and (B.4) together determine the compute-efﬁcient frontier.  

# B.2 Efﬁcient Training  

Now we assemble the implications of (B.3) and (B.4). First, note that inserting (B.4) into (B.3) yields  

$$
L\left(N_{\mathrm{eff}}\left(C\right),C\right)=\left(1+\frac{\alpha_{N}}{\alpha_{S}}\right)L\left(N_{\mathrm{eff}},\infty\right),
$$  

which implies that for compute-efﬁcient training, we should train to a  ﬁxed percentage    $\begin{array}{r}{\frac{\alpha_{N}}{\alpha_{S}}\approx10\%}\end{array}$   above the converged loss. Next, let’s determine how the optimal loss depends on the compute budget. Eliminating  $N$   yields a power-law dependence of performance on compute:  

$$
L\left(C\right)=\left(\frac{C_{c}}{C}\right)^{\alpha_{C}}
$$  

where we deﬁned  

$$
\begin{array}{l}{\alpha_{C}=1/\left(1/\alpha_{S}+1/\alpha_{B}+1/\alpha_{N}\right)\approx0.052}\\ {C_{c}=6N_{c}B_{*}S_{c}\left(1+\displaystyle\frac{\alpha_{N}}{\alpha_{S}}\right)^{1/\alpha_{S}+1/\alpha_{N}}\left(\displaystyle\frac{\alpha_{S}}{\alpha_{N}}\right)^{1/\alpha_{S}}.}\end{array}
$$  

Similarly, we can eliminate  $L$   to ﬁnd    $N\left(C\right)$  :  

$$
{\frac{N\left(C\right)}{N_{c}}}=\left({\frac{C}{C_{c}}}\right)^{\alpha_{C}/\alpha_{N}}\left(1+{\frac{\alpha_{N}}{\alpha_{S}}}\right)^{1/\alpha_{N}}
$$  

and  

$$
S\left(C\right)=\frac{C_{c}}{6N_{c}B_{*}}\left(1+\frac{\alpha_{N}}{\alpha_{S}}\right)^{-1/\alpha_{N}}\left(\frac{C}{C_{c}}\right)^{\alpha_{C}/\alpha_{S}}
$$  

9 There is a slight ambiguity here: we can imagine training either at a constant batch size    $B\left(L_{\mathrm{target}}\right)$  , or we could instead train at a variable batch size  $\tilde{B}\left(L\right)$  , where  $\tilde{B}$   is the instantaneous critical batch size (as opposed to    $B$  , which is the averaged version). These two prescriptions result in the same number of steps, so we can ignore this subtlety (see [MKAT18]).  

# B.3 Comparison to Inefﬁcient  

Typically, researchers train models until they appear to be close to convergence. In this section, we compare the efﬁcient training procedure described above to this more typical setup. We deﬁne a the convergence factor  $f$   as the percent deviation from the converged loss:  

$$
L\left(N,C\right)=\left(1+f\right)L\left(N,\infty\right).
$$  

For compute-efﬁcient training we have    $f\;=\;\alpha_{N}/\alpha_{S}\;\approx\;10\%$   from the previous section, but researchers typically use a much smaller value. Here, we choose  $f^{\prime}=2\%$   as an estimate. For a ﬁxed value of the loss, we predict:  

$$
\begin{array}{r l}&{\frac{N_{f}}{N_{f^{\prime}}}=\left(\frac{1+f}{1+f^{\prime}}\right)^{1/\alpha_{N}}\approx2.7}\\ &{\frac{S_{f}}{S_{f^{\prime}}}=\left(\frac{1+\frac{1}{f}}{1+\frac{1}{f^{\prime}}}\right)^{1/\alpha_{S}}\approx0.13}\\ &{\frac{C_{f}}{C_{f^{\prime}}}=\frac{N_{f}}{N_{f^{\prime}}}\frac{S_{f}}{S_{f^{\prime}}}\approx0.35}\end{array}
$$  

So that compute-efﬁcient training uses   $7.7\mathrm{x}$   fewer parameter updates,   $2.7\mathrm{x}$   more parameters, and  $65\%$   less compute to reach the same loss.  

# B.4 Suboptimal Model Sizes  

We can solve A.1 to ﬁnd an expression for the amount of compute needed to reach a given value of the loss  $L$   with a model of size  $N$  :  

$$
C\left(N,L\right)=\left(6B_{*}S_{c}\frac{N}{L^{1/\alpha_{B}}}\right)\left(L-\left(\frac{N_{c}}{N}\right)^{\alpha_{N}}\right)^{-1/\alpha_{S}}.
$$  

Using A.6 and A.9, we can eliminate    $L$   in favor of    $N_{\mathrm{eff}}\left(L\right)$  , the model size which reaches    $L$   most efﬁciently. From there, we ﬁnd an expression for the excess compute needed as a consequence of using a suboptimal model size:  

$$
\frac{C\left(N,N_{\mathrm{eff}}\right)}{C\left(N_{\mathrm{eff}},N_{\mathrm{eff}}\right)}=\frac{N}{N_{\mathrm{eff}}}\left[1+\frac{\alpha_{S}}{\alpha_{N}}\left(1-\left(\frac{N_{\mathrm{eff}}}{N}\right)^{\alpha_{N}}\right)\right]^{-1/\alpha_{S}}.
$$  

The result is shown in Figure X. Models between   $0.6\ensuremath{\mathrm{x}}$   and   $2.2\mathbf{x}$   the optimal size can be used with only a  $20\%$   increase in compute budget. Using a smaller model is useful when accounting for the cost inference. A larger model can be trained the the same level of performance in fewer steps, allowing for more parallelism and faster training if sufﬁcient harware is available (see Figure Y):  

$$
\frac{S\left(N,N_{\mathrm{eff}}\right)}{S\left(N_{\mathrm{eff}},N_{\mathrm{eff}}\right)}=\left[1+\frac{\alpha_{S}}{\alpha_{N}}\left(1-\left(\frac{N_{\mathrm{eff}}}{N}\right)^{\alpha_{N}}\right)\right]^{-1/\alpha_{S}}.
$$  

A  $2.2\mathbf{x}$   larger model requires  $45\%$   fewer steps at a cost of  $20\%$   more training compute. Note that this equation should not be trusted for very large models, as it is only valid in the power-law region of the learning curve after initial transient effects.  

# C Caveats  

In this section we list some potential caveats to our analysis.  

•  At present we do not have a solid theoretical understanding for any of our proposed scaling laws. The scaling relations with model size and compute are especially mysterious. It may be possible to understand scaling at very large  $D$   holding model size ﬁxed [AS17], and also the shape of learning curves late in training, by modeling the loss with a noisy quadratic. But the scaling with    $D$   at very large model size still remains mysterious. Without a theory or a systematic understanding of the corrections to our scaling laws, it’s difﬁcult to determine in what circumstances they can be trusted.  

![](images/70647fd2094c5b3f72ffc8b84417e4a97032ac5e0975b841b51286301c914a52.jpg)  
Figure 16 Left:  We characterize the step on which early stopping occurs, as a function of the extent of overﬁtting. The red line indicates a lower bound for early stopping that is derived in Section 5.3.  Right: We display train and test loss for a series of 300M parameter models trained on different sized dataset sub- samples. The test loss typically follows that of a run done with unrestricted data until diverging. Note that the degree of overﬁtting (as compared to the inﬁnite data limit) is signiﬁcantly overestimated by    $L_{\mathrm{test}}-L_{\mathrm{train}}$  (denoted by a black bar for each run).  

•  We are not especially conﬁdent in the iction of    $B_{\mathrm{crit}}(L)$   for values of the loss far outside the range we have explored. Changes in  $B_{\mathrm{crit}}$   could have a signiﬁcant impact on trade-offs between data parallelism and the number of serial training steps required, which would have a major impact on training time. •  We did not thoroughly vestigate the small data regime, and our ﬁts for    $L(N,D)$   were poor for the smallest values of  D  (where an epoch corresponded to only  40  steps). Furthermore, we did not experiment with regularization and data augmentation. Improvements in these could alter our results, quantitatively or qualitatively. •  We used estimated training compute    $C\approx6N B S$  , which did not include contributions propor- tional to  $n_{\mathrm{{ctx}}}$   (see Section 2.1). So our scalings with compute may be confounded in practice in the regime of very large    $n_{\mathrm{{ctx}}}$  , speciﬁcally where    $n_{\mathrm{ctx}}\gtrsim12d_{\mathrm{model}}$  . •  We tuned learning rates, and we experimented with learning rate schedules. But we may have neglected to tune some hyperparameter (e.g. intialization scale or momentum) that have an important effect on scaling. •  The optimal choice of learning rate is sensitive to the target loss. When training close to convergence, it may be necessary to use a smaller learning rate to avoid divergences. But when conducting a short training run (eg due to compute limitations), it may be possible to use a larger learning rate. We did not experiment with higher learning rates for training runs that did not proceed to convergence.  

# D Supplemental Figures  

# D.1 Early Stopping and Test vs Train  

In section 5.3 we described the result shown in Figure 16, which provides a prediction for a lower bound on the early stopping step. We also show the train and test loss for a given model size when training on different sized datasets.  

# D.2 Universal Transformers  

We compare the performance of standard Transformers to recurrent Transformers   $[\mathrm{DGeV^{+}}18]$   in Figure 17. These models re-use parameters, and so perform slightly better as a function of    $N$  , but slightly worse as a function of compute  $C$  . We include several different different possibilities for parameter re-use.  

# D.3 Batch Size  

We measure the critical batch size using the data displayed in ﬁgure 18. This made it possible to estimate  $B_{\mathrm{crit}}(L)$   in ﬁgure 10.  

![](images/09dbaa6b31762c5008936fa2004788cf5bfb70ae6108c7da00ae564a17ee3001.jpg)  
Figure 17 We compare recurrent Transformers   $[\mathrm{DGeV^{+}}18]$  , which re-use parameters, to standard Trans- formers. Recurrent Transformers perform slightly better when comparing models with equal parameter count, but slightly worse when accounting for reuse and comparing per FLOP.  

![](images/6d4d01d947f02f82c9357827badf5d9b812eb86a5b305ec348381671046079d0.jpg)  
Figure 18 These ﬁgures demonstrate ﬁts to Equation (5.1) for a large number of values of the loss  $L$  , and for two different Transformer model sizes. These ﬁts were used to measure  $B_{\mathrm{crit}}(L)$   for Figure 10.  

# D.4 Sample Efﬁciency vs Model Size  

It is easy to see from ﬁgure 2 that larger models train faster, and are therefore more sample efﬁcient. We provide another way of looking at this phenomenon in ﬁgure 19, which shows when different models reach various ﬁxed values of the loss.  

![](images/8c50ac44e2dddd522ccff854e25a415bdbe2c88de16fcfff1b0f6623c6df592e.jpg)  
Figure 19 The number of minimum serial steps needed to reach any ﬁxed value of the test loss decreases precipitously with model size. Sample efﬁciency (show here for training far below the critical batch size) improves greatly as well, improving by a factor of almost 100 when comparing the smallest possible model to a very large one.  

![](images/56ca7002a52f06628a4c60c78724506c00f8cc61c353e68d954133c2f2868662.jpg)  
Figure 20 This ﬁgure provides information about the performance per token as a function of model size and training time.  Left:  Loss per token as a function of its position  $T$   in the 1024-token context. Loss scales predictably as a power-law in  $T$  .  Right:  Test loss per token as a function of training step.  

![](images/4d044727d6689745025ef0a46042b4ed22cebb292e5451521b1ecd417c382c9a.jpg)  
Figure 21 In addition to the averaged loss, individual tokens within the 1024-token context also improve smoothly as model size increases. Training runs with shorter context  $n_{\mathrm{{ctx}}}=8$   (dashed lines) perform better on early tokens, since they can allocate all of their capacity to them.  

# D.5 Context Dependence  

The trends for loss as a function of model size are displayed for different tokens in the context in Figure 21. We see that models trained on    $n_{\mathrm{ctx}}\,=\,1024$   show steady improvement with model size on all but the ﬁrst token.  

Fixing model size, it appears that the loss scales as a power-law as a function of position  $T$   in the context, see Figure 20. This may be a consequence of underlying power-law correlations in language [EP94, ACDE12, LT16], or a more general feature of the model architecture and optimization. It provides some suggestion for the potential beneﬁts (or lack thereof) from training on larger contexts. Not only do larger models converge to better performance at    $T=1024$  , but they also improve more quickly at early tokens, suggesting that larger models are more efﬁcient at detecting patterns with less contextual information. In the right-hand plot we show how per-token performance varies for a ﬁxed model as a function of the training step. The model begins by learning short-range information, and only learns longer-range correlations later in training.  

We have also included models trained with a tiny context    $n_{\mathrm{{ctx}}}\,=\,8$   in order to compare with our longer context models. Even modestly sized models trained on  $n_{\mathrm{{ctx}}}\,=\,8$   can dominate our largest    $n_{\mathrm{ctx}}\,=\,1024$  models on very early tokens. This also suggests that further improvements should be possible with much larger models trained on large contexts.  

# D.6 Learning Rate Schedules and Error Analysis  

We experimented with a variety of learning rates and schedules. A host of schedules and resulting test performances for a small language model are plotted in Figure 22. We conclude that the choice of learning rate schedule is mostly irrelevant, as long as the total summed learning rate is sufﬁciently large, and the schedule includes a warmup period and a ﬁnal decay to near-vanishing learning rate. Variations among  

![](images/28ed2000ee6d05a456b95bd4e24059d7db46d28994bf87cb982fe0ea7dfe687e.jpg)  
Figure 22 We test a variety of learning rate schedules including cosine decay, linear decay, as well as other faster/slower decays schedules on a 3 million parameter model, shown on the left. For these experiments we do not decay to zero, since we ﬁnd that this tends to give a ﬁxed improvement close to the end of training. We ﬁnd that, as long as the learning rate is not too small and does not decay too quickly, performance does not depend strongly on learning rate. Run-to-run variation is at the level of 0.05 in the loss, so averaging multiple runs is necessary to validate performance changes smaller than this level.  

![](images/60190451fd5756e5a083b4a5718b50739de9c3b154313797851d68306c319bba.jpg)  
Figure 23 The trend for performance as a function of parameter count,    $L(N)$  , is ﬁt better by a power law than by other functions such as a logarithm at a qualitative level.  

schedules appear to be statistical noise, and provide a rough gauge for the scale of variation between different training runs. Experiments on larger models suggest that the variation in the ﬁnal test loss between different random seeds is roughly constant in magnitude for different model sizes.  

We found that larger models require a smaller learning rate to prevent divergence, while smaller models can tolerate a larger learning rate. To implement this, the following rule of thumb was used for most runs:  

$$
\mathrm{LR}(N)\approx0.003239+-0.0001395\log(N)
$$  

We expect that this formula could be improved. There may be a dependence on network width, likely set by the initialization scale. The formula also breaks down for  $\dot{N}>10^{10}$    parameters. Nevertheless, we found that it works sufﬁciently well for the models we considered.  

# D.7 Fit Details and Power Law Quality  

We experimented with a number of functional forms for the ﬁts to    $L(N),L(C)$  , and  $L(D)$  ; the power-law ﬁts were qualitatively much more accurate than other functions such as logarithms (see Figure 23).  

For  $L(C)$  , we do not include small models with only 1 layer in the ﬁt, as the transition from 1 to 2 layers causes a noticable lump in the data. For  $L(N)$   we also do not include very small models with only 1 layer in the ﬁt, and we exclude the largest models that have not trained fully to convergence. Fit parameters change marginally if we do include them, and the trend extrapolates well in both directions regardless.  

# D.8 Generalization and Architecture  

In ﬁgure 24 we show that generalization to other data distributions does not depend on network depth when we hold the total parameter count ﬁxed. It seems to depend only on the performance on the training distribution.  

![](images/50f1ffeb7a7f5e8e08bb72155794015aa8f770e194255be96a5a53a68320fcb7.jpg)  
Figure 24 We show evaluations on a series of datasets for models with approximately 1.5 Billion param- eters. We observe no effect of depth on generalization; generalization performance depends primarily on training distribution performance. The 12-layer model overﬁt the Internet Books dataset and we show the early-stopped performance; we have not seen this surprising result in other experiments.  

# List of Figures  

1 Summary of simple power laws. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3 2 Illustration of sample efﬁciency and compute efﬁciency. . . . . . . . . . . . . . . . . . . . . 4 3 How to scale up model size, batch size, and serial steps . . . . . . . . . . . . . . . . . . . . 4 4 Performance when varying model and data size, or model and training steps, simultaneously 5 5 Weak dependence of performance on hyperparameter tuning . . . . . . . . . . . . . . . . . 8 6 Comparison of performance trend when including or excluding embeddings . . . . . . . . . 8 7 LSTM and Transformer performance comparison . . . . . . . . . . . . . . . . . . . . . . . 9 8 Generalization to other test datasets . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 10 9 Universality of overﬁtting . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 11 10 Critical batch size . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 12 11 Performance versus compute budget or number of parameter updates . . . . . . . . . . . . . 14 12 Training on suboptimal models . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 15 13 Comparison between empirical and adjusted compute trends . . . . . . . . . . . . . . . . . 15 14 Optimal model size and serial number of steps versus compute budget . . . . . . . . . . . . 16 15 Contradiction between compute and data trends . . . . . . . . . . . . . . . . . . . . . . . . 17 16 Early stopping lower bound and training curves for overﬁt models . . . . . . . . . . . . . . 23 17 Universal transformers . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 24 18 Batch size scans . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 24 19 Another look at sample efﬁciency . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 24 20 Power-law dependence of performance on position in context . . . . . . . . . . . . . . . . . 25 21 Performance at different context positions versus model size . . . . . . . . . . . . . . . . . 25 22 Learning rate schedule scan . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 26 23 Comparison of Power-Law and Logarithmic Fits . . . . . . . . . . . . . . . . . . . . . . . 26 24 Generalization versus depth . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 27  

# List of Tables  

1 Parameter and compute counts for Transformer . . . . . . . . . . . . . . . . . . . . . . . . 7 2 Fits to  $L(N,D)$   . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 11 3 Fits to  $L(N,S)$   . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 14 4 Key trend equations . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 20 5 Key parameters to trend ﬁts . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 20 6 Trends for compute-efﬁcient training . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 20  

# References  

[ACDE12] Eduardo G Altmann, Giampaolo Cristadoro, and Mirko Degli Esposti. On the origin of long- range correlations in texts.  Proceedings of the National Academy of Sciences , 109(29):11582– 11587, 2012. 25

 [AS17] Madhu S. Advani and Andrew M. Saxe. High-dimensional dynamics of generalization error in neural networks.  arXiv , 2017, 1710.03667. 11, 18, 22

 [BB01] Michele Banko and Eric Brill. Scaling to very very large corpora for natural language disam- biguation. In  Proceedings of the 39th annual meeting on association for computational linguis- tics , pages 26–33. Association for Computational Linguistics, 2001. 18

 [BHMM18] Mikhail Belkin, Daniel Hsu, Siyuan Ma, and Soumik Mandal. Reconciling modern machine learning and the bias-variance trade-off.  arXiv , 2018, 1812.11118. 18

 [Bia12] GÃŠrard Biau. Analysis of a random forests model.  Journal of Machine Learning Research , 13(Apr):1063–1095, 2012. 18

 [CGRS19] Rewon Child, Scott Gray, Alec Radford, and Ilya Sutskever. Generating long sequences with sparse transformers.  CoRR , abs/1904.10509, 2019, 1904.10509. URL  http://arxiv.org/ abs/1904.10509 . 19

 [DCLT18] Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. Bert: Pre-training of deep bidirectional transformers for language understanding, 2018, arXiv:1810.04805. 2

  $[\mathrm{DGeV^{+}}18]$   Mostafa Dehghani, Stephan Gouws, Oriol Vinyals, Jakob Uszkoreit, and Lukasz Kaiser. Uni- versal transformers.  CoRR , abs/1807.03819, 2018, 1807.03819. URL  http://arxiv.org/ abs/1807.03819 . 6, 9, 23, 24

 [EP94] Werner Ebeling and Thorsten Pöschel. Entropy and long-range correlations in literary english. EPL (Europhysics Letters) , 26(4):241, 1994. 25

 [Fou] The Common Crawl Foundation. Common crawl. URL  http://commoncrawl.org . 7

 [GARD18] Guy Gur-Ari, Daniel A. Roberts, and Ethan Dyer. Gradient descent happens in a tiny subspace. 2018, arXiv:1812.04754. 18

  $\mathrm{[GJS^{+}19]}$  Mario Geiger, Arthur Jacot, Stefano Spigler, Franck Gabriel, Levent Sagun, Stéphane d’Ascoli, Giulio Biroli, Clément Hongler, and Matthieu Wyart. Scaling description of generalization with number of parameters in deep learning.  arXiv , 2019, 1901.01608. 18

 [GKX19] Behrooz Ghorbani, Shankar Krishnan, and Ying Xiao. An investigation into neural net op- timization via hessian eigenvalue density.  CoRR , abs/1901.10159, 2019, 1901.10159. URL http://arxiv.org/abs/1901.10159 . 18

 [Goo01] Joshua Goodman. A bit of progress in language modeling.  CoRR , cs.CL/0108005, 2001. URL http://arxiv.org/abs/cs.CL/0108005 . 18

 [GRK17] Scott Gray, Alec Radford, and Diederik P Kingma. Gpu kernels for block-sparse weights.  ope- nai.com , 2017. 19

 [HAD19] Joel Hestness, Newsha Ardalani, and Gregory Diamos. Beyond human-level accuracy: Compu- tational challenges in deep learning. In  Proceedings of the 24th Symposium on Principles and Practice of Parallel Programming, PPoPP ’19, pages 1–14, New York, NY, USA, 2019. ACM.doi:10.1145/3293883.3295710. 18  

$[\mathrm{HCC^{+}18}]$  Yanping Huang, Yonglong Cheng, Dehao Chen, HyoukJoong Lee, Jiquan Ngiam, Quoc V. Le, and Zhifeng Chen. Gpipe: Efﬁcient training of giant neural networks using pipeline parallelism. CoRR , abs/1811.06965, 2018, 1811.06965. URL  http://arxiv.org/abs/1811.06965 . 19

  $[\mathrm{HNA^{+}17}]$   Joel Hestness, Sharan Narang, Newsha Ardalani, Gregory Diamos, Heewoo Jun, Hassan Kia- ninejad, Md. Mostofa Ali Patwary, Yang Yang, and Yanqi Zhou. Deep learning scaling is pre- dictable, empirically, 2017, 1712.00409. 18

 [JGH18] Arthur Jacot, Franck Gabriel, and Clément Hongler. Neural tangent kernel: Convergence and generalization in neural networks. In  Advances in neural information processing systems , pages 8571–8580, 2018. 18

 [KB14] Diederik P. Kingma and Jimmy Ba. Adam: A method for stochastic optimization, 2014, 1412.6980. 7

[Kom19] Aran Komatsuzaki. One epoch is all you need, 2019, arXiv:1906.06669. 18

 [KSH12] Alex Krizhevsky, Ilya Sutskever, and Geoffrey E. Hinton. Imagenet classiﬁcation with deep convolutional neural networks. In  Proceedings of the 25th International Conference on Neural Information Processing Systems - Volume 1 , NIPS’12, pages 1097–1105, USA, 2012. Curran Associates Inc. URL  http://dl.acm.org/citation.cfm?id  $\bar{\;}$  2999134.2999257 . 19

  $[\mathrm{LCG^{+}}19]$  Zhenzhong Lan, Mingda Chen, Sebastian Goodman, Kevin Gimpel, Piyush Sharma, and Radu Soricut. Albert: A lite bert for self-supervised learning of language representations, 2019, 1909.11942. 9

  $[\mathrm{LOG^{+}}19]$  ] Yinhan Liu, Myle Ott, Naman Goyal, Jingfei Du, Mandar Joshi, Danqi Chen, Omer Levy, Mike Lewis, Luke Zettlemoyer, and Veselin Stoyanov. Roberta: A robustly optimized BERT pretrain- ing approach.  CoRR , abs/1907.11692, 2019, 1907.11692. URL  http://arxiv.org/abs/ 1907.11692 . 2

  $[\mathrm{L}S\mathrm{P}^{+}18]$  Peter J. Liu, Mohammad Saleh, Etienne Pot, Ben Goodrich, Ryan Sepassi, Lukasz Kaiser, and Noam Shazeer. Generating wikipedia by summarizing long sequences.  arXiv:1801.10198 [cs] , 2018, 1801.10198. URL  http://arxiv.org/abs/1801.10198 . 2, 6

 [LT16] Henry W Lin and Max Tegmark. Criticality in formal languages and statistical physics.  arXiv preprint arXiv:1606.06737 , 2016. 25

  $[\mathrm{LX}\mathrm{S}^{+}19]$  Jaehoon Lee, Lechao Xiao, Samuel S. Schoenholz, Yasaman Bahri, Roman Novak, Jascha Sohl- Dickstein, and Jeffrey Pennington. Wide neural networks of any depth evolve as linear models under gradient descent, 2019, arXiv:1902.06720. 18

 [MKAT18] Sam McCandlish, Jared Kaplan, Dario Amodei, and OpenAI Dota Team. An empirical model of large-batch training, 2018, arXiv:1812.06162. 3, 5, 6, 12, 13, 21

 [Pap18] Vardan Papyan. The full spectrum of deep net hessians at scale: Dynamics with sample size. CoRR , abs/1811.07062, 2018, 1811.07062. URL  http://arxiv.org/abs/1811.07062 . 18

 [RNSS18] Alec Radford, Karthik Narasimhan, Tim Salimans, and Ilya Sutskever. Improving language understanding by generative pre-training.  URL https://s3-us-west-2. amazonaws. com/openai- assets/research-covers/language unsupervised/language understanding paper. pdf , 2018. 2, 6

 [RRBS19a] Jonathan S. Rosenfeld, Amir Rosenfeld, Yonatan Belinkov, and Nir Shavit. A constructive prediction of the generalization error across scales, 2019, 1909.12673. 18

 [RRBS19b] Jonathan S. Rosenfeld, Amir Rosenfeld, Yonatan Belinkov, and Nir Shavit. A constructive prediction of the generalization error across scales, 2019, arXiv:1909.12673. 18

  $[\mathrm{RSR^{+}}19]$  Colin Raffel, Noam Shazeer, Adam Roberts, Katherine Lee, Sharan Narang, Michael Matena, Yanqi Zhou, Wei Li, and Peter J. Liu. Exploring the limits of transfer learning with a uniﬁed text-to-text transformer, 2019, arXiv:1910.10683. 2

  $[\mathrm{RWC^{+}}19]$   Alec Radford, Jeff Wu, Rewon Child, David Luan, Dario Amodei, and Ilya Sutskever. Language models are unsupervised multitask learners.  openai.com , 2019. 2, 5, 6, 7, 8

  $[\mathrm{SCP^{+}}18]$  Noam Shazeer, Youlong Cheng, Niki Parmar, Dustin Tran, Ashish Vaswani, Penporn Koanan- takool, Peter Hawkins, HyoukJoong Lee, Mingsheng Hong, Cliff Young, Ryan Sepassi, and Blake Hechtman. Mesh-tensorﬂow: Deep learning for supercomputers, 2018, 1811.02084. 19

 [SHB15] Rico Sennrich, Barry Haddow, and Alexandra Birch. Neural machine translation of rare words with subword units.  CoRR , 2015, 1508.07909. 6  

$[\mathrm{SLA^{+}18}]$  Christopher J. Shallue, Jaehoon Lee, Joe Antognini, Jascha Sohl-Dickstein, Roy Frostig, and George E. Dahl. Measuring the effects of data parallelism on neural network training, 2018, arXiv:1811.03600. 12

 [SS18] Noam Shazeer and Mitchell Stern. Adafactor: Adaptive learning rates with sublinear memory cost.  CoRR , abs/1804.04235, 2018, 1804.04235. URL  http://arxiv.org/abs/1804.04235 . 7

 [THK18] Stefan Thurner, Rudolf Hanel, and Peter Klimek.  Introduction to the theory of complex systems . Oxford University Press, 2018. 18

 [TL19] Mingxing Tan and Quoc V. Le. Efﬁcientnet: Rethinking model scaling for convolutional neural networks.  CoRR , abs/1905.11946, 2019, 1905.11946. URL  http://arxiv.org/abs/1905. 11946 . 18

  $[\mathrm{V}\mathrm{S}\mathbf{P}^{+}17]$  Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N Gomez, Ł ukasz Kaiser, and Illia Polosukhin. Attention is all you need. In I. Guyon, U. V. Luxburg, S. Bengio, H. Wallach, R. Fergus, S. Vishwanathan, and R. Garnett, editors,  Advances in Neural Information Processing Systems 30 , pages 5998–6008. Curran Associates, Inc., 2017. URL http://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf . 2, 6

 [VWB16] Andreas Veit, Michael Wilber, and Serge Belongie. Residual networks behave like ensembles of relatively shallow networks, 2016, arXiv:1605.06431. 8, 18

 [Was06] Larry Wasserman.  All of nonparametric statistics . Springer Science & Business Media, 2006. 18

  $\mathrm{[WDN^{+}19]}$   Alex Wang, Yada Pruksachatkun, Nikita Nangia, Amanpreet Singh, Julian Michael, Felix Hill, Omer Levy, and Samuel R. Bowman. Superglue: A stickier benchmark for general-purpose language understanding systems, 2019, 1905.00537. 2

 [WRH17] Yu-Xiong Wang, Deva Ramanan, and Martial Hebert. Growing a brain: Fine-tuning by in- creasing model capacity.  2017 IEEE Conference on Computer Vision and Pattern Recognition (CVPR) , Jul 2017. doi:10.1109/cvpr.2017.323. 19

 [WYL19] Wei Wen, Feng Yan, and Hai Li. Autogrow: Automatic layer growing in deep convolutional networks, 2019, 1906.02909. 19

  $[\mathrm{YbY^{+}}19]$   Zhilin Yang, Zihang Dai, Yiming Yang, Jaime Carbonell, Ruslan Salakhutdinov, and Quoc V. Le. Xlnet: Generalized autoregressive pretraining for language understanding, 2019, arXiv:1906.08237. 2

 [ZK16] Sergey Zagoruyko and Nikos Komodakis. Wide residual networks.  Procedings of the British Machine Vision Conference 2016 , 2016. doi:10.5244/c.30.87. 18

  $[Z\!\!K Z^{+}15]$  Yukun Zhu, Ryan Kiros, Rich Zemel, Ruslan Salakhutdinov, Raquel Urtasun, Antonio Tor- ralba, and Sanja Fidler. Aligning books and movies: Towards story-like visual explanations by watching movies and reading books.  2015 IEEE International Conference on Computer Vision (ICCV) , Dec 2015. doi:10.1109/iccv.2015.11. 7

  $\mathrm{[ZLN^{+}19]}$  Guodong Zhang, Lala Li, Zachary Nado, James Martens, Sushant Sachdeva, George E. Dahl, Christopher J. Shallue, and Roger B. Grosse. Which algorithmic choices matter at which batch sizes? insights from a noisy quadratic model.  CoRR , abs/1907.04164, 2019, 1907.04164. URL http://arxiv.org/abs/1907.04164 . 12, 18  