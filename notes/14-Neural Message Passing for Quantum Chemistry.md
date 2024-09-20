# Neural Message Passing for Quantum Chemistry  

# Justin Gilmer   1   Samuel S. Schoenholz   1   Patrick F. Riley   2   Oriol Vinyals   3   George E. Dahl   1  

# Abstract  

Supervised learning on molecules has incredi- ble potential to be useful in chemistry, drug dis- covery, and materials science. Luckily, sev- eral promising and closely related neural network models invariant to molecular symmetries have already been described in the literature. These models learn a message passing algorithm and aggregation procedure to compute a function of their entire input graph. At this point, the next step is to ﬁnd a particularly effective variant of this general approach and apply it to chemical prediction benchmarks until we either solve them or reach the limits of the approach. In this pa- per, we reformulate existing models into a sin- gle common framework we call Message Pass- ing Neural Networks (MPNNs) and explore ad- ditional novel variations within this framework. Using MPNNs we demonstrate state of the art re- sults on an important molecular property predic- tion benchmark; these results are strong enough that we believe future work should focus on datasets with larger molecules or more accurate ground truth labels.  

# 1. Introduction  

The past decade has seen remarkable success in the use of deep neural networks to understand and translate nat- ural language ( Wu et al. ,  2016 ), generate and decode com- plex audio signals ( Hinton et al. ,  2012 ), and infer fea- tures from real-world images and videos ( Krizhevsky et al. , 2012 ). Although chemists have applied machine learn- ing to many problems over the years, predicting the prop- erties of molecules and materials using machine learning (and especially deep learning) is still in its infancy. To date, most research applying machine learning to chemistry tasks ( Hansen et al. ,  2015 ;  Huang & von Lilienfeld ,  2016 ;  

![](images/089913abdd1085d3050ab8108f01e139588039e5295285a21a21482df9ff5d56.jpg)  
Figure 1.  A Message Passing Neural Network predicts quantum properties of an organic molecule by modeling a computationally expensive DFT calculation.  

Rupp et al. ,  2012 ;  Rogers & Hahn ,  2010 ;  Montavon et al. , 2012 ;  Behler & Parrinello ,  2007 ;  Schoenholz et al. ,  2016 ) has revolved around feature engineering. While neural net- works have been applied in a variety of situations ( Merk- wirth & Lengauer ,  2005 ;  Micheli ,  2009 ;  Lusci et al. ,  2013 ; Duvenaud et al. ,  2015 ), they have yet to become widely adopted. This situation is reminiscent of the state of image models before the broad adoption of convolutional neural networks and is due, in part, to a dearth of empirical evi- dence that neural architectures with the appropriate induc- tive bias can be successful in this domain.  

Recently, large scale quantum chemistry calculation and molecular dynamics simulations coupled with advances in high throughput experiments have begun to generate data at an unprecedented rate. Most classical techniques do not make effective use of the larger amounts of data that are now available. The time is ripe to apply more power- ful and ﬂexible machine learning methods to these prob- lems, assuming we can ﬁnd models with suitable inductive biases. The symmetries of atomic systems suggest neu- ral networks that operate on graph structured data and are invariant to graph isomorphism might also be appropriate for molecules. Sufﬁciently successful models could some- day help automate challenging chemical search problems in drug discovery or materials science.  

In this paper, our goal is to demonstrate effective ma- chine learning models for chemical prediction problems that are capable of learning their own features from molec- ular graphs directly and are invariant to graph isomorphism. To that end, we describe a general framework for super- vised learning on graphs called Message Passing Neural Networks (MPNNs) that simply abstracts the commonali- ties between several of the most promising existing neural models for graph structured data, in order to make it easier to understand the relationships between them and come up with novel variations. Given how many researchers have published models that ﬁt into the MPNN framework, we believe that the community should push this general ap- proach as far as possible on practically important graph problems and only suggest new variations that are well motivated by applications, such as the application we con- sider here: predicting the quantum mechanical properties of small organic molecules (see task schematic in ﬁgure  1 ).  

In general, the search for practically effective machine learning (ML) models in a given domain proceeds through a sequence of increasingly realistic and interesting bench- marks. Here we focus on the QM9 dataset as such a bench- mark ( Ramakrishnan et al. ,  2014 ). QM9 consists of   $130\mathbf{k}$  molecules with 13 properties for each molecule which are approximated by an expensive 1   quantum mechanical simu- lation method (DFT), to yield 13 corresponding regression tasks. These tasks are plausibly representative of many im- portant chemical prediction problems and are (currently) difﬁcult for many existing methods. Additionally, QM9 also includes complete spatial information for the single low energy conformation of the atoms in the molecule that was used in calculating the chemical properties. QM9 therefore lets us consider both the setting where the com- plete molecular geometry is known (atomic distances, bond angles, etc.) and the setting where we need to compute properties that might still be  deﬁned  in terms of the spa- tial positions of atoms, but where only the atom and bond information (i.e. graph) is available as input. In the lat- ter case, the model must implicitly ﬁt something about the computation used to determine a low energy 3D conforma- tion and hopefully would still work on problems where it is not clear how to compute a reasonable 3D conformation.  

When measuring the performance of our models on QM9, there are two important benchmark error levels. The ﬁrst is the estimated average error of the DFT approximation to nature, which we refer to as “DFT error.” The sec- ond, known as “chemical accuracy,” is a target error that has been established by the chemistry community. Esti- mates of DFT error and chemical accuracy are provided for each of the 13 targets in  Faber et al.  ( 2017 ). One im- portant goal of this line of research is to produce a model which can achieve chemical accuracy with respect to the true  targets as measured by an extremely precise experi- ment. The dataset containing the true targets on all   $134\mathrm{k}$  molecules does not currently exist. However, the ability to ﬁt the DFT approximation to within chemical accuracy would be an encouraging step in this direction. For all 13 targets, achieving chemical accuracy is at least as hard as achieving DFT error. In the rest of this paper when we talk about chemical accuracy we generally mean with respect to our available ground truth labels.  

In this paper, by exploring novel variations of models in the MPNN family, we are able to both achieve a new state of the art on the QM9 dataset and to predict the DFT calcula- tion to within chemical accuracy on all but two targets. In particular, we provide the following key contributions:  

•  We develop an MPNN which achieves state of the art results on all 13 targets and predicts DFT to within chemical accuracy on 11 out of 13 targets. •  We develop several different MPNNs which predict DFT to within chemical accuracy on 5 out of 13 tar- gets while operating on the topology of the molecule alone (with no spatial information as input). •  We develop a general method to train MPNNs with larger node representations without a corresponding increase in computation time or memory, yielding a substantial savings over previous MPNNs for high di- mensional node representations.  

We believe our work is an important step towards making well-designed MPNNs the default for supervised learning on modestly sized molecules. In order for this to hap- pen, researchers need to perform careful empirical stud- ies to ﬁnd the proper way to use these types of models and to make any necessary improvements to them, it is not sufﬁcient for these models to have been described in the literature if there is only limited accompanying empir- ical work in the chemical domain. Indeed convolutional neural networks existed for decades before careful empiri- cal work applying them to image classiﬁcation ( Krizhevsky et al. ,  2012 ) helped them displace SVMs on top of hand- engineered features for a host of computer vision problems.  

# 2. Message Passing Neural Networks  

There are at least eight notable examples of models from the literature that we can describe using our Message Pass- ing Neural Networks (MPNN) framework. For simplicity we describe MPNNs which operate on undirected graphs  $G$   with node features  $x_{v}$   and edge features    $e_{v w}$  . It is triv- ial to extend the formalism to directed multigraphs. The forward pass has two phases, a message passing phase and a readout phase. The message passing phase runs for    $T$  time steps and is deﬁned in terms of message functions  $M_{t}$  and vertex update functions    $U_{t}$  . During the message pass- ing phase, hidden states  $h_{v}^{t}$    at each node in the graph are updated based on messages    $m_{v}^{t+1}$  according to  

