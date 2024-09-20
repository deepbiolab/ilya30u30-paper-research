# Neural Turing Machines  

Alex Graves gravesa@google.com Greg Wayne gregwayne@google.com Ivo Danihelka danihelka@google.com  

Google DeepMind, London, UK  

# Abstract  

We extend the capabilities of neural networks by coupling them to external memory re- sources, which they can interact with by attentional processes. The combined system is analogous to a Turing Machine or Von Neumann architecture but is differentiable end-to- end, allowing it to be efﬁciently trained with gradient descent. Preliminary results demon- strate that  Neural Turing Machines  can infer simple algorithms such as copying, sorting, and associative recall from input and output examples.  

# 1 Introduction  

Computer programs make use of three fundamental mechanisms: elementary operations (e.g., arithmetic operations), logical ﬂow control (branching), and external memory, which can be written to and read from in the course of computation (Von Neumann, 1945). De- spite its wide-ranging success in modelling complicated data, modern machine learning has largely neglected the use of logical ﬂow control and external memory.  

Recurrent neural networks (RNNs) stand out from other machine learning methods for their ability to learn and carry out complicated transformations of data over extended periods of time. Moreover, it is known that RNNs are Turing-Complete (Siegelmann and Sontag, 1995), and therefore have the capacity to simulate arbitrary procedures,    $i f$   properly wired. Yet what is possible in principle is not always what is simple in practice. We therefore enrich the capabilities of standard recurrent networks to simplify the solution of algorithmic tasks. This enrichment is primarily via a large, addressable memory, so, by analogy to Turing’s enrichment of ﬁnite-state machines by an inﬁnite memory tape, we dub our device a “Neural Turing Machine” (NTM). Unlike a Turing machine, an NTM is a differentiable computer that can be trained by gradient descent, yielding a practical mechanism for learning programs.  

In human cognition, the process that shares the most similarity to algorithmic operation is known as “working memory.” While the mechanisms of working memory remain some- what obscure at the level of neurophysiology, the verbal deﬁnition is understood to mean a capacity for short-term storage of information and its rule-based manipulation (Badde- ley et al., 2009). In computational terms, these rules are simple programs, and the stored information constitutes the arguments of these programs. Therefore, an NTM resembles a working memory system, as it is designed to solve tasks that require the application of approximate rules to “rapidly-created variables.” Rapidly-created variables (Hadley, 2009) are data that are quickly bound to memory slots, in the same way that the number 3 and the number 4 are put inside registers in a conventional computer and added to make 7 (Minsky, 1967). An NTM bears another close resemblance to models of working memory since the NTM architecture uses an attentional process to read from and write to memory selectively. In contrast to most models of working memory, our architecture can learn to use its working memory instead of deploying a ﬁxed set of procedures over symbolic data.  

The organisation of this report begins with a brief review of germane research on work- ing memory in psychology, linguistics, and neuroscience, along with related research in artiﬁcial intelligence and neural networks. We then describe our basic contribution, a mem- ory architecture and attentional controller that we believe is well-suited to the performance of tasks that require the induction and execution of simple programs. To test this architec- ture, we have constructed a battery of problems, and we present their precise descriptions along with our results. We conclude by summarising the strengths of the architecture.  

# 2 Foundational Research  

# 2.1 Psychology and Neuroscience  

The concept of working memory has been most heavily developed in psychology to explain the performance of tasks involving the short-term manipulation of information. The broad picture is that a “central executive” focuses attention and performs operations on data in a memory buffer (Baddeley et al., 2009). Psychologists have extensively studied the capacity limitations of working memory, which is often quantiﬁed by the number of “chunks” of information that can be readily recalled (Miller, 1956).   These capacity limitations lead toward an understanding of structural constraints in the human working memory system, but in our own work we are happy to exceed them.  

In neuroscience, the working memory process has been ascribed to the functioning of a system composed of the prefrontal cortex and basal ganglia (Goldman-Rakic, 1995). Typ- ical experiments involve recording from a single neuron or group of neurons in prefrontal cortex while a monkey is performing a task that involves observing a transient cue, waiting through a “delay period,” then responding in a manner dependent on the cue. Certain tasks elicit persistent ﬁring from individual neurons during the delay period or more complicated neural dynamics. A recent study quantiﬁed delay period activity in prefrontal cortex for a complex, context-dependent task based on measures of “dimensionality” of the population code and showed that it predicted memory performance (Rigotti et al., 2013).  

Modeling studies of working memory range from those that consider how biophysical circuits could implement persistent neuronal ﬁring (Wang, 1999) to those that try to solve explicit tasks (Hazy et al., 2006) (Dayan, 2008) (Eliasmith, 2013). Of these, Hazy et al.’s model is the most relevant to our work, as it is itself analogous to the Long Short-Term Memory architecture, which we have modiﬁed ourselves. As in our architecture, Hazy et al.’s has mechanisms to gate information into memory slots, which they use to solve a memory task constructed of nested rules. In contrast to our work, the authors include no sophisticated notion of memory addressing, which limits the system to storage and recall of relatively simple, atomic data. Addressing, fundamental to our work, is usually left out from computational models in neuroscience, though it deserves to be mentioned that Gallistel and King (Gallistel and King, 2009) and Marcus (Marcus, 2003) have argued that addressing must be implicated in the operation of the brain.  

# 2.2 Cognitive Science and Linguistics  

Historically, cognitive science and linguistics emerged as ﬁelds at roughly the same time as artiﬁcial intelligence, all deeply inﬂuenced by the advent of the computer (Chomsky, 1956) (Miller, 2003). Their intentions were to explain human mental behaviour based on information or symbol-processing metaphors. In the early 1980s, both ﬁelds considered recursive or procedural (rule-based) symbol-processing to be the highest mark of cogni- tion. The Parallel Distributed Processing (PDP) or connectionist revolution cast aside the symbol-processing metaphor in favour of a so-called “sub-symbolic” description of thought processes (Rumelhart et al., 1986).  

