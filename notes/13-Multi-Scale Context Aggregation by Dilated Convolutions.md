# M ULTI -S CALE  C ONTEXT  A GGREGATION BY D ILATED  C ONVOLUTIONS  

Fisher Yu Princeton University  

Vladlen Koltun Intel Labs  

# A BSTRACT  

State-of-the-art models for semantic segmentation are based on adaptations of convolutional networks that had originally been designed for image classiﬁca- tion. However, dense prediction problems such as semantic segmentation are structurally different from image classiﬁcation. In this work, we develop a new convolutional network module that is speciﬁcally designed for dense prediction. The presented module uses dilated convolutions to systematically aggregate multi- scale contextual information without losing resolution. The architecture is based on the fact that dilated convolutions support exponential expansion of the receptive ﬁeld without loss of resolution or coverage. We show that the presented context module increases the accuracy of state-of-the-art semantic segmentation systems. In addition, we examine the adaptation of image classiﬁcation networks to dense prediction and show that simplifying the adapted network can increase accuracy.  

# 1 I NTRODUCTION  

Many natural problems in computer vision are instances of dense prediction. The goal is to com- pute a discrete or continuous label for each pixel in the image. A prominent example is semantic segmentation, which calls for classifying each pixel into one of a given set of categories (He et al., 2004; Shotton et al., 2009; Kohli et al., 2009; Kr¨ ahenb¨ uhl & Koltun, 2011). Semantic segmenta- tion is challenging because it requires combining pixel-level accuracy with multi-scale contextual reasoning (He et al., 2004; Galleguillos & Belongie, 2010).  

Signiﬁcant accuracy gains in semantic segmentation have recently been obtained through the use of convolutional networks (LeCun et al., 1989) trained by backpropagation (Rumelhart et al., 1986). Speciﬁcally, Long et al. (2015) showed that convolutional network architectures that had originally been developed for image classiﬁcation can be successfully repurposed for dense prediction. These reporposed networks substantially outperform the prior state of the art on challenging semantic seg- mentation benchmarks. This prompts new questions motivated by the structural differences between image classiﬁcation and dense prediction. Which aspects of the repurposed networks are truly nec- essary and which reduce accuracy when operated densely? Can dedicated modules designed specif- ically for dense prediction improve accuracy further?  

Modern image classiﬁcation networks integrate multi-scale contextual information via succes- sive pooling and subsampling layers that reduce resolution until a global prediction is obtained (Krizhevsky et al., 2012; Simonyan & Zisserman, 2015). In contrast, dense prediction calls for multi- scale contextual reasoning in combination with full-resolution output. Recent work has studied two approaches to dealing with the conﬂicting demands of multi-scale reasoning and full-resolution dense prediction. One approach involves repeated up-convolutions that aim to recover lost resolu- tion while carrying over the global perspective from downsampled layers (Noh et al., 2015; Fischer et al., 2015). This leaves open the question of whether severe intermediate downsampling was truly necessary. Another approach involves providing multiple rescaled versions of the image as input to the network and combining the predictions obtained for these multiple inputs (Farabet et al., 2013; Lin et al., 2015; Chen et al., 2015b). Again, it is not clear whether separate analysis of rescaled input images is truly necessary.  

In this work, we develop a convolutional network module that aggregates multi-scale contextual information without losing resolution or analyzing rescaled images. The module can be plugged into existing architectures at any resolution. Unlike pyramid-shaped architectures carried over from image classiﬁcation, the presented context module is designed speciﬁcally for dense prediction. It is a rectangular prism of convolutional layers, with no pooling or subsampling. The module is based on dilated convolutions, which support exponential expansion of the receptive ﬁeld without loss of resolution or coverage.  

As part of this work, we also re-examine the performance of repurposed image classiﬁcation net- works on semantic segmentation. The performance of the core prediction modules can be uninten- tionally obscured by increasingly elaborate systems that involve structured prediction, multi-column architectures, multiple training datasets, and other augmentations. We therefore examine the leading adaptations of deep image classiﬁcation networks in a controlled setting and remove vestigial com- ponents that hinder dense prediction performance. The result is an initial prediction module that is both simpler and more accurate than prior adaptations.  

Using the simpliﬁed prediction module, we evaluate the presented context network through con- trolled experiments on the Pascal VOC 2012 dataset (Everingham et al., 2010). The experiments demonstrate that plugging the context module into existing semantic segmentation architectures re- liably increases their accuracy.  

# 2 D ILATED  C ONVOLUTIONS  