$$
\begin{array}{r l}&{m_{v}^{t+1}=\displaystyle\sum_{w\in N(v)}M_{t}(h_{v}^{t},h_{w}^{t},e_{v w})}\\ &{h_{v}^{t+1}=U_{t}(h_{v}^{t},m_{v}^{t+1})}\end{array}
$$  

where in the sum,  $N(v)$   denotes the neighbors of    $v$   in graph  $G$  . The readout phase computes a feature vector for the whole graph using some readout function    $R$   according to  

$$
\hat{y}=R(\{h_{v}^{T}\mid v\in G\}).
$$  

The message functions    $M_{t}$  , vertex update functions  $U_{t}$  , and readout function    $R$   are all learned differentiable functions.  $R$   operates on the set of node states and must be invariant to permutations of the node states in order for the MPNN to be invariant to graph isomorphism. In what follows, we deﬁne previous models in the literature by specifying the message function  $M_{t}$  , vertex update function  $U_{t}$  , and readout func- tion    $R$   used. Note one could also learn edge features in an MPNN by introducing hidden states for all edges in the graph  $h_{e_{v w}}^{t}$    and updating them analogously to equations  1 and  2 . Of the existing MPNNs, only  Kearnes et al.  ( 2016 ) has used this idea.  

# Convolutional Networks for Learning Molecular Fin- gerprints,  Duvenaud et al.  ( 2015 )  

The message function used is  $\begin{array}{r l}{M(h_{v},h_{w},e_{v w})}&{{}=}\end{array}$   $(h_{w},e_{v w})$   where    $(.,.)$   denotes concatenation. The vertex update function used is    $U_{t}\big(h_{v}^{t},m_{v}^{t+1}\big)=\sigma\big(H_{t}^{\deg(v)}m_{v}^{t+1}\big)$  , where    $\sigma$   is the sigmoid function,   $\deg(v)$   is the degree of vertex    $v$   and    $H_{t}^{N}$  is a learned matrix for each time step  $t$  and vertex degree  $N,R$   has skip connections to all previous hidden states    $h_{v}^{t}$    and is equal to  $f\left(\sum_{v,t}\mathrm{softmax}(W_{t}h_{v}^{t})\right).$  , where    $f$   is a neural network and    $\dot{W_{t}}$   are learned readout matrices, one for each time step    $t$  . This message pass- ing scheme may be problematic since the resulting mes- sage vector is    $\begin{array}{r}{m_{v}^{t+1}=\left(\sum h_{w}^{t},\sum e_{v w}\right)}\end{array}$  P  P  ,  which separately sums over connected nodes and connected edges. It fol- lows that the message passing implemented in  Duvenaud et al.  ( 2015 ) is unable to identify correlations between edge states and node states.  

# Gated Graph Neural Networks (GG-NN),  Li et al. ( 2016 )  

The message function used is    $M_{t}\big(h_{v}^{t},h_{w}^{t},e_{v w}\big)=A_{e_{v w}}h_{w}^{t}$  , where    $A_{e_{v w}}$   is a learned matrix, one for each edge label  $e$  (the model assumes discrete edge types). The update func- tion is    $U_{t}\ =\ \mathrm{{GRU}}(h_{v}^{t},m_{v}^{t+1})$  , where GRU is the Gated Recurrent Unit introduced in  Cho et al.  ( 2014 ). This work used weight tying, so the same update function is used at each time step  $t$  . Finally,  

$$
R=\sum_{v\in V}\sigma\left(i(h_{v}^{(T)},h_{v}^{0})\right)\odot\left(j(h_{v}^{(T)})\right)
$$  

where    $i$   and  $j$   are neural networks, and  $\odot$  denotes element- wise multiplication.  

# Interaction Networks,  Battaglia et al.  ( 2016 )  

This work considered both the case where there is a tar- get at each node in the graph, and where there is a graph level target. It also considered the case where there are node level effects applied at each time step, in such a case the update function takes as input the concatenation  $(h_{v},x_{v},m_{v})$   where    $x_{v}$   is an external vector representing some outside inﬂuence on the vertex  $v$  . The message func- tion    $M(h_{v},h_{w},e_{v w})$   is a neural network which takes the concatenation    $(h_{v},h_{w},e_{v w})$  . The vertex update function  $U(h_{v},x_{v},m_{v})$   is a neural network which takes as input the concatenation    $(h_{v},x_{v},m_{v})$  . Finally, in the case where there   graph level output,    $R=f(\sum_{v\in G}h_{v}^{T})$   where    $f$   is ∈ a neural network which takes the sum of the ﬁnal hidden states  $h_{v}^{T}$    . Note the original work only deﬁned the model for  $T=1$  .  

# Molecular Graph Convolutions, Kearnes et al. (2016)  

This work deviates slightly from other MPNNs in that it introduces edge representations  $e_{v w}^{t}$  which are updated during the message passing phase. The message function used for node messages is  $M(h_{v}^{t},h_{w}^{t},e_{v w}^{t})\ \ =\ \ e_{v w}^{t}$  . The vertex update function is    $\begin{array}{r l r}{U_{t}(h_{v}^{t},m_{v}^{t+1})}&{{}=}&{\alpha(W_{1}(\alpha(W_{0}h_{v}^{t}),m_{v}^{t+1}))}\end{array}$   where  $(.,.)$   denotes concatenation,    $\alpha$   is the ReLU activation and    $W_{1},W_{0}$   are learned weight matrices. The edge state update is deﬁned by    $e_{v w}^{t+1}\ =\ U_{t}^{\prime}(e_{v w}^{t},h_{v}^{t},h_{w}^{t})\ =$   $\alpha(W_{4}(\alpha(W_{2},e_{v w}^{t}),\alpha(W_{3}(h_{v}^{t},h_{w}^{t}))))$   where the    $W_{i}$    are also learned weight matrices.  

# Deep Tensor Neural Networks,  Sch¨ utt et al.  ( 2017 )  

The message from  $w$   to  $v$   is computed by  

$$
M_{t}=\operatorname{tanh}\left(W^{f c}((W^{c f}h_{w}^{t}+b_{1})\odot(W^{d f}e_{v w}+b_{2}))\right)
$$  

where    $W^{f c},\,W^{c f},\,W^{d f}$   are matrices and    $b_{1},\ b_{2}$   are bias vectors. The update function used is    $U_{t}(h_{v}^{t},m_{v}^{t+1})\ =$   $h_{v}^{t}+m_{v}^{t+1}$  . The readout function passes each node inde- pendently through a single hidden layer neural network and sums the outputs, in particular  

$$
R=\sum_{v}\mathbf{NN}(h_{v}^{T}).
$$  

Laplacian Based Methods,  Bruna et al.  ( 2013 );  Deffer- rard et al.  ( 2016 );  Kipf & Welling  ( 2016 )  

These methods generalize the notion of the convolution op- eration typically applied to image datasets to an operation that operates on an arbitrary graph  $G$   with a real valued ad- jacency matrix    $A$  . The operations deﬁned in  Bruna et al. ( 2013 );  Defferrard et al.  ( 2016 ) result in message functions of the form    ${\cal M}_{t}(h_{v}^{t},h_{w}^{t})\ =\ C_{v w}^{t}h_{w}^{t}$  , where the matrices  $C_{v w}^{t}$    are parameterized by the eigenvectors of the graph laplacian    $L$  , and the learned parameters of the model. The vertex update function used is    $U_{t}(h_{v}^{t},m_{v}^{t+1})\,=\,\sigma(m_{v}^{t+1})$  where    $\sigma$   is some pointwise non-linearity (such as ReLU).  