Fodor and Pylyshyn (Fodor and Pylyshyn, 1988) famously made two barbed claims about the limitations of neural networks for cognitive modeling. They ﬁrst objected that connectionist theories were incapable of  variable-binding , or the assignment of a particular datum to a particular slot in a data structure. In language, variable-binding is ubiquitous; for example, when one produces or interprets a sentence of the form, “Mary spoke to John,” one has assigned “Mary” the role of subject, “John” the role of object, and “spoke to” the role of the transitive verb. Fodor and Pylyshyn also argued that neural networks with ﬁxed- length input domains could not reproduce human capabilities in tasks that involve process- ing  variable-length structures . In response to this criticism, neural network researchers including Hinton (Hinton, 1986), Smolensky (Smolensky, 1990), Touretzky (Touretzky, 1990), Pollack (Pollack, 1990), Plate (Plate, 2003), and Kanerva (Kanerva, 2009) inves- tigated speciﬁc mechanisms that could support both variable-binding and variable-length structure within a connectionist framework. Our architecture draws on and potentiates this work.  

Recursive processing of variable-length structures continues to be regarded as a hall- mark of human cognition. In the last decade, a ﬁreﬁght in the linguistics community staked several leaders of the ﬁeld against one another. At issue was whether recursive processing is the “uniquely human” evolutionary innovation that enables language and is specialized to language, a view supported by Fitch, Hauser, and Chomsky (Fitch et al., 2005), or whether multiple new adaptations are responsible for human language evolution and recursive pro- cessing predates language (Jackendoff and Pinker, 2005). Regardless of recursive process- ing’s evolutionary origins, all agreed that it is essential to human cognitive ﬂexibility.  

# 2.3 Recurrent Neural Networks  

Recurrent neural networks constitute a broad class of machines with dynamic state; that is, they have state whose evolution depends both on the input to the system and on the current state. In comparison to hidden Markov models, which also contain dynamic state, RNNs have a distributed state and therefore have signiﬁcantly larger and richer memory and computational capacity. Dynamic state is crucial because it affords the possibility of context-dependent computation; a signal entering at a given moment can alter the behaviour of the network at a much later moment.  

A crucial innovation to recurrent networks was the Long Short-Term Memory (LSTM) (Hochreiter and Schmidhuber, 1997). This very general architecture was developed for a speciﬁc purpose, to address the “vanishing and exploding gradient” problem (Hochreiter et al., 2001a), which we might relabel the problem of “vanishing and exploding sensitivity.” LSTM ameliorates the problem by embedding perfect integrators (Seung, 1998) for mem- ory storage in the network. The simplest example of a perfect integrator is the equation  $\mathbf{x}(t+1)=\mathbf{x}(t)+\mathbf{i}(t)$  , where  $\mathbf{i}(t)$   is an input to the system. The implicit identity matrix  $I{\bf x}(t)$   means that signals do not dynamically vanish or explode. If we attach a mechanism to this integrator that allows an enclosing network to choose when the integrator listens to inputs, namely, a programmable gate depending on context, we have an equation of the form  $\mathbf{x}(t+1)=\mathbf{x}(t)+g(\mathrm{convex})\mathbf{i}(t)$  . We can now selectively store information for an indeﬁnite length of time.  

Recurrent networks readily process variable-length structures without modiﬁcation. In sequential problems, inputs to the network arrive at different times, allowing variable- length or composite structures to be processed over multiple steps. Because they natively handle variable-length structures, they have recently been used in a variety of cognitive problems, including speech recognition (Graves et al., 2013; Graves and Jaitly, 2014), text generation (Sutskever et al., 2011), handwriting generation (Graves, 2013) and machine translation (Sutskever et al., 2014). Considering this property, we do not feel that it is ur- gent or even necessarily valuable to build explicit parse trees to merge composite structures greedily (Pollack, 1990) (Socher et al., 2012) (Frasconi et al., 1998).  

Other important precursors to our work include differentiable models of attention (Graves,  

![](images/f9e3a1b56717d1972f1eb31ed03657fb8ec6609e48176ed8b2c30a639baa264f.jpg)  
Figure 1: Neural Turing Machine Architecture.  During each update cycle, the controller network receives inputs from an external environment and emits outputs in response. It also reads to and writes from a memory matrix via a set of parallel read and write heads. The dashed line indicates the division between the NTM circuit and the outside world.  

2013) (Bahdanau et al., 2014) and program search (Hochreiter et al., 2001b) (Das et al., 1992), constructed with recurrent neural networks.  

# 3 Neural Turing Machines  

A Neural Turing Machine (NTM) architecture contains two basic components: a neural network  controller  and a memory bank. Figure 1 presents a high-level diagram of the NTM architecture. Like most neural networks, the controller interacts with the external world via input and output vectors. Unlike a standard network, it also interacts with a memory matrix using selective read and write operations. By analogy to the Turing machine we refer to the network outputs that parametrise these operations as “heads.”  

Crucially, every component of the architecture is differentiable, making it straightfor- ward to train with gradient descent. We achieved this by deﬁning ‘blurry’ read and write operations that interact to a greater or lesser degree with all the elements in memory (rather than addressing a single element, as in a normal Turing machine or digital computer). The degree of blurriness is determined by an attentional “focus” mechanism that constrains each read and write operation to interact with a small portion of the memory, while ignoring the rest. Because interaction with the memory is highly sparse, the NTM is biased towards storing data without interference. The memory location brought into attentional focus is determined by specialised outputs emitted by the heads. These outputs deﬁne a normalised weighting over the rows in the memory matrix (referred to as memory “locations”). Each weighting, one per read or write head, deﬁnes the degree to which the head reads or writes at each location. A head can thereby attend sharply to the memory at a single location or weakly to the memory at many locations.  

# 3.1 Reading  

Let    $\mathbf{M}_{t}$   be the contents of   $N\times M$   memory matrix at time t, where    $N$   is the number of memory locations, and  M  is the vector size at each location. Let    $\mathbf{w}_{t}$   be a vector of weightings over the    $N$   locations emitted by a read head at time  $t$  . Since all weightings are normalised, the  $N$   elements    $w_{t}(i)$   of    $\mathbf{w}_{t}$   obey the following constraints:  