Let    $F:\mathbb{Z}^{2}\to\mathbb{R}$   discrete function. Let    $\Omega_{r}=[-r,r]^{2}\cap\mathbb{Z}^{2}$    and let    $k:\Omega_{r}\to\mathbb{R}$   be a discrete ﬁlter of size  $(2r+1)^{2}$  . The discrete convolution operator can be deﬁned as  

$$
(F*k)(\mathbf{p})=\sum_{\mathbf{s}+\mathbf{t}=\mathbf{p}}F(\mathbf{s})\,k(\mathbf{t}).
$$  

We now generalize this operator. Let    $l$   be a dilation factor and let  $^*l$   be deﬁned as  

$$
(F*_{l}k)(\mathbf{p})=\sum_{\mathbf{s}+l\mathbf{t}=\mathbf{p}}F(\mathbf{s})\,k(\mathbf{t}).
$$  

We w  refer to    $^*l$   as a dilated convolution or an    $l$  -dilated convolution. The familiar discrete convo- lution is simply the  1 -dilated convolution.  

The dilated convolution operator has been referred to in the past as “convolution with a dilated ﬁlter”. It plays a key role in the  algorithme \` a trous , an algorithm for wavelet decomposition (Holschneider et al., 1987; Shensa, 1992).   We use the term “dilated convolution” instead of “convolution with a dilated ﬁlter” to clarify that no “dilated ﬁlter” is constructed or represented. The convolution opera- tor itself is modiﬁed to use the ﬁlter parameters in a different way. The dilated convolution operator can apply the same ﬁlter at different ranges using different dilation factors. Our deﬁnition reﬂects the proper implementation of the dilated convolution operator, which does not involve construction of dilated ﬁlters.  

In recent work on convolutional networks for semantic segmentation, Long et al. (2015) analyzed ﬁlter dilation but chose not to use it. Chen et al. (2015a) used dilation to simplify the architec- ture of Long et al. (2015). In contrast, we develop a new convolutional network architecture that systematically uses dilated convolutions for multi-scale context aggregation.  

Our architecture is motivated by the fact that dilated convolutions support exponentially expanding receptive ﬁelds w coverage. Le  $F_{0},F_{1},.\,.\,.\,,F_{n-1}\,\colon\mathbb{Z}^{2}\to\bar{\mathbb{R}}$   be discrete functions and let  $k_{0},k_{1},.\,.\,.\,,k_{n-2}:\Omega_{1}\to\mathbb{R}$  −  →  be discrete  $3\times3$  ×  ﬁlters. Consider applying the ﬁlters with exponentially increasing dilation:  

$$
F_{i+1}=F_{i}*_{2^{i}}k_{i}\quad\mathrm{for}\,\,\,i=0,1,\mathrm{.}\,.\,.\,,n-2.
$$  

Deﬁne the receptive ﬁeld of an element    $\mathbf{p}$   in    $F_{i+1}$   as the set of elements in    $F_{0}$   that modify the value of    $F_{i+1}(\mathbf{p})$  . Let the size of the receptive ﬁeld of    $\mathbf{p}$   in    $F_{i+1}$   be the number of these elements. It is  

![](images/41d72619b4aa0abd88554fd13cb511b3e0a6346cecb4a4d416f411bfeb3c4229.jpg)  

Figure 1: Systematic dilation supports exponential expansion of the receptive ﬁeld without loss of resolution or coverage. (a)  $F_{1}$   is produced from    $F_{0}$   by a 1-dilated convolution; each element in    $F_{1}$  ha  receptive ﬁeld of  $3\times3$  .   $F_{2}$   is p uced from    $F_{1}$   by a dilated convolution; each element in  $F_{2}$   has  ceptive ﬁeld of  $7\times7$  × .   $F_{3}$  is produced from  $F_{2}$   by a 4-dilated convolution; each element in  $F_{3}$   has a receptive ﬁeld of  $15\!\times\!15$  × . The number of parameters associated with each layer is identical. The receptive ﬁeld grows exponentially while the number of parameters grows linearly.  

easy to see that the size of the receptive ﬁeld of each element in    $F_{i+1}$   is    $(2^{i+2}-1)\!\times\!(2^{i+2}-1)$  . The receptive ﬁeld is a square of exponentially increasing size. This is illustrated in Figure 1.  

# 3 M ULTI -S CALE  C ONTEXT  A GGREGATION  

The context module is designed to increase the performance of dense prediction architectures by aggregating multi-scale contextual information. The module takes    $C$   feature maps as input and produces  $C$   feature maps as output. The input and output have the same form, thus the module can be plugged into existing dense prediction architectures.  

We begin by describing a basic form of the context module. In this basic form, each layer has    $C$  channels. The representation in each layer is the same and could be used to directly obtain a dense per-class prediction, although the feature maps are not normalized and no loss is deﬁned inside the module. Intuitively, the module can increase the accuracy of the feature maps by passing them through multiple layers that expose contextual information.  

The basic context module has 7 layers that apply  $3\times3$   convolutions with different dilation factors. The dilations are 1, 1, 2, 4, 8, 16, and 1. Each convolution operates on all layers: strictly speaking, these are  $3\!\times\!3\!\times\!C$   convolutions with d the ﬁrst two dimensions.  hese convolutions is followed by a pointwise truncation  $\operatorname*{max}(\cdot,0)$  · . A ﬁnal layer performs  $1\!\times\!1\!\times\!C$  × ×  convolutions and produces the output of the module. The architecture is summarized in Table 1. Note that the front- end module that provides the input to the context network in our experiments produces feature maps at  $64\!\times\!64$   resolution. We therefore stop the exponential expansion of the receptive ﬁeld after layer 6.  

Our initial attempts to train the context module failed to yield an improvement in prediction accuracy. Experiments revealed that standard initialization procedures do not readily support the training of the module. Convolutional networks are commonly initialized using samples from random distributions (Glorot & Bengio, 2010; Krizhevsky et al., 2012; Simonyan   $\&$   Zisserman, 2015). However, we found that random initialization schemes were not effective for the context module. We found an alternative initialization with clear semantics to be much more effective:  

$$
k^{b}({\mathbf t},a)=1_{[{\mathbf t}=0]}1_{[a=b]},
$$  

where    $a$   is the index of the input feature map and  $b$   is the index of the output map. This is a form of identity initialization, which has recently been advocated for recurrent networks (Le et al., 2015). This initialization sets all ﬁlters such that each layer simply passes the input directly to the next. A natural concern is that this initialization could put the network in a mode where backpropagation cannot signiﬁcantly improve the default behavior of simply passing information through. However, experiments indicate that this is not the case. Backpropagation reliably harvests the contextual information provided by the network to increase the accuracy of the processed maps.  

![Table 1: Context network architecture. The network processes    $C$   feature maps by aggregating contextual information at progressively increasing scales without losing resolution. ](images/a530d6f2613ef9f14a1f9b647b59ce544bcd673e344be2ec66c1df22f8ad3c06.jpg)  

This completes the presentation of the basic context network. Our experiments show that even this basic module can increase dense prediction accuracy both quantitatively and qualitatively. This is particularly notable given the small number of parameters in the network:    $\approx{\bf\bar{6}}4C^{2}$    parameters in total.  

We have also trained a larger context network that uses a larger number of feature maps in the deeper layers. The number of maps in the large network is summarized in Table 1. We generalize the initialization scheme to account for the difference in the number of feature maps in different layers. Let  $c_{i}$   and    $c_{i+1}$   be the number of feature maps in two consecutive layers. Assume that    $C$  divides both    $c_{i}$   and    $c_{i+1}$  . The initialization is  

$$
k^{b}(\mathbf{t},a)=\left\{{\frac{C}{c_{i+1}}}\quad\mathbf{t}=0\ {\mathrm{and}}\ \left\lfloor{\frac{a C}{c_{i}}}\right\rfloor=\left\lfloor{\frac{b C}{c_{i+1}}}\right\rfloor\right.
$$  

Here    $\varepsilon\sim\mathcal{N}(0,\sigma^{2})$   and    $\sigma\ll C/c_{i+1}$  . The use of random noise breaks ties among feature maps with a common predecessor.  

# 4 F RONT  E ND  

We implemented and trained a front-end prediction module that takes a color image as input and produces    $C\,=\,21$   feature maps as output. The front-end module follows the work of Long et al.

 (2015) and Chen et al. (2015a), but was implemented separately. We adapted the VGG-16 network

 (Simonyan & Zisserman, 2015) for dense prediction and removed the last two pooling and striding layers. Speciﬁcally, each of these pooling and striding layers was removed and convolutions in all subsequent layers were dilated by a factor of 2 for each pooling layer that was ablated. Thus convolutions in the ﬁnal layers, which follow both ablated pooling layers, are dilated by a factor of 4. This enables initialization with the parameters of the original classiﬁcation network, but produces higher-resolution output. The front-end module takes padded images as input and produces feature maps at resolution    $64\times64$  . We use reﬂection padding: the buffer zone is ﬁlled by reﬂecting the image about each edge.  

Our front-end module is obtained by removing vestiges of the classiﬁcation network that are counter- productive for dense prediction. Most signiﬁcantly, we remove the last two pooling and striding layers entirely, whereas Long et al. kept them and Chen et al. replaced striding by dilation but kept the pooling layers. We found that simplifying the network by removing the pooling layers made it more accurate. We also remove the padding of the intermediate feature maps. Intermediate padding was used in the original classiﬁcation network, but is neither necessary nor justiﬁed in dense prediction.  

This simpliﬁed prediction module was trained on the Pascal VOC 2012 training set, augmented by the annotations created by Hariharan et al. (2011). We did not use images from the VOC-2012 validation set for training and therefore only used a subset of the annotations of Hariharan et al. (2011). Training was performed by stochastic gradient descent (SGD) with mini-batch size 14, learning rate  $10^{-3}$  , and momentum  0 . 9 . The network was trained for 60K iterations.  

We now compare the accuracy of our front-end module to the FCN-8s design of Long et al. (2015) and the DeepLab network of Chen et al. (2015a). For FCN-8s and DeepLab, we evaluate the public  

![](images/722e9ad429d2b11ed482eed3c92b844cf7668dc2982b8f2f09ca658350e9393e.jpg)  
Figure 2: Semantic segmentations produced by different adaptations of the VGG-16 classiﬁcation network. From left to right: (a) input image, (b) prediction by FCN-8s (Long et al., 2015), (c) prediction by DeepLab (Chen et al., 2015a), (d) prediction by our simpliﬁed front-end module, (e) ground truth.  

![](images/eb2eea8998a315c3d5c56d789fe53709960bbc09b72faf8d6592c38b76461989.jpg)  

Table 2: Our front-end prediction module is simpler and more accurate than prior models. This table reports accuracy on the VOC-2012 test set.  

models trained by the original authors on VOC-2012. Segmentations produced by the different models on images from the VOC-2012 dataset are shown in Figure 2. The accuracy of the models on the VOC-2012 test set is reported in Table 2.  

Our front-end prediction module is both simpler and more accurate than the prior models. Specif- ically, our simpliﬁed model outperforms both FCN-8s and the DeepLab network by more than 5 percentage points on the test set. Interestingly, our simpliﬁed front-end module outperforms the leaderboard accuracy of DeepLab  $^+$  CRF on the test set by more than a percentage point   $(67.6\%$  vs.  $66.4\%$  ) without using a CRF.  

# 5 E XPERIMENTS  

Our implementation is based on the Caffe library (Jia et al., 2014). Our implementation of dilated convolutions is now part of the stanfard Caffe distribution.  

For fair comparison with recent high-performing systems, we trained a front-end module that has the same structure as described in Section 4, but is trained on additional images from the Microsoft COCO dataset (Lin et al., 2014). We used all images in Microsoft COCO with at least one object from the VOC-2012 categories. Annotated objects from other categories were treated as background.  

Training was performed in two stages. In the ﬁrst stage, we trained on VOC-2012 images and Microsoft COCO images together. Training was performed by SGD with mini-batch size 14 and momentum 0.9. 100K iterations were performed with a learning rate of    $10^{-3}$    and 40K subsequent iterations were performed with a learning rate of    $10^{-4}$  . In the second stage, we ﬁne-tuned the network on VOC-2012 images only. Fine-tuning was performed for 50K iterations with a learning rate of    $10^{-5}$  . Images from the VOC-2012 validation set were not used for training.  

The front-end module trained by this procedure achieves    $69.8\%$   mean IoU on the VOC-2012 vali- dation set and    $71.3\%$   mean IoU on the test set. Note that this level of accuracy is achieved by the front-end alone, without the context module or structured prediction. We again attribute this high accuracy in part to the removal of vestigial components originally developed for image classiﬁcation rather than dense prediction.  

Controlled evaluation of context aggregation. We now perform controlled experiments to eval- uate the utility of the context network presented in Section 3. We begin by plugging each of the two context modules (Basic and Large) into the front end. Since the receptive ﬁeld of the context net- work is  $67\times67$  , we pad the input feature maps by a buffer of width 33. Zero padding and reﬂection padding yielded similar results in our experiments. The context module accepts feature maps from the front end as input and is given this input during training. Joint training of the context module and the front-end module did not yield a signiﬁcant improvement in our experiments. The learning rate was set to    $10^{-3}$  . Training was initialized as described in Section 3.  

Table 3 shows the effect of adding the context module to three different architectures for semantic segmentation. The ﬁrst architecture (top) is the front end described in Section 4. It performs seman- tic segmentation without structured prediction, akin to the original work of Long et al. (2015). The second architecture (Table 3, middle) uses the dense CRF to perform structured prediction, akin to the system of Chen et al. (2015a). We use the implementation of Kr¨ ahenb¨ uhl & Koltun (2011) and train the CRF parameters by grid search on the validation set. The third architecture (Table 3, bot- tom) uses the CRF-RNN for structured prediction (Zheng et al., 2015). We use the implementation of Zheng et al. (2015) and train the CRF-RNN in each condition.  

The experimental results demonstrate that the context module improves accuracy in each of the three conﬁgurations. The basic context module increases accuracy in each conﬁguration. The large context module increases accuracy by a larger margin. The experiments indicate that the context module and structured prediction are synergisic: the context module increases accuracy with or without subsequent structured prediction. Qualitative results are shown in Figure 3.  

Evaluation on the test set. We now perform an evaluation on the test set by submitting our re- sults to the Pascal VOC 2012 evaluation server. The results are reported in Table 4. We use the large context module for these experiments. As the results demonstrate, the context module yields a signiﬁcant boost in accuracy over the front end. The context module alone, without subsequent structured prediction, outperforms DeepLab-CRF-COCO-LargeFOV (Chen et al., 2015a). The con- text module with the dense CRF, using the original implementation of Kr¨ ahenb¨ uhl & Koltun (2011), performs on par with the very recent CRF-RNN (Zheng et al., 2015). The context module in com- bination with the CRF-RNN further increases accuracy over the performance of the CRF-RNN.  

# 6 C ONCLUSION  

We have examined convolutional network architectures for dense prediction. Since the model must produce high-resolution output, we believe that high-resolution operation throughout the network  

![](images/4a3e4b90bda70dff52933cfc9a1931c98d4ac83d71676b06385b15b2c8d3f74b.jpg)  
Figure 3: Semantic segmentations produced by different models. From left to right: (a) input image, (b) prediction by the front-end module, (c) prediction by the large context network plugged into the front end, (d) prediction by the front end  $^+$  context module  $^+$   CRF-RNN, (e) ground truth.  

![](images/4dc924f8cc150ff9703e0a36c60536f0b3aa87d75756baaba29917611c9b742e.jpg)  

Table 3: Controlled evaluation of the effect of the context module on the accuracy of three different architectures for semantic segmentation. Experiments performed on the VOC-2012 validation set. Validation images were not used for training. Top: adding the context module to a semantic segmen- tation front end with no structured prediction (Long et al., 2015). The basic context module increases accuracy, the large module increases it by a larger margin. Middle: the context module increases accuracy when plugged into a front-end  $^+$   dense CRF conﬁguration (Chen et al., 2015a). Bottom: the context module increases accuracy when plugged into a front-end  $^+$   CRF-RNN conﬁguration (Zheng et al., 2015).  

is both feasible and desirable. Our work shows that the dilated convolution operator is particularly suited to dense prediction due to its ability to expand the receptive ﬁeld without losing resolution or coverage. We have utilized dilated convolutions to design a new network structure that reliably increases accuracy when plugged into existing semantic segmentation systems. As part of this work, we have also shown that the accuracy of existing convolutional networks for semantic segmentation can be increased by removing vestigial components that had been developed for image classiﬁcation.  

![](images/5cd270f7ec0ac49ce6a8b0a15f543913dcc111191170376bebe5e3167034f077.jpg)  
Table 4: Evaluation on the VOC-2012 test set. ‘DeepLab  $^{++}$  ’ stands for DeepLab-CRF-COCO- LargeFOV and ‘DeepLab-  $\mathrm{\bfNsc++}^{\prime}$   stands for DeepLab-MSc-CRF-LargeFOV-COCO-CrossJoint (Chen et al., 2015a). ‘CRF-RNN’ is the system of Zheng et al. (2015). ‘Context’ refers to the large context module plugged into our front end. The context network yields very high accuracy, ourperforming the DeepLab  $^{++}$   architecture without performing structured prediction. Combining the context network with the CRF-RNN structured prediction module increases the accuracy of the CRF-RNN system.  

We believe that the presented work is a step towards dedicated architectures for dense prediction that are not constrained by image classiﬁcation precursors. As new sources of data become available, future architectures may be trained densely end-to-end, removing the need for pre-training on image classiﬁcation datasets. This may enable architectural simpliﬁcation and uniﬁcation. Speciﬁcally, end-to-end dense training may enable a fully dense architecture akin to the presented context net- work to operate at full resolution throughout, accepting the raw image as input and producing dense label assignments at full resolution as output.  

State-of-the-art systems for semantic segmentation leave signiﬁcant room for future advances. Fail- ure cases of our most accurate conﬁguration are shown in Figure 4. We will release our code and trained models to support progress in this area.  

# A CKNOWLEDGEMENTS  

We thank Vibhav Vineet for proofreading, help with experiments, and related discussions. We are also grateful to Jonathan Long and the Caffe team for their feedback and for rapidly pulling our implementation into the Caffe library.  

# R EFERENCES  

Badrinarayanan, Vijay, Handa, Ankur, and Cipolla, Roberto. SegNet: A deep convolutional encoder-decoder architecture for robust semantic pixel-wise labelling.  arXiv:1505.07293 , 2015.  

Brostow, Gabriel J., Fauqueur, Julien, and Cipolla, Roberto. Semantic object classes in video: A high-deﬁnition ground truth database.  Pattern Recognition Letters , 30(2), 2009.  

Chen, Liang-Chieh, Papandreou, George, Kokkinos, Iasonas, Murphy, Kevin, and Yuille, Alan L. Semantic image segmentation with deep convolutional nets and fully connected CRFs. In  ICLR , 2015a.  

![](images/5fb8aa78312c23ae19c8cce0003c10f078b3560fcba70e93e89f1bac04057209.jpg)  
Figure 4: Failure cases from the VOC-2012 validation set. The most accurate architecture we trained (Context  $^+$   CRF-RNN) performs poorly on these images.  

Chen, Liang-Chieh, Yang, Yi, Wang, Jiang, Xu, Wei, and Yuille, Alan L. Attention to scale: Scale-aware semantic image segmentation.  arXiv:1511.03339 , 2015b. Cordts, Marius, Omran, Mohamed, Ramos, Sebastian, Rehfeld, Timo, Enzweiler, Markus, Benenson, Rodrigo, Franke, Uwe, Roth, Stefan, and Schiele, Bernt. The Cityscapes dataset for semantic urban scene understand- ing. In  CVPR , 2016. Everingham, Mark, Gool, Luc J. Van, Williams, Christopher K. I., Winn, John M., and Zisserman, Andrew. The Pascal visual object classes (VOC) challenge.  IJCV , 88(2), 2010. Farabet, Cl´ ement, Couprie, Camille, Najman, Laurent, and LeCun, Yann. Learning hierarchical features for scene labeling.  PAMI , 35(8), 2013. Fischer, Philipp, Dosovitskiy, Alexey, Ilg, Eddy, H¨ ausser, Philip, Hazrba, Caner, Golkov, Vladimir, van der Smagt, Patrick, Cremers, Daniel, and Brox, Thomas. Learning optical ﬂow with convolutional neural net- works. In  ICCV , 2015. Galleguillos, Carolina and Belongie, Serge J. Context based object categorization: A critical survey.  Computer Vision and Image Understanding , 114(6), 2010. Geiger, Andreas, Lenz, Philip, Stiller, Christoph, and Urtasun, Raquel. Vision meets robotics: The KITTI dataset.  International Journal of Robotics Research , 32(11), 2013. Glorot, Xavier and Bengio, Yoshua. Understanding the difﬁculty of training deep feedforward neural networks. In  AISTATS , 2010. Hariharan, Bharath, Arbelaez, Pablo, Bourdev, Lubomir D., Maji, Subhransu, and Malik, Jitendra. Semantic contours from inverse detectors. In  ICCV , 2011. He, Xuming, Zemel, Richard S., and Carreira-Perpi˜ n´ an, Miguel  A. Multiscale conditional random ﬁelds for image labeling. In  CVPR , 2004. Holschneider, M., Kronland-Martinet, R., Morlet, J., and Tchamitchian, Ph. A real-time algorithm for signal analysis with the help of the wavelet transform. In  Wavelets: Time-Frequency Methods and Phase Space. Proceedings of the International Conference , 1987. Jia, Yangqing, Shelhamer, Evan, Donahue, Jeff, Karayev, Sergey, Long, Jonathan, Girshick, Ross B., Guadar- rama, Sergio, and Darrell, Trevor. Caffe: Convolutional architecture for fast feature embedding. In  Proc. ACM Multimedia , 2014. Kohli, Pushmeet, Ladicky, Lubor, and Torr, Philip H. S. Robust higher order potentials for enforcing label consistency.  IJCV , 82(3), 2009. Kr¨ ahenb¨ uhl, Philipp and Koltun, Vladlen. Efﬁcient inference in fully connected CRFs with Gaussian edge potentials. In  NIPS , 2011. Krizhevsky, Alex, Sutskever, Ilya, and Hinton, Geoffrey E. ImageNet classiﬁcation with deep convolutional neural networks. In  NIPS , 2012. Kundu, Abhijit, Vineet, Vibhav, and Koltun, Vladlen. Feature space optimization for semantic video segmen- tation. In  CVPR , 2016. Ladicky, Lubor, Russell, Christopher, Kohli, Pushmeet, and Torr, Philip H. S. Associative hierarchical CRFs for object class image segmentation. In  ICCV , 2009. Le, Quoc V., Jaitly, Navdeep, and Hinton, Geoffrey E. A simple way to initialize recurrent networks of rectiﬁed linear units.  arXiv:1504.00941 , 2015. LeCun, Yann, Boser, Bernhard, Denker, John S., Henderson, Donnie, Howard, Richard E., Hubbard, Wayne, and Jackel, Lawrence D. Backpropagation applied to handwritten zip code recognition.  Neural Computation , 1(4), 1989. Lin, Guosheng, Shen, Chunhua, Reid, Ian, and van dan Hengel, Anton. Efﬁcient piecewise training of deep structured models for semantic segmentation.  arXiv:1504.01013 , 2015. Lin, Tsung-Yi, Maire, Michael, Belongie, Serge, Hays, James, Perona, Pietro, Ramanan, Deva, Doll´ ar, Piotr, and Zitnick, C. Lawrence. Microsoft COCO: Common objects in context. In  ECCV , 2014. Liu, Buyu and He, Xuming. Multiclass semantic video segmentation with object-level active inference. In CVPR , 2015. Long, Jonathan, Shelhamer, Evan, and Darrell, Trevor. Fully convolutional networks for semantic segmenta- tion. In  CVPR , 2015.  

Noh, Hyeonwoo, Hong, Seunghoon, and Han, Bohyung. Learning deconvolution network for semantic seg- mentation. In  ICCV , 2015. Ros, Germ´ an, Ramos, Sebastian, Granados, Manuel, Bakhtiary, Amir, V´ azquez, David, and L´ opez, Anto- nio Manuel. Vision-based ofﬂine-online perception paradigm for autonomous driving. In  WACV , 2015. Rumelhart, David E., Hinton, Geoffrey E., and Williams, Ronald J. Learning representations by back- propagating errors.  Nature , 323, 1986. Shensa, Mark J. The discrete wavelet transform: wedding the \` a trous and Mallat algorithms.  IEEE Transactions on Signal Processing , 40(10), 1992. Shotton, Jamie, Winn, John M., Rother, Carsten, and Criminisi, Antonio. TextonBoost for image understanding: Multi-class object recognition and segmentation by jointly modeling texture, layout, and context.  IJCV , 81 (1), 2009. Simonyan, Karen and Zisserman, Andrew. Very deep convolutional networks for large-scale image recognition. In  ICLR , 2015. Sturgess, Paul, Alahari, Karteek, Ladicky, Lubor, and Torr, Philip H. S. Combining appearance and structure from motion features for road scene understanding. In  BMVC , 2009. Tighe, Joseph and Lazebnik, Svetlana. Superparsing – scalable nonparametric image parsing with superpixels. IJCV , 101(2), 2013. Zheng, Shuai, Jayasumana, Sadeep, Romera-Paredes, Bernardino, Vineet, Vibhav, Su, Zhizhong, Du, Dalong, Huang, Chang, and Torr, Philip. Conditional random ﬁelds as recurrent neural networks. In  ICCV , 2015.  

# A PPENDIX  A U RBAN  S CENE  U NDERSTANDING  

In this appendix, we report experiments on three datasets for urban scene understanding: the CamVid dataset (Brostow et al., 2009), the KITTI dataset (Geiger et al., 2013), and the new Cityscapes dataset (Cordts et al., 2016). As the accuracy measure we use the mean IoU (Everingham et al., 2010). We only train our model on the training set, even when a validation set is available. The results reported in this section do not use conditional random ﬁelds or other forms of structured prediction. They were obtained with convolutional networks that combine a front-end module and a context module, akin to the “Front  $^+$   Basic” network evaluated in Table 3. The trained models can be found at  https://github.com/fyu/dilation .  

We now summarize the training procedure used for training the front-end module. This procedure applies to all datasets. Training is performed with stochastic gradient descent. Each mini-batch contains 8 crops from randomly sampled images. Each crop is of size  $628\times628$   and is randomly sampled from a padded image. Images are padded using reﬂection padding. No padding is used in the intermediate layers. The learning rate is    $10^{-4}$    and momentum is set to  0 . 99 . The number of iterations depends on the number of images in the dataset and is reported for each dataset below.  

The context modules used for these datasets are all derived from the “Basic” network, using the terminology of Table 1. The number of channels in each layer is the number of predicted classes  $C$  . (For example,    $C=19$   for the Cityscapes dataset.) Each layer in the context module is padded such that the input and response maps have the same size. The number of layers in the context module depends on the resolution of the images in the dataset. Joint training of the complete model, composed of the front-end and the context module, is summarized below for each dataset.  

# A.1 C AM V ID  

We use the split of Sturgess et al. (2009), which partitions the dataset into 367 training images, 100 validation images, and 233 test images. 11 semantic classes are used. The images are downsampled to    $640\!\times\!480$  .  

The context module has 8 layers, akin to the model used for the Pascal VOC dataset in the main body of the paper. The overall training procedure is as follows. First, the front-end module is trained for 20K iterations. Then the complete model (front-end  $^+$   context) is jointly trained by sampling crops of size    $852\times852$   with batch size 1. The learning rate for joint training is set to    $10^{\bar{-}5}$    and the momentum is set to  0 . 9 .  

Results on the CamVid test set are reported in Table 5. We refer to our complete convolutional network (front-end  $^+$   context) as Dilation8, since the context module has 8 layers. Our model out- performs the prior work. This model was used as the unary classiﬁer in the recent work of Kundu et al. (2016).  

![](images/204748a604b9298cf406e52e862031913fef71d7c4ab02377904a71b55b7d239.jpg)  

Table 5: Semantic segmentation results on the CamVid dataset. Our model (Dilation8) is com- pared to ALE (Ladicky et al., 2009), SuperParsing (Tighe & Lazebnik, 2013), Liu and He (Liu & He, 2015), SegNet (Badrinarayanan et al., 2015), and the DeepLab-LargeFOV model (Chen et al., 2015a). Our model outperforms the prior work.  

# A.2 KITTI  

We use the training and validation split of Ros et al. (2015): 100 training images and 46 test images. The images were all collected from the KITTI visual odometry/SLAM dataset. The image resolution is    $1226\!\times\!370$  . Since the vertical resolution is small compared to the other datasets, we rem ve Layer 6 in Table 1. The resulting context module has 7 layers. The complete network (front-end + context) is referred to as Dilation7.  

The front-end is trained for 10K iterations. Next, the front-end and the context module are trained jointly. For joint training, the crop size is    $900\times900$   and momentum is set to 0.99, while the other parameters are the same as the ones used for the CamVid dataset. Joint training is performed for 20K iterations.  

![The results are shown in Table 6. As the table demonstrates, our model outperforms the prior work. ](images/5971c0c43dd887e0b97300f31a2b3083ccb982caa20f041cd4fd5f699612a232.jpg)  

Table 6: Semantic segmentation results on the KITTI dataset. We compare our results to Ros et al. (2015) and to the DeepLab-LargeFOV model (Chen et al., 2015a). Our network (Dilation7) yields higher accuracy than the prior work.  

# A.3 C ITYSCAPES  

The Cityscapes dataset contains 2975 training images, 500 validation images, and 1525 test im- ages (Cordts et al., 2016). Due to the high image resolution   $(2048\!\times\!1024)$  ), we add two layers to the context network after Layer 6 in Table 1. These two layers have dilation 32 and 64, respectively. The total number of layers in the context module is 10 and we refer to the complete model (front-end  $^+$  context) as Dilation10.  

The Dilation10 network was trained in three stages. First, the front-end prediction module was trained for 40K iterations. Second, the context module was trained for 24K iterations on whole (uncropped) images, with learning rate    $10^{-4}$  , momentum  0 . 99 , and batch size 100. Third, the complete model (front-end  $^+$   context) was jointly trained for 60K iterations on halves of images (input size    $1396\times1396$  , including padding), with learning rate    $10^{-5}$  , momentum  0 . 99 , and batch size 1.  

Figure 5 visualizes the effect of the training stages on the performance of the model. Quantitative results are given in Tables 7 and 8.  

The performance of Dilation10 was compared to prior work on the Cityscapes dataset by Cordts et al. (2016). In their evaluation, Dilation10 outperformed all prior models (Cordts et al., 2016). Dilation10 was also used as the unary classiﬁer in the recent work of Kundu et al. (2016), which used structured prediction to increase accuracy further.  

![](images/e0ee4e9e1dc943c9407c54e4cff424a7531210eb3228324c6256d0d730574ced.jpg)  
Figure 5: Results produced by the Dilation10 model after different training stages. (a) Input image. (b) Ground truth segmentation. (c) Segmentation produced by the model after the ﬁrst stage of training (front-end only). (d) Segmentation produced after the second stage, which trains the context module. (e) Segmentation produced after the third stage, in which both modules are trained jointly.  

![](images/e6063c18dbcf902989e40e3d69a4b52c6043f80e192f6fa604898f24f0ce4a27.jpg)  

Table 7: Per-class and mean class-level IoU achieved by our model (Dilation10) on the Cityscapes dataset.  

![Table 8: Per-category and mean category-level IoU on the Cityscapes dataset. ](images/0535713607ca3196800873942a97b2b5bdc2db1adc4ba45ac63ce4b67dd019b4.jpg)  