The  Kipf & Welling  ( 2016 ) model results in a mes- sage function    $\begin{array}{c c c}{{{\cal M}_{t}(h_{v}^{t},h_{w}^{t})}}&{{=}}&{{c_{v w}h_{w}^{t}}}\end{array}$    where    $\begin{array}{r l}{c_{v w}}&{{}=}\end{array}$   $\left(\mathbf{deg}(v)\mathbf{deg}(w)\right)^{-1/2}A_{v w}$  . The vertex update function is  $U_{v}^{t}(h_{v}^{t},m_{v}^{t+1})\,=\,\mathrm{ReLU}(W^{t}m_{v}^{t+1})$  ) . For the exact expres- sions for the  $C_{v w}^{t}$    and the derivation of the reformulation of these models as MPNNs, see the supplementary material.  

# 2.1. Moving Forward  

Given how many instances of MPNNs have appeared in the literature, we should focus on pushing this general fam- ily as far as possible in a speciﬁc application of substan- tial practical importance. This way we can determine the most crucial implementation details and potentially reach the limits of these models to guide us towards future mod- eling improvements.  

One downside of all of these approaches is computation time. Recent work has adapted the GG-NN architecture to larger graphs by passing messages on only subsets of the graph at each time step ( Marino et al. ,  2016 ). In this work we also present a MPNN modiﬁcation that can improve the computational costs.  

# 3. Related Work  

Although in principle quantum mechanics lets us compute the properties of molecules, the laws of physics lead to equations that are far too difﬁcult to solve exactly. There- fore scientists have developed a hierarchy of approxima- tions to quantum mechanics with varying tradeoffs of speed and accuracy, such as Density Functional Theory (DFT) with a variety of functionals ( Becke ,  1993 ;  Hohenberg & Kohn ,  1964 ), the GW approximation ( Hedin ,  1965 ), and Quantum Monte-Carlo ( Ceperley & Alder ,  1986 ). Despite being widely used, DFT is simultaneously still too slow to be applied to large systems (scaling as  $\mathcal{O}(N_{e}^{3})$   where    $N_{e}$    is the number of electrons) and exhibits systematic as well as random errors relative to exact solutions to Schr¨ odinger’s equation. For example, to run the DFT calculation on a sin- gle 9 heavy atom molecule in QM9 takes around an hour on a single core of a Xeon E5-2660   $_{2.2\,\mathrm{GHz}})$   using a ver- sion of Gaussian G09 (ES64L-G09RevD.01) ( Bing et al. , 2017 ). For a 17 heavy atom molecule, computation time is up to 8 hours. Empirical potentials have been developed, such as the Stillinger-Weber potential ( Stillinger & Weber , 1985 ), that are fast and accurate but must be created from scratch, from ﬁrst principles, for every new composition of atoms.  

Hu et al.  ( 2003 ) used neural networks to approximate a par- ticularly troublesome term in DFT called the exchange cor- relation potential to improve the accuracy of DFT. How- ever, their method fails to improve upon the efﬁciency of DFT and relies on a large set of  ad hoc  atomic descriptors.  

Two more recent approaches by  Behler & Parrinello  ( 2007 ) and  Rupp et al.  ( 2012 ) attempt to approximate solutions to quantum mechanics directly without appealing to DFT. In the ﬁrst case single-hidden-layer neural networks were used to approximate the energy and forces for conﬁgura- tions of a Silicon melt with the goal of speeding up molec- ular dynamics simulations. The second paper used Ker- nel Ridge Regression (KRR) to infer atomization energies over a wide range of molecules. In both cases hand en- gineered features were used (symmetry functions and the Coulomb matrix, respectively) that built physical symme- tries into the input representation. Subsequent papers have replaced KRR by a neural network.  

Both of these lines of research used hand engineered fea- tures that have intrinsic limitations. The work of  Behler & Parrinello  ( 2007 ) used a representation that was manifestly invariant to graph isomorphism, but has difﬁculty when ap- plied to systems with more than three species of atoms and fails to generalize to novel compositions. The represen- tation used in  Rupp et al.  ( 2012 ) is not invariant to graph isomorphism. Instead, this invariance must be learned by the downstream model through dataset augmentation.  

In addition to the eight MPNNs discussed in Section  2  there have been a number of other approaches to machine learn- ing on graphical data which take advantage of the symme- tries in a number of ways. One such family of approaches deﬁne a preprocessing step which constructs a canonical graph representation which can then be fed into into a stan- dard classiﬁer. Examples in this family include  Niepert et al.  ( 2016 ) and  Rupp et al.  ( 2012 ). Finally  Scarselli et al. ( 2009 ) deﬁne a message passing process on graphs which is run until convergence, instead of for a ﬁnite number of time steps as in MPNNs.  

# 4. QM9 Dataset  

To investigate the success of MPNNs on predicting chem- ical properties, we use the publicly available QM9 dataset ( Ramakrishnan et al. ,  2014 ). Molecules in the dataset con- sist of Hydrogen (H), Carbon (C), Oxygen (O), Nitrogen (N), and Flourine (F) atoms and contain up to 9 heavy (non Hydrogen) atoms. In all, this results in about 134k drug- like organic molecules that span a wide range of chemistry. For each molecule DFT is used to ﬁnd a reasonable low energy structure and hence atom “positions” are available. Additionally a wide range of interesting and fundamental chemical properties are computed. Given how fundamen- tal some of the QM9 properties are, it is hard to believe success on more challenging chemical tasks will be possi- ble if we can’t make accurate statistical predictions for the properties computed in QM9.  

We can group the different properties we try to predict into four broad categories. First, we have four properties re- lated to how tightly bound together the atoms in a molecule are. These measure the energy required to break up the molecule at different temperatures and pressures. These include the atomization energy at    $0K$  ,    $U_{0}$   (eV), atomiza- tion energy at room temperature,    $U$   (eV), enthalpy of at- omization at room temperature,    $H$   (eV), and free energy of atomization,  $G\,(\mathrm{eV})$  

Next there are properties related to fundamental vibrations of the molecule, including the highest fundamental vibra- tional frequency  $\omega_{1}$     $(c m^{-1})$   and the zero point vibrational energy (ZPVE) (eV).  

Additionally, there are a number of properties that concern the states of the electrons in the molecule. They include the energy of the electron in the highest occupied molecu- lar orbital (HOMO)  ε HOMO  (eV), the energy of the lowest unoccupied molecular orbital (LUMO)  ε LUMO  (eV), and the electron energy gap   $(\Delta\varepsilon\,(\mathrm{eV}))$  ). The electron energy gap is simply the difference  ε HOMO ε LUMO .  

Finally, there are several measures of the spatial distribu- tion of electrons in the molecule. These include the elec- tronic spatial extent    $\langle R^{2}\rangle$   $(\mathrm{Bohr^{2}})$  , the norm of the dipo moment  $\mu$   (Debye), and the norm of static polarizability  α  $(\mathbf{Bohr^{3}})$  . For a more detailed description of these proper- ties, see the supplementary material.  

# 5. MPNN Variants  

We began our exploration of MPNNs around the GG-NN model which we believe to be a strong baseline. We fo- cused on trying different message functions, output func- tions, ﬁnding the appropriate input representation, and properly tuning hyperparameters.  

For the rest of the paper we use    $d$   to denote the dimension of the internal hidden representation of each node in the graph, and    $n$   to denote the number of nodes in the graph. Our implementation of MPNNs in general operates on di- rected graphs with a separate message channel for incom- ing and outgoing edges, in which case the incoming mes- sage  $m_{v}$   is the concatenation of  $m_{v}^{\mathrm{in}}$    and  $m_{v}^{\mathrm{out}}$  , this was also used in  Li et al.  ( 2016 ). When we apply this to undirected chemical graphs we treat the graph as directed, where each original edge becomes both an incoming and outgoing edge with the same label. Note there is nothing special about the direction of the edge, it is only relevant for parameter tying. Treating undirected graphs as directed means that the size of the message channel is  $2d$   instead of    $d$  .  