$$
\sum_{i}w_{t}(i)=1,\qquad0\leq w_{t}(i)\leq1,\,\forall i.
$$  

The length    $M$   read vector  $\mathbf{r}_{t}$   returned by the head is deﬁned as a convex combination of the row-vectors    $\mathbf{M}_{t}(i)$   in memory:  

$$
\mathbf{r}_{t}\longleftarrow\sum_{i}w_{t}(i)\mathbf{M}_{t}(i),
$$  

which is clearly differentiable with respect to both the memory and the weighting.  

# 3.2 Writing  

Taking inspiration from the input and forget gates in LSTM, we decompose each write into two parts: an  erase  followed by an  add .  

Given a weighting    $\mathbf{w}_{t}$   emitted by a write head at time    $t$  , along with an  erase vector  $\mathbf{e}_{t}$   whose    $M$   elements all lie in the range    $(0,1)$  , the memory vectors    $\mathbf{M}_{t-1}(i)$   from the previous time-step are modiﬁed as follows:  

$$
\mathbf{\widetilde{M}}_{t}(i)\longleftarrow\mathbf{M}_{t-1}(i)\left[\mathbf{1}-w_{t}(i)\mathbf{e}_{t}\right],
$$  

where  1  is a row-vector of all  1 -s, and the multiplication against the memory location acts point-wise. Therefore, the elements of a memory location are reset to zero only if both the weighting at the location and the erase element are one; if either the weighting or the erase is zero, the memory is left unchanged. When multiple write heads are present, the erasures can be performed in any order, as multiplication is commutative.  

Each write head also produces a length    $M$   add vector    ${\bf{a}}_{t}$  , which is added to the memory after the erase step has been performed:  

$$
\mathbf{M}_{t}(i)\longleftarrow\tilde{\mathbf{M}}_{t}(i)+w_{t}(i)\,\mathbf{a}_{t}.
$$  

Once again, the order in which the adds are performed by multiple heads is irrelevant. The combined erase and add operations of all the write heads produces the ﬁnal content of the memory at time    $t$  . Since both erase and add are differentiable, the composite write oper- ation is differentiable too. Note that both the erase and add vectors have    $M$   independent components, allowing ﬁne-grained control over which elements in each memory location are modiﬁed.  

![](images/7cfdf9dd5b4be82a9f5f9524e1530d4e976112d8f0780b5857c4e0ef177ce63d.jpg)  
Figure 2: Flow Diagram of the Addressing Mechanism. The  key vector ,    $\mathbf{k}_{t}$  , and  key strength ,    $\beta_{t}$  , are used to perform content-based addressing of the memory matrix,    $\mathbf{M}_{t}$  . The resulting content-based weighting is interpolated with the weighting from the previous time step based on the value of the  interpolation gate ,    $g_{t}$  . The  shift weighting ,    $\mathbf{s}_{t}$  , determines whether and by how much the weighting is rotated. Finally, depending on  $\gamma_{t}$  , the weighting is sharpened and used for memory access.  

# 3.3 Addressing Mechanisms  

Although we have now shown the equations of reading and writing, we have not described how the weightings are produced. These weightings arise by combining two addressing mechanisms with complementary facilities. The ﬁrst mechanism, “content-based address- ing,” focuses attention on locations based on the similarity between their current values and values emitted by the controller. This is related to the content-addressing of Hopﬁeld networks (Hopﬁeld, 1982). The advantage of content-based addressing is that retrieval is simple, merely requiring the controller to produce an approximation to a part of the stored data, which is then compared to memory to yield the exact stored value.  

However, not all problems are well-suited to content-based addressing. In certain tasks the content of a variable is arbitrary, but the variable still needs a recognisable name or ad- dress. Arithmetic problems fall into this category: the variable  $x$   and the variable  $y$   can take on any two values, but the procedure    $f(x,y)=x\times y$   should still be deﬁned. A controller for this task could take the values of the variables  $x$   and  $y$  , store them in different addresses, then retrieve them and perform a multiplication algorithm. In this case, the variables are addressed by location, not by content. We call this form of addressing “location-based ad- dressing.” Content-based addressing is strictly more general than location-based addressing as the content of a memory location could include location information inside it. In our ex- periments however, providing location-based addressing as a primitive operation proved essential for some forms of generalisation, so we employ both mechanisms concurrently.  

Figure 2 presents a ﬂow diagram of the entire addressing system that shows the order of operations for constructing a weighting vector when reading or writing.  

# 3.3.1 Focusing by Content  

For content-addressing, each head (whether employed for reading or writing) ﬁrst produces a length    $M$   key vector    $\mathbf{k}_{t}$   that is compared to each vector  $\mathbf{M}_{t}(i)$   by a similarity measure  $K[\cdot,\cdot]$    . The content-based system produces a normalised weighting    $w_{t}^{c}$    based on the sim- ilarity and a positive  key strength ,  $\beta_{t}$  , which can amplify or attenuate the precision of the focus:  

$$
w_{t}^{c}(i)\ \ \longleftarrow\ \ \frac{\exp\bigg(\beta_{t}K\big[{\bf k}_{t},{\bf M}_{t}(i)\big]\bigg)}{\sum_{j}\exp\bigg(\beta_{t}K\big[{\bf k}_{t},{\bf M}_{t}(j)\big]\bigg)}.
$$  

In our current implementation, the similarity measure is cosine similarity:  

$$
K[\mathbf{u},\mathbf{v}]\;\;=\;\;\frac{\mathbf{u}\cdot\mathbf{v}}{||\mathbf{u}||\cdot||\mathbf{v}||}.
$$  

# 3.3.2 Focusing by Location  

The location-based addressing mechanism is designed to facilitate both simple iteration across the locations of the memory and random-access jumps. It does so by implementing a rotational shift of a weighting. For example, if the current weighting focuses entirely on a single location, a rotation of  1  would shift the focus to the next location. A negative shift would move the weighting in the opposite direction.  

Prior to rotation, each head emits a scalar  interpolation gate    $g_{t}$   in the range    $(0,1)$  . The value of  $g$   is used to blend between the weighting  ${\bf w}_{t-1}$   produced by the head at the previous time-step and the weighting    $\mathbf{w}_{t}^{c}$    produced by the content system at the current time-step, yielding the  gated weighting    $\mathbf{w}_{t}^{g}$  :  

$$
\mathbf{w}_{t}^{g}\longleftarrow g_{t}\mathbf{w}_{t}^{c}+(1-g_{t})\mathbf{w}_{t-1}.
$$  

If the gate is zero, then the content weighting is entirely ignored, and the weighting from the previous time step is used. Conversely, if the gate is one, the weighting from the previous iteration is ignored, and the system applies content-based addressing.  

After interpolation, each head emits a  shift weighting  $\mathbf{s}_{t}$   that deﬁnes a normalised distri- bution over the allowed integer shifts. For example, if shifts between  $^{-1}$   and 1 are allowed,  $\mathbf{s}_{t}$   has three elements corresponding to the degree to which shifts of -1, 0 and 1 are per- formed. The simplest way to deﬁne the shift weightings is to use a softmax layer of the appropriate size attached to the controller. We also experimented with another technique, where the controller emits a single scalar that is interpreted as the lower bound of a width one uniform distribution over shifts. For example, if the shift scalar is 6.7, then    $s_{t}(6)=0.3$  ,  $s_{t}(7)=0.7$  , and the rest of  $\mathbf{s}_{t}$   is zero.  

If we index the    $N$   memory locations from  0  to    $N-1$  , the rotation applied to    $\mathbf{w}_{t}^{g}$    by  $\mathbf{s}_{t}$  can be expressed as the following circular convolution:  

$$
\tilde{w}_{t}(i)\longleftarrow\sum_{j=0}^{N-1}w_{t}^{g}(j)\,s_{t}(i-j)
$$  

where all index arithmetic is computed modulo    $N$  . The convolution operation in Equa- tion (8) can cause leakage or dispersion of weightings over time if the shift weighting is not sharp. For example, if shifts of -1, 0 and 1 are given weights of 0.1, 0.8 and 0.1, the rotation will transform a weighting focused at a single point into one slightly blurred over three points. To combat this, each head emits one further scalar  $\gamma_{t}\geq1$   whose effect is to sharpen the ﬁnal weighting as follows:  

$$
w_{t}(i)\longleftarrow\frac{\tilde{w}_{t}(i)^{\gamma_{t}}}{\sum_{j}\tilde{w}_{t}(j)^{\gamma_{t}}}
$$  

The combined addressing system of weighting interpolation and content and location- based addressing can operate in three complementary modes. One, a weighting can be chosen by the content system without any modiﬁcation by the location system. Two, a weighting produced by the content addressing system can be chosen and then shifted. This allows the focus to jump to a location next to, but not on, an address accessed by content; in computational terms this allows a head to ﬁnd a contiguous block of data, then access a particular element within that block. Three, a weighting from the previous time step can be rotated without any input from the content-based addressing system. This allows the weighting to iterate through a sequence of addresses by advancing the same distance at each time-step.  

# 3.4 Controller Network  

The NTM architecture architecture described above has several free parameters, including the size of the memory, the number of read and write heads, and the range of allowed lo- cation shifts. But perhaps the most signiﬁcant architectural choice is the type of neural network used as the controller. In particular, one has to decide whether to use a recurrent or feedforward network. A recurrent controller such as LSTM has its own internal memory that can complement the larger memory in the matrix. If one compares the controller to the central processing unit in a digital computer (albeit with adaptive rather than predeﬁned instructions) and the memory matrix to RAM, then the hidden activations of the recurrent controller are akin to the registers in the processor. They allow the controller to mix infor- mation across multiple time steps of operation. On the other hand a feedforward controller can mimic a recurrent network by reading and writing at the same location in memory at every step. Furthermore, feedforward controllers often confer greater transparency to the network’s operation because the pattern of reading from and writing to the memory matrix is usually easier to interpret than the internal state of an RNN. However, one limitation of a feedforward controller is that the number of concurrent read and write heads imposes a bottleneck on the type of computation the NTM can perform. With a single read head, it can perform only a unary transform on a single memory vector at each time-step, with two read heads it can perform binary vector transforms, and so on. Recurrent controllers can internally store read vectors from previous time-steps, so do not suffer from this limitation.  

# 4 Experiments  

This section presents preliminary experiments on a set of simple algorithmic tasks such as copying and sorting data sequences. The goal was not only to establish that NTM is able to solve the problems, but also that it is able to do so by learning compact internal programs. The hallmark of such solutions is that they generalise well beyond the range of the training data. For example, we were curious to see if a network that had been trained to copy sequences of length up to 20 could copy a sequence of length 100 with no further training.  

For all the experiments we compared three architectures: NTM with a feedforward controller, NTM with an LSTM controller, and a standard LSTM network. Because all the tasks were episodic, we reset the dynamic state of the networks at the start of each input sequence. For the LSTM networks, this meant setting the previous hidden state equal to a learned bias vector. For NTM the previous state of the controller, the value of the previous read vectors, and the contents of the memory were all reset to bias values. All the tasks were supervised learning problems with binary targets; all networks had logistic sigmoid output layers and were trained with the cross-entropy objective function. Sequence prediction errors are reported in bits-per-sequence. For more details about the experimental parameters see Section 4.6.  

# 4.1 Copy  

The copy task tests whether NTM can store and recall a long sequence of arbitrary in- formation. The network is presented with an input sequence of random binary vectors followed by a delimiter ﬂag. Storage and access of information over long time periods has always been problematic for RNNs and other dynamic architectures. We were particularly interested to see if an NTM is able to bridge longer time delays than LSTM.  

The networks were trained to copy sequences of eight bit random vectors, where the sequence lengths were randomised between 1 and 20. The target sequence was simply a copy of the input sequence (without the delimiter ﬂag). Note that no inputs were presented to the network while it receives the targets, to ensure that it recalls the entire sequence with no intermediate assistance.  