The input to our MPNN model is a set of feature vectors for the nodes of the graph,    $x_{v}$  , and an adjacency matrix    $A$  with vector valued entries to indicate different bonds in the molecule as well as pairwise spatial distance between two atoms. We experimented as well with the message func- tion used in the GG-NN family, which assumes discrete edge labels, in which case the matrix  $A$   has entries in a dis- crete alphabet of size    $k$  . The initial hidden states  $h_{v}^{0}$    are set to be the atom input feature vectors  $x_{v}$   and are padded up to some larger dimension    $d$  . All of our experiments used weight tying at each time step    $t$  , and a GRU ( Cho et al. , 2014 ) for the update function as in the GG-NN family.  

# 5.1. Message Functions  

Matrix Multiplication:  We started with the message func- tion used in GG-NN which is deﬁned by the equation  $M(h_{v},h_{w},e_{v w})=A_{e_{v w}}h_{w}$  .  

Edge Network: To allow vector valued edge features we propose the message function    $\begin{array}{r l}{{\cal M}(h_{v},h_{w},e_{v w})}&{{}=}\end{array}$   $A(e_{v w})h_{w}$   where    $A(e_{v w})$   is a neural network which maps the edge vector  $e_{v w}$   to a  $d\times d$   matrix.  

Pair Message:  One property that the matrix multiplication rule has is that the message from node    $w$   to node    $v$   is a function only of the hidden state  $h_{w}$   and the edge  $e_{v w}$  . In particular, it does not depend on the hidden state    $h_{v}^{t}$  . In theory, a network may be able to use the message channel more efﬁciently if the node messages are allowed to de- pend on both the source and destination node. Thus we also tried using a variant on the message function as de- scribed in ( Battaglia et al. ,  2016 ). Here the message from  $w$   to    $v$   along edge  $e$   is  $m_{w v}=f\left(h_{w}^{t},h_{v}^{t},e_{v w}\right)$   where  $f$   is a neural network.  

When we apply the above message functions to directed graphs, there are two separate functions used,    $M^{\mathrm{in}}$    and an  $M^{\mathrm{out}}$  . Which function is applied to a particular edge    $e_{v w}$  depends on the direction of that edge.  

# 5.2. Virtual Graph Elements  

We explored two different ways to change how the mes- sages are passed throughout the model. The simplest mod- iﬁcation involves adding a separate “virtual” edge type for pairs of nodes that are not connected. This can be imple- mented as a data preprocessing step and allows information to travel long distances during the propagation phase.  

We also experimented with using a latent “master” node, which is connected to every input node in the graph with a special edge type. The master node serves as a global scratch space that each node both reads from and writes to in every step of message passing. We allow the master node to have a separate node dimension    $d_{m a s t e r}$  , as well as sep- arate weights for the internal update function (in our case a GRU). This allows information to travel long distances during the propagation phase. It also, in theory, allows ad- ditional model capacity (e.g. large values of  $d_{m a s t e r_{c}}\grave{}$  ) with- out a substantial hit in performance, as the complexity of the master node model is  $O(|E|d^{2}+n d_{m a s t e r}^{2})$  .  

# 5.3. Readout Functions  

We experimented with two readout functions. First is the readout function used in GG-NN, which is deﬁned by equa- tion  4 . Second is a set2set model from  Vinyals et al.  ( 2015 ). The set2set model is speciﬁcally designed to operate on sets and should have more expressive power than simply sum- ming the ﬁnal node states. This model ﬁrst applies a linear projection to each tuple   $(h_{v}^{T},x_{v})$  ) and then takes as input the set of projected tuples    $\bar{T}=\{(h_{v}^{T},x_{v})\}$  } . Then, after  $M$  steps of computation, the set2set model produces a graph level embedding  $q_{t}^{*}$    which is invariant to the order of the of the tuples  $T$  . We feed this embedding    $q_{t}^{*}$    through a neural network to produce the output.  

# 5.4. Multiple Towers  

One issue with MPNNs is scalability. In particular, a sin- gle step of the message passing phase for a dense graph requires    $O(n^{2}d^{2})$   ﬂoating point multiplications. As  $n$   or    $d$  get large this can be computationally expensive. To address this issue we break the  $d$   dimensional node embeddings  $h_{v}^{t}$  into  $k$   different    $d/k$   dimensional embeddings    $h_{v}^{t,k}$  and run a propagation step on each of the  $k$   copies separately to get temporary embeddings    $\{\tilde{h}_{v}^{t+1,k},v\ \in\ G\}$   ∈ } , using s arate message and update functions for each copy. The  k  tem- porary embeddings of each node are then mixed together according to the equation  

$$
\left(h_{v}^{t,1},h_{v}^{t,2},.\,.\,.\,,h_{v}^{t,k}\right)=g\left(\tilde{h}_{v}^{t,1},\tilde{h}_{v}^{t,2},.\,.\,.\,,\tilde{h}_{v}^{t,k}\right)
$$  

where    $g$   denotes a neural network and    $\left(x,y,.\cdot.\right)$   denotes concatenation, with  $g$   shared across all nodes in the graph. This mixing preserves the invariance to permutations of the nodes, while allowing the different copies of the graph to communicate with each other during the propagation phase. This can be advantageous in that it allows larger hidden states for the same number of parameters, which yields a computational speedup in practice. For exam- ple, when the message function is matrix multiplication (as in GG-NN) a propagation step of a single copy takes  $O\left(n^{2}(d/k)^{2}\right)$  time, and there are  $k$   copies, therefore the  

![](images/1947925f6bd4fd969775c97ac6f496ff66d1bbde2c683a41cac8e88b4eb7585b.jpg)  

overall time complexity is    $O\left(n^{2}d^{2}/k\right)$     , with some addi- tional overhead due to the mixing network. For    $k\,=\,8$  ,  $n=9$   and    $d=200$   we see a factor of 2 speedup in infer- ence time over a    $k=1$  ,  $n=9$  , and  $d=200$   architecture. This variation would be most useful for larger molecules, for instance molecules from GDB-17 ( Ruddigkeit et al. , 2012 ).  

# 6. Input Representation  

There are a number of features available for each atom in a molecule which capture both properties of the electrons in the atom as well as the bonds that the atom participates in. For a list of all of the features see table  1 . We experi- mented with making the hydrogen atoms explicit nodes in the graph (as opposed to simply including the count as a node feature), in which case graphs have up to 29 nodes. Note that having larger graphs signiﬁcantly slows training time, in this case by a factor of roughly 10. For the adja- cency matrix there are three edge representations used de- pending on the model.  

Chemical Graph:  In the abscence of distance information, adjacency matrix entries are discrete bond types: single, double, triple, or aromatic.  

Distance bins:  The matrix multiply message function as- sumes discrete edge types, so to include distance informa- tion we bin bond distances into 10 bins, the bins are ob- tained by uniformly partitioning the interval  [2 ,  6]  into 8 bins, followed by adding a bin  [0 ,  2]  and    $[6,\infty]$  . These bins were hand chosen by looking at a histogram of all dis- tances. The adjacency matrix then has entries in an alpha- bet of size 14, indicating bond type for bonded atoms and distance bin for atoms that are not bonded. We found the distance for bonded atoms to be almost completely deter- mined by bond type.  

Raw distance feature:  When using a message function which operates on vector valued edges, the entries of the adjacency matrix are then 5 dimensional, where the ﬁrst di- mension indicates the euclidean distance between the pair of atoms, and the remaining four are a one-hot encoding of  

# the bond type.  

# 7. Training  

Each model and target combination was trained using a uni- form random hyper parameter search with 50 trials.    $T$   was ined to be in the range  $3\leq T\leq8$   (in practice,   $T\ \geq\ 3$   ≥  works). The num  computations  M was chosen from the range  $1\leq M\leq12$   ≤  ≤ . All models were trained using SGD with the ADAM optimizer ( Kingma & Ba  ( 2014 )), with batch size 20 for 3 million steps ( 540 epochs). The initial learning rate was chosen uniformly be- tween  $1e^{-5}$    and  $5e^{-4}$  . We used a linear learning rate decay that began between   $10\%$   and  $90\%$   of the way through train- ing and the initial learning rate  $l$   decayed to a ﬁnal learning rate  $l*F$  , using a decay factor  $F$   in the range  [ . 01 ,  1] .  