As can be seen from Figure 3, NTM (with either a feedforward or LSTM controller) learned much faster than LSTM alone, and converged to a lower cost. The disparity be- tween the NTM and LSTM learning curves is dramatic enough to suggest a qualitative,  

![](images/8953b0d721cdff7f80b7a9f54cd8f711e6972bbfd61f67e5463ac5d396fc8e63.jpg)  

# Figure 3: Copy Learning Curves.  

rather than quantitative, difference in the way the two models solve the problem.  

We also studied the ability of the networks to generalise to longer sequences than seen during training (that they can generalise to novel vectors is clear from the training error). Figures 4 and 5 demonstrate that the behaviour of LSTM and NTM in this regime is rad- ically different. NTM continues to copy as the length increases 2 , while LSTM rapidly degrades beyond length 20.  

The preceding analysis suggests that NTM, unlike LSTM, has learned some form of copy algorithm. To determine what this algorithm is, we examined the interaction between the controller and the memory (Figure 6). We believe that the sequence of operations per- formed by the network can be summarised by the following pseudocode:  

initialise:  move head to start location while  input delimiter not seen  do receive input vector write input to head location increment head location by 1 end while return head to start location while  true  do read output vector from head location emit output increment head location by 1 end while  

This is essentially how a human programmer would perform the same task in a low-  

![](images/5ef5f6235a13ed28d8d8ca6f03667169c925f5a99110fd7d853bc88812215c04.jpg)  
Figure 4: NTM Generalisation on the Copy Task.  The four pairs of plots in the top row depict network outputs and corresponding copy targets for test sequences of length 10, 20, 30, and 50, respectively. The plots in the bottom row are for a length 120 sequence. The network was only trained on sequences of up to length 20. The ﬁrst four sequences are reproduced with high conﬁdence and very few mistakes. The longest one has a few more local errors and one global error: at the point indicated by the red arrow at the bottom, a single vector is duplicated, pushing all subsequent vectors one step back. Despite being subjectively close to a correct copy, this leads to a high loss.  

level programming language. In terms of data structures, we could say that NTM has learned how to create and iterate through arrays. Note that the algorithm combines both content-based addressing (to jump to start of the sequence) and location-based address- ing (to move along the sequence). Also note that the iteration would not generalise to long sequences without the ability to use relative shifts from the  previous  read and write weightings (Equation 7), and that without the focus-sharpening mechanism (Equation 9) the weightings would probably lose precision over time.  

# 4.2 Repeat Copy  

The repeat copy task extends copy by requiring the network to output the copied sequence a speciﬁed number of times and then emit an end-of-sequence marker. The main motivation was to see if the NTM could learn a simple nested function. Ideally, we would like it to be able to execute a “for loop” containing any subroutine it has already learned.  

The network receives random-length sequences of random binary vectors, followed by a scalar value indicating the desired number of copies, which appears on a separate input channel. To emit the end marker at the correct time the network must be both able to interpret the extra input and keep count of the number of copies it has performed so far. As with the copy task, no inputs are provided to the network after the initial sequence and repeat number. The networks were trained to reproduce sequences of size eight random binary vectors, where both the sequence length and the number of repetitions were chosen randomly from one to ten. The input representing the repeat number was normalised to have mean zero and variance one.  

![](images/b23e6904a21abe54f0572f3f81f3a899f83650af99b3d6f3ca4d4dd2f0698a4c.jpg)  
Figure 5: LSTM Generalisation on the Copy Task.  The plots show inputs and outputs for the same sequence lengths as Figure 4. Like NTM, LSTM learns to reproduce sequences of up to length 20 almost perfectly. However it clearly fails to generalise to longer sequences. Also note that the length of the accurate preﬁx decreases as the sequence length increases, suggesting that the network has trouble retaining information for long periods.  

![](images/30d4e1a4433a1a2abd2fa99480ade3f53b5480eeb3f57e31597be574c5281077.jpg)  

Figure 6: NTM Memory Use During the Copy Task.  The plots in the left column depict the inputs to the network (top), the vectors added to memory (middle) and the corresponding write weightings (bottom) during a single test sequence for the copy task. The plots on the right show the outputs from the network (top), the vectors read from memory (middle) and the read weightings (bottom). Only a subset of memory locations are shown. Notice the sharp focus of all the weightings on a single location in memory (black is weight zero, white is weight one). Also note the translation of the focal point over time, reﬂects the network’s use of iterative shifts for location-based addressing, as described in Section 3.3.2. Lastly, observe that the read locations exactly match the write locations, and the read vectors match the add vectors. This suggests that the network writes each input vector in turn to a speciﬁc memory location during the input phase, then reads from the same location sequence during the output phase.  

![](images/023b022f72c90b36c921ab2b1ef7cb9dfa8ce7f1786d6c05b9c9298e0a3716b3.jpg)  

Figure 7 shows that NTM learns the task much faster than LSTM, but both were able to solve it perfectly.   The difference between the two architectures only becomes clear when they are asked to generalise beyond the training data. In this case we were interested in generalisation along two dimensions: sequence length and number of repetitions. Figure 8 illustrates the effect of doubling ﬁrst one, then the other, for both LSTM and NTM. Whereas LSTM fails both tests, NTM succeeds with longer sequences and is able to perform more than ten repetitions; however it is unable to keep count of of how many repeats it has completed, and does not predict the end marker correctly. This is probably a consequence of representing the number of repetitions numerically, which does not easily generalise beyond a ﬁxed range.  

Figure 9 suggests that NTM learns a simple extension of the copy algorithm in the previous section, where the sequential read is repeated as many times as necessary.  

# 4.3 Associative Recall  