The QM-9 dataset has 130462 molecules in it. We ran- domly chose 10000 samples for validation, 10000 samples for testing, and used the rest for training. We use the vali- dation set to do early stopping and model selection and we report scores on the test set. All targets were normalized to have mean 0 and variance 1. We minimize the mean squared error between the model output and the target, al- though we evaluate mean absolute error.  

# 8. Results  

In all of our tables we report the ratio of the mean ab- solute error (MAE) of our models with the provided esti- mate of chemical accuracy for that target. Thus any model with error ratio less than 1 has achieved chemical accu- racy for that target. In the supplementary material we list the chemical accuracy estimates for each target, these are the same estimates that were given in  Faber et al.  ( 2017 ). In this way, the MAE of our models can be calculated as ( Error Ratio )  $\times$   ( Chemical Accuracy ) . Note, unless other- wise indicated, all tables display result of models trained individually on each target (as opposed to training one model to predict all 13).  

We performed numerous experiments in order to ﬁnd the best possible MPNN on this dataset as well as the proper input representation. In our experiments, we found that in- cluding the complete edge feature vector (bond type, spatial distance) and treating hydrogen atoms as explicit nodes in the graph to be very important for a number of targets. We also found that training one model per target consistently outperformed jointly training on all 13 targets. In some cases the improvement was up to   $40\%$  . Our best MPNN variant used the edge network message function, set2set output, and operated on graphs with explicit hydrogens. We were able to further improve performance on the test set by ensembling the predictions of the ﬁve models with lowest validation error.  

In table  2  we compare the performance of our best MPNN variant (denoted with  enn-s2s ) and the corresponding en- semble (denoted with  enn-s2s-ens5 ) with the previous state of the art on this dataset as reported in  Faber et al.  ( 2017 ). For clarity the error ratios of the best non-ensemble mod- els are shown in bold. This previous work performed a comparison study of several existing ML models for QM9 and we have taken care to use the same train, val- idation, and test split. These baselines include 5 differ- ent hand engineered molecular representations, which then get fed through a standard, off-the-shelf classiﬁer. These input representations include the Coulomb Matrix ( CM , Rupp et al.  ( 2012 )), Bag of Bonds ( BoB ,  Hansen et al.

 ( 2015 )), Bonds Angles, Machine Learning ( BAML ,  Huang

 & von Lilienfeld  ( 2016 )), Extended Connectivity Finger- prints ( ECPF4 ,  Rogers & Hahn  ( 2010 )), and “Projected Histograms” ( HDAD ,  Faber et al.  ( 2017 )) representations. In addition to these hand engineered features we include two existing baseline MPNNs, the Molecular Graph Con- volutions model ( GC ) from  Kearnes et al.  ( 2016 ), and the original GG-NN model  Li et al.  ( 2016 ) trained with dis- tance bins. Overall, our new MPNN achieves chemical ac- curacy on 11 out of 13 targets and state of the art on all 13 targets.  

Training Without Spatial Information:  We also experi- mented in the setting where spatial information is not in- cluded in the input. In general, we ﬁnd that augmenting the MPNN with some means of capturing long range interac- tions between nodes in the graph greatly improves perfor- mance in this setting. To demonstrate this we performed 4 experiments, one where we train the GG-NN model on the sparse graph, one where we add virtual edges, one where we add a master node, and one where we change the graph level output to a set2set output. The error ratios averaged across the 13 targets are shown in table  3 . Overall, these three modiﬁcations help on all 13 targets, and the Set2Set output achieves chemical accuracy on 5 out of 13 targets. For more details, consult the supplementary material. The experiments shown tables  3  and  4  were run with a partial charge feature as a node input. This feature is an output of the DFT calculation and thus could not be used in an ap- plied setting. The state of art numbers we report in table  2 do not use this feature.  

Towers:  Our original intent in developing the towers vari- ant was to improve training time, as well as to allow the model to be trained on larger graphs. However, we also found some evidence that the multi-tower structure im- proves generalization performance. In table  4  we com- pare GG-NN  $^+$   towers  $^+$   set2set output vs a baseline GG-  $\mathbf{NN}+\mathrm{set}2\mathrm{set}$   output when distance bins are used. We do this comparison in both the joint training regime and when training one model per target. The towers model outper- forms the baseline model on 12 out of 13 targets in both  

![Table 2.  Comparison of Previous Approaches (left) with MPNN baselines (middle) and our methods (right) ](images/15edbd000a4cf469e09d64af1fb4ce55af9909ad9ba3e3398c53bfbfca1f12e5.jpg)  

![Table 3.  Models Trained Without Spatial Information ](images/17ff38b67abe723fba05a55264d7f7d65d8a0e78cd273eb4fcf61a584c1d056d.jpg)  

![Table 4.  Towers vs Vanilla GG-NN (no explicit hydrogen) ](images/32c0ce4b5f07907755114fcf7fedafacc49c59fd6c745df5524fe64b02b2c027.jpg)  
GG-NN  $^+$   individual training 1.53 towers8  $^+$   individual training 1.37  

individual and joint target training. We believe the beneﬁt of towers is that it resembles training an ensemble of mod- els. Unfortunately, our attempts so far at combining the towers and edge network message function have failed to further improve performance, possibly because the combi- nation makes training more difﬁcult. Further training de- tails, and error ratios on all targets can be found in the sup- plementary material.  

Additional Experiments:  In preliminary experiments, we tried disabling weight tying across different time steps. However, we found that the most effective way to increase performance was to tie the weights and use a larger hidden dimension  $d$  . We also early on found the pair message func- tion to perform worse than the edge network function. This included a toy pathﬁnding problem which was originally designed to beneﬁt from using pair messages. Also, when trained jointly on the 13 targets the edge network function outperforms pair message on 11 out of 13 targets, and has an average error ratio of 1.53 compared to 3.98 for pair message. Given the difﬁculties with training this function we did not pursue it further. For performance on smaller sized training sets, consult the supplementary material.  

# 9. Conclusions and Future Work  

Our results show that MPNNs with the appropriate mes- sage, update, and output functions have a useful induc- tive bias for predicting molecular properties, outperforming several strong baselines and eliminating the need for com- plicated feature engineering. Moreover, our results also re- veal the importance of allowing long range interactions be- tween nodes in the graph with either the master node or the set2set output. The towers variation makes these models more scalable, but additional improvements will be needed to scale to much larger graphs.  

An important future direction is to design MPNNs that can generalize effectively to larger graphs than those appear- ing in the training set or at least work with benchmarks designed to expose issues with generalization across graph sizes. Generalizing to larger molecule sizes seems partic- ularly challenging when using spatial information. First of all, the pairwise distance distribution depends heavily on the number of atoms. Second, our most successful ways of using spatial information create a fully connected graph where the number of incoming messages also depends on the number of nodes. To address the second issue, we be- lieve that adding an attention mechanism over the incom- ing message vectors could be an interesting direction to ex- plore.  

# Acknowledgements  

We would like to thank Lukasz Kaiser, Geoffrey Irving, Alex Graves, and Yujia Li for helpful discussions. Thank you to Adrian Roitberg for pointing out an issue with the use of partial charges in an earlier version of this paper.  

# References  

Battaglia, Peter, Pascanu, Razvan, Lai, Matthew, Rezende, Danilo Jimenez, and Kavukcuoglu, Koray. Interac- tion networks for learning about objects, relations and physics. In  Advances in Neural Information Processing Systems , pp. 4502–4510, 2016.  

Becke, Axel D. Density-functional thermochemistry. iii. the role of exact exchange. The Journal of Chemi- cal Physics , 98(7):5648–5652, 1993. doi: 10.1063/1. 464913. URL  http://dx.doi.org/10.1063/1. 464913 .  

Behler, J¨ org and Parrinello, Michele. General- ized neural-network representation of high-dimensional potential-energy surfaces. Phys. Rev. Lett. , 98: 146401, Apr 2007. doi: 10.1103/PhysRevLett.98. 146401. URL  http://link.aps.org/doi/10. 1103/PhysRevLett.98.146401 .  

Bing, Huang, von Lillenfeld, O. Anatole, and Bakowies, Dirk. personal communication, 2017.  

Bruna, Joan, Zaremba, Wojciech, Szlam, Arthur, and Le- Cun, Yann. Spectral networks and locally connected net- works on graphs.  arXiv preprint arXiv:1312.6203 , 2013.  

Ceperley, David and Alder, B. Quantum monte carlo.  Sci- ence , 231, 1986.  

Cho, Kyunghyun, Van Merri¨ enboer, Bart, Bahdanau, Dzmitry, and Bengio, Yoshua. On the properties of neu- ral machine translation: Encoder-decoder approaches. arXiv preprint arXiv:1409.1259 , 2014.  

Defferrard, Micha¨ el, Bresson, Xavier, and Vandergheynst, Pierre. Convolutional neural networks on graphs with fast localized spectral ﬁltering. In  Advances in Neural Information Processing Systems , pp. 3837–3845, 2016.  

Duvenaud, David K, Maclaurin, Dougal, Iparraguirre, Jorge, Bombarell, Rafael, Hirzel, Timothy, Aspuru- Guzik, Al´ an, and Adams, Ryan P. Convolutional net- works on graphs for learning molecular ﬁngerprints. In Advances in neural information processing systems , pp. 2224–2232, 2015.  

Faber, Felix, Hutchison, Luke, Huang, Bing, Gilmer, Justin, Schoenholz, Samuel S., Dahl, George E., Vinyals, Oriol, Kearnes, Steven, Riley, Patrick F., and von  

Lilienfeld, O. Anatole. Fast machine learning mod- els of electronic and energetic properties consistently reach approximation errors better than dft accuracy. https://arxiv.org/abs/1702.05532 , 2017.  

Hansen, Katja, Biegler, Franziska, Ramakrishnan, Raghu- nathan, Pronobis, Wiktor, von Lilienfeld, O. Anatole, Mller, Klaus-Robert, and Tkatchenko, Alexandre. Ma- chine learning predictions of molecular properties: Ac- curate many-body potentials and nonlocality in chem- ical space. The journal of physical chemistry let- ters , 6(12):2326–2331, 2015. doi: 10.1021/acs.jpclett. 5b00831. URL  http://dx.doi.org/10.1021/ acs.jpclett.5b00831 .  

Hedin, Lars. New method for calculating the one-particle green’s function with application to the electron-gas problem.  Phys. Rev. , 139:A796–A823, Aug 1965. doi: 10.1103/PhysRev.139.A796. URL  http://link. aps.org/doi/10.1103/PhysRev.139.A796 .  

Hinton, Geoffrey, Deng, Li, Yu, Dong, Dahl, George E., Mohamed, Abdel-rahman, Jaitly, Navdeep, Senior, An- drew, Vanhoucke, Vincent, Nguyen, Patrick, Sainath, Tara N, et al. Deep neural networks for acoustic mod- eling in speech recognition: The shared views of four research groups.  IEEE Signal Processing Magazine , 29 (6):82–97, 2012.  

Hohenberg, P. and Kohn, W. Inhomogeneous electron gas. Phys. Rev. , 136:B864–B871, Nov 1964. doi: 10.1103/ PhysRev.136.B864. URL  http://link.aps.org/ doi/10.1103/PhysRev.136.B864 .  

Hu, LiHong, Wang, XiuJun, Wong, LaiHo, and Chen, GuanHua. Combined ﬁrst-principles calculation and neural-network correction approach for heat of forma- tion.  The Journal of Chemical Physics , 119(22):11501– 11507, 2003.  

Huang, Bing and von Lilienfeld, O. Anatole. Commu- nication: Understanding molecular representations in machine learning: The role of uniqueness and target similarity.  The Journal of Chemical Physics , 145(16): 161102, 2016. doi: 10.1063/1.4964627. URL  http: //dx.doi.org/10.1063/1.4964627 .  

Kearnes, Steven, McCloskey, Kevin, Berndl, Marc, Pande, Vijay, and Riley, Patrick. Molecular graph convolutions: Moving beyond ﬁngerprints.  Journal of Computer-Aided Molecular Design , 30(8):595–608, 2016.  

Kingma, Diederik and Ba, Jimmy. Adam: A method for stochastic optimization. arXiv preprint arXiv:1412.6980 , 2014.  

Kipf, T. N. and Welling, M. Semi-Supervised Classiﬁ- cation with Graph Convolutional Networks. ArXiv e- prints , September 2016.  

Krizhevsky, Alex, Sutskever, Ilya, and Hinton, Geoffrey E. Imagenet classiﬁcation with deep convolutional neural networks. In  Advances in neural information processing systems , pp. 1097–1105, 2012.  

Li, Yujia, Tarlow, Daniel, Brockschmidt, Marc, and Zemel, Richard. Gated graph sequence neural networks.  ICLR , 2016.  

Lusci, Alessandro, Pollastri, Gianluca, and Baldi, Pierre. Deep architectures and deep learning in chemoinformat- ics: the prediction of aqueous solubility for drug-like molecules.  Journal of chemical information and mod- eling , 53(7):1563–1575, 2013.  

Marino, Kenneth, Salakhutdinov, Ruslan, and Gupta, Ab- hinav. The more you know: Using knowledge graphs for image classiﬁcation.  arXiv preprint arXiv:1612.04844 , 2016.  

Merkwirth, Christian and Lengauer, Thomas. Automatic generation of complementary descriptors with molecular graph networks.  Journal of chemical information and modeling , 45(5):1159–1168, 2005.  

Micheli, Alessio. Neural network for graphs: A contex- tual constructive approach.  IEEE Transactions on Neu- ral Networks , 20(3):498–511, 2009.  

Montavon, Gr´ egoire, Hansen, Katja, Fazli, Siamac, Rupp, Matthias, Biegler, Franziska, Ziehe, Andreas, Tkatchenko, Alexandre, von Lilienfeld, O. Anatole, and M¨ uller, Klaus-Robert. Learning invariant representa- tions of molecules for atomization energy prediction. In Advances in Neural Information Processing Systems , pp. 440–448, 2012.  

Niepert, Mathias, Ahmed, Mohamed, and Kutzkov, Kon- stantin. Learning convolutional neural networks for graphs. In  Proceedings of the 33rd annual international conference on machine learning. ACM , 2016.  

Ramakrishnan, Raghunathan, Dral, Pavlo O, Rupp, Matthias, and Von Lilienfeld, O Anatole. Quan- tum chemistry structures and properties of 134 kilo molecules.  Scientiﬁc data , 1, 2014.  

Rogers, David and Hahn, Mathew. Extended-connectivity ﬁngerprints.  Journal of chemical information and mod- eling , 50(5):742–754, 2010.  

Ruddigkeit, Lars, Van Deursen, Ruud, Blum, Lorenz C, and Reymond, Jean-Louis. Enumeration of 166 bil- lion organic small molecules in the chemical universe  

database gdb-17.  Journal of chemical information and modeling , 52(11):2864–2875, 2012.  

Rupp, Matthias, Tkatchenko, Alexandre haand M¨ uller, Klaus-Robert, and von Lilienfeld, O. Anatole. Fast and accurate modeling of molecular atomization ener- gies with machine learning.  Physical review letters , 108 (5):058301, Jan 2012. URL  http://dx.doi.org/ 10.1103/PhysRevLett.108.058301 .  