The previous tasks show that the NTM can apply algorithms to relatively simple, linear data structures. The next order of complexity in organising data arises from “indirection”—that is, when one data item points to another. We test the NTM’s capability for learning an instance of this more interesting class by constructing a list of items so that querying with one of the items demands that the network return the subsequent item. More speciﬁcally, we deﬁne an item as a sequence of binary vectors that is bounded on the left and right by delimiter symbols. After several items have been propagated to the network, we query by showing a random item, and we ask the network to produce the next item. In our experiments, each item consisted of three six-bit binary vectors (giving a total of 18 bits  

![](images/7463bcfdc88b3a61fd66666d4bf4c97726d5db389166cf239b9f0e3e1577bdc4.jpg)  
Figure 8: NTM and LSTM Generalisation for the Repeat Copy Task.  NTM generalises almost perfectly to longer sequences than seen during training. When the number of repeats is increased it is able to continue duplicating the input sequence fairly accurately; but it is unable to predict when the sequence will end, emitting the end marker after the end of every repetition beyond the eleventh. LSTM struggles with both increased length and number, rapidly diverging from the input sequence in both cases.  

per item). During training, we used a minimum of 2 items and a maximum of 6 items in a single episode.  

Figure 10 shows that NTM learns this task signiﬁcantly faster than LSTM, terminating at near zero cost within approximately  30 ,  000  episodes, whereas LSTM does not reach zero cost after a million episodes. Additionally, NTM with a feedforward controller learns faster than NTM with an LSTM controller. These two results suggest that NTM’s external memory is a more effective way of maintaining the data structure than LSTM’s internal state. NTM also generalises much better to longer sequences than LSTM, as can be seen in Figure 11. NTM with a feedforward controller is nearly perfect for sequences of up to 12 items (twice the maximum length used in training), and still has an average cost below 1 bit per sequence for sequences of 15 items.  

In Figure 12, we show the operation of the NTM memory, controlled by an LSTM with one head, on a single test episode. In “Inputs,” we see that the input denotes item delimiters as single bits in row 7. After the sequence of items has been propagated, a  

![](images/bd7e6be6ea4734d87c4e0e2e75c33e1feec0aac85e87b2de82551dfbef3ab89e.jpg)  

Figure 9: NTM Memory Use During the Repeat Copy Task.  As with the copy task the network ﬁrst writes the input vectors to memory using iterative shifts. It then reads through the sequence to replicate the input as many times as necessary (six in this case). The white dot at the bottom of the read weightings seems to correspond to an intermediate location used to redirect the head to the start of the sequence (The NTM equivalent of a  goto  statement).  

![](images/1ff31e1feb3b4521c97170217fb4fa8862d01a545405ea078eb7f7b3f15276e6.jpg)  
sequence number (thousands)  

![](images/cd6dcae6100b3d47a48224abd37320a24a55fccf6599e7e5e22bab864d9ceb90.jpg)  
number of items per sequence  

delimiter in row 8 prepares the network to receive a query item. In this case, the query item corresponds to the second item in the sequence (contained in the green box). In “Outputs,” we see that the network crisply outputs item 3 in the sequence (from the red box). In “Read Weightings,” on the last three time steps, we see that the controller reads from contiguous locations that each store the time slices of item 3. This is curious because it appears that the network has jumped directly to the correct location storing item 3. However we can explain this behaviour by looking at “Write Weightings.” Here we see that the memory is written to even when the input presents a delimiter symbol between items. One can conﬁrm in “Adds” that data are indeed written to memory when the delimiters are presented (e.g., the data within the black box); furthermore, each time a delimiter is presented, the vector added to memory is different. Further analysis of the memory reveals that the network accesses the location it reads after the query by using a content-based lookup that produces a weighting that is shifted by one. Additionally, the key used for content-lookup corresponds to the vector that was added in the black box. This implies the following memory-access algorithm: when each item delimiter is presented, the controller writes a compressed representation of the previous three time slices of the item. After the query arrives, the controller recomputes the same compressed representation of the query item, uses a content-based lookup to ﬁnd the location where it wrote the ﬁrst representation, and then shifts by one to produce the subsequent item in the sequence (thereby combining content-based lookup with location-based offsetting).  

# 4.4 Dynamic N-Grams  

The goal of the dynamic N-Grams task was to test whether NTM could rapidly adapt to new predictive distributions. In particular we were interested to see if it were able to use its  

![](images/dc2df148125aa6eb41f8daf8ef6784ce8a211d78b204864eeb4230134e7433e0.jpg)  

Figure 12: NTM Memory Use During the Associative Recall Task.  In “Inputs,” a se- quence of items, each composed of three consecutive binary random vectors is propagated to the controller. The distinction between items is designated by delimiter symbols (row 7 in “Inputs”). After several items have been presented, a delimiter that designates a query is presented (row 8 in “Inputs”). A single query item is presented (green box), and the network target corresponds to the subsequent item in the sequence (red box). In “Outputs,” we see that the network cor- rectly produces the target item. The red boxes in the read and write weightings highlight the three locations where the target item was written and then read. The solution the network ﬁnds is to form a compressed representation (black box in “Adds”) of each item that it can store in a single location. For further analysis, see the main text.  

memory as a re-writable table that it could use to keep count of transition statistics, thereby emulating a conventional N-Gram model.  

We considered the set of all possible 6-Gram distributions over binary sequences. Each 6-Gram distribution can be expressed as a table of    $2^{5}=32$   numbers, specifying the prob- ability that the next bit will be one, given all possible length ﬁve binary histories. For each training example, we ﬁrst generated random 6-Gram probabilities by independently drawing all  32  probabilities from the    $B e t a(\textstyle\frac{1}{2},\textstyle\frac{1}{2})$   distribution.  

We then generated a particular training sequence by drawing 200 successive bits using the current lookup table.   The network observes the sequence one bit at a time and is then asked to predict the next bit. The optimal estimator for the problem can be determined by  

![](images/21bf3512f189bdc2f538eb38fdfc276d806716521aa740dbbc06f66000777c64.jpg)  

# Figure 13: Dynamic N-Gram Learning Curves.  

Bayesian analysis (Murphy, 2012):  

$$
P(B=1|N_{1},N_{0},\mathbf{c})=\frac{N_{1}+\frac{1}{2}}{N_{1}+N_{0}+1}
$$  

where  c  is the ﬁve bit previous context,    $B$   is the value of the next bit and    $N_{0}$   and    $N_{1}$   are respectively the number of zeros and ones observed after  c  so far in the sequence. We can therefore compare NTM to the optimal predictor as well as LSTM. To assess performance we used a validation set of 1000 length 200 sequences sampled from the same distribu- tion as the training data. As shown in Figure 13, NTM achieves a small, but signiﬁcant performance advantage over LSTM, but never quite reaches the optimum cost.  