Scarselli, Franco, Gori, Marco, Tsoi, Ah Chung, Hagen- buchner, Markus, and Monfardini, Gabriele. The graph neural network model. IEEE Transactions on Neural Networks , 20(1):61–80, 2009.  

Schoenholz, Samuel S., Cubuk, Ekin D., Sussman, Daniel M, Kaxiras, Efthimios, and Liu, Andrea J. A structural approach to relaxation in glassy liquids.  Na- ture Physics , 2016.  

Sch¨ utt, Kristof T, Arbabzadah, Farhad, Chmiela, Stefan, M¨ uller, Klaus R, and Tkatchenko, Alexandre. Quantum- chemical insights from deep tensor neural networks.  Na- ture Communications , 8, 2017.  

Stillinger, Frank H. and Weber, Thomas A. Computer sim- ulation of local order in condensed phases of silicon. Phys. Rev. B , 31:5262–5271, Apr 1985. doi: 10.1103/ PhysRevB.31.5262. URL  http://link.aps.org/ doi/10.1103/PhysRevB.31.5262 .  

Vinyals, Oriol, Bengio, Samy, and Kudlur, Manjunath. Order matters: Sequence to sequence for sets. arXiv preprint arXiv:1511.06391 , 2015.  

Wu, Yonghui, Schuster, Mike, Chen, Zhifeng, Le, Quoc V., Norouzi, Mohammad, Macherey, Wolfgang, Krikun, Maxim, Cao, Yuan, Gao, Qin, Macherey, Klaus, et al. Google’s neural machine translation system: Bridging the gap between human and machine translation.  arXiv preprint arXiv:1609.08144 , 2016.  

# 10. Appendix  

# 10.1. Interpretation of Laplacian based models as MPNNs  

Another family of models deﬁned in  Defferrard et al. ( 2016 ),  Bruna et al.  ( 2013 ),  Kipf & Welling  ( 2016 ) can be interpreted as MPNNs. These models generalize the notion of convolutions a general graph    $G$   with    $N$   nodes. In the language of MPNNs, these models tend to have very simple message functions and are typically applied to much larger graphs such as social network data. We closely follow the notation deﬁned in  Bruna et al.  ( 2013 ) equation (3.2). The model discussed in  Defferrard et al.  ( 2016 ) (equation 5)  

and  Kipf & Welling  ( 2016 ) can be viewed as special cases. Given an adjacency m  $W~\in~\mathbb{R}^{N\times N}$  ne the aph Laplacian to be  $L\,=\,I_{n}\,-\,D^{-1/2}W D^{-1/2}$   − where  $D$   is the diagonal degree matrix with  $D_{i i}=\deg(v_{i})$  . Let  $V$   denote the eigenvectors of    $L$  , ordered by eigenvalue. Let  $\sigma$   be a real valued nonlinearity (such as ReLU). We now deﬁne an operation which transforms an input vector    $x$   of size    $N\times d_{1}$   to a vector    $y$   of size  $N\times d_{2}$   (the full model can deﬁned as stacking these operations).  

$$
y_{j}=\sigma\left(\sum_{i=1}^{d_{1}}V F_{i,j}V^{T}x_{i}\right)\quad(j=1\ldots d_{2})
$$  

Here  $y_{j}$   and  $x_{i}$   are all  $N$   dimensional vectors correspond- ing to a scalar feature at each node. The matrices  $F_{i,j}$   are all diagonal    $N\times N$   matrices and contain all of the learned pa- rameters in the layer. We now expand equation  6  in terms of the full    $N\times d_{1}$   vector    $x$   and  $N\times d_{2}$   ve tor    $y$  , using  $v$   and    $w$   to index nodes in the graph  G  and    $i,\,j$   to index the dimensions of the node states. In this way    $x_{w,i}$   de- notes the    $i^{:}$  ’th dimension of node  $w$  , and    $y_{v,j}$   denotes the  $j$  ’th dimension of node    $v$  , furthermore we use    $x_{w}$   to de- note the    $d_{1}$   dimensional vector for node state    $w$  , and    $y_{v}$   to denote the    $d_{2}$   dimensional vector for node    $v$  . Deﬁne the rank 4 tensor  $\tilde{L}$  sion    $N\times N\times d_{1}\times d_{2}$   where  $\tilde{L}_{v,w,i,j}=(V F_{i,j}V^{T})_{v,w}$  . We will use  $\tilde{L}_{i,j}$   to denote the  $N\times N$   dimensional matrix where    $(\tilde{L}_{i,j})_{v,w}\ =\ \tilde{L}_{v,w,i,j}$  and  $\tilde{L}_{v,w}$  he    $d_{1}\times d_{2}$   dimensional matrix where  $(\tilde{L}_{v,w})_{i,j}\,=\,\tilde{L}_{v,w,i,j}$  . Writing equation  6  in this notation we have  

$$
\begin{array}{r l}&{y_{j}=\sigma\left(\displaystyle\sum_{i=1}^{d_{1}}\tilde{L}_{i,j}x_{i}\right)}\\ &{y_{v,j}=\sigma\left(\displaystyle\sum_{i=1,w=1}^{d_{1},N}\tilde{L}_{v,w,i,j}x_{w,i}\right)}\\ &{y_{v}=\sigma\left(\displaystyle\sum_{w=1}^{N}\tilde{L}_{v,w}x_{w}\right).}\end{array}
$$  

Relabelling  $y_{v}$   as  $h_{v}^{t+1}$  and  $x_{w}$   as  $h_{w}^{t}$    this last line can be in- terpreted as the message passing update in an MPNN where  $\dot{M(h_{v}^{t},h_{w}^{t})}=\tilde{L}_{v,w}h_{w}^{\bar{t}}$    and    $U(h_{v}^{t},m_{v}^{t+1})=\sigma(m_{v}^{t+1})$  .  

Motivated as a ﬁrst order approximation of the graph lapla- cian methods,  Kipf & Welling  ( 2016 ) propose the follow- ing layer-wise propagation rule:  

$$
H^{l+1}=\sigma\left(\tilde{D}^{-1/2}\tilde{A}\tilde{D}^{-1/2}H^{l}W^{l}\right)
$$  

Here  ${\tilde{A}}\,=\,A+I_{N}$   where    $A$   is the real valued adjacency matrix for an undirected graph  $G$  . Adding the identity ma- trix  $I_{N}$   corresponds to adding self loops to the graph. Also  $\begin{array}{r}{\tilde{D}_{i i}=\sum_{j}\tilde{A}_{i j}}\end{array}$    P  denotes the degree matrix for the graph with self loops,    $W^{l}\in\mathbb{R}^{D\times D}$    is a layer-speciﬁc trainable weight trix,  $\sigma(\cdot)$  ·  denotes a real valued nonlinea y. Each  $H^{l}$    is a  R  $\mathbb{R}^{N\times D}$    dimensional matrix indicating the  D  dimen- sional node states for the    $N$   nodes in the graph.  

In what follows, given a matrix  $M$   we use  $M_{(v)}$   to denote the row in    $M$   indexed by  $v$     $\cdot_{v}$   will always correspond to a node in the graph    $G$  ). Let  ${\cal L}=\tilde{D}^{-1/2}\tilde{A}\tilde{D}^{-1/2}$  . To get the updated node state for node    $v$   we have  

$$
\begin{array}{l}{{\displaystyle{\cal H}_{(v)}^{l+1}=\sigma\left({\cal L}_{(v)}{\cal H}^{l}{\cal W}^{l}\right)}}\\ {{\displaystyle~~~~~~~~~=\sigma\left(\sum_{w}{\cal L}_{v w}{\cal H}_{(w)}^{l}{\cal W}^{l}\right)}}\end{array}
$$  

Relabelling the row vector  $H_{(v)}^{l+1}$    as an    $N$   dimensional col- umn vector    $h_{v}^{t+1}$  the above equation is equivalent to  

$$
h_{v}^{t+1}=\sigma\left((W^{l})^{T}\sum_{w}L_{v w}h_{w}^{t}\right)
$$  