The evolution of the two architecture’s predictions as they observe new inputs is shown in Figure 14, along with the optimal predictions. Close analysis of NTM’s memory usage (Figure 15) suggests that the controller uses the memory to count how many ones and zeros it has observed in different contexts, allowing it to implement an algorithm similar to the optimal estimator.  

# 4.5 Priority Sort  

This task tests whether the NTM can sort data—an important elementary algorithm. A sequence of random binary vectors is input to the network along with a scalar priority rating for each vector. The priority is drawn uniformly from the range [-1, 1]. The target sequence contains the binary vectors sorted according to their priorities, as depicted in Figure 16.  

Each input sequence contained 20 binary vectors with corresponding priorities, and each target sequence was the 16 highest-priority vectors in the input.   Inspection of NTM’s  

#  

Figure 14: Dynamic N-Gram Inference.  The top row shows a test sequence from the N-Gram task, and the rows below show the corresponding predictive distributions emitted by the optimal estimator, NTM, and LSTM. In most places the NTM predictions are almost indistinguishable from the optimal ones. However at the points indicated by the two arrows it makes clear mistakes, one of which is explained in Figure 15. LSTM follows the optimal predictions closely in some places but appears to diverge further as the sequence progresses; we speculate that this is due to LSTM “forgetting” the observations at the start of the sequence.  

![](images/5126566c56312b6d61a24793f869fdcb59e4ddec63822207064d7186e288f08a.jpg)  

Figure 15: NTM Memory Use During the Dynamic N-Gram Task.  The red and green arrows indicate point where the same context is repeatedly observed during the test sequence  $(\,^{"}00010^{"}$   for the green arrows, “01111” for the red arrows). At each such point the same location is accessed by the read head, and then, on the next time-step, accessed by the write head. We postulate that the network uses the writes to keep count of the fraction of ones and zeros following each context in the sequence so far. This is supported by the add vectors, which are clearly anti-correlated at places where the input is one or zero, suggesting a distributed “counter.” Note that the write weightings grow fainter as the same context is repeatedly seen; this may be because the memory records a ratio of ones to zeros, rather than absolute counts. The red box in the prediction sequence corresponds to the mistake at the ﬁrst red arrow in Figure 14; the controller appears to have accessed the wrong memory location, as the previous context was “01101” and not “01111.”  

#  

![](images/ae85e91d4a1c0449a58cac68721013138303c5c7c5e67ea5b238a052836fef54.jpg)  
Figure 16: Example Input and Target Sequence for the Priority Sort Task.  The input sequence contains random binary vectors and random scalar priorities. The target sequence is a subset of the input vectors sorted by the priorities.  

memory use led us to hypothesise that it uses the priorities to determine the relative location of each write. To test this hypothesis we ﬁtted a linear function of the priority to the observed write locations. Figure 17 shows that the locations returned by the linear function closely match the observed write locations. It also shows that the network reads from the memory locations in increasing order, thereby traversing the sorted sequence.  

The learning curves in Figure 18 demonstrate that NTM with both feedforward and LSTM controllers substantially outperform LSTM on this task. Note that eight parallel read and write heads were needed for best performance with a feedforward controller on this task; this may reﬂect the difﬁculty of sorting vectors using only unary vector operations (see Section 3.4).  

# 4.6 Experimental Details  

For all experiments, the  RMSProp  algorithm was used for training in the form described in (Graves, 2013) with momentum of 0.9. Tables 1 to 3 give details about the network conﬁgurations and learning rates used in the experiments. All LSTM networks had three stacked hidden layers. Note that the number of LSTM parameters grows quadratically with  

![](images/83a6347c1006dec23cf6a34fca17717364a7a54ee8735c09b842e25e0f6f58f9.jpg)  

![Table 1: NTM with Feedforward Controller Experimental Settings ](images/ee93f42d6559efe61ce60f6c6e8c990ae5c5602c77f302e8fa6b92158b3273c1.jpg)  

the number of hidden units (due to the recurrent connections in the hidden layers). This contrasts with NTM, where the number of parameters does not increase with the number of memory locations. During the training backward pass, all gradient components are clipped elementwise to the range (-10, 10).  

# 5 Conclusion  

We have introduced the Neural Turing Machine, a neural network architecture that takes inspiration from both models of biological working memory and the design of digital com- puters. Like conventional neural networks, the architecture is differentiable end-to-end and can be trained with gradient descent. Our experiments demonstrate that it is capable of learning simple algorithms from example data and of using these algorithms to generalise well outside its training regime.  

![Table 2: NTM with LSTM Controller Experimental Settings ](images/670c4f8f9e60c2265a0fb7a8a4aa7c73cbe4e0da8d6183e984b37f507e507a92.jpg)  

![Table 3: LSTM Network Experimental Settings ](images/1073738203c00bd7036ed2c4c8f611bf239f352c55f0e39cb0f1da4636ffe8ac.jpg)  

# 6 Acknowledgments  

Many have offered thoughtful insights, but we would especially like to thank Daan Wier- stra, Peter Dayan, Ilya Sutskever, Charles Blundell, Joel Veness, Koray Kavukcuoglu, Dharshan Kumaran, Georg Ostrovski, Chris Summerﬁeld, Jeff Dean, Geoffrey Hinton, and Demis Hassabis.  

# References  

Baddeley, A., Eysenck, M., and Anderson, M. (2009).  Memory . Psychology Press.  

Bahdanau, D., Cho, K., and Bengio, Y. (2014). Neural machine translation by jointly learning to align and translate. abs/1409.0473.  

Barrouillet, P., Bernardin, S., and Camos, V. (2004). Time constraints and resource shar- ing in adults’ working memory spans.  Journal of Experimental Psychology: General , 133(1):83.  

Chomsky, N. (1956). Three models for the description of language.  Information Theory, IEEE Transactions on , 2(3):113–124.  