which is equivalent to a message function  

$$
M_{t}(h_{v}^{t},h_{w}^{t})=L_{v w}h_{w}^{t}=\tilde{A}_{v w}(\deg(v)\deg(w))^{-1/2}h_{w}^{t},
$$  

and update function  

$$
U_{t}(h_{v}^{t},m_{v}^{t+1})=\sigma((W^{t})^{T}m^{t+1}).
$$  

Note that the  $L_{v w}$   are all scalar valued, so this model corre- sponds to taking a certain weighted average of neighboring nodes at each time step.  

# 10.2. A More Detailed Description of the Quantum Properties  

First there the four atomization energies.  

•  Atomization energy at    $0K\ U_{0}$   (eV): This is the en- ergy required to break up the molecule into all of its constituent atoms if the molecule is at absolute zero. This calculation assumes that the molecules are held at ﬁxed volume. •  Atomization energy at room temperature    $U$   (eV): Like    $U_{0}$  , this is the energy required to break up the molecule if it is at room temperature. •  Enthalpy of atomization at room temperature    $H$   (eV): The enthalpy of atomization is similar in spirit to the energy of atomization,    $U$  . However, unlike the energy this calculation assumes that the constituent molecules are held at ﬁxed pressure.  

•  Free energy   atom ation    $G$   (eV): Once again this is similar to  U  and  H , but assumes that the system is held at ﬁxed temperature and pressure during the dissociation.  

Next there are properties related to fundamental vibrations of the molecule:  

•   t fundamental vibrational frequency  $\omega_{1}$   $(c m^{-1})$  ): Every molecule has fundamental vibrational modes that it can naturally oscillate at.    $\omega_{1}$   is the mode  

•  Zero Point Vibrational Energy (ZPVE) (eV): Even at zero temperature quantum mechanical uncertainty implies that atoms vibrate. This is known as the zero point vibrational energy and can be calculated once the allowed vibrational modes of a molecule are known.  

Additionally, there are a number of properties that concern the states of the electrons in the molecule:  

•  Highest Occupied Molecular Orbital (HOMO)  ε HOMO (eV): Quantum mechanics dictates that the allowed states that electrons can occupy in a molecule are dis- crete. The Pauli exclusion principle states that no two electrons may occupy the same state. At zero temper- ature, therefore, electrons stack in states from lowest energy to highest energy. HOMO is the energy of the highest occupied electronic state. •  Lowest Unoccupied Molecular Orbital (LUMO) ε LUMO  (eV): Like HOMO, LUMO is the lowest en- ergy electronic state that is unoccupied. •  Electron energy gap  $\Delta\varepsilon$   (eV): This is the difference in energy between LUMO and HOMO. It is the lowest energy transition that can occur when an electron is excited from an occupied state to an unoccupied state.  $\Delta\varepsilon$   also dictates the longest wavelength of light that the molecule can absorb.  

Finally, there are several measures of the spatial distribu- tion of electrons in the molecule:  

•  Electronic Spatial Extent    $\langle R^{2}\rangle$   $(\mathrm{Bohr^{2}})$  ): The elec- tronic spatial extent is the second moment of the charge distribution,    $\rho(r)$  , or in other words    $\langle R^{2}\rangle\;=\;$   $\begin{array}{r}{\int d r r^{2}\rho(r)}\end{array}$  . •  Norm of the dipole moment    $\mu$   (Debye): The dipole moment,    $p(r)\ =\ \textstyle\int d r^{\prime}p(r^{\prime})(r\,-\,r^{\prime})$  R , approximates the electric ﬁeld far from a molecule. The norm of the dipole moment is related to how anisotropically  

![Table 5.  Chemical Accuracy For Each Target ](images/c5fa507d2a77dd419caf83633f92f52804a44d37a502d771bd29088ebfbdb91d.jpg)  

the charge is distributed (and hence the strength of the ﬁeld far from the molecule). This degree of anisotropy is in turn related to a number of material properties (for example hydrogen bonding in water causes the dipole moment to be large which has a large effect on dynamics and surface tension).  

•  Norm of the static polarizability    $\alpha$     $(\mathbf{Bohr^{3}})$  ):    $\alpha$   mea- sures the extent to which a molecule can sponta- neously incur a dipole moment in response to an ex- ternal ﬁeld. This is in turn related to the degree to which i.e. Van der Waals interactions play a role in the dynamics of the medium.  

# 10.3. Chemical Accuracy and DFT Error  

In Table  5  we list the mean absolute error numbers for chemical accuracy. These are the numbers used to com- pute the error ratios of all models in the tables. The mean absolute errors of our models can thus be calculated as ( Error Ratio )  $\times$   ( Chemical Accuracy ) . We also include some estimates of the mean absolute error for the DFT cal- culation to the ground truth. Both the estimates of chem- ical accruacy and DFT error were provided in  Faber et al. ( 2017 ).  

# 10.4. Additional Results  

In Table  6  we compare the performance of the best archi- tecture (edge network  $^+$   set2set output) on different sized training sets. It is surprising how data efﬁcient this model is on some targets. For example, on both R2 and Omega our models are equal or better with 11k samples than the best baseline is with 110k samples.  

In Table  7  we compare the performance of several mod- els trained without spatial information. The left 4 columns show the results of 4 experiments, one where we train the  

![Table 6.  Results from training the edge network   $^+$   set2set model on different sized training sets (N denotes the number of training samples) ](images/96ec694cac229e19ffd729124beb4ce3193e0421af35ba986e63da6f56826b09.jpg)  

GG-NN model on the sparse graph, one where we add vir- tual edges ( ve ), one where we add a master node ( mn ), and one where we change the graph level output to a set2set output ( s2s ). In general, we ﬁnd that it’s important to al- low the model to capture long range interactions in these graphs.  

In Table  8  we compare   $\mathrm{GGG}\mathrm{-NN}+$   towers  $^+$   set2set output ( tow8 ) vs a baseline   $\mathrm{{GC-NN+set}}2\mathrm{{set}}$  t output ( GG-NN ) when distance bins are used. We do this comparison in both the joint training regime   $\mathbf{\eta}^{\left(\mathbf{j}\right)}$   and when training one model per target ( i ). For joint training of the baseline we used 100 trials with  $d=200$   as well as 200 trials where  $d$   was chosen randomly in the set    $\{43,73,113,153\}$  , we report the minimum test error across all 300 trials. For individual training of the baseline we used 100 trials where  $d$   was cho- sen uniformly in the range  [43 ,  200] . The towers model was always trained with    $d=200$   and  $k\,=\,8$  , with 100 tuning trials for joint training and 50 trials for individual training. The towers model outperforms the baseline model on 12 out of 13 targets in both individual and joint target training.  

In Table  9  right 2 columns compare the edge network ( enn ) with the pair message network   $\left(\mathbf{pm}\right)$   in the joint training regime ( j ). The edge network consistently outperforms the pair message function across most targets.  

In Table  10  we compare our MPNNs with different input featurizations. All models use the Set2Set output and GRU update functions. The no distance model uses the matrix multiply message function, the distance models use the edge neural network message function.  

![Table 7.  Comparison of models when distance information is ex- cluded ](images/5515faf2eb7e157291bdef1223951dd3bc2dcc0a83ef7ad0c518da8f4e9d62a0.jpg)  

![Table 8.  Towers vs Vanilla Model (no explicit hydrogen) ](images/46b73dba9a474c769e65b7ff6928b41aefb6a60f6c0e57b3efd6e11f40d1de3b.jpg)  

Table 9.  Pair Message vs Edge Network in joint training  

![](images/096d5c3ec4ce21c1bd8c05ab0b29ca9a57e99be04f8cbfa0eb9f8a6b1e4e153b.jpg)  

![Table 10.  Performance With Different Input Information ](images/5386217fc00ec1d252666de186cddffe75357b41fb09c195490e2e6967989d45.jpg)  
Average2.570.980.68  