Das, S., Giles, C. L., and Sun, G.-Z. (1992). Learning context-free grammars: Capabil- ities and limitations of a recurrent neural network with an external stack memory. In Proceedings of The Fourteenth Annual Conference of Cognitive Science Society. Indiana University .  

Dayan, P. (2008). Simple substrates for complex cognition.  Frontiers in neuroscience , 2(2):255.  

Eliasmith, C. (2013).  How to build a brain: A neural architecture for biological cognition . Oxford University Press.  

Fitch, W., Hauser, M. D., and Chomsky, N. (2005). The evolution of the language faculty: clariﬁcations and implications.  Cognition , 97(2):179–210.  

Fodor, J. A. and Pylyshyn, Z. W. (1988). Connectionism and cognitive architecture: A critical analysis.  Cognition , 28(1):3–71.  

Frasconi, P., Gori, M., and Sperduti, A. (1998). A general framework for adaptive process- ing of data structures.  Neural Networks, IEEE Transactions on , 9(5):768–786.  

Gallistel, C. R. and King, A. P. (2009).  Memory and the computational brain: Why cogni- tive science will transform neuroscience , volume 3. John Wiley & Sons.  

Goldman-Rakic, P. S. (1995). Cellular basis of working memory.  Neuron , 14(3):477–485.  

Graves, A. (2013). Generating sequences with recurrent neural networks.  arXiv preprint arXiv:1308.0850 .  

Graves, A. and Jaitly, N. (2014). Towards end-to-end speech recognition with recurrent neural networks. In  Proceedings of the 31st International Conference on Machine Learn- ing (ICML-14) , pages 1764–1772.  

Graves, A., Mohamed, A., and Hinton, G. (2013). Speech recognition with deep recurrent neural networks. In  Acoustics, Speech and Signal Processing (ICASSP), 2013 IEEE International Conference on , pages 6645–6649. IEEE. Hadley, R. F. (2009). The problem of rapid variable creation. Neural computation , 21(2):510–532. Hazy, T. E., Frank, M. J., and O’Reilly, R. C. (2006). Banishing the homunculus: making working memory work.  Neuroscience , 139(1):105–118. Hinton, G. E. (1986). Learning distributed representations of concepts. In  Proceedings of the eighth annual conference of the cognitive science society , volume 1, page 12. Amherst, MA. Hochreiter, S., Bengio, Y., Frasconi, P., and Schmidhuber, J. (2001a). Gradient ﬂow in recurrent nets: the difﬁculty of learning long-term dependencies. Hochreiter, S. and Schmidhuber, J. (1997). Long short-term memory.  Neural computation , 9(8):1735–1780. Hochreiter, S., Younger, A. S., and Conwell, P. R. (2001b). Learning to learn using gradient descent. In  Artiﬁcial Neural Networks?ICANN 2001 , pages 87–94. Springer. Hopﬁeld, J. J. (1982). Neural networks and physical systems with emergent collective computational abilities.  Proceedings of the national academy of sciences , 79(8):2554– 2558. Jackendoff, R. and Pinker, S. (2005). The nature of the language faculty and its implications for evolution of language (reply to ﬁtch, hauser, and chomsky).  Cognition , 97(2):211– 225. Kanerva, P. (2009). Hyper dimensional computing: An introduction to computing in dis- tributed representation with high-dimensional random vectors.  Cognitive Computation , 1(2):139–159. Marcus, G. F. (2003).  The algebraic mind: Integrating connectionism and cognitive sci- ence . MIT press. Miller, G. A. (1956). The magical number seven, plus or minus two: some limits on our capacity for processing information.  Psychological review , 63(2):81. Miller, G. A. (2003). The cognitive revolution: a historical perspective.  Trends in cognitive sciences , 7(3):141–144. Minsky, M. L. (1967).  Computation: ﬁnite and inﬁnite machines . Prentice-Hall, Inc.  

Murphy, K. P. (2012).  Machine learning: a probabilistic perspective . MIT press.  

Plate, T. A. (2003).  Holographic Reduced Representation: Distributed representation for cognitive structures . CSLI.  

Pollack, J. B. (1990). Recursive distributed representations. Artiﬁcial Intelligence , 46(1):77–105.  

Rigotti, M., Barak, O., Warden, M. R., Wang, X.-J., Daw, N. D., Miller, E. K., and Fusi, S. (2013). The importance of mixed selectivity in complex cognitive tasks.  Nature , 497(7451):585–590.  

Rumelhart, D. E., McClelland, J. L., Group, P. R., et al. (1986).  Parallel distributed pro- cessing , volume 1. MIT press.  

Seung, H. S. (1998). Continuous attractors and oculomotor control.  Neural Networks , 11(7):1253–1258.  

Siegelmann, H. T. and Sontag, E. D. (1995). On the computational power of neural nets. Journal of computer and system sciences , 50(1):132–150.  

Smolensky, P. (1990). Tensor product variable binding and the representation of symbolic structures in connectionist systems.  Artiﬁcial intelligence , 46(1):159–216.  

Socher, R., Huval, B., Manning, C. D., and Ng, A. Y. (2012). Semantic compositional it y through recursive matrix-vector spaces. In  Proceedings of the 2012 Joint Conference on Empirical Methods in Natural Language Processing and Computational Natural Lan- guage Learning , pages 1201–1211. Association for Computational Linguistics.  

Sutskever, I., Martens, J., and Hinton, G. E. (2011). Generating text with recurrent neural networks. In  Proceedings of the 28th International Conference on Machine Learning (ICML-11) , pages 1017–1024.  

Sutskever, I., Vinyals, O., and Le, Q. V. (2014). Sequence to sequence learning with neural networks.  arXiv preprint arXiv:1409.3215 .  

Touretzky, D. S. (1990). Boltzcons: Dynamic symbol structures in a connectionist network. Artiﬁcial Intelligence , 46(1):5–46.  

Von Neumann, J. (1945). First draft of a report on the edvac.  

Wang, X.-J. (1999). Synaptic basis of cortical persistent activity: the importance of nmda receptors to working memory.  The Journal of Neuroscience , 19(21):9587–9603.  