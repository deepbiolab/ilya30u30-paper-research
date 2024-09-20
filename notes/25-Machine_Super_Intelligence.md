# Machine Super Intelligence  

Doctoral Dissertation submitted to the Faculty of Informatics of the University of Lugano in partial fulﬁllment of the requirements for the degree of Doctor of Philosophy  

presented by Shane Legg  

under the supervision of Prof. Dr. Marcus Hutter  

# Copyright ©  Shane Legg 2008  

This document is licensed under a Creative Commons Attribution-Share Alike 2.5 Switzerland License.  

# Dissertation Committee  

Prof. Dr. Marcus Hutter Prof. Dr. J¨ urgen Schmidhuber Prof. Dr. Fernando Pedone Prof. Dr. Matthias Hauswirth Prof. Dr. Marco Wiering  

Australian National University, Australia IDSIA, Switzerland Technical University of Munich, Germany University of Lugano, Switzerland University of Lugano, Switzerland Utrecht University, The Netherlands  

PhD program director Prof. Dr. Fabio Crestani  

# Contents  

# Preface  

Thesis outline . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . iii Prerequisite knowledge . . . . . . . . . . . . . . . . . . . . . . . . . . vi Acknowledgements . . . . . . . . . . . . . . . . . . . . . . . . . . . . vi  

# 1. Nature and Measurement of Intelligence 1  

1.1. Theories of intelligence . . . . . . . . . . . . . . . . . . . . . . . 3

 1.2. Deﬁnitions of human intelligence . . . . . . . . . . . . . . . . . 4

 1.3. Deﬁnitions of machine intelligence . . . . . . . . . . . . . . . . 9

 1.4. Intelligence testing . . . . . . . . . . . . . . . . . . . . . . . . . 11

 1.5. Human intelligence tests . . . . . . . . . . . . . . . . . . . . . . 13

 1.6. Animal intelligence tests . . . . . . . . . . . . . . . . . . . . . . 15

 1.7. Machine intelligence tests . . . . . . . . . . . . . . . . . . . . . 16

 1.8. Conclusion . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 22  

# 2. Universal Artiﬁcial Intelligence 23  

2.1. Inductive inference . . . . . . . . . . . . . . . . . . . . . . . . . 23

 2.2. Bayes’ rule . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 25

 2.3. Binary sequence prediction . . . . . . . . . . . . . . . . . . . . 27

 2.4. Solomonoﬀ’s prior and Kolmogorov complexity . . . . . . . . . 30

 2.5. Solomonoﬀ-Levin prior . . . . . . . . . . . . . . . . . . . . . . . 32

 2.6. Universal inference . . . . . . . . . . . . . . . . . . . . . . . . . 36

 2.7. Solomon o induction . . . . . . . . . . . . . . . . . . . . . . . . 38

 2.8. Agent-environment model . . . . . . . . . . . . . . . . . . . . . 39

 2.9. Optimal informed agents . . . . . . . . . . . . . . . . . . . . . . 43

 2.10. Universal AIXI agent . . . . . . . . . . . . . . . . . . . . . . . . 47  

# 3. Taxonomy of Environments 53  

3.1. Passive environments . . . . . . . . . . . . . . . . . . . . . . . . 53

 3.2. Active environments . . . . . . . . . . . . . . . . . . . . . . . . 57

 3.3. Some common problem classes . . . . . . . . . . . . . . . . . . 60

 3.4. Ergodic MDPs . . . . . . . . . . . . . . . . . . . . . . . . . . . 62

 3.5. Environments that admit self-optimising agents . . . . . . . . . 65

 3.6. Conclusion . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 68  

# 4. Universal Intelligence Measure 71  

4.1. A formal deﬁnition of machine intelligence . . . . . . . . . . . . 72 4.2. Universal intelligence of various agents . . . . . . . . . . . . . . 78

 4.3. Properties of universal intelligence . . . . . . . . . . . . . . . . 83

 4.4. Response to common criticisms . . . . . . . . . . . . . . . . . . 86

 4.5. Conclusion . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 93  

# 5. Limits of Computational Agents  

# 95  

5.1. Preliminaries . . . . . . . . . . . . . . . . . . . . . . . . . . . . 96

 5.2. Prediction of computable sequences . . . . . . . . . . . . . . . . 98

 5.3. Prediction of simple computable sequences . . . . . . . . . . . . 100

 5.4. Complexity of prediction . . . . . . . . . . . . . . . . . . . . . . 102

 5.5. Hard to predict sequences . . . . . . . . . . . . . . . . . . . . . 103

 5.6. The limits of mathematical analysis . . . . . . . . . . . . . . . 104

 5.7. Conclusion . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 106  

# 6. Temporal Diﬀerence Updating without a Learning Rate 109  

6.1. Temporal diﬀerence learning . . . . . . . . . . . . . . . . . . . . 110

 6.2. Derivation . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 112

 6.3. Estimating a small Markov process . . . . . . . . . . . . . . . . 115

 6.4. A larger Markov process . . . . . . . . . . . . . . . . . . . . . . 117

 6.5. Random Markov process . . . . . . . . . . . . . . . . . . . . . . 118

 6.6. Non-stationary Markov process . . . . . . . . . . . . . . . . . . 119

 6.7. Windy Gridworld . . . . . . . . . . . . . . . . . . . . . . . . . . 120

 6.8. Conclusion . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 123  

# 7. Discussion 125  

7.1. Are super intelligent machines possible? . . . . . . . . . . . . . 126

 7.2. How could intelligent machines be developed? . . . . . . . . . . 128

 7.3. Is building intelligent machines a good idea? . . . . . . . . . . . 135  

# A. Notation and Conventions 139  

# B. Ergodic MDPs admit self-optimising agents 143  

B.1. Basic deﬁnitions . . . . . . . . . . . . . . . . . . . . . . . . . . 143 B.2. Analysis of stationary Markov chains . . . . . . . . . . . . . . . 146 B.3. An optimal stationary policy . . . . . . . . . . . . . . . . . . . 152 B.4. Convergence of expected average value . . . . . . . . . . . . . . 155  

# C. Deﬁnitions of Intelligence 159  

C.1. Collective deﬁnitions . . . . . . . . . . . . . . . . . . . . . . . . 159 C.2. Psychologist deﬁnitions . . . . . . . . . . . . . . . . . . . . . . 161 C.3. AI researcher deﬁnitions . . . . . . . . . . . . . . . . . . . . . . 164  

# Bibliography  

167  

![](images/15203919600496befd6ff0f1af148186c3e510daf7846c688ab4bae32de1a5c2.jpg)  

Mystics exult in mystery and want it to stay mysterious. Scientists exult in mystery for a diﬀerent reason: it gives them something to do.  

Richard Dawkins in  The God Delusion  

![](images/92c19ffc398f456166f75f0caffbd0d1a6447a481e896dbd74f7c1bbc8cd8730.jpg)  

# Preface  

This thesis concerns the optimal behaviour of agents in unknown computable environments, also known as  universal artiﬁcial intelligence . These theoretical agents are able to learn to perform optimally in many types of environments. Although they are able to optimally use prior information about the environ- ment if it is available, in many cases they also learn to perform optimally in the absence of such information. Moreover, these agents can be proven to upper bound the performance of general purpose computable agents. Clearly such agents are extremely powerful and general, hence the name  universal  artiﬁcial intelligence.  

That such agents can be mathematically deﬁned at all might come as a sur- prise to some. Surely then artiﬁcial intelligence has been solved? Not quite. The problem is that the theory behind these universal agents assumes inﬁnite computational resources. Although this greatly simpliﬁes the mathematical deﬁnitions and analysis, it also means that these models cannot be directly implemented as artiﬁcial intelligence algorithms. Eﬀorts have been made to scale these ideas down, however as yet none of these methods have produced practical algorithms that have been adopted by the mainstream. The main use of universal artiﬁcial intelligence theory thus far has been as a theoreti- cal tool with which to mathematically study the properties of machine super intelligence.  

The foundations of universal intelligence date back to the origins of philos- ophy and inductive inference. Universal artiﬁcial intelligence proper started with the work of Ray J. Solomonoﬀin the 1960’s. Solomonoﬀwas considering the problem of predicting binary sequences. What he discovered was a for- mulation for an inductive inference system that can be proven to very rapidly learn to optimally predict  any sequence  that has a computable probability distribution. Not only is this theory astonishingly powerful, it also brings to- gether and elegantly formalises key philosophical principles behind inductive inference. Furthermore, by considering special cases of Solomonoﬀ’s model, one can recover well known statistical principles such as maximum likelihood, minimum description length and maximum entropy. This makes Solomonoﬀ’s model a kind of grand uniﬁed theory of inductive inference. Indeed, if it were not for its incomputability, the problem of induction might be considered solved. Whatever practical concerns one may have about Solomonoﬀ’s model, most would agree that it is nonetheless a beautiful blend of mathematics and philosophy.  

The main theoretical limitation of Solomon o induction is that it only addresses the problem of passive inductive learning, in particular sequence prediction. Whether the agent’s predictions are correct or not has no eﬀect on the future observed sequence. Thus the agent is passive in the sense that it is unable to inﬂuence the future. An example of this might be predicting the movement of the planets across the sky, or maybe the stock market, assuming that one is not wealthy enough to inﬂuence the market.  

In the more general active case the agent is able to take actions which may aﬀect the observed future. For example, an agent playing chess not only observes the other player, it is also able to make moves itself in order to increase its chances of winning the game. This is a very general setting in which seemingly any kind of goal directed problem can be framed. It is not necessary to assume, as is typically done in game theory, that the environment, in this case other player, plays optimally. We also do not assume that the behaviour of the environment is Markovian, as is typically done in control theory and reinforcement learning.  

In the late 1990’s Marcus Hutter extended Solomonoﬀ’s passive induction model to the active case by combining it with sequential decision theory. This produced a theory of universal agents, and in particular a universal agent for a very general class of interactive environments, known as the  AIXI  agent. Hutter was able to prove that the behaviour of universal agents converges to optimal in any setting where this is at all possible for a general agent, and that these agents are Pareto optimal in the sense that no agent can perform as well in all environments and strictly better in at least one. These are the strongest known results for a completely general purpose agent. Given that AIXI  has such generality and extreme performance characteristics, it can be considered to be a theoretical model of a super intelligent agent.  

Unfortunately, even stronger results showing that  AIXI  converges to optimal behaviour rapidly, similar to Solomonoﬀ’s convergence result, have been shown to be impossible in some settings, and remain open questions in others. Indeed, many questions about universal artiﬁcial intelligence remain open. In part this is because the area is quite new with few people working in it, and partly because proving results about universal intelligent agents seems to be diﬃcult.  

The goal of this thesis is to explore some of the open issues surrounding universal artiﬁcial intelligence. In particular: In which settings the behaviour of universal agents converges to optimal, the way in which  AIXI  theory relates to the concept and deﬁnition of intelligence, the limitations that computable agents face when trying to approximate theoretical super intelligent agents such as  AIXI , and ﬁnally some of the big picture implications of super intelli- gent machines and whether this is a topic that deserves greater study.  

# Thesis outline  

Much of the work presented in this thesis comes from prior publications. In some cases whole chapters are heavily based on prior publications, in other cases prior work is only mentioned in passing. Furthermore, while I wrote the text of the thesis, naturally not all of the ideas and work presented are my own. Besides the presented background material, many of the results and ideas in this thesis have been developed through collaboration with various colleagues, in particular my supervisor Marcus Hutter. This section outlines the contents of the thesis and also provides some guidance on the nature of my contribution to each chapter.  

1) Nature and Measurement of Intelligence. Chapter 1 begins the thesis with the most fundamental question of all: What is intelligence? Amazingly, books and papers on artiﬁcial intelligence rarely delve into what intelligence actually is, or what artiﬁcial intelligence is trying to achieve. When they do address the topic they usually just mention the Turing test and that the concept of intelligence is poorly deﬁned, before moving on to algorithms that presumably have this mysterious quality. As this thesis concerns theoretical models of systems that we claim to be extremely intelligent, we must ﬁrst ex- plore the diﬀerent tests and deﬁnitions of intelligence that have been proposed for humans, animals and machines. We draw from these an informal deﬁnition of intelligence that we will use throughout the rest of the thesis.  

This overview of the theory, deﬁnition and testing of intelligence is my own work. This chapter is based on (Legg and Hutter, 2007c), in particular the parts which built upon (Legg and Hutter 2007b; 2007a).  

2) Universal Artiﬁcial Intelligence. At present  AIXI  is not widely known in academic circles, though it has captured the imagination of a community in- terested in new approaches to general purpose artiﬁcial intelligence, so called artiﬁcial general intelligence  (AGI). However even within this community, it is clear that there is some confusion about  AIXI  and universal artiﬁcial intelli- gence. This may be attributable in part to the fact that current expositions of AIXI  are diﬃcult for non-mathematicians to digest. As such, a less technical introduction to the subject would be helpful. Not only should this help clear up some misconceptions, it may also serve as an appetiser for the more techni- cal treatments that have been published by Hutter. Chapter 2 provides such an introduction. It starts with the basics of inductive inference and slowly builds up to the  AIXI  agent and its key theoretical properties.  

This introduction to universal artiﬁcial intelligence has not been published before, though small parts of it were derived from (Hutter et al., 2007) and (Legg, 1997). Section 2.6 is largely based on the material in (Hutter, 2007a), and the sections that follow this on (Hutter, 2005).  

3) Optimality of AIXI. Hutter has proven that universal agents converge to optimal behaviour in any environment where this is possible for a general agent. He further showed that the result holds for certain types of Markov decision processes, and claimed that this should generalise to related classes of environments. Formally deﬁning these environments and identifying the additional conditions for the convergence result to hold was left as an open problem. Indeed, it seems that nobody has ever documented the many abstract environment classes that are studied and formally shown how they are related to each other. In Chapter 3 we create such a taxonomy and identify the environment classes in which universal agents are able to learn to behave optimally. The diversity of these classes of environments adds weight to our claim that  AIXI  is super intelligent.  

Most of the classes of environments are well known, though their exact for- malisations as presented are my own. The proofs of the relationships between them and the resulting taxonomy of environment classes is my work. This chapter is largely based on (Legg and Hutter, 2004).  

4) Universal Intelligence Measure. If  AIXI  really is an optimally intelligent machine, this suggests that we may be able to turn the problem around and use universal artiﬁcial intelligence theory to formally deﬁne a universal mea- sure of machine intelligence. In Chapter 4 we take the informal deﬁnition of intelligence from Chapter 1 and abstract and formalise it using ideas from the theory of universal artiﬁcial intelligence in Chapter 2. The result is an alternate characterisation of Hutter’s  intelligence order relation . This gives us a formal deﬁnition of machine intelligence that we then compare with other formal deﬁnitions and tests of machine intelligence that have been proposed.  

The speciﬁc formulation of the  universal intelligence measure  is of my own creation. The chapter is largely based on (Legg and Hutter, 2007c), in partic- ular the parts of this paper which build upon (Legg and Hutter 2005b; 2006).  

5) Limits of Computational Agents. One of the key reasons for studying in- computable but elegant theoretical models, such as Solomon o induction and AIXI , is that it is hoped that these will someday guide us towards powerful computable models of artiﬁcial intelligence. Although there have been a num- ber of attempts at converting these universal theories into practical methods, the resulting methods have all been a mere shadow of their original founding theory. Is this because we have not yet seen how to properly convert these theories into practical algorithms, or are there more fundamental limitations at work?  

Chapter 5 explores this question mathematically. Speciﬁcally, it looks at the existence and nature of computable agents which are powerful and ex- tremely general. The results reveal a number of fundamental constraints on any endeavour to construct very general artiﬁcial intelligence algorithms.  

The elementary results at the start of the chapter are already well known, nevertheless the proofs given are my own. The more signiﬁcant results towards the end are entirely original and are my own work. The chapter is based primarily on (Legg, 2006b) which built upon the results in (Legg, 2006a). The core results also appear with other related work in the book chapter (Legg et al., 2008).  

6) Fundamental Temporal Diﬀerence Learning. Although deriving practical theories based on universal artiﬁcial intelligence is problematic, there still exist many opportunities for theory to contribute to the development of new learning techniques, albeit on a somewhat less grand scale. In Chapter 6 we derive an equation for temporal diﬀerence learning from statistical principles. We start with the variational principle and then bootstrap to produce an update-rule for discounted state value estimates. The resulting equation is similar to the standard equation for temporal diﬀerence learning with eligibility traces, so called   $\operatorname{TD}(\lambda)$  , however it lacks the parameter that speciﬁes the learning rate. In the place of this free parameter there is now an equation for the learning rate that is speciﬁc to each state transition. We experimentally test this new learning rule against   $\operatorname{TD}(\lambda)$  . Finally, we make some preliminary investigations into how to extend our new temporal diﬀerence algorithm to reinforcement learning.  

The derivation of the temporal diﬀerence learning rate comes from a col- lection of unpublished derivations by Hutter. I went through this collect of handwritten notes, checked the proofs and took out what seemed to be the most promising candidate for a new learning rule. The presented proof has some reworking for improved presentation. The implementation and testing of this update-rule is my own work, as is the extension to reinforcement learning by merging it with Sarsa( λ ) and Q(  $\lambda$  ). These results were published in (Hutter and Legg, 2007).  

7) Discussion The concluding discussion on the future development of ma- chine intelligence is my own. This has not been published before.  

Appendix A A description of the mathematical notation used.  

Appendix B A convergence proof for ergodic  MDP s needed for key results in Chapter 2  

Appendix C This collection of deﬁnitions of intelligence, seemly the largest in existence, is my own work. This section of the appendix was based on (Legg and Hutter, 2007a).  

Some of my other publications which are only mentioned in passing in this thesis include (Smith et al., 1994; Legg, 1996; Cleary et al., 1996; Calude et al., 2000; Legg et al., 2004; Legg and Hutter, 2005a; Hutter and Legg, 2006). Coverage of the research in this thesis in the popular scientiﬁc press includes New Scientist  magazine (Graham-Rowe, 2005),  Le Monde de l’intelligence (Fi´ evet, 2005), as well as numerous blog and online newspaper articles.  

# Prerequisite knowledge  

The thesis aims to be fairly self contained, however some knowledge of math- ematics, statistics and theoretical computer science is assumed. From math- ematics the reader should be familiar with linear algebra, calculus, basic set theory and logic. From statistics, basic probability theory and elementary distributions such as the uniform and binomial distributions. A knowledge of measure theory would be beneﬁcial, but is not essential. From theoretical com- puter science a knowledge of the basics such as Turing computation, universal Turing machines, incomputability and the halting problem are needed. The mathematical notation and conventions adopted are described in Appendix A. The reader may want to consult this before beginning Chapter 2 as this is where the mathematical material begins.  

# Acknowledgements  

First and foremost I would like to thank my supervisor Marcus Hutter. Get- ting a PhD is a somewhat long process and I have appreciated his guidance throughout this endeavour. I am especially grateful for the way in which he has always gone through my work carefully and provided detailed feedback on where there was room for improvement. Not every graduate student receives such careful appraisal and guidance during this long voyage.  

Essentially all of the research contained in this thesis was carried out at the Dalle Molle Institute for Artiﬁcial Intelligence (IDSIA) near Lugano, Switzer- land. It has been a pleasure to work with such a talented group of people over the last 4 years. In particular I would like to thank Alexey Chernov for encouraging me to develop a few short proofs on the limits of computational prediction systems into a full length paper. For me, that was a turning point in my thesis.  

A special thanks goes to my reading group: JeﬀRose, Cyrus Hall, Giovanni Luca Ciampaglia, Katerina Barone-Adesi, Tom Schaul and Daan Wierstra. They went through most of my thesis ﬁnding typos and places where things were not well explained. The thesis is no doubt far more intelligible due to their eﬀorts. Special thanks also to my mother Gail Legg and Christoph Kolodziejski for further proof reading eﬀorts.  

My research has beneﬁted from interaction with many other colleagues, both at IDSIA and other research centres, in particular J¨ urgen Schmidhu- ber, Jan Poland, Daniil Ryabko, Faustino Gomez, Matteo Gagliolo, Frederick Ducatelle, Alex Graves, Bram Bakker, Viktor Zhumatiy and Laurent Orseau. I would also like to thank the institute secretary, Cinzia Daldini, for her amaz- ing ability to ﬁnd solutions to all manner of issues. It made coming to work at IDSIA and living in Switzerland a breeze. Finally, thanks to  etomchek  for designing the beautiful electric sheep on the front cover, and releasing it under the creative commons licence. I always wanted a sheep on the cover of my PhD thesis.  

This research was funded by the Swiss National Science Foundation under grants 2100-67712.0 and 200020-107616. Many funding agencies are not willing to support such blue-sky research. Their backing has been greatly appreciated.  

Lugano, Switzerland, June 2008 Shane Legg  

# 1. Nature and Measurement of Intelligence  

“Innumerable tests are available for measuring intelligence, yet no one is quite certain of what intelligence is, or even just what it is that the available tests are measuring.” Gregory (1998)  

What is intelligence? It is a concept that we use in our daily lives that seems to have a fairly concrete, though perhaps naive, meaning. We say that our friend who got an A in his calculus test is very intelligent, or perhaps our cat who has learnt to go into hiding at the ﬁrst mention of the word “vet”. Although this intuitive notion of intelligence presents us with no diﬃculties, if we attempt to dig deeper and deﬁne it in precise terms we ﬁnd the concept to be very diﬃcult to nail down. Perhaps the ability to learn quickly is central to intelligence? Or perhaps the total sum of one’s knowledge is more important? Perhaps communication and the ability to use language play a central role? What about “thinking” or the ability to perform abstract reasoning? How about the ability to be creative and solve problems? Intelligence involves a perplexing mixture of concepts, many of which are equally diﬃcult to deﬁne.  

Psychologists have been grappling with these issues ever since humans ﬁrst became fascinated with the nature of the mind. Debates have raged back and forth concerning the correct deﬁnition of intelligence and how best to measure the intelligence of individuals. These debates have in many instances been very heated as what is at stake is not merely a scientiﬁc deﬁnition, but a fundamen- tal issue of how we measure and value humans: Is one employee smarter than another? Are men on average more intelligent than women? Are white people smarter than black people? As a result intelligence tests, and their creators, have on occasion been the subject of intense public scrutiny. Simply deter- mining whether a test, perhaps quite unintentionally, is partly a reﬂection of the race, gender, culture or social class of its creator is a subtle, complex and often politically charged issue (Gould, 1981; Herrnstein and Murray, 1996). Not surprisingly, many have concluded that it is wise to stay well clear of this topic.  

In reality the situation is not as bad as it is sometimes made out to be. Although the details of the deﬁnition are debated, in broad terms a fair de- gree of consensus has been achieved about the scientiﬁc deﬁnition of human intelligence and how to measure it (Gottfredson, 1997a; Sternberg and Berg, 1986). Indeed it is widely recognised that when standard intelligence tests are correctly applied and interpreted, they all measure approximately the same thing (Gottfredson, 1997a). Furthermore, what they measure is both stable over time in individuals and has signiﬁcant predictive power, in particular for future academic performance and other mentally demanding pursuits. The is- sues that continue to draw debate are questions such as whether the tests test only a part or a particular type of intelligence, or whether they are somehow biased towards a particular group or set of mental skills. Great eﬀort has gone into dealing with these issues, but they are diﬃcult problems with no easy solutions.  

Somewhat disconnected from this exists a parallel debate over the nature of intelligence in the context of machines. While the debate is less politically charged, in some ways the central issues are even more diﬃcult. Machines can have physical forms, sensors, actuators, means of communication, information processing abilities and exist in environments that are totally unlike those that we experience. This makes the concept of “machine intelligence” particularly diﬃcult to get a handle on. In some cases, a machine may have properties that are similar to human intelligence, and so it might be reasonable to describe the machine as also being intelligent. In other situations this view is far too limited and anthropocentric. Ideally we would like to be able to measure the intelligence of a wide range of systems: humans, dogs, ﬂies, robots or even disembodied systems such as chat-bots, expert systems, classiﬁcation systems and prediction algorithms (Johnson, 1992; Albus, 1991).  

One response to this problem might be to develop speciﬁc kinds of tests for speciﬁc kinds of entities, just as intelligence tests for children diﬀer to intelligence tests for adults. While this works well when testing humans of diﬀerent ages, it comes undone when we need to measure the intelligence of entities which are profoundly diﬀerent to each other in terms of their cognitive capacities, speed, senses, environments in which they operate, and so on. To measure the intelligence of such diverse systems in a meaningful way we must step back from the speciﬁcs of particular systems and establish fundamentally what it is that we are really trying to measure.  

The diﬃculty of forming a highly general notion of intelligence is readily apparent. Consider, for example, that memory and numerical computation tasks were once regarded as deﬁning hallmarks of human intelligence. We now know that these tasks are absolutely trivial for a machine and do not test its intelligence in any meaningful sense. Indeed, even the mentally demanding task of playing chess can now be largely reduced to brute force search (Hsu et al., 1995). What else may in time be possible with relatively simple algo- rithms running on powerful machines is hard to say. What we can be sure of is that, as technology advances, our concept of intelligence will continue to evolve with it.  

How then are we to develop a concept of intelligence that is applicable to all kinds of systems? Any proposed deﬁnition must encompass the essence of human intelligence, as well as other possibilities, in a consistent way. It should not be limited to any particular set of senses, environments or goals, nor should it be limited to any speciﬁc kind of hardware, such as silicon or biological neurons. It should be based on principles which are fundamental and thus unlikely to alter over time. Furthermore, the deﬁnition of intelligence should ideally be formally expressed, objective, and practically realisable as an eﬀective test. Before attempting to construct such a formal deﬁnition in Chapter 4, in this chapter we will ﬁrst survey existing deﬁnitions, tests and theories of intelligence. We are particularly interested in common themes and general perspectives on intelligence that could be applicable to many kinds of systems, including machines.  

# 1.1. Theories of intelligence  

A central question in the study of intelligence concerns whether intelligence should be viewed as one ability, or many. On one side of the debate are the theories that view intelligence as consisting of many diﬀerent components and that identifying these components is important to understanding intelli- gence. Diﬀerent theories propose diﬀerent ways to do this. One of the ﬁrst was Thurstone’s “multiple-factors” theory which considers seven “primary mental abilities”: verbal comprehension, word ﬂuency, number facility, spatial visuali- sation, associative memory, perceptual speed and reasoning (Thurstone, 1938). Another approach is Sternberg’s “Triarchic Mind” which breaks intelligence down into analytical intelligence, creative intelligence, and practical intelli- gence (Sternberg, 1985), however this model is now considered outdated, even by Sternberg himself.  

Taking the number of components to an extreme is Guilford’s “Structure of Intellect” theory. Under this theory there are three fundamental dimensions: contents, operations, and products. Together these give rise to 120 diﬀerent categories (Guilford, 1967). In later work this increased to 150 categories. This theory has been criticised due to the fact that measuring such precise combi- nations of cognitive capacities in individuals seems to be infeasible and thus it is diﬃcult to experimentally study such a ﬁne-grained model of intelligence.  

A recently popular approach is Gardner’s “multiple intelligences” where he argues that the components of human intelligence are suﬃciently separate that they are actually diﬀerent “intelligences”(Gardner, 1993). Based on the structure of the human brain he identiﬁes these intelligences to be linguistic, musical, logical-mathematical, spatial, bodily kinaesthetic, intra-personal and inter-personal intelligence. Although Gardner’s theory of multiple intelligences has certainly captured the imagination of the public, it remains to be seen to what degree it will have a lasting impact in professional circles.  

At the other end of the spectrum is the work of Spearman and those that have followed in his approach. Here intelligence is seen as a very general mental ability that underlies and contributes to all other mental abilities. As evidence they point to the fact that an individual’s performance levels in rea- soning, association, linguistic, spatial thinking, pattern identiﬁcation etc. are positively correlated. Spearman called this positive statistical correlation be- tween diﬀerent mental abilities the “  $g$  -factor”, where    $g$   stands for “general intelligence”(Spearman, 1927). Because standard IQ tests measure a range of key cognitive abilities, from a collection of scores on diﬀerent cognitive tasks we can estimate an individual’s    $g$  -factor. Some who consider the generality of intelligence to be primary take the    $g$  -factor to be the very deﬁnition of intelligence (Gottfredson, 2002).  

A well known reﬁnement to the    $g$  -factor theory due to Cattell is to distin- guish between “ﬂuid intelligence”, which is a very general and ﬂexible innate ability to deal with problems and complexity, and “crystallized intelligence”, which measures the knowledge and abilities that an individual has acquired over time (Cattell, 1987). For example, while an adolescent may have a similar level of ﬂuid intelligence to that of an adult, their level of crystallized intelli- gence is typically lower due to less life experience (Horn, 1970). Although it is diﬃcult to determine to what extent these two inﬂuence each other, the dis- tinction is an important one because it captures two distinct notions of what the word “intelligence” means.  

As the    $g$  -factor is simply the statistical correlation between diﬀerent kinds of mental abilities, it is not fundamentally inconsistent with the view that intelligence can have multiple aspects or dimensions. Thus a synthesis of the two perspectives is possible by viewing intelligence as a hierarchy with the    $g$  - factor at its apex and increasing levels of specialisation for the diﬀerent aspects of intelligence forming branches (Carroll, 1993). For example, an individual might have a high    $g$  -factor, which contributes to all of their cognitive abilities, but also have an especially well developed musical sense. This hierarchical view of intelligence is now quite popular (Neisser et al., 1996).  

# 1.2. Deﬁnitions of human intelligence  

“Viewed narrowly, there seem to be almost as many deﬁnitions of intelligence as there were experts asked to deﬁne it.” R. J. Stern- berg quoted in (Gregory, 1998)  

In this section and the next we will overview a range of deﬁnitions of in- telligence that have been given by psychologists. For an even more extensive collection of deﬁnitions of intelligence, indeed the largest collection that we are aware of, see Appendix C or visit our online collection (Legg and Hutter, 2007a). Although deﬁnitions diﬀer, there are many reoccurring features; in some cases these are explicitly stated, while in others they are more implicit. We start by considering ten deﬁnitions that take a similar perspective:  

“It seems to us that in intelligence there is a fundamental faculty, the alteration or the lack of which, is of the utmost importance for practi- cal life. This faculty is judgement, otherwise called good sense, practi- cal sense, initiative, the faculty of adapting oneself to circumstances.” Binet and Simon (1905)  

“The capacity to learn or to proﬁt by experience.” Dearborn quoted in (Sternberg, 2000)  

“Ability to adapt oneself adequately to relatively new situations in life.” Pinter quoted in (Sternberg, 2000)  

“A person possesses intelligence insofar as he has learned, or can learn, to adjust himself to his environment.” Colvin quoted in (Sternberg, 2000)  

“We shall use the term ‘intelligence’ to mean the ability of an organism to solve new problems . . . ” Bingham (1937)  

“A global concept that involves an individual’s ability to act purposefully, think rationally, and deal eﬀectively with the environment.” Wechsler (1958)  

“Individuals diﬀer from one another in their ability to understand com- plex ideas, to adapt eﬀectively to the environment, to learn from expe- rience, to engage in various forms of reasoning, to overcome obstacles by taking thought.” American Psychological Association (Neisser et al., 1996)  

“. . . I prefer to refer to it as ‘successful intelligence.’ And the reason is that the emphasis is on the use of your intelligence to achieve success in your life. So I deﬁne it as your skill in achieving whatever it is you want to attain in your life within your sociocultural context — meaning that people have diﬀerent goals for themselves, and for some it’s to get very good grades in school and to do well on tests, and for others it might be to become a very good basketball player or actress or musician.” Sternberg (2003)  

“Intelligence is part of the internal environment that shows through at the interface between person and external environment as a function of cognitive task demands.” R. E. Snow quoted in (Slatter, 2001)  

“. . . certain set of cognitive capacities that enable an individual to adapt and thrive in any given environment they ﬁnd themselves in, and those cognitive capacities include things like memory and retrieval, and prob- lem solving and so forth. There’s a cluster of cognitive abilities that lead to successful adaptation to a wide range of environments.” Simonton (2003)  

Perhaps the most elementary common feature of these deﬁnitions is that intelligence is seen as a property of an individual who is interacting with an external environment, problem or situation. Indeed, at least this much is common to practically all proposed deﬁnitions of intelligence.  

Another common feature is that an individual’s intelligence is related to their ability to succeed or proﬁt. This implies the existence of some kind of objective or goal. What the goal is, is not speciﬁed, indeed individuals’ goals may be varied. The important thing is that the individual is able to carefully choose their actions in a way that leads to them accomplishing their goals. The greater this capacity to succeed with respect to various goals, the greater the individual’s intelligence.  

The strong emphasis on learning, adaption and experience in these deﬁni- tions implies that the environment is not fully known to the individual and may contain new situations that could not have been anticipated in advance. Thus intelligence is not the ability to deal with a fully known environment, but rather the ability to deal with some range of possibilities which cannot be wholly anticipated. What is important then is that the individual is able to quickly learn and adapt so as to perform as well as possible over a wide range of environments, situations, tasks and problems. Collectively we will refer to these as “environments”, similar to some of the deﬁnitions above.  

Bringing these key features together gives us what we believe to be the essence of intelligence in its most general form:  

Intelligence measures an agent’s ability to achieve goals in a wide range of environments.  

The remainder of this section considers a range of other deﬁnitions that are not as strongly connected to our adopted deﬁnition. Usually it is not that they are entirely incompatible with our deﬁnition, but rather they stress diﬀerent aspects of intelligence. The following deﬁnition is an especially interesting deﬁnition as it was given as part of a group statement signed by 52 experts in the ﬁeld. As such it obviously represents a fairly mainstream perspective:  

“Intelligence is a very general mental capability that, among other things, involves the ability to reason, plan, solve problems, think abstractly, comprehend complex ideas, learn quickly and learn from experience.” Gottfredson (1997a)  

Reasoning, planning, solving problems, abstract thinking, learning from ex- perience and so on, these are all mental abilities that allow us to successfully achieve goals. If we were missing any one of these capacities, we would clearly be less able to successfully deal with such a wide range of environments. Thus, these capacities are implicit in our deﬁnition also. The diﬀerence is that our deﬁnition does not attempt to specify what capabilities might be needed, some- thing which is clearly very diﬃcult and would depend on the particular tasks that the agent must deal with. Our approach is to consider intelligence to be the  eﬀect  of capacities such as those listed above. It is not the result of having any speciﬁc set of capacities. Indeed, intelligence could also be the eﬀect of many other capacities, some of which humans may not have. In summary, our deﬁnition is not in conﬂict with the above deﬁnition, rather it is that our deﬁnition is more abstract and general.  

“. . . in its lowest terms intelligence is present where the individual an- imal, or human being, is aware, however dimly, of the relevance of his behaviour to an objective. Many deﬁnitions of what is indeﬁnable have been attempted by psychologists, of which the least unsatisfactory are 1. the capacity to meet novel situations, or to learn to do so, by new adap- tive responses and 2. the ability to perform tests or tasks, involving the grasping of relationships, the degree of intelligence being proportional to the complexity, or the abstractness, or both, of the relationship.” Drever (1952)  

This deﬁnition has many similarities to ours. Firstly, it emphasises the agent’s ability to choose its actions so as to achieve an objective, or in our terminology, a goal. It then goes on to stress the agent’s ability to deal with situations which have not been encountered before. In our terminology, this is the ability to deal with a wide range of environments. Finally, this deﬁnition highlights the agent’s ability to perform tests or tasks, something which is entirely consistent with our performance orientated perspective of intelligence.  

“Intelligence is not a single, unitary ability, but rather a composite of sev- eral functions. The term denotes that combination of abilities required for survival and advancement within a particular culture.” Anastasi (1992)  

This deﬁnition does not specify exactly which capacities are important, only that they should enable the individual to survive and advance with the culture. As such this is a more abstract “success” orientated deﬁnition of intelligence, like ours. Naturally, culture is a part of the agent’s environment, though only complex environments with other agents would have true culture.  

“The ability to carry on abstract thinking.” L. M. Terman quoted in (Sternberg, 2000)  

This is not really much of a deﬁnition as it simply shifts the problem of deﬁning intelligence to the problem of deﬁning abstract thinking. The same is true of many other deﬁnitions that refer to things such as imagination, creativity or consciousness. The following deﬁnition has a similar problem:  

“The capacity for knowledge, and knowledge possessed.” Henmon (1921)  

What exactly constitutes “knowledge”, as opposed to perhaps data or infor- mation? For example, does a library contain a lot of knowledge, and if so, is it intelligent? Or perhaps the internet? Modern concepts of the word knowledge stress the fact that the information has to be in some sense properly contex- tualised so that it has meaning. Deﬁning this more precisely appears to be diﬃcult however. Because this deﬁnition of intelligence dates from 1921, per- haps it reﬂects pre-information age thinking when computers with vast storage capacities did not exist.  

Nonetheless, our deﬁnition of intelligence is not entirely inconsistent with the above deﬁnition in that an individual may be required to know many things, or have a signiﬁcant capacity for knowledge, in order to perform well in some environments. However, our deﬁnition is narrower in that knowledge, or the capacity for knowledge, is not by itself suﬃcient. We require that the knowledge can be used eﬀectively. Indeed, unless information can be eﬀectively utilised for various purposes, it seems reasonable to consider it to be merely “data”, rather than “knowledge”.  

“The capacity to acquire capacity.” H. Woodrow quoted in (Sternberg, 2000)  

The deﬁnition of Woodrow is typical of those that emphasise not the current ability of the individual, but rather the individual’s ability to expand and develop new abilities. This is a fundamental point of divergence for many views on intelligence. Consider the following question: Is a young child as intelligent as an adult? From one perspective, children are very intelligent because they can learn and adapt to new situations quickly. On the other hand, a child is unable to do many things due to a lack of knowledge and experience and thus will make mistakes an adult would know to avoid. These need not just be physical acts, they could also be more subtle things like errors in reasoning as their mind, while very malleable, has not yet matured. In which case, perhaps their intelligence is currently low, but will increase with time and experience?  

Fundamentally, this diﬀerence in perspective is a question of time scale: Must an agent be able to tackle some task immediately, or perhaps after a short period of time during which learning can take place, or perhaps it only matters that they can eventually learn to deal with the problem? Being able to deal with a diﬃcult problem immediately is a matter of experience, rather than intelligence. While being able to deal with it in the very long run might not require much intelligence at all, for example, simply trying a vast number of possible solutions might eventually produce the desired results. Intelligence then seems to be the ability to adapt and learn as quickly as possible given the constraints imposed by the problem at hand.  

“Intelligence is a general factor that runs through all types of perfor- mance.” A. Jensen  

At ﬁrst this might not look like a deﬁnition of intelligence, but it makes an important point: Intelligence is not really the ability to do anything in partic- ular, rather it is a very general ability that aﬀects many kinds of performance. Conversely, by measuring many diﬀerent kinds of performance we can estimate an individual’s intelligence. This is consistent with our deﬁnition’s emphasis on the agent’s ability to perform well in many environments.  

“Intelligence is what is measured by intelligence tests.” Boring (1923)  

Boring’s famous deﬁnition of intelligence takes this idea a step further. If in- telligence is not the ability to do anything in particular, but rather an abstract ability that indirectly aﬀects performance in many tasks, then perhaps it is most concretely described as the ability to do the kind of abstract problems that appear in intelligence tests? In which case, Boring’s deﬁnition is not as facetious as it ﬁrst appears. This deﬁnition also highlights the fact that the concept of intelligence, and how it is measured, are intimately related. In the context of this paper we refer to these as deﬁnitions of intelligence, and tests of intelligence, respectively, although in some cases the distinction is not sharp.  

# 1.3. Deﬁnitions of machine intelligence  

The following sample of informal deﬁnitions of machine intelligence capture a range of perspectives. There also exist several formal deﬁnitions and tests of machine intelligence, however we will deal with those in Chapter 4. We begin with ﬁve deﬁnitions that have clear connections to our informal deﬁnition:  

“. . . the mental ability to sustain successful life.” K. Warwick quoted in (Asohan, 2003)  

“. . . doing well at a broad range of tasks is an empirical deﬁnition of ‘intelligence’ ” Masum et al. (2002)  

“Intelligence is the computational part of the ability to achieve goals in the world. Varying kinds and degrees of intelligence occur in people, many animals and some machines.” McCarthy (2004)  

“Any system . . . that generates adaptive behaviour to meet goals in a range of environments can be said to be intelligent.” Fogel (1995)  

“. . . the ability of a system to act appropriately in an uncertain environ- ment, where appropriate action is that which increases the probability of success, and success is the achievement of behavioral subgoals that support the system’s ultimate goal.” Albus (1991)  

The position taken by Albus is especially similar to ours. Although the quote above does not explicitly mention the need to be able to perform well in a wide range of environments, at a later point in the same paper he mentions the need to be able to succeed in a “large variety of circumstances”.  

“Intelligent systems are expected to work, and work well, in many dif- ferent environments. Their property of intelligence allows them to max- imize the probability of success even if full knowledge of the situation is not available. Functioning of intelligent systems cannot be considered separately from the environment and the concrete situation including the goal.” Gudwin (2000)  

While this deﬁnition is consistent with the position we have taken, when trying to actually test the intelligence of an agent Gudwin does not believe that a “black box” behaviour based approach is suﬃcient, rather his approach is to look at the “. . . architectural details of structures, organizations, processes and algorithms used in the construction of the intelligent systems,” (Gudwin, 2000). Our perspective is simply to not care whether an agent looks intelligent on the inside. If it is able to perform well in a wide range of environments, that is all that matters.  

“We deﬁne two perspectives on artiﬁcial system intelligence: (1) na- tive intelligence, expressed in the speciﬁed complexity inherent in the information content of the system, and (2) performance intelligence, ex- pressed in the successful (i.e., goal-achieving) performance of the system in a complicated environment.” Horst (2002)  

Here we see two distinct notions of intelligence, a performance based one and an information content one. This is similar to the distinction between ﬂuid intelligence and crystallized intelligence made by the psychologist Cattell (see Section 1.1). The performance based notion of intelligence is similar to our deﬁnition with the exception that performance is measured in a complex environment rather than across a wide range of environments. This perspective appears in some other deﬁnitions also,  

“. . . the ability to solve hard problems.” Minsky (1985) “Achieving complex goals in complex environments” Goertzel (2006)  

The emphasis on complex goals and environments is not really so diﬀerent to our “wide range of environments” in that any agent which could not achieve simple goals in simple environments presumably would not be considered in- telligent. One might argue that the ability to achieve truly complex goals in complex environments requires the ability to achieve simple ones, in which case the two perspectives are equivalent.  

Some deﬁnitions emphasise not just the ability to perform well, but also the need for eﬃciency:  

“[An intelligent agent does what] is appropriate for its circumstances and its goal, it is ﬂexible to changing environments and changing goals, it learns from experience, and it makes appropriate choices given per- ceptual limitations and ﬁnite computation.” Poole et al. (1998)  

“. . . in any real situation behavior appropriate to the ends of the system and adaptive to the demands of the environment can occur, within some limits of speed and complexity.” Newell and Simon (1976)  

“Intelligence is the ability to use optimally limited resources – including time – to achieve goals.” Kurzweil (2000)  

“Intelligence is the ability for an information processing agent to adapt to its environment with insuﬃcient knowledge and resources.” Wang (1995)  

We consider the addition of resource limitations to the deﬁnition of intel- ligence to be either superﬂuous, or wrong. In the ﬁrst case, if limited com- putational resources are a fundamental and unavoidable part of reality, which certainly seems to be the case, then their addition to the deﬁnition of intel- ligence is unnecessary. Perhaps the ﬁrst three deﬁnitions above fall into this category.  

On the other hand, if limited resources are not a fundamental restriction, for example a new model of computation was discovered that was vastly more powerful than the current model, then it would be odd to claim that the unbelievably powerful machines that would then result were not intelligent. Normally we do not judge the intelligence of something relative to the resources it uses. For example, if a rat had human level learning and problem solving abilities, we would not think of the rat as being more intelligent than a human due to the fact that its brain was much smaller.  

While we do not consider eﬃciency to be a part of the deﬁnition of intelli- gence, this is not to say that considering the eﬃciency of agents is unimportant. Indeed, a key goal of artiﬁcial intelligence is to ﬁnd algorithms which have the greatest eﬃciency of intelligence, that is, which achieve the most intelligence per unit of computational resources consumed.  

# 1.4. Intelligence testing  

Having explored what intelligence is, we now turn to how it is measured. Contrary to popular public opinion, most psychologists believe that standard psychometric tests of intelligence, such as IQ tests, reliably measure some- thing important in humans (Neisser et al., 1996; Gottfredson, 1997b). In fact, standard intelligence tests are among the most statistically stable and reli- able psychological tests. Furthermore, it is well known that these scores are a good predictor of various things, such as academic performance. The question then is not whether these tests are useful or measure something meaningful, but rather whether what they measure is indeed “intelligence”. Some experts believe that they do, while others think that they only succeed in measuring certain aspects of, or types of, intelligence.  

There are many properties that a good test of intelligence should have. One important property is that the test should be repeatable, in the sense that it consistently returns about the same score for a given individual. For example, the test subject should not be able to signiﬁcantly improve their performance if tested again a short time later. Statistical variability can also be a problem in short tests. Longer tests help in this regard, however they are naturally more costly to administer.  

Another important reliability factor is the bias that might be introduced by the individual administering the test. Purely written tests avoid this problem as there is minimal interaction between the tested individual and the tester. However, this lack of interaction also has disadvantages as it may mean that other sources of bias, such as cultural diﬀerences, language problems or even something as simple as poor eyesight, might not be properly identiﬁed. Thus, even with a written test the individual being tested should ﬁrst be examined by an expert in order to ensure that the test is appropriate.  

Cultural bias in particular is a diﬃcult problem, and tests should be de- signed to minimise this problem where possible, or at least detect potential bias problems when they occur. One way to do this is to test each ability in multiple ways, for example both verbally and visually. While language is an obvious potential source of cultural bias, more subtle forms of bias are diﬃ- cult to detect and remedy. For example, diﬀerent cultures emphasise diﬀerent cognitive abilities and thus it is diﬃcult, perhaps impossible, to compare intel- ligence scores in a way that is truly objective. Indeed, this choice of emphasis is a key issue for any intelligence test, it depends on the perspective taken on what intelligence is.  

An intelligence test should be valid in the sense that it appears to be testing what it claims it is testing for. One way to check this is to show that the test produces results consistent with other manifestations of intelligence. A test should also have predictive power, for example the ability to predict future academic performance, or performance in other cognitively demanding tasks. This ensures that what is being measured is somehow meaningful, beyond just the ability to answer the questions in the test. Standard intelligence tests are thoroughly tested for years on the above criteria, and many others, before they are ready for wide spread use.  

Finally, when testing large numbers of individuals, for example when testing army recruits, the cost of administering the test becomes important. In these cases less accurate but more economical test procedures may be used, such as purely written tests without any direct interaction between the individuals being tested and a psychologist.  

Standard intelligence tests, such as those described in the next section, are all examples of “static tests”. By this we mean that they test an individual’s knowledge and ability to solve one-oﬀproblems. They do not directly measure the ability to learn and adapt over time. If an individual was good at learning and adapting then we might expect this to be reﬂected in their total knowledge and thus be picked up in a static test. However, it could be that an individual has a great capacity to learn, but that this is not reﬂected in their knowledge due to limited education. In which case, if we consider the capacity to learn and adapt to be a deﬁning characteristic of intelligence, rather than the sum of knowledge, then to class an individual as unintelligent due to limited access to education would be a mistake.  

What is needed is a more direct test of an individual’s ability to learn and adapt: a so called “dynamic test”(Sternberg and Grigorenko, 2002) (for re- lated work see also Johnson-Laird and Wason, 1977). In a dynamic test the individual interacts over a period of time with the tester, who now becomes a kind of teacher. The tester’s task is to present the test subject with a series of problems. After each attempt at solving a problem, the tester provides feedback to the individual who then has to adapt their behaviour accordingly in order to solve the next problem.  

Although dynamic tests could in theory be very powerful, they are not yet well established due to a number of diﬃculties. One of the drawbacks is that they require a much greater degree of interaction between the test subject and the tester. This makes dynamic testing more costly to perform and increases the danger of tester bias.  

# 1.5. Human intelligence tests  

The ﬁrst modern style intelligence test was developed by the French psychol- ogist Alfred Binet in 1905. Binet believed that intelligence was best studied by looking at relatively complex mental tasks, unlike earlier tests developed by Francis Galton which focused on reaction times, auditory discrimination ability, physical coordination and so on. Binet’s test consisted of 30 short tasks related to everyday problems such as: naming parts of the body, com- paring lengths and weights, counting coins, remembering digits and deﬁnitions of words. For each task category there were a number of problems of increas- ing diﬃculty. The child’s results were obtained by normalising their raw score against peers of the same age. Initially his test was designed to measure the mental performance of children with learning problems (Binet and Simon, 1905). Later versions were also developed for normal children (Binet, 1911). It was found that Binet’s test results were a good predictor of children’s academic performance.  

Lewis Terman of Stanford University developed a version of Binet’s test in English. As the age norms for French children did not correspond well with American children, he revised Binet’s test in various ways, in particular he increased the upper age limit. This resulted in the now famous Stanford-Binet test (Terman and Merrill, 1950). This test formed the basis of a number of other intelligence tests, such as the Army Alpha and Army Beta tests which were used to classify recruits. Since its development, the Stanford-Binet has been periodically revised, with updated versions being widely used today.  

David Wechsler believed that the original Binet tests were too focused on verbal skills and thus disadvantaged certain otherwise intelligent individuals, for example the deaf or people who did not speak the test language as a ﬁrst language. To address this problem, he proposed that tests should con- tain a combination of both verbal and nonverbal problems. He also believed that in addition to an overall IQ score, a proﬁle should be produced show- ing the performance of the individual in the various areas tested. Borrowing signiﬁcantly from the Stanford-Binet, the US army Alpha test, and others, he developed a range of tests targeting speciﬁc age groups from preschool- ers up to adults (Wechsler, 1958). Due in part to problems with revisions of the Stanford-Binet test in the 1960’s and 1970’s, Wechsler’s tests became the standard. They continue to be well respected and widely used.  

Modern versions of the Wechsler and the Stanford-Binet have a similar basic structure (Kaufman, 2000). Both test the individual in a number of verbal and non-verbal ways. In the case of a Stanford-Binet the test is broken up into ﬁve key areas: ﬂuid reasoning, knowledge, quantitative reasoning, visual- spatial processing, and working memory. In the case of the Wechsler Adult Intelligence Scale (WAIS-III), the verbal tests include areas such as knowledge, basic arithmetic, comprehension, vocabulary, and short term memory. Non- verbal tests include picture completion, spatial perception, problem solving, symbol search and object assembly.  

As part of an eﬀort to make intelligence tests more culture neutral John Raven developed the progressive matrices test (Raven, 2000). In this test each problem consists of a short sequence of basic shapes. For example, a circle in a box, then a circle with a cross in the middle followed by a circle with a triangle inside. The test subject then has to select from a second list the image that best continues the pattern. Simple problems have simple patterns, while diﬃcult problems have more subtle and complex patterns. In each case, the simplest pattern that can explain the observed sequence is the one that correctly predicts its continuation. Thus, not only is the ability to recognise patterns tested, but also the ability to evaluate the complexity of diﬀerent explanations and then correctly apply the philosophical principle of Occam’s razor (see Section 2.1). This will play a key role for us in later chapters.  

Today several diﬀerent versions of the Raven test exist designed for diﬀer- ent age groups and ability levels. As the tests depend strongly on the ability to identify abstract patterns, rather than knowledge, they are considered to be some of the most “g-loaded” intelligence tests available (see Section 1.1). The Raven tests remain in common use today, particularly when it is thought that culture or language bias could be an issue. The universality of abstract sequence prediction tests makes them potentially useful in the context of ma- chine intelligence, indeed we will see that some tests of machine intelligence take this approach.  

The intelligence quotient, or IQ, was originally introduced by Stern (1912). It was computed by taking the age of a child as estimated by their performance in an intelligence test, and then dividing this by their true biological age and multiplying by 100. Thus a 10 year old child whose mental performance was equal to that of a normal 12 year old, had an IQ of 120. As the concept of mental age has now been discredited, and was never applicable to adults any- way, modern IQ scores are simply normalised to a Gaussian distribution with a mean of 100. The standard deviation used varies: in the United States 15 is commonly used, while in Europe 25 is common. For children the normalising Gaussian is based on peers of the same age.  

Whatever normalising distribution is used, by deﬁnition an individual’s IQ is always an indication of their cognitive performance relative to some larger group. Clearly this would be problematic in the context of machines where the performance of some machines could be many orders of magnitude greater than others. Furthermore, the distribution of machine performance would be continually changing due to advancing technology. Thus, for machine intelli- gence, an absolute measure is more meaningful than a traditional IQ type of measure.  

For an overview of the history of intelligence testing and the structure of modern tests, see (Kaufman, 2000).  

# 1.6. Animal intelligence tests  

Testing the intelligence of animals is of particular interest to us as it moves beyond strictly human focused concepts of intelligence and testing methods. Diﬃcult problems in human intelligence testing, such as bias due to language diﬀerences or physical handicap, become even more diﬃcult if we try to com- pare animals with diﬀerent perceptual and cognitive capacities. Even within a single species measurement is diﬃcult as it is not always obvious how to conduct the tests, or even what should be tested for. Furthermore, as humans devise the tests, there is a persistent danger that the tests may be biased in terms of our sensory, motor, and motivational systems (Macphail, 1985). For example, it is known that rats can learn some types of relationships much more easily through smell rather than other senses (Slotnick and Katz, 1974). Fur- thermore, while an IQ test for children might in some sense be validated by its ability to predict future academic or other success, it is not always clear how to validate an intelligence test for animals: if survival or the total number of oﬀspring was a measure of success, then bacteria would be the most intelligent life on earth!  

As is often the case when we try to generalise concepts, abstraction is neces- sary. When attempting to measure the intelligence of lower animals it is nec- essary to focus on simple things like short and long term memory, the forming of associations, the ability to generalise simple patterns and make predictions, simple counting and basic communication. It is only with relatively intelligent social animals, such as birds and apes, that more sophisticated properties such as deception, imitation and the ability to recognise oneself are relevant. For simpler animals, the focus is more on the animal’s essential information pro- cessing capacity. For example, the work on measuring the capacity of ants to remember patterns (Reznikova and Ryabko, 1986).  

One interesting diﬃculty when testing animal intelligence is that we are unable to directly explain to the animal what its goal is. Instead, we have to guide the animal towards a problem by carefully rewarding selected behaviours with something like food. In general, when testing machine intelligence we face a similar problem in that we cannot assume that a machine will have a suﬃcient level of language comprehension to be able to understand commands. A simple solution is to use basic “rewards” to guide behaviour, as we do with animals. Although this approach is extremely general, one diﬃculty is that solving the task, and simply learning what the task is, become confounded and thus the results need to be interpreted carefully (Zentall, 1997).  

For good overviews of animal intelligence research see (Zentall, 2000), (Her- man and Pack, 1994) or (Reznikova, 2007).  

# 1.7. Machine intelligence tests  

This section surveys proposed tests of machine intelligence. Given that the measurement of machine intelligence is fundamental to the ﬁeld of artiﬁcial intelligence, it is remarkable that few researchers are aware of research in this area beyond the Turing test and some of its variants. Indeed, to the best of our knowledge the survey presented in this section (derived from Legg and Hutter, 2007b) is the only general survey of tests of machine intelligence that has been published!  

Turing test and derivatives. The classic approach to determining whether a machine is intelligent is the so called Turing test (Turing, 1950) which has been extensively debated over the last 50 years (Saygin et al., 2000). Turing realised how diﬃcult it would be to directly deﬁne intelligence and thus at- tempted to side step the issue by setting up his now famous imitation game: if human judges cannot eﬀectively discriminate between a computer and a hu- man through teletyped conversation then we must conclude that the computer is intelligent.  

Though simple and clever, the test has attracted much criticism. Block and Searle argue that passing the test is not  suﬃcient  to establish intelligence (Block, 1981; Searle, 1980; Eisner, 1991). Essentially they both argue that a machine could appear to be intelligent without having any “real intelligence”, perhaps by using a very large table of answers to questions. While such a machine would be impossible in practice, due to the vast size of the table required, it is not logically impossible. Thus, an unintelligent machine could, at least in theory, consistently pass the Turing test. Some consider this to bring the validity of the test into question.  

In response to these challenges, even more demanding versions of the Turing test have been proposed such as the total Turing test in which the machine must respond to all forms of input that a human could, rather than just teletyped text (Harnad, 1989). For example, the machine should have senso- rimotor capabilities. Going further, the truly total Turing test demands the performance of not just one machine, but of the whole “race” of machines over an extended period of time (Schweizer, 1998). Another extension is the inverted Turing test in which the machine takes the place of a judge and must be able to distinguish between humans and machines (Watt, 1996). Dowe argues that the Turing test should be extended by ensuring that the agent has a compressed representation of the domain area, thus ruling out look-up table counter arguments (Dowe and Hajek, 1998). Of course these attacks on the Turing test can be applied to any test of intelligence that considers only a system’s external behaviour, that is, most intelligence tests.  

A more common criticism is that passing the Turing test is not  necessary to establish intelligence. Usually this argument is based on the fact that the test requires the machine to have a highly detailed model of human knowledge and patterns of thought, making it a test of humanness rather than intelli- gence (French, 1990; Ford and Hayes, 1998). Indeed, even small things like pretending to be unable to perform complex arithmetic quickly and faking human typing errors become important, something which clearly goes against the purpose of the test.  

The Turing test has other problems as well. Current AI systems are a long way from being able to pass an unrestricted Turing test. From a practical point of view this means that the full Turing test is unable to oﬀer much guid- ance to our work. Indeed, even though the Turing test is the most famous test of machine intelligence, almost no current research in artiﬁcial intelligence is speciﬁcally directed towards passing it. Simply restricting the domain of con- versation in the Turing test to make the test easier, as is done in the Loebner competition (Loebner, 1990), is not suﬃcient. With restricted conversation possibilities the most successful Loebner entrants are even more focused on fak- ing human fallibility, rather than anything resembling intelligence (Hutchens, 1996). Finally, the Turing test returns diﬀerent results depending on who the human judges are. Its unreliability has in some cases lead to clearly unintelli- gent machines being classiﬁed as human, and at least one instance of a human actually failing a Turing test. When queried about the latter, one of the judges explained that “no human being would have that amount of knowledge about Shakespeare”(Shieber, 1994).  

Compression tests. Mahoney has proposed a particularly simple solution to the binary pass or fail problem with the Turing test: replace the Turing test with a text compression test (Mahoney, 1999). In essence this is somewhat similar to a “Cloze test” where an individual’s comprehension and knowledge in a domain is estimated by having them guess missing words from a passage of text.  

While simple text compression can be performed with symbol frequencies, the resulting compression is relatively poor. By using more complex models that capture higher level features such as aspects of grammar, the best com- pressors are able to compress text to about 1.5 bits per character for English. However humans, which can also make use of general world knowledge, the logical structure of the argument etc., are able to reduce this down to about 1 bit per character. Thus the compression statistic provides an easily com- puted measure of how complete a machine’s models of language, reasoning and domain knowledge are, relative to a human.  

To see the connection to the Turing test, consider a compression test based on a very large corpus of dialogue. If a compressor could perform extremely well on such a test, this is mathematically equivalent to being able to determine which sentences are probable at a give point in a dialogue, and which are not (for the equivalence of compression and prediction see Bell et al., 1990). Thus, as failing a Turing test occurs when a machine (or person!) generates a sentence which would be improbable for a human, extremely good performance on dialogue compression implies the ability to pass a Turing test.  

A recent development in this area is the Hutter Prize (Hutter, 2006). In this test the corpus is a 100 MB extract from Wikipedia. The idea is that this should represent a reasonable sample of world knowledge and thus any compressor that can perform very well on this test must have a good model of not just English, but also world knowledge in general.  

One criticism of compression tests is that it is not clear whether a powerful compressor would easily translate into a general purpose artiﬁcial intelligence. Also, while a young child has a signiﬁcant amount of elementary knowledge about how to interact with the world, this knowledge would be of little use when trying to compress an encyclopedia full of abstract “adult knowledge” about the world.  

Linguistic complexity. A more linguistic approach is taken by the HAL project at the company Artiﬁcial Intelligence NV (Treister-Goren and Hutchens, 2001). They propose to measure a system’s level of conversational ability by using techniques developed to measure the linguistic ability of chil- dren. These methods examine things such as vocabulary size, length of ut- terances, response types, syntactic complexity and so on. This would allow systems to be “. . . assigned an age or a maturity level beside their binary Tur- ing test assessment of ‘intelligent’ or ‘not intelligent’ ”(Treister-Goren et al., 2000). As they consider communication to be the basis of intelligence, and the Turing test to be a valid test of machine intelligence, in their view the best way to develop intelligence is to retrace the way in which human linguistic development occurs. Although they do not explicitly refer to their linguistic measure as a test of intelligence, because it measures progress towards what they consider to be a valid intelligence test, it acts as one.  

Multiple cognitive abilities. A broader developmental approach is being taken by IBM’s Joshua Blue project (Alvarado et al., 2002). In this project they measure the performance of their system by considering a broad range of linguistic, social, association and learning tests. Their goal is to ﬁrst pass what they call a toddler Turing test, that is, to develop an AI system that can pass as a young child in a similar set up to the Turing test.  

Another company pursuing a similar developmental approach based on mea- suring system performance through a broad range of cognitive tests is the a2i2 project at Adaptive AI (Voss, 2005). Rather than toddler level intelligence, their current goal is to work toward a level of cognitive performance simi- lar to that of a small mammal. The idea being that even a small mammal has many of the key cognitive abilities required for human level intelligence working together in an integrated way.  

Competitive games. The Turing Ratio method of Masum et al. has more emphasis on tasks and games rather than cognitive tests. Similar to our own deﬁnition, they propose that “. . . doing well at a broad range of tasks is an em- pirical deﬁnition of ‘intelligence’.”(Masum et al., 2002) To quantify this they seek to identify tasks that measure important abilities, admit a series of strate- gies that are qualitatively diﬀerent, and are reproducible and relevant over an extended period of time. They suggest a system of measuring performance through pairwise comparisons between AI systems that is similar to that used to rate players in the international chess rating system. The key diﬃculty however, which the authors acknowledge is an open challenge, is to work out what these tasks should be, and to quantify just how broad, important and relevant each is. In our view these are some of the most central problems that must be solved when attempting to construct an intelligence test. Thus we consider this approach to be incomplete in its current state.  

Collection of psychometric tests. An approach called Psychometric AI tries to address the problem of what to test for in a pragmatic way. In the view of Bringsjord and Schimanski, “Some agent is intelligent if and only if it ex- cels at all established, validated tests of [human] intelligence.”(Bringsjord and Schimanski, 2003) They later broaden this to also include “tests of artistic and literary creativity, mechanical ability, and so on.” With this as their goal, their research is focused on building robots that can perform well on standard psy- chometric tests designed for humans, such as the Wechsler Adult Intelligence Scale and Raven Progressive Matrices (see Section 1.5).  

As eﬀective as these tests are for humans, we believe that they are unlikely to be adequate for measuring machine intelligence. For a start they are highly anthropocentric. Another problem is that they embody basic assumptions about the test subject that are likely to be violated by computers. For example, consider the fundamental assumption that the test subject is not simply a collection of specialised algorithms designed only for answering common IQ test questions. While this is obviously true of a human, or even an ape, it may not be true of a computer. The computer could be nothing more than a collection of speciﬁc algorithms designed to identify patterns in shapes, predict number sequences, write poems on a given subject or solve verbal analogy problems — all things that AI researchers have worked on. Such a machine might be able to obtain a respectable IQ score (Sanghi and Dowe, 2003), even though outside of these speciﬁc test problems it would be next to useless. If we try to correct for these limitations by expanding beyond standard tests, as Bringsjord and Schimanski seem to suggest, this once again opens up the diﬃculty of exactly what, and what not, to test for. Thus we consider Psychometric AI, at least as it is currently formulated, to only partially address this central question.  

C-Test. One perspective among psychologists is that intelligence is “the abil- ity to deal with complexity”(Gottfredson, 1997b). Thus, in a test of intelli- gence, the most diﬃcult questions are the ones that are the most complex because these will, by deﬁnition, require the most intelligence to solve. It fol- lows then that if we could formally deﬁne and measure the complexity of test problems using complexity theory we could construct a formal test of intel- ligence. The possibility of doing this was perhaps ﬁrst suggested by Chaitin (1982). While this path requires numerous diﬃculties to be dealt with, we believe that it is the most natural and oﬀers many advantages: it is formally motivated and precisely deﬁned, and potentially could be used to measure the performance of both computers and biological systems on the same scale without the problem of bias towards any particular species or culture.  

The C-Test consists of a number of sequence prediction and abduction prob- lems similar to those that appear in many standard IQ tests (Hern´ andez- Orallo, 2000b). This test has been successfully applied to humans with inter- esting results showing a positive correlation between individual’s IQ test scores and C-Test scores (Hern´ andez-Orallo and Minaya-Collado, 1998; Hern´ andez- Orallo, 2000a). Similar to standard IQ tests, the C-Test always ensures that each question has an unambiguous answer in the sense that there is always one hypothesis that is consistent with the observed pattern that has signiﬁcantly lower complexity than the alternatives. Other than making the test easier to score, it has the added advantage of reducing the test’s sensitivity to changes in the reference machine used to deﬁne the complexity measure.  

The key diﬀerence to sequence problems that appear in standard intelli- gence tests is that the questions are based on a formally expressed measure of complexity. As Kolmogorov complexity is not computable (see Section 2.5), the C-Test instead uses Levin’s related    $K t$   complexity (Levin, 1973). In order to retain the invariance property of Kolmogorov complexity, Levin complexity requires the additional assumption that the universal Turing machines are able to simulate each other in linear time, for example, pointer machines. As far as we know, this is the only formal deﬁnition of intelligence that has so far produced a usable test of intelligence.  

To illustrate the C-Test, below are some example problems taken from (Hern´ andez-Orallo and Minaya-Collado, 1998). Beside each question is its complexity, naturally more complex patterns are also more diﬃcult:  

Complexity Sequence  

8 a, , a, z, a, y, a, . . . 10 a, x, , v, w, t, u, . . . 13 a, y, w, , w, u, w, u, s, . . .  

Our main criticism of the C-Test is that it does not require the agent to be able to deal with problems that require interacting with an environment. For example, an agent could have a very high C-Test score due to being a very good sequence predictor, and yet be unable to deal with more general kinds of problems. This falls short of what is required by our informal deﬁnition of intelligence, that is, the ability to achieve goals in a wide range of environments.  

Smith’s Test. Another complexity based formal deﬁnition of intelligence that appeared recently in an unpublished report is due to Smith (2006). His ap- proach has a number of connections to our work, indeed Smith states that his work is largely a “. . . rediscovery of recent work by Marcus Hutter”. Perhaps this is over stating the similarities because while there are some connections, there are also many important diﬀerences.  

The basic structure of Smith’s deﬁnition is that an agent faces a series of problems that are generated by an algorithm. In each iteration the agent must try to produce the correct response to the problem that it has been given. The problem generator then responds with a score of how good the agent’s answer was. If the agent so desires it can submit another answer to the same problem. At some point the agent requests the problem generator to move onto the next problem and the score that the agent received for its last answer to the current problem is then added to its cumulative score. Each interaction cycle counts as one time step and the agent’s intelligence is then its total cumulative score considered as a function of time. In order to keep things feasible, the problems must all be in the complexity class P, that is, decision problems which can be solved by a deterministic Turing machine using a polynomial amount of computation time.  

We have three main criticisms of Smith’s deﬁnition. Firstly, while for prac- tical reasons it might make sense to restrict problems to be in P, we do not see why this practical restriction should be a part of the very deﬁnition of intelli- gence. If some breakthrough meant that agents could solve diﬃcult problems in not just P but sometimes also in the larger complexity class NP, then surely these new agents would be more intelligent? We had similar objections to in- formal deﬁnitions of machine intelligence that included eﬃciency requirements in Section 1.3.  

Our second criticism is similar to that of the C-Test. Although there is some interaction between the agent and the environment, this interaction is rather limited. The problem-answer format of the test is too limited to fully test an agent’s capabilities.  

The ﬁnal criticism is that while the deﬁnition is somewhat formally deﬁned, it still leaves open the important question of what exactly the individual tests should be. Smith suggests that researchers should dream up tests and then contribute them to some common pool of tests. As such, this intelligence test is not fully speciﬁed.  

# 1.8. Conclusion  

Although this chapter provides only a short treatment of the complex topic of intelligence, for a work on artiﬁcial intelligence to devote more than a few paragraphs to the topic is rare. We believe that this is a mistake: if artiﬁcial intelligence research is ever to produce systems with real intelligence, questions of what intelligence actually means and how to measure it in machines need to be taken seriously. At present practically nobody is doing this. The reason, it appears, is that the deﬁnition and measurement of intelligence are viewed as being too diﬃcult. We accept that the topic is diﬃcult, however we do not accept that the topic is so diﬃcult as to be hopeless and best avoided. As we have seen in our survey of deﬁnitions, there are many commonalities across the various proposals. This leads to our informal deﬁnition of intelligence that we argue captures the essence of these. Furthermore, although intelligence tests for humans are widely treated with suspicion by the public, by various metrics these tests have proven to be very eﬀective and reliable when correctly applied. This gives us hope that useful tests of machine intelligence may also be possible. At present only a handful of researchers are working on these problems, mostly in obscurity. No doubt these fundamental issues will someday return to the fore when the ﬁeld is more advanced.  

# 2. Universal Artiﬁcial Intelligence  

Having reviewed what intelligence is and how it is measured, we now turn our attention to artiﬁcial systems that appear to be intelligent, at least in theory. The problem is that although machines and algorithms are becoming progressively more powerful, as yet no existing system can be said to have true intelligence — they simply lack the power, and in particular the breadth, to really be called intelligent. However, among theoretical models which are free of practical concerns such as computational resource limitations, intelli- gent machines can be deﬁned and analysed. In this chapter we introduce a very powerful theoretical model: Hutter’s universal artiﬁcial intelligence agent, known as  AIXI .  

A full treatment of this topic requires a signiﬁcant amount of technical math- ematics. The goal here is to explain the foundations of the topic and some of the key results in the area in a relatively easy to understand fashion. For the full details, including precise mathematical deﬁnitions, proofs and connections to other ﬁelds, see (Hutter, 2005), or for a more condensed presentation (Hut- ter, 2007b). At this point the reader may wish to browse Appendix A that describes the mathematical notation and conventions used in this thesis.  

# 2.1. Inductive inference  

Inductive inference is the process by which one observes the world and then infers the causes behind what has been observed. This is a key process by which we try to understand the universe and so naturally examples of it abound. Indeed much of science can be viewed as a process of inductively inferring natural causes. For example, at a microscopic level, one may ﬁre sub-atomic particles into a gas chamber, observe the patterns they trace out, and then try to infer what the underlying principles are that govern these events. At a larger scale one may observe that global temperatures are changing along with other atmospheric conditions, and from this information attempt to infer what processes may be driving climate change.  

Science is not the only domain where inductive inference is important. A businessman may observe stock prices over time and then attempt to infer a model of this process in order to predict the market. A parent may return home from work to discover a chair propped against the refrigerator with the cookie jar on top a little emptier. Whether we are a detective trying to catch a thief, a scientist trying to discover a new physical law, or a businessman attempting to understand a recent change in demand, we are all in the process of collecting information and trying to infer the underlying causes.  

Formally we can abstract the inductive inference problem as follows: An agent has observed some data    $\mathcal{D}:=x_{1},x_{2},.\,.\,.\,x_{t}$   and has a set of hypotheses

  ${\mathcal{H}}:=h_{1},h_{2},.\,.\,.\,.$  , som  of which may be good models of the unknown process

  $\mu$    at is generating  D . The task is to decide which hypothesis, or hypotheses in  H  are the most likely to accurately reﬂect  $\mu$  . For example,    $x_{1},x_{2},\ldots.$   might be the market value of a stock over time and    $\mathcal{H}$   might consist of a set of mathematical models of the stock price. Once we have identiﬁed which model or models are likely to accurately describe the price behaviour, we may want to use this information to predict future stock prices. Typically this is the case: Often our goal is not just to understand our observations, but also to be able to predict future observations. It is in prediction that good models become truly useful.  

Inductive inference has a long history in philosophy. An early thinker on the subject was the Greek philosopher Epicurus (342? B.C. – 270 B.C.) who noted that there typically exist many hypotheses which are consistent with all of the available data. Logically then, we cannot use the data to rule out any of these hypotheses; they must all be kept as potential explanations. This is known as  Epicurus’ principle of multiple explanations  and is often stated as,  

Keep all hypotheses that are consistent with the data.  

To illustrate this, consider the cookie jar example again and place yourself in the position of the parent returning home from work. Having observed the chair by the refrigerator and missing cookies, one seemingly likely hypothesis is that your daughter has pushed the chair over to the refrigerator, climbed on top of it, and then removed some cookies. Another hypothesis is that a hungry but unusually short thief picked the lock on the back door, saw the cookie jar and decided to move the chair over to the refrigerator in order to get some cookies. Although this seems much less likely, you cannot completely rule out this possibility, or even more elaborate explanations, based solely on the scene in the kitchen. Philosophically this leaves you in the uncomfortable situation of having to consider all sorts of strange explanations as being theoretically possible given the information available. The need to keep these hypotheses would become clear if you were then to walk into the living room and no- tice that your new television and other expensive items were also missing — suddenly the unlikely seems more plausible.  

Although we may accept that all hypotheses which are consistent with the observed facts should be considered at least possible, it is intuitively clear that some hypotheses are much more likely than others. For example, if you had previously observed similar techniques being employed by your daughter to access the cookie jar, but had never been burgled, it would be natural to consider that the small thief in question was of your own ﬂesh and blood, rather than a career criminal. However, you are basing this judgement on your experience prior to returning home. What if you really had very little prior knowledge? How then should you judge the relative likelihood of diﬀerent hypotheses?  

2.1.1 Example. Consider the following sequence:  

What process do you think is generating these numbers? What do you predict will come next?  

An obvious hypothesis is that these are the positive odd numbers. If this is true then the next number is going to be 9. A more complex hypotheses is that the sequence is being generated by the equation   $2n-1+(n-1)(n-$   $2)(n-3)(n-4)$   for    $n\in\mathbb N$  . In this case the next number would be 33. Even when people are aware that this equation generates a sequence consistent with the digits above, most would not consider it to be very likely at all.  $\diamondsuit$  

The philosophical principle behind this intuition was ﬁrst clearly stated by the English logician and Franciscan friar, William of Ockham (1285 – 1349, also spelt Occam). He argued that when inferring a cause one should not include in the explanation anything that is not strictly required to explain the observations. Or as originally stated, “entia non sunt multiplicanda praeter necessitatem”, which translates as “entities should not be multiplied beyond necessity”. A more modern and perhaps clearer expression is,  

Among all hypotheses consistent with the observations, the simplest is the most likely.  

This philosophical principle is known as  Occam’s razor  as it allows one to cut away unnecessary baggage from explanations. If we consider the number prediction problem again, it is clear that the principle of Occam’s razor agrees with our intuition: the simple hypothesis seemed to be more likely, a priori, than the complex hypothesis. As we saw in the previous chapter, the ability to apply Occam’s razor is a standard feature of intelligence tests.  

# 2.2. Bayes’ rule  

Although fundamental, the principles of Epicurus and Occam by themselves are insuﬃcient to provide us with a mechanism for performing inductive in- ference. A major step forward in this direction came from the English math- ematician and Presbyterian minister, Thomas Bayes (1702 – 1761).  

In inductive inference we seek to ﬁnd the most likely hypothesis, or hypothe- ses, given the data. Expresse  in term  of probability, we seek to ﬁnd    $h\in{\mathcal{H}}$  such that the probability of  h  given  D , written    $P(h|\mathcal{D})$  , is high. From the deﬁnition of conditional probability,    $P(h|\mathcal{D}):=P(h\cap\mathcal{D})/P(\mathcal{D})$  . Rearranging this we get    $P(h|\mathcal{D})P(\mathcal{D})\,=\,P(h\cap\mathcal{D})\,=\,P(\mathcal{D}|h)P(h)$  , from which it follows that,  

$$
P(h|\mathcal{D})=\frac{P(\mathcal{D}|h)P(h)}{P(\mathcal{D})}=\frac{P(\mathcal{D}|h)P(h)}{\sum_{h^{\prime}\in\mathcal{H}}P(\mathcal{D}|h^{\prime})P(h^{\prime})}.
$$  

This equation is known as  Bayes’ rule . It allows one to compute the probabil- ity of diﬀerent hypotheses  $h\in{\mathcal{H}}$   given the observed data  $\mathcal{D}$  , and a distribution  $P(h)$   over    $\mathcal{H}$  . The probability of the observed data,    $P(\mathcal{D})$  , is known as the evidence .    $P(h)$   is known as the  prior distribution  as it is the distribution over the space of hypotheses  before  taking into account the observed data. The distribution    $P(h|D)$   is known as the  posterior distribution  as it is the distri- bution  after  taking the data into account. Thus in essence, Bayes’ rule takes some beliefs that we may have about the world and updates these according to some observed data. In the above formulation we have assumed that the set of hypotheses    $\mathcal{H}$   is countable. For uncountable sets the sum is replaced by an integral.  

Despite its elegance and simplicity, Bayesian inference is controversial. To this day professional statisticians can be roughly divided into Bayesians who accept the rule, and classical statisticians who do not. The debate is a subtle and complex one and there are many diﬀerent positions within each of the two camps. At the core of the debate is the very notion of what probability means, and in particular what the prior probability    $P(h)$   means.  

How can one talk about the probability of a hypothesis before seeing any data? Even if this prior probability is meaningful, how can one know what its value is? We need to take this question seriously because in Bayes’ rule the choice of prior aﬀects the relative values of    $P(h|\mathcal{D})$   for each    $h$  , and thus inﬂuences the inference results. Indeed, if Bayesians are free to choose the prior over  $\mathcal{H}$  , how can they claim to have objective results?  

Bayesians respond to this in a number of ways. Firstly, they point out that the problem is generally small, in the sense that with a reasonable prior and qu tity of data, the posterior distribution    $P(h|\mathcal{D})$   depends almost entirely on  D  rather than the chosen prior    $P(h)$  . In fact on any sizable data set, not only does the choice of prior not especially matter, but Bayesian and classical statistical methods typically produce similar results, as one would expect. It is only with relatively small data sets or complex models that the choice of prior becomes an issue.  

If classical statistical methods could avoid the problem of prior bias when dealing with small data sets then this would be a signiﬁcant argument in their favour. However Bayesians argue that all systems of inductive inference that obey some basic consistency principles deﬁne, either explicitly or implicitly, a prior distribution over hypotheses. Thus, methods from classical statistics make assumptions that are in eﬀect equivalent to deﬁning a prior. The dif- ference is that in Bayesian statistics these assumptions take the form of an explicit prior distribution. In other words, it is not that prior bias in Bayesian statistics is necessarily any better or worse than in classical statistics, it is simply more transparent.  

In practical terms, if priors cannot be avoided, one strategy to reduce the potential for prior selection abuse is to use well known priors whenever possi- ble. To this end many standard prior distributions have been developed. The key desirable property is that a prior should not strongly inﬂuence the poste- rior distribution and thus unduly aﬀect the inference results. This means that the prior should express a high degree of impartiality by treating the various hypotheses somewhat equally. For example, when the set    $\mathcal{H}$   is ﬁnite, an ob- vious choice is to assign equal prior probability to each hypothesis, formally,  $\textstyle P(h):=\,{\frac{1}{|{\mathcal{H}}|}}$    for all    $h\,\in\,{\mathcal{H}}$  . Things become more problematic in inﬁnite hy- |H| pothesis spaces as it is then mathematically impossible to assign an equal ﬁnite probability to each of the hypotheses in    $\mathcal{H}$  , and still have    $P({\mathcal{H}})=1$  . Some Bayesians abandon this condition and use so called  improper priors  which are not true probability distributions. For the classical statistician, such a radical departure from the deﬁnition of probability does not really solve the prob- lem of the unknown prior, rather it suggests that something is fundamentally amiss.  

Instead of mathematical tricks or other workarounds, what Bayesians would ideally like is to solve the unknown prior problem once and for all by having a universal prior distribution. Only then would the Bayesian approach be truly complete. The principles of Epicurus and Occam provide some hints on how this might be done. From Epicurus, whenever a hypothesis is consistent with the data, that is    $P(\mathcal{D}|h)>0$  , we should keep this hypothesis by having  $P(h|\mathcal{D})>0$  . From Bayes’ rule this requires that    $\forall h\in{\mathcal{H}}:P(h)>0$  . From Occam we see that    $P(h)$   should decrease with the complexity of  h , and thus we need a way to measure the complexity of hypotheses. However before continuing with this, we ﬁrst consider the inference problem from another perspective.  

# 2.3. Binary sequence prediction  

An alternate character is ation of inductive inference can be made in terms of binary sequence prediction. One reason this is useful is that binary sequences and strings provide a more natural setting in which to deal with issues of com- putability. The problem can be formulated as follows: There is an unknown probability distribution    over the space of binary sequences    $\mathbb{B}^{\infty}$  . From this  $\mu$  distribution a sequence    $\omega$   is drawn one bit at a time. At time    $t\in\mathbb N$   we have observed the initial string    $\omega_{1:t}:=\omega_{1}\omega_{2}\ldots.\omega_{t}$  , and our task is to predict what the next bit in the sequence will be, that is,    $\omega_{t+1}$  . To do this we select a model, or models, from a set of potential models that explain the observed sequence so far and that, we hope, will be good at predicting future bits in the sequence.  

In terms of inductive inference the observed initial binary string    $\omega_{1:t}$   is the observed d a    $\mathcal{D}$  , and our set of potential mo f the data is the set of hypotheses  H . We would like to ﬁnd a model  ν  ∈H , or models, that are as close as possible to the unknown true model of the data    $\mu$  , in the sense that    $\nu$  will allow us to predict future bits in the sequence as accurately as possible.  

We begin by clarifying what we mean by a  probability distribution . In math- ematical statistics a probability distribution is known as a  probability measure as it belongs to the class of functions known as measures. Over the space of binary strings these can be deﬁned as follows:  

2.3.1 Deﬁnition. A  probability measure  is a function    $\nu:\mathbb{B}^{*}\to[0,1]$   such that,  

$$
\begin{array}{r}{\nu(\underline{{\epsilon}})=1,\qquad\qquad}\\ {\forall x\in\mathbb{B}^{*}\quad\nu(\underline{{x}})=\nu(\underline{{x}}\underline{{0}})+\nu(\underline{{x}}\underline{{1}}).}\end{array}
$$  

In this thesis we will interpret    $\nu(\underline{{x}})$   to mean the probability that a binary sequence sampled according to the distribution    $\nu$   begins with the string  $\boldsymbol{x}\in\mathbb{B}^{\ast}$  . As all strings and sequences begin with the null string    $\epsilon$  , by deﬁnition, the ﬁrst condition above simply says that the    $\nu$   probability that a sequence belongs to the set of all sequences is 1. The second condition says that the    $\nu$   probability that a sequence begins with string    $x0$  , plus the    $\nu$   probability that it begins with  $x1$  , is equal to the    $\nu$   probability that it begins with    $x$  . This makes sense given that all sequences that begin with    $x$   must have either a   $0$   or a 1 as their next bit and so we would expect the probabilities of these sets of sequences to add up. This style of notation for measures will be convenient for our purposes, however it is somewhat unusual. To see how it relates to conventional measure theory see Appendix A.  

Sequence prediction forms a large part of this thesis and thus we will often be interested in what comes next in a sequence given an initial string. More precisely, if a sequence    $\omega$   has been sampled from the distribution    $\mu$  , and    $\omega$  begins with the string    $y\in\mathbb{B}^{*}$  , what is the probability that the next bits from  $\omega$   will be the string    $\boldsymbol{x}\in\mathbb{B}^{\ast}$  ? For this will we adopt the following notation for the conditional probability,    $\mu(y\underline{{x}}):=\mu(\underline{{y}}\underline{{x}})/\mu(\underline{{y}})$  . The beneﬁt of this notation will become apparent later when we need to deal with complex interaction sequences. Not only does it preserve the order in which the sequence occurs, it also allows for more compact expressions when we need to condition on only certain parts of a sequence.  

As noted earlier in this section, sequence prediction can be viewed as an inductive inference problem. Thus, we can use Bayes’ rule to estimate how likely some model    $\nu\in{\mathcal{H}}$   is given the observed sequence    $\omega_{1:t}$  :  

$$
P\big(\nu|\omega_{1:t}\big)=\frac{P(\underline{{\omega}}_{1:t}|\nu)P(\nu)}{P(\underline{{\omega}}_{1:t})}=\frac{\nu(\underline{{\omega}}_{1:t})P(\nu)}{\sum_{\varrho\in\mathcal{H}}\varrho(\underline{{\omega}}_{1:t})P(\varrho)}.
$$  

2.3.2 Example.  Consider the problem of inferring whether a coin is a nor- mal fair coin based on a sample of coin ﬂips. To simplify things, assume that the coin is either heads on both sides, tails on both sides, or a nor- mal fair coin. Further assume that    $t~=~4$   and we have the observed data  $\mathcal{D}=\mathtt{h e a d}$  ,  head ,  head ,  head . In terms of binary sequence prediction, the out- come of  t  coin tosses can be expressed as a s ring    $\omega_{1:t}\in\mathbb{B}^{t}$  , with  ng represented as a 0 bit, and each head as a 1 bit. Thus we have  $\omega_{1:4}=1111$   = 1111.  

Let    $\mathcal{H}$   be the set of models consisting of the distributions    $\nu_{p}(\underline{{\omega}}_{1:t}):=p^{r}(1-$   $p)^{t-r}$  , where    $p\in\left\{0,{\frac{1}{2}},1\right\}$  and    $\begin{array}{r}{r:=\sum_{i=1}^{t}\omega_{i}}\end{array}$   is the number of observed heads.  

As there are just three models in    $\mathcal{H}$  , assume a uniform prior, that is,    $\forall p\in$   $\left\{0,{\textstyle\frac{1}{2}},1\right\}$  let    $\begin{array}{r}{P(\nu_{p}):=\frac{1}{3}}\end{array}$  . Now from Bayes’ rule,  

$$
P(\nu_{\frac{1}{2}}|\omega_{1:4}=1111)\ =\ \frac{\frac{1}{3}\left(\frac{1}{2}\right)^{4}\left(1-\frac{1}{2}\right)^{0}}{\frac{1}{3}\left[0^{4}(1-0)^{0}+\left(\frac{1}{2}\right)^{4}\left(1-\frac{1}{2}\right)^{0}+1^{4}(1-1)^{0}\right]}\ =\ \frac{1}{17}.
$$  

Similarly,  $P(\nu_{0}|\omega_{1:4}=1111)=0$   and    $\begin{array}{r}{P(\nu_{1}|\omega_{1:4}=1111)=\frac{16}{17}}\end{array}$  . Thus the results clearly point towards the coin being double headed, as we would expect having just observed four heads in a row.  $\diamondsuit$  

More complex examples could involve data collected from medical measure- ments, music or weather satellite images. Good models would then have to describe biological processes, music styles, or the dynamics of weather systems respectively. In each case the binary string representing    $\mathcal{D}$   could be simply the string of bits as they would appear in a computer ﬁle. However, ﬁnding a good prior over such spaces is not trivial. Furthermore, actually computing Bayes’ rule and ﬁnding the most likely models, as we did in the example above, can become very computationally diﬃcult and may need to be approximated. In any case, Bayes’ rule at least tells us how to solve the induction problem in theory, so long as we have a prior distribution.  

Rather than just estimating which model or models are the most likely, we may be interested in actually predicting the sequence. One possibility is to calculate the probability that the next bit is a 1 based on the most likely model. The full Bayesian approach, however, is to consider each possible model    $\nu\in\mathcal H$  and weight the prediction made by each according to how conﬁdent we are about each model, i.e.    $P(\nu|\omega_{1:t})$  . This gives us the  mixture model predictor ,  

$$
\begin{array}{r c l}{P(\omega_{1:t}\underline{{1}})}&{=}&{\displaystyle\sum_{\nu\in\mathcal{H}}P(\nu|\omega_{1:t})\,\nu(\omega_{1:t}\underline{{1}})}\\ &{=}&{\displaystyle\sum_{\nu\in\mathcal{H}}\frac{\nu(\underline{{\omega}}_{1:t})P(\nu)}{P(\underline{{\omega}}_{1:t})}\,\frac{\nu(\underline{{\omega}}_{1:t}\underline{{1}})}{\nu(\underline{{\omega}}_{1:t})}~=~\frac{P(\underline{{\omega}}_{1:t}\underline{{1}})}{P(\underline{{\omega}}_{1:t})}.}\end{array}
$$  

As we can see, the Bayes mixture predictor reduces to the deﬁnition of condi- tional probability. This has  moved the prior over    $\mathcal{H}$  , and in its place we now have the related prior over  D , in this setting the space of binary sequences. The fact that we can use one prior to deﬁne the other means that the two unknown priors are in fact two perspectives on the same fundamental problem of specifying our prior knowledge.  

2.3.3 Example.  Continuing Example 2.3.2, we can compute the prior distri- bution over sequences from the prior distribution over  $\mathcal{H}$  ,  

$$
\begin{array}{l c l}{P(\underline{{\omega}}_{1:t})}&{=}&{\displaystyle\sum_{\nu\in\mathcal{H}}\nu(\underline{{\omega}}_{1:t})P(\nu)}\\ &{=}&{\displaystyle\frac{1}{3}\left[0^{t}(1-0)^{t-r}+\left(\frac{1}{2}\right)^{t}\left(1-\frac{1}{2}\right)^{t-r}+1^{t}(1-1)^{t-r}\right]}\\ &{=}&{\displaystyle\frac{1}{3}\left[\left(\frac{1}{2}\right)^{2t-r}+\delta_{t r}\right].}\end{array}
$$  

$\begin{array}{r}{r:=\sum_{i=1}^{t}\omega_{i}}\end{array}$   is the number of observed heads, and the  Kronecker delta symbol  δ  $\delta_{a b}$   is deﬁned to be 1 if    $a\,=\,b$  , and 0 otherwise. Thus, given that  = 1111, according to the mixture model the probability that the next bit is a   $1$   is,  

$$
P(1111{\underline{{1}}})={\frac{{\frac{1}{3}}\left[\left({\frac{1}{2}}\right)^{2(5)-5}+\delta_{5,5}\right]}{{\frac{1}{3}}\left[\left({\frac{1}{2}}\right)^{2(4)-4}+\delta_{4,4}\right]}}={\frac{\left({\frac{1}{2}}\right)^{5}+1}{\left({\frac{1}{2}}\right)^{4}+1}}={\frac{33}{34}}.
$$  

# 2.4. Solomonoﬀ’s prior and Kolmogorov complexity  

In the 1960’s Ray J. Solomonoﬀ(1926–) investigated the problem of inductive inference from the perspective of binary sequence prediction (Solomonoﬀ1964; 1978). He was interested in a very general form of the problem, speciﬁcally, learning to predict a binary sequence that has been sampled from an arbitrary unknown computable distribution. Solomonoﬀdeﬁned his prior distribution over sequences as follows: The prior probability that a sequence begins with a string    $\boldsymbol{x}\,\in\,\mathbb{B}^{\ast}$  is the probability that a universal Turing machine running a randomly generated program computes a sequence that begins with    $x$  . By randomly generated, we mean that the bits of the program have a uniform distribution, for example, they could come from ﬂipping a fair coin. Formally,  

2.4.1 Deﬁnition. The  Solomonoﬀprior probability  that a sequence begins with the string    $\boldsymbol{x}\in\mathbb{B}^{\ast}$  is,  

$$
M(\underline{{x}}):=\sum_{p:\mathcal{U}(p)=x*}2^{-\ell(p)},
$$  

where    ${\mathcal{U}}(p)\,=\,x*$  means that the universal Turing machine    $\mathcal{U}$   computes an output sequence that begins with    $\boldsymbol{x}\in\mathbb{B}^{\ast}$  when it runs the program   , and    $\ell(p)$   $p$  is the length of    $p$   in bits.  

Note that the   $2^{-\ell(p)}$    term in this deﬁnition comes from the fact that the probability of    $p$   under a uniform distribution halves for each additional bit.  

We will assume that    $\mathcal{U}$  s a  preﬁx universal Turing machine . This means that no valid program  r  U  is a preﬁx of any other. More pre if    $p,q\in\mathbb{B}^{*}$  are valid programs on  U , then there does not exist a string  x  $\boldsymbol{x}\in\mathbb{B}^{\ast}$   ∈ ∗ such that  $p\,=\,q x$  . Preﬁx universal Turing machines have technical properties that we will need an so throughout this thesis we will assume that    $\mathcal{U}$   is of this type. Technically,  U  is actually a type of preﬁx universal Turing machine known as a  monotone universal Turing machine  (see Section 5.1). For the moment we can safely gloss over these details.  

2.4.2 Example. Rather than a classic universal Turing machine running a program speciﬁed by a binary string on an input tape, it is often more intuitive to think in terms of a program written in a high level programing language that is being executed on a real computer. Indeed, if a computer had inﬁnite memory and never broke down it would be technically equivalent to a universal Turing machine. Consider a short program in C that prints a binary sequence of all 1’s:  

# main(){while(1)printf("1");}  

As far as C programs go, this is nearly as simple as they get. This is not surprising given that the output is also very simple. If we want a program that generates a more complex sequence, such as an inﬁnite sequence of successive digits of the mathematical constant    $\pi=3.141592\ldots.$  , such a program would be at least ten times as long. It follows then that the probability of randomly generating a program that outputs all   $1$  ’s is far higher than the probability of randomly generating a program that computes    $\pi$  . Thus, Solomonoﬀ’s prior assigns much higher probability to the sequences of all   $1$  ’s than to the sequence for    $\pi$  . More complex sequences would require still larger programs and thus have even lower prior probability.  $\diamondsuit$  

Although Solomonoﬀ’s deﬁnition requires nothing more than random bits being fed into a universal Turing machine, we can see that the resulting dis- tribution over sequences neatly formalises Occam’s razor. Speciﬁcally, sets of sequences that have short programs, and are thus in some sense simple, are given higher prior probability than sets of sequences that have only long programs.  

The idea that the complexity of a sequence is related to the length of the shortest program that generates the sequence motivates the following deﬁni- tion:  

2.4.3 Deﬁnition. The  Kolmogorov complexity  of a sequence    $\omega\in\mathbb{B}^{\infty}$  is,  

$$
K(\omega)\,:=\,\operatorname*{min}_{p\in\mathbb{B}^{*}}\{\,\ell(p):\mathcal{U}(p)=\omega\},
$$  

where    $\mathcal{U}$   is a preﬁx univers ng machine. If no such    $p$   exists, we deﬁne  $K(\omega)\,=\,\infty$  . For a string  x  $\boldsymbol{x}\ \in\ \mathbb{B}^{*}$   ∈ ∗ , we deﬁne    $K(x)$   to be the length of the shortest program that outputs    $x$   and then halts.  

Kolmogorov complexity has many powerful theoretical properties and is a central ingredient in the theory of universal artiﬁcial intelligence. Its most important property is that the complexity it assigns to strings and sequences does not depend too much on the choice of the universal Turing machine    $\mathcal{U}$  . This comes from the fact that universal Turing machines are universal in the sense that they are able to simulate each other with a constant number of additional inpu its. Thus, if we change    $\mathcal{U}$   above to some other universal Turing machine  U ′ , the minimal value of    $\ell(p)$   and thus    $K(x)$  , c  only change by a bounded number of bits. This bound depends on  U  and  U , but not on  $x$  .  

The biggest problem with Kolmogorov complexity is that the value of    $K$  is not in general computable. It can only be approximated from above. The reason for this is that in general we cannot ﬁnd the shortest program to com- pute a string    $x$   on    $\mathcal{U}$   due to the halting problem. Intuitively, there might exist a very short program    $p^{*}$  such that    $\mathcal{U}(p^{*})=x$  , however we do not know this because    $p^{*}$  takes such a long time to run. Nevertheless, in theoretical appli- cations the simplicity and theoretical power of Kolmogorov complexity often outweighs this computability problem. In practical applications Kolmogorov complexity is approximated, for example by using a compression algorithm to estimate the length of the shortest program (Cilibrasi and Vit´ anyi, 2005).  

# 2.5. Solomonoﬀ-Levin prior  

Besides the prior described in the previous section, Solomonoﬀalso suggested to deﬁne a universal prior by taking a mixture of distributions (Solomonoﬀ, 1964). In the 1970’s this alternate approach was generalised and further de- veloped (Zvonkin and Levin, 1970; Levin, 1974). As Leonid Levin (1948–) played an important role in this, here we will refer to this as the  Solomonoﬀ- Levin prior . It is closely related to the universal prior in the previous section: they lie within a multiplicative constant of each other and share key technical properties.  

Although taking mixtures is perhaps less intuitive, it has the advantage of making important theoretical properties of the prior more transparent. It also gives an explicit prior over both the hypothesis space and the space of sequences. The topic is quite technical, however it is worth spending some time on as it lies at the heart of universal artiﬁcial intelligence.  

The hypotheses we have been working with up to now have all been proba- bility measures. These can be generalised as follows:  

2.5.1 Deﬁnition. A  semi-measure  is a function    $\nu:\mathbb{B}^{*}\to[0,1]$   such that,  

$$
\nu(\underline{{\epsilon}})\leq1,
$$  

#  $\forall x\in\mathbb{B}^{*}\quad\nu(\underline{{x}})\geq\nu(\underline{{x}}\underline{{0}})+\nu(\underline{{x}}\underline{{1}}).$  .  

Intuitively one may think of a semi-measure that is not a probability measure as being a kind of defective probability measure whose probabilities do not quite add up as they should. This defect can be ﬁxed in the sense that a semi-measure can be built up to be a probability measure by appropriately normalising things.  

Intuitively, a function is  enumerable  if it can be progressively approximated from below. More formally,    $f:X\to\mathbb{R}$   is enumerable if there exists a com- putable function    $g:X\times\mathbb{N}\to\mathbb{Q}$   such that    $\forall x\in X,\forall i\in\mathbb{N}:g_{i+1}(x)\geq g_{i}(x)$  and  $\forall x\in X:\operatorname*{lim}_{i\to\infty}g_{i}(x)=f(x)$  . Enumerability is weaker than computabil- ity because for any    $x\,\in\,X$   we only ever have the lower bound    $g_{i}(x)$   on the value of    $f(x)$  . Thus we can never know for sure how far our bound is from the true value of    $f(x)$  , that is, we do not know how large    $f(x)-g_{i}(x)$   might be.  

If a similar condition holds, but with the approximation function converging to    $f$   from above rather than below, we say that    $f$   is  coenumerable . One example of such a function is the Kolmogorov complexity function    $K$   in the previous section. If a function is both enumerable and coenumerable, then we have both upper and lower bounds and thus can compute the value of    $f$  to any required accuracy. In this case we simply say that    $f$   is a real valued computable function . Clearly then, the enumerable functions are a superset of the computable functions.  

Our task is to construct a prior distribution over the enumerable semi- measures. To do this we need to formalise Occam’s razor, and for that we need to deﬁne a way to measure the complexity of enumerable semi-measures. Solomon o measured the complexity of sequences according to the length of their programs, here we can do something similar.  

By deﬁnition, all enumerable functions can be approximated from below by a computable function. Thus, it is not too hard to prove that the set of enumerable functions can be indexed by a Turing machine, and that this can be further restricted to just the set of enumerable semi-measures. More precisely, there exists a Turing machine  $T$   that for any enumerable semi-measure    there  $\mu$  exist an index    $i\in\mathbb N$   such that    $\begin{array}{r}{\forall x\in\mathbb{B}^{*}:\mu(\underline{{x}})=\nu_{i}(\underline{{x}}):=\operatorname*{lim}_{k\to\infty}T(i,k,x)}\end{array}$  with  T  increasing in  k . In eﬀect, the index    $i$   is a description of    in that once  $\mu$  we know    $i$   we can approximate the value of    from below for any    by using  $\mu$   $x$  the Turing machine    $T$  . As    $k$   increases, these approximations increase towards the true value of    $\mu(x)$  . For details on how all this is done see Section 4.5 of (Li and Vit´ anyi, 1997) or (Legg, 1997). The main thing we will need is the computable enumeration of enumerable semi-measures itself, which we will denote by    $\mathcal{M}_{e}:=\nu_{1},\nu_{2},\nu_{3},.\,.\,.$  . As all probability measures are semi-measures by deﬁnition, and all computable functions are enumerable, it follows that the set of enumerable semi-measures is a superset of the set of computable probability measures. We write    $\mathcal{M}_{c}$   to denote an enumeration of just the computable measures.  

Note the two uses of the word “enumerable” above. When we say that a set can be enumerated, what we mean is that there exists a way to step though all the elements in this set. When we say that an individual function is enumer- able, such as a semi-measure, what we mean is that it can be approximated from below by a series of computable functions. Some authors avoid this dual usage by referring to enumerable functions as  lower semi-computable . In this terminology, what    $\mathcal{M}_{e}$   provides is a computable enumeration of the lower semi-computable semi-measures. In any case, it is still a mouthful!  

We can now return to the question of how to measure the complexity of an enumerable semi-measure. As noted above, for    $\nu_{i}\in\mathcal{M}_{e}$   the index    $i$   is in eﬀect a description of    $\nu_{i}$  . At this point, it might seem that the natural thing to do is to take the value of an enumerable semi-measure’s index to be its complexity. The problem, however, is that some extremely large index values, such as   $2^{1000}$  , contain a lot less information than far smaller index values which are not as easily described: for example, an index whose binary representation is a string of 100 random bits. The solution is that we must measure not the value, but the information content of the index in order to measure the complexity of the enumerable semi-measure it describes. We do this by taking the Kolmogorov complexity of the index. That is, we deﬁne the complexity of an enumerable semi-measure to be the length of the shortest program that computes its index. Formally,  

2.5.2 Deﬁnition. The  Kolmogorov complexity  of    $\mu\in\mathcal{M}_{e}$   is,  

$$
K(\mu)\,:=\,\displaystyle\operatorname*{min}_{p\in\mathbb{B}^{*}}\,\{\,\ell(p):\mathcal{U}(p)=i\,\},
$$  

where    $\mu$   is the    $i^{t h}$    element in the recursive enumeration of all enumerable semi-measures    $\mathcal{M}_{e}$  , and    $\mathcal{U}$   is a preﬁx universal Turing machine.  

In essence this is just an extension of the Kolmogorov complexity function for strings and sequences (Deﬁnition 2.4.3), to enumerable semi-measures. Indeed, all the key theoretical properties of the complexity function remain the same. We can again see echos of Solomonoﬀ’s prior for sequences in that enumerable semi-measures that can be described by short programs are considered to be simple, while ones that require long programs are complex.  

Having deﬁned a suitable complexity measure for enumerable semi- measures, we can now construct a prior distribution over    $\mathcal{M}_{e}$   in a way that is similar to what Solomonoﬀdid. As each enumerable semi-measure    $\mu$   has some shortest program    $p\in\mathbb{B}^{*}$  that speciﬁes its index, we can set the prior probability of    $\mu$   to be the probability of randomly generating    $p$   by ﬂipping a coin to get each bit. Formally:  

2.5.3 Deﬁnition. The  algorithmic prior probability  of    $\mu\in\mathcal{M}_{e}$   is,  

$$
P_{\mathcal M_{e}}(\mu)\,:=\,2^{-K(\mu)}.
$$  

As    $K$   is coenumerable, from the deﬁnition above we can see that    $P_{\mathcal{M}_{e}}$   is enumerable. Thus, this distribution can only be approximated from below.  

With    $K$   as the deﬁnition of hypothesis complexity,    $P_{\mathcal{M}_{e}}$   clearly respects Occam’s razor as each hypothesis    $\mu\in\mathcal{M}_{e}$   is assigned a prior probability that is a decreasing function of its complexity. Furthermore, every enumerable semi-measure has some shortest program that speciﬁes its index and so    $\forall\mu\in$   $\mathcal{M}_{e}:K(\mu)>0$   and thus    $\forall\mu\in\mathcal{M}_{e}:P_{\mathcal{M}_{e}}(\mu)>0$  . It follows then that for an induction system based on  ayes’ rule and the prior  $P_{\mathcal{M}_{e}}$  , the value of    $P(\nu|\mathcal{D})$  will be non-zero whenever  D  is consistent with    $\nu$  , that is,    $P(\mathcal{D}|\nu)>0$  . Such systems will not discard hypotheses that are consistent with the data and thus respect Epicurus’ principle of multiple explanations.  

With a prior over our hypothesis space    $\mathcal{M}_{e}$  , we can now take a mixture to deﬁne a prior over the space of sequences, just as we did in Example 2.3.3:  

2.5.4 Deﬁnition. The  Solomonoﬀ-Levin prior probability  of a binary se- quence beginning with the string    $\boldsymbol{x}\in\mathbb{B}^{\ast}$  is,  

$$
\xi(\underline{{x}})\,:=\sum_{\nu\in\mathcal{M}_{e}}P_{\mathcal{M}_{e}}(\nu)\,\nu(\underline{{x}}).
$$  

Clearly this distribution respects Occam’s razor as sets of sequences which have high probability under some simple distribution    $\nu$  , will also have high probability under    $\xi$  , and vice versa.  

It is easy to see that the presence of just one semi-measure in the above mixture is suﬃcient to cause    $\xi$   to also be a semi-measure, rather than a prob- ability measure. Furthermore, it can be proven that    $\xi$   is enumerable but not computable. Thus, we have that    $\xi\in\mathcal{M}_{e}$  . The fact that    $\xi$   is not a probabil- ity measure is not too much of a problem because, as mentioned earlier, it is possible to normalise a semi-measure to convert it into a probability measure. In situations where we need a universal probability measure the normalised version of    $\xi$   is useful. Its main drawback is that it is no longer enumerable, and thus no longer a member of    $\mathcal{M}_{e}$  . In most theoretical applications it is usual to work with the plain    $\xi$   as deﬁned above.  

A fundamental result is that the two priors are strongly related:  

2.5.5 Theorem.  The Solomonoﬀprior    $M$   and Solomonoﬀ-Levin prior    $\xi$   lie within a multiplicative constant of each other. That is,    $M\stackrel{\times}{=}\xi$  .  

Due to this relation, in many theoretical applications the diﬀerences between the two priors are unimportant. Indeed, it is their shared property of domi- nance that is the key to their theoretical power:  

2.5.6 Deﬁnition. For some set of semi-measures    $\mathcal{M}$  , we say that    $\nu\in\mathcal{M}$   is dominant  if    $\forall\varrho\in\mathcal{M}$   there exists a constant    $c_{\varrho}>0$   such that    $\forall x:c_{\varrho}\,\nu({\underline{{x}}})\,\geq$   $\varrho(\underline{{x}})$  . Or more compactly,    $\forall\varrho\in\mathcal{M}:\nu\stackrel{\times}{\geq}\varrho$   ̺ .  

It is easy to see that    $\xi$   is dominant over the set of enumerable semi-measures from its construction: for    $\boldsymbol{x}\in\mathbb{B}^{\ast}$  we have    $\xi(\underline{{x}})\geq P_{\mathcal{M}_{e}}(\mu)\,\mu(\underline{{x}})=2^{-K(\mu)}\mu(\underline{{x}})$  . It fol ws then that  $\forall\mu\in\mathcal{M}_{e},\forall x\in\mathbb{B}^{*}:2^{K(\mu)}\xi(\underline{{x}})\geq\mu(\underline{{x}})$  . As    $M\stackrel{\times}{=}\xi$  , we see that  M  is also dominant. We call distributions that are dominant over large spaces, such as    $\mathcal{M}_{e}$  ,  universal priors . This is due to their extreme generality and performance as prior distributions, something that we will explore in the next two sections: ﬁrstly in the context of Bayesian theory in general, and then in the context of sequence prediction.  

# 2.6. Universal inference  

As we saw in Section 2.2, Bayes’ rule partially solves the induction problem by providing an equation for updating beliefs. This is only a partial solution because it leaves open two important issues: how do we choose the class of hypotheses    $\mathcal{H}$  , and what prior distribution    $P_{\mathcal{H}}$   should we use over this class? Various principles and methods have been proposed to solve these problems, however they tend to run into trouble, especially for large    $\mathcal{H}$  . In this section we will look at how taking the universal prior    $P_{\mathcal{M}_{e}}$   over the hypothesis space  $\mathcal{M}_{e}$   theoretically solves these problems.  

When approaching an inductive inference problem from a Bayesian perspec- tive, the ﬁrst step is to deﬁne    $\mathcal{H}$  . The most obvious consideration is that    $\mathcal{H}$  should be large enough to contain the correct hypothesis, or at least a suﬃ- ciently close one. Being forced to expand our initial    $\mathcal{H}$   due to new evidence is problematic as both the redistribution of the prior probabilities, and the way in which    $\mathcal{H}$   is extended, can bias the  duction process. In order to avoid these problems, we should make sure that  H  is large enough to contain a good hypothesis to start with. One solution is to simply choose    $\mathcal{H}$   to be very large, as we did in the previous section where we set    $\mathcal{H}=\mathcal{M}_{e}$  . As this contains  all computable stochastic hypotheses, a larger hypothesis space should never be required.  

Having selected    $\mathcal{H}$  , the next problem is to deﬁne a good prior distribution over this space. Essentially, a prior distribution    $P_{\mathcal{H}}$   over a space of hypotheses  $\mathcal{H}$   is an expression of how likely we think diﬀerent hypotheses are  before  taking the data into account. If this prior knowledge is easily quantiﬁable we can use it to construct a prior. In the case of    $P_{\mathcal{M}_{e}}$   this simply means taking the conditional form of the Kolmogorov complexity function and conditioning on this prior information.  

More often, however, we either have insuﬃcient prior information to con- struct a prior, or we simply wish the data to ‘speak for itself’. The latter case is important when we want to present our ﬁndings to others who may not share our prior beliefs. The standard solution is to select a prior that is in some sense neutral about the relative likelihood of diﬀerent hypotheses. This is known as the  indiﬀerence principle . It is what we applied in the coin estimation problem in Example 2.3.2 when we set    $\begin{array}{r}{P(\nu_{p}):=|\mathcal{H}|^{-1}=\frac{1}{3}}\end{array}$  . That is, we simply assumed that a priori all hypotheses were equally likely.  

The indiﬀerence principle works well for small discrete    $\mathcal{H}$  , however if we extend the concept to even small continuous parametrised classes by deﬁning a probability density function in terms of the volume of    $\mathcal{H}$  , problems start to arise. Consider again the coin problem, however this time allow the bias of the coin to be    $\theta\in[0,1]$  . By the indiﬀerence principle the prior is the uniform probability density    $P(\nu_{\theta})=1$  . Now consider what happens if we look at this coin estimation problem in a diﬀerent way, where the parameter of interest is actually    $\theta^{\prime}\,:=\,\sqrt{\theta}$  √ . Obviously, if we take a uniform prior over    $\theta^{\prime}$    this is not equivalent to taking a uniform prior over    $\theta$  . In other words, the way in which we view a problem, and thus the way in which we parametrise it, aﬀects the prior probabilities assigned by a ‘uniform prior’. Obviously this is not as neutral and objective as we would like.  

Consider how the algorithmic prior probability    $P_{\mathcal{M}_{e}}$   behaves under a simple re para met r is ation. Let    ${\mathcal{H}}\;:=\;\{\nu_{\theta}\;\in\;{\mathcal{M}}_{c}\;:\;\theta\;\in\;\Theta\}$   b  probab ity measures indexed by a parameter  θ  $\theta\,\in\,\Theta$   ∈ Θ, and deﬁne  θ  $\theta^{\prime}:=\;f(\theta)$  ) where  f  is a computable bijection. It is an elementary fact of Kolmogorov complexity theory that    $K(f(\theta))\,\stackrel{+}{<}\,K(\theta)\,+\,K(f)$  ), and similarly    $K(f^{-1}(\theta^{\prime}))\,\stackrel{+}{<}\,K(\theta^{\prime})\,+$   $K(f)$  , from which it follows that    $K(\theta)\;\stackrel{+}{=}\;K(\theta^{\prime})$  ). With a straight forward extension, the same argument can be applied to the Kolmogorov complexity of the indexed measures, resulting in    $K\bigl(\nu_{\theta}\bigr)\stackrel{+}{=}K\bigl(\nu_{\theta^{\prime}}\bigr)$  ). From Deﬁnition 2.5.3 we then see that,  

$$
P_{\mathcal M_{e}}\bigl(\nu_{\theta}\bigr)\,:=\,2^{-K(\nu_{\theta})}\,\stackrel{\times}{=}\,2^{-K(\nu_{\theta^{\prime}})}\,=:\,P_{{\mathcal M}_{e}}\bigl(\nu_{\theta^{\prime}}\bigr).
$$  

That is, for  any  bijective re para met r is ation    $f$   the algorithmic prior probability assigned to parametrised hypotheses is invariant up to a multiplicative con- stant. If    $f$   is simple this constant is small and thus quickly washes out in the posterior distribution, leaving the inference results essentially unaﬀected by the change.  

A more diﬃcult version of the above problem occurs when the transforma- tion is non-bijective. For example, deﬁne the new parameter    $\theta^{\prime}:=(\theta-{\textstyle\frac{1}{2}})^{2}$  . Now    $\theta\ =\ {\textstyle{\frac{1}{4}}}$    and    $\theta\:=\:\frac{3}{4}$    both correspond to the same value of  θ . Unlike in the bijective case, non-bijective transformations also cause problems for ﬁnite discrete hypothesis spaces. For example, we might have three hypothe- ses,    ${\mathcal{H}}_{3}\;:=\;\left\{\begin{array}{l l}{{\begin{array}{r l r}\end{array}}}\end{array}\right.$  heads biased ,  tails biased ,  fair } . Alternatively, we could regroup to have just two hypotheses,    $\mathcal{H}_{2}:=\mathtt{\{b i a s e d,f a i r\}}$  . Both    ${\mathcal{H}}_{3}$   and  $\mathcal{H}_{2}$   cover the full range of possibilities for the coin. However, a uniform prior over    ${\mathcal{H}}_{3}$   assigns a prior probability of  $\frac13$    to the coin being fair, while a uniform prior over  $\mathcal{H}_{2}$   assigns a prior probability of just    $\frac{1}{2}$    to the same thing.  

The standard Bayesian approach is to try to ﬁnd a symmetry group for the problem and a prior that is invariant under group transformations. However, in some cases there may be no obvious symmetry, and even if there is the resulting prior may be improper, meaning that the area under the distribution is no longer 1. Invariance under group transformations is a highly desirable but diﬃcult property to attain. Remarkably, under simple group transformations  $P_{\mathcal{M}_{e}}$   can be proven to be invariant, again up to a small multiplicative constant. For a proof of this, as well as further powerful properties of the universial prior distribution, see the paper that this section is based on (Hutter, 2007a).  

# 2.7. Solomon o induction  

Given a prior distribution    $\xi$   over  $\mathbb{B}^{\infty}$  , it is straightforward to predict the contin- uation of a binary sequence using the same approach as we used in Section 2.3. rior distribution    $\xi$   and the observed string    $\omega_{1:t}\in\mathbb{B}^{\infty}$  from a sequence

  $\omega\,\in\,\mathbb{B}^{\infty}$   ∈ ∞ that has been sampled from an unknown computable distribution

  $\mu\in\mathcal{M}_{c}$  , our estimate of the probability that the next bit will be   $0$   is,  

$$
\xi(\omega_{1:t}\underline{{0}})=\frac{\xi(\underline{{\omega}}_{1:t}\underline{{0}})}{\xi(\underline{{\omega}}_{1:t})}.
$$  

Is this predictor based on    $\xi$   any good? By deﬁnition, the best possible predictor would be based on the unknown true distribution    $\mu$   that    $\omega$   has been sampled from. That is, the true probability that the next bit is a   $0$   given an observed initial string    $\omega_{1:t}$   is,  

$$
\mu(\omega_{1:t}\underline{{0}})=\frac{\mu(\underline{{\omega}}_{1:t}\underline{{0}})}{\mu(\underline{{\omega}}_{1:t})}.
$$  

As this predictor is optimal by construction, it can be used to quantify the relative performance of the predictor based on    $\xi$  . For example, consider the expected squared error in the estimated probability that the    $t^{t h}$    bit will be a  $0$  :  

$$
S_{t}=\sum_{x\in\mathbb{B}^{t-1}}\mu(\underline{{x}})\big(\xi(x\underline{{0}})-\mu(x\underline{{0}})\big)^{2}.
$$  

If    $\xi$   is a good predictor, then its predictions should be close to those made by the optimal predictor    $\mu$  , and thus    $S_{t}$   will be small.  

Solomonoﬀ(1978) was able to prove the following remarkable convergence theorem:  

2.7.1 Theorem.  For any computable probability measure    $\mu\in\mathcal{M}_{c}$  ,  

$$
\sum_{t=1}^{\infty}S_{t}\;\leq\;\frac{\ln2}{2}\,K(\mu).
$$  

That is, the total of  all  the prediction errors over the length of the inﬁ- nite sequence    $\omega$   is bounded by a constant. This implies rapid convergence for any  unknown hypothesis that can be described by a computable distribution (for a precise analysis see Hutter, 2007a). This set includes all computable hypotheses over binary strings, which is essentially the set of all well deﬁned hypotheses. If it were not for the fact that the universal prior    $\xi$   is not com- putable, Solomon o induction would be the ultimate all purpose universal predictor.  

Although we will not present Solomonoﬀ’s proof, the following highlights the key step required to obtaining the convergence result. For any probability measure    $\mu$   the following relation can be proven,  

$$
\sum_{t=1}^{n}S_{t}\ \leq\ \frac{1}{2}\sum_{x\in\mathbb{B}^{n}}\mu(\underline{{x}})\ln\frac{\mu(\underline{{x}})}{\xi(\underline{{x}})}.
$$  

This in fact holds for any semi-measure    $\xi$  , thus no special properties of the universal distribution have been used up to this point in the proof. Now, by the universal dominance property of  $\xi$  , we know that  $\forall x\in\mathbb{B}^{*}:\xi(\underline{{x}})\geq2^{-K(\mu)}\mu(\underline{{x}})$  . Substituting this into the above equation,  

$$
\sum_{t=1}^{n}S_{t}\ \leq\ \frac{1}{2}\sum_{x\in\mathbb{B}^{n}}\mu(\underline{{x}})\ln\frac{\mu(\underline{{x}})}{2^{-K(\mu)}\mu(\underline{{x}})}\ =\ \frac{\ln2}{2}K(\mu)\sum_{x\in\mathbb{B}^{n}}\mu(\underline{{x}})\ =\ \frac{\ln2}{2}K(\mu).
$$  

As this holds for all    $n~\in~\mathbb{N}$  , the result follows. It is this application of dominance to obtain powerful convergence results that lies at the heart of Solomon o induction, and indeed universal artiﬁcial intelligence in general.  

Although Solomon o induction is not computable and is thus impractical, it nevertheless has many connections to practical principles and methods that are used for inductive inference. Clearly, if we deﬁne a computable prior rather than    $\xi$  , we recover normal Bayesian inference. If we deﬁne our prior to be uni- form, for example by assuming that all models have the same complexity, then the result is maximum a posteriori (MAP) estimation, which in turn is related to maximum likelihood (ML) estimation. Relations can also be established to Minimum Message Length (MML), Minimum Description Length (MDL), and Maximum entropy (ME) based prediction (see Chapter 5 of Li and Vit´ anyi, 1997). Thus, although Solomon o induction does not yield a prediction al- gorithm itself, it does provide a theoretical framework that can be used to understand various practical inductive inference methods. It is a kind of ideal, but unattainable, model of optimal inductive inference.  

# 2.8. Agent-environment model  

Up to this point we have only considered the inductive inference problem, either in terms of inferring hypotheses, or predicting the continuation of a sequence. In both cases the agents were  passive  in the sense that they were unable to take actions that aﬀect the future. Obviously this greatly limits them. More powerful is the class of  active  agents which not only observe their environment, they are also able to take actions that may aﬀect the environ- ment. Such agents are able to explore and achieve goals in their environment.  

![](images/0624552689458d9bbffda0bb18fb23e9343761d3fce4e030beeca5395337d00f.jpg)  
Figure 2.1.: The agent and the environment interact by sending action, obser- vation and reward signals to each other.  

We will need to consider active agents in order to satisfy our deﬁnition of in- telligence, that is, the ability to achieve goals in a wide range of environments. Solomon o induction, although extremely powerful for sequence prediction, operates in too limited a setting.  

2.8.1 Example. Consider an agent that plays chess. It is not suﬃcient for the agent to merely observe the other player. The agent actually has to decide which moves to make, so as to win the game. Of course an important part of this will be to carefully observe the other player, infer the strategy they are using, and then predict which moves they are likely to make in the future. Clearly then, inductive inference still plays an important role in the active case. Now, however, the agent has to somehow take this inferred knowledge and use it to develop a strategy of moves that will likely lead to winning the game. This second part may not be easy. Indeed, even if the agent knew the other player’s strategy in detail, it might take considerable eﬀort to ﬁnd a way to overcome the other player’s strategy and win the game. 3  

The framework in which we describe active agents is what we call the  agent- environment model . The model consists of two entities called the  agent  and the  environment . The agent receives input information from the environment, which we will refer to as  perceptions , and sends output information back to the environment, which we call  actions . The environment on the other hand receives actions from the agent as input and generates perceptions as output. Each perception consists of an  observation  component and a  reward  com- ponent. Observations are just regular information, however rewards have a special signiﬁcance because the goal of the agent is to try to gain as much reward as possible from the environment. The basic structure of this agent- environment interaction model is illustrated in Figure 2.1.  

The only way that the agent can inﬂuence the environment, and thus the rewards it receives, is through its action signals. Thus a good agent is one that carefully selects its actions so as to cause the environment to generate as much reward as possible. Presumably such an agent will make good use of any useful information contained in past rewards, actions and observations. For example, the agent might ﬁnd that certain actions tend to produce rewards while others do not. In more complex environments the relationship between the agent’s actions, what it observes and the rewards it receives might be very diﬃcult to discover.  

The agent-environment model is the framework used in the area of artiﬁcial intelligence known as  reinforcement learning . It is equivalent to the  controller- plant  framework used in control theory, where the controller takes the place of the agent, and the plant is the environment that must be controlled. With a little imagination, a huge variety of problems can be expressed in this frame- work: everything from playing a game of chess, to landing an aeroplane, to writing an award winning novel. Furthermore, the model says nothing about how the agent or the environment work, it only describes their role within the framework and thus many diﬀerent environments and agents are possible.  

2.8.2 Example. (Two coins game)  To illustrate the agent-model consider the following game. In each cycle two   $50\Phi$   coins are tossed. Before the coins settle the player must guess at the number of heads that will result: either 0, 1, or 2. If the guess is correct the player gets to keep both coins and then two new coins are produced and the game repeats. If the guess is incorrect the player does not receive any coins, and the game is repeated.  

In terms of the agent-environment model, the player is the agent and the system that produces all the coins, tosses them and distributes the reward when appropriate, is the environment. The agent’s actions are its guesses at the number of heads in each iteration of the game: 0, 1 or 2. The observation is the state of the coins when they settle, and the reward is either  $\S0$   or  $\S1$  .  

It is easy to see that for unbiased coins the most likely outcome is 1 head and thus the optimal strategy for the agent is to always guess 1. However, if the coins are signiﬁcantly biased it might be optimal to guess either 0 or 2 heads depending on the bias.  $\diamondsuit$  

Having introduced the framework, we now formalise it. The agent sends information to the environment by sending  symbols  from some ﬁnite  alphabet of symbols, for examp ,    $\{\mathtt{l e f t},\mathtt{r i g h t},\mathtt{u p},\mathtt{d o w n}\}$  . We call this set the  action space  and denote it by  A . Similarly, the environment sends signals to the agent with symbols from an alphabet called the  perception space , which we denote  $\mathcal{X}$  . The  reward space , denoted by    $\mathcal{R}$  , is always a subset of the rational unit interval   $[0,1]\cap\mathbb{Q}$  . Restricting to ration numbers is a technical detail to ensure that the information contained in each perception is ﬁnite. Every perception consists of two separate parts: an observation and a reward. For example, we might have    $\mathcal{X}:=\{(\mathtt{c o l d},0.0),(\mathtt{w a r m},1.0),(\mathtt{h o t},0.3)\}$   where the ﬁrst part describes what the agent observes ( cold ,  warm  or  hot ) and the second part describes the reward (0.0, 1.0 or 0.3).  

To denote symbols being sent we use the lower case variable names    $a$  ,    $o$   and  $r$   for actions, observations and rewards respectively. We index these in the order in which they occur, thus    $a_{1}$   is the agent’s ﬁrst action,    $a_{2}$   is the sec- ond action and so on. The agent and the environment take turns at sending symbols, starting with the agent. This produces a history of actions, observa- tions and rewards which can be written,    $a_{1}o_{1}r_{1}a_{2}o_{2}r_{2}a_{3}o_{3}r_{3}\ldots$  . As we refer to interaction histories a lot, we need to be able to represent these compactly. Firstly, we introduce the symbol    $x\in\mathcal{X}$   to stand for a perception that con- sists of an observation and a reward. That is,    $\forall k:x_{k}:=o_{k}r_{k}$  . Our second trick is to squeeze symbols together and then index them as blocks of sym- bols. For the complete interaction history up to and including cycle    $t$  , we can write    $a x_{1:t}:=\,a_{1}x_{1}a_{2}x_{2}a_{3}\,.\,.\,.\,a_{t}x_{t}$  . For the history before cycle    $t$   we use  $\boldsymbol{u}x_{<t}:=\boldsymbol{u}x_{1:t-1}$  .  

Before this section all our strings and sequences have been binary, now we ve strings and sequences from potentially larger alphabets, such as    $\mathcal{A}$   and X . Either we can encode symbols from these alphabets as uniquely identiﬁable binary strings, or we can extend our previous deﬁnitions of strings, measures etc. to larger alphabets in the obvious way. In some results technical problems can arise, for example, it takes some work to extend Theorem 2.7.1 to arbitrary alphabets (Hutter, 2001). Here we can safely ignore these technical issues and simply extend our previous deﬁnitions to general alphabets.  

Formally, the  agent  is a function, denoted by    $\pi$  , which takes the current history as input and chooses the next action as output. We do not want to restrict the agent in any way, in particular we do not require that it is deter- ministic. A convenient way of representing the agent then is as a probability measure over actions conditioned on the complete interaction history. Thus,  $\pi(a x_{1}\underline{{a}}_{2})$   is the probability of action    $a_{2}$   in the second cycle, given that the current history is    $a x_{1}$  . A deterministic agent is simply one that always assigns a probability of   $1$   to a single action for any given history. As the history that the agent can use to select its action expands indeﬁnitely, the agent need not be Markovian. Indeed, how the agent produces its distribution over actions for any given history is left open. In practical artiﬁcial intelligence the agent will of course be a machine and so    $\pi$   will be a computable function. In general however, the system generating the probabilities for diﬀerent actions could be just about anything: An algorithm that generates probabilities according to successive digits of    $\sqrt{e}$  , an incomputable function, or even a human pushing buttons on a keyboard.  

We deﬁne the  environment , denoted by    $\mu$  , in a similar way. Speciﬁcally, for all    $k\in\mathbb N$   the probability of    $x_{k}$  , given the current interaction history    $\boldsymbol{a x}_{<k}\boldsymbol{a}_{k}$  , is given by the conditional probability measure    $\mu(a x_{<k}a x_{k})$  .  

Technically, neither    $\mu$   nor    $\pi$   completely deﬁne a measure over the space of interaction sequences. They only deﬁne the conditional probability of certain symbols given an interaction history:    $\pi$   deﬁnes the conditional probability over the actions, and    $\mu$   of the perceptions. However, taken together they do deﬁne a measure over the interaction sequences that we will denote    $\overset{\triangledown}{\mu}$  . Speciﬁcally, we can chain together the conditional probabilities deﬁned by    $\pi$   and    $\mu$   to work out the probability of any interaction. For example,  

$$
\begin{array}{r}{{\pi}_{\mu}^{\pi}(\underline{{a}}\underline{{x}}_{1:2}):=\pi(\underline{{a}}_{1})\,\mu(\underline{{a}}\underline{{x}}_{1})\,\pi(\underline{{a}}x_{1}\underline{{a}}_{2})\,\mu(\underline{{a}}x_{1}\underline{{a}}\underline{{x}}_{2}).}\end{array}
$$  

When we need to take an expectation over interaction sequences this is the measure we will use. However in most other cases we will only need the conditional probabilities deﬁned by    $\pi$   or    $\mu$  .  

2.8.3 Example.  To illustrate this formalism, consider again the Two Coins Game introduced in Example 2.8.2. Let    $\mathcal{X}:=\{0,1,2\}\times\{0,1\}$   be the percep- tion space representing the number of heads after tossing the two coins and the value of the received reward. Likewise let    $\mathcal{A}:=\,\{0,1,2\}$   be the action space representing the agent’s guess at the number of heads that will occur. Assuming two fair coins, and recalling that    $x_{k}:=o_{k}r_{k}$  , we can represent this environment by deﬁning  $\forall k\in\mathbb{N}$  :  

$$
\begin{array}{r}{\mu(a x_{<k}a\underline{{x}}_{k}):=\left\{\begin{array}{l l}{\frac{1}{4}}&{\mathrm{~if~}a_{k}=0\land\omega_{k}=0\land r_{k}=1,}\\ {\frac{3}{4}}&{\mathrm{~if~}a_{k}=0\land\omega_{k}\neq0\land r_{k}=0,}\\ {\frac{1}{2}}&{\mathrm{~if~}a_{k}=1\land\omega_{k}=1\land r_{k}=1,}\\ {\frac{1}{2}}&{\mathrm{~if~}a_{k}=1\land\omega_{k}\neq1\land r_{k}=0,}\\ {\frac{1}{4}}&{\mathrm{~if~}a_{k}=2\land\omega_{k}=2\land r_{k}=1,}\\ {\frac{3}{4}}&{\mathrm{~if~}a_{k}=2\land\omega_{k}\neq2\land r_{k}=0,}\\ {0}&{\mathrm{~otherwise.}}\end{array}\right.}\end{array}
$$  

An agent that performs well in this environment would be,  

$$
\pi(a x_{<k}\underline{{{a}}}_{k}):=\left\{\begin{array}{l l}{{1}}&{{\mathrm{for}\ a_{k}=1,}}\\ {{0}}&{{\mathrm{otherwise.}}}\end{array}\right.
$$  

That is, always guess that one head will be the result of the two coins being tossed. A more complex agent might keep count of how many heads occur in each cycle and then adapt its strategy if it seems that the coins are suﬃciently biased. For example, a Bayesian agent might use techniques similar to those used to predict coin ﬂips in Examples 2.3.2 and 2.3.3. 3  

# 2.9. Optimal informed agents  

In the agent-environment model, the agent’s goal is to receive as much reward as possible. Unfortunately, this is not suﬃciently precise as there may be many possible reward sequences in a given environment and it is not clear which is preferred.  

2.9.1 Example.  Consider the following two agents: Agent    $\pi^{1}$    immediately ﬁnds a way to get a reward of 0.5 and does so in every cycle. Thus, after 100 cycles it has received a total reward of 50. Agent    $\pi^{2}$  , however, spends the ﬁrst 90 cycles trying to ﬁnd the best possible way to receive reward in each cycle. During this time it gets an average reward of 0.1 in each cycle. At cycle 90 it works out the optimal behaviour and then receives a reward of 1 in every cycle thereafter. Thus, after 100 cycles it has received a total reward    $90\times0.1+10=19$  . In terms of the total reward received after 10 cycles,  $\pi^{1}$    is superior to  π  $\pi^{2}$  . However, after 1,000 cycles this has reversed as  π  $\pi^{1}$    has a total reward of 500, while    $\pi^{2}$    has a total reward of 919.  $\diamondsuit$  

Which of these two agents is the better one? The answer depends on how we value reward at diﬀerent points in the future. In some situations we may want our agent to perform well quickly, and thus place more value on short term rewards. In others, we might only care that it eventually reaches a level of performance that is as high as possible, and thus place relatively high value on rewards far into the future. Before we can deﬁne an optimal agent, we ﬁrst need to formally express our temporal preferences.  

A general approach is to weight, or discount, each reward in a way that depends on which cycle it occurs in. Let    $\gamma_{1},\gamma_{2},.\cdot\cdot.$   be the discounts we apply  the reward in each succ ssive cycle, where    $\forall i\,:\,\gamma_{i}\,\geq\,0$  , and    $\scriptstyle\sum_{i=1}^{\infty}\gamma_{i}\ <$  ∞ in order to avoid inﬁnite weighted sums. Now deﬁne the  expected future discounted reward  for agent  π  interacting with environment  µ  given interaction history    $\ a x_{<t}$   to be,  

$$
\begin{array}{r c l}{{V_{\gamma}^{\pi\mu}(a x_{<t})}}&{{:=}}&{{\mathbf{E}\Bigg(\displaystyle\sum_{i=t}^{\infty}\gamma_{i}r_{i}\,\Big|\,a x_{<t}\Bigg)}}\\ {{}}&{{=}}&{{\displaystyle\operatorname*{lim}_{m\to\infty}\sum_{a x_{t:m}}\left(\gamma_{t}r_{t}+\cdot\cdot\cdot+\gamma_{m}r_{m}\right)\,{}_{\mu}^{\pi}(a x_{<t}\underline{{a}}x_{t:m}).}}\end{array}
$$  

As the sum is monotonically increasing in    $m$  , and ﬁnitely upper bounded, the limit always exists. For    $t=1$   we drop the interaction history from the notation and simply write    $V_{\gamma}^{\pi\mu}$  .  

One of the most common ways to set the discount parameters is to decrease them geometrically into the future. That is, set    $\forall i:\gamma_{i}:=\alpha^{i}$    for some discount rate    $\alpha\in(0,1)$  . By increasing    $\alpha$   towards 1 we weight long term rewards more heavily, conversely by reducing it we weight them less so. Thus, the parameter  $\alpha$   controls how short term greedy, or long term farsighted, the agent should be.  

2.9.2 Example. Consider again the two agents from Example 2.9.1. As the rewards are deterministic for    $\pi^{1}$    we can drop the expectation,  

$$
V_{\gamma}^{\pi^{1}\mu}=\sum_{i=1}^{\infty}\alpha^{i}0.5=0.5A,
$$  

where    $\textstyle A:={\frac{\alpha}{1-\alpha}}$    is fr m the standard formulae for geometric series. On the − other hand for agent  π  $\pi^{2}$  ,  

$$
V_{\gamma}^{\pi^{2}\mu}=\sum_{i=1}^{90}\alpha^{i}0.1+\sum_{i=91}^{\infty}\alpha^{i}=0.1A(1-\alpha^{90})+A\alpha^{90}.
$$  

Equating the two and then solving, we ﬁnd that    $\pi^{2}$    has higher expected future discounted reward than  $\pi^{1}$    when    $\alpha>\sqrt[90]{4/9}\approx0.991$   ≈ 3  

A major advantage of geometric discounting is that it is mathematically convenient to work with. Indeed, it is what we will use in Chapter 6 for our reinforcement learning algorithm. In the present context, however, we want to keep the development fairly general and thus we will leave the structure of    $\gamma$   unspeciﬁed. An even more general approach is to consider the space of all bounded enumerable discount sequences. We will take this approach when formally deﬁning intelligence in Chapter 4 as it will allow us to completely remove  $\gamma$   from the model. Here we will follow the more conventional approach to  AIXI  and simply take    $\gamma$   to be a free parameter.  

Having formalised the agent’s temporal preference in terms of    $\gamma$  , we can now deﬁne the optimal agent:  

2.9.3 Deﬁnition. The  optimal agent  for an environment    $\mu$   and discount- ing    is the agent    $\pi^{\mu}$    that has maximal expected future discounted reward.  $\gamma$  Formally,  

$$
\pi^{\mu}\,:=\,\arg\operatorname*{max}_{\pi}V_{\gamma}^{\pi\mu}.
$$  

The superscript  $\mu$   emphasises the fact that the agent is optimal with respect to the speciﬁc environment    $\mu$  . This optimality is possible because the agent was constructed using    $\mu$  . In a sense the agent knows what its environment is before it has even interacted with it. This is similar to Section 2.7 where the optimal sequence predictor was deﬁned using the distribution that was generating the sequence to be predicted.  

To understand how the optimal agent    $\pi^{\mu}$    behaves in each cycle, we ﬁrst express the value function in a recursive form for an arbitrary agent    $\pi$  . From the deﬁnition of    $V$  ,  

$$
\begin{array}{r l r}{\lefteqn{V_{\gamma}^{\pi\mu}(a x_{<t})\;=\;\operatorname*{lim}_{m\to\infty}\sum_{a x_{t:m}}\left(\gamma_{t}r_{t}+\cdot\cdot\cdot+\gamma_{m}r_{m}\right)\,_{\mu}^{\pi}(a x_{<t}\underline{{a x}}_{t:m})}}\\ &{}&{\;=\;\sum_{a x_{t}}\Bigg[\operatorname*{lim}_{m\to\infty}\sum_{a x_{t+1:m}}\left(\gamma_{t}r_{t}+\cdot\cdot\cdot+\gamma_{m}r_{m}\right)\,_{\mu}^{\pi}(a x_{1:t}\,\underline{{a x}}_{t+1:m})\Bigg]\pi_{\mu}(a x_{<t}\,\underline{{a x}}_{t:m})}\\ &{}&{\;=\;\sum_{a x_{t}}\bigg[\gamma_{t}r_{t}\,+\,V_{\gamma}^{\pi\mu}(a x_{1:t})\,\bigg]\,_{\mu}^{\pi}(a x_{<t}\,\underline{{a x}}_{t}).\qquad\qquad\qquad\qquad\quad(2.1\mu)}\end{array}
$$  

In the ﬁrst step we broke cycle    $t$   oﬀboth the sum and    $\overset{\triangledown}{\mu}$  . As these do not involve    $m$  , we pushed them outside the square brackets and moved the limit inside. In the second step we broke oﬀthe ﬁrst discounted reward and dropped the sum for this term as it was redundant. The remaining discounted rewards are just    $V$   with    $t$   advanced by one, thus producing the desired recursion in    $V$   .  

This is essentially a discrete time form of the  Bellman equation  commonly used in control theory, ﬁnance, reinforcement learning and other ﬁelds con- cerned with optimising dynamic systems (Bellman, 1957; Sutton and Barto, 1998). Usually it is assumed that the environment is Markovian and thus only a limited history needs to be taken into account. Here, however, we include the entire interaction history and thus are able to avoid these restrictions on the environment. Again, this is to keep the model as general as possible.  

Consider now how    is chosen by the optimal agent    $\pi^{\mu}$  . By deﬁnition, the  $a_{t}$  optimal action is the one that maximises    $V$  . Therefore    $\pi^{\mu}(a x_{<t}\underline{{a}}_{t})\,=\,1$   for the expected future discounted reward maximising action, and zero otherwise (ties can be broken arbitrarily). Thus, after expanding Equation (2.1) with    $\pi$  replaced by    $\pi^{\mu}$  , we can replace    $\sum{a_{t}}$    and    $\pi^{\mu}(a x_{<t}\underline{{a}}_{t})$   with simply a maximum t over the possible actions,  

$$
\begin{array}{r l}{\gamma^{\pi^{\mu}\mu}(a x_{<t})=\displaystyle\sum_{a_{t}}\,\displaystyle\sum_{x_{t}}\Big[\gamma_{t}r_{t}+V_{\gamma}^{\pi^{\mu}\mu}(a x_{1:t})\Big]\pi^{\mu}(a x_{<t}\underline{{a}}_{t})\,\mu(a x_{<t}\underline{{a}}_{t})}&{}\\ {=\displaystyle\operatorname*{max}_{a_{t}}\sum_{x_{t}}\Big[\gamma_{t}r_{t}+V_{\gamma}^{\pi^{\mu}\mu}(a x_{1:t})\Big]\mu(a x_{<t}a\underline{{x}}_{t})}&{}\\ {=\displaystyle\operatorname*{max}_{a_{t}}\sum_{x_{t}}\cdot\cdot\cdot\displaystyle\operatorname*{max}_{a_{m}}\sum_{x_{m}}\Big[\gamma_{t}r_{t}+\cdot\cdot\cdot+\gamma_{m}r_{m}+V_{\gamma}^{\pi^{\mu}\mu}(a x_{1:m})\Big]\mu(a x_{<t}a\underline{{x}}_{t:m})}&{}\end{array}
$$  

In the last step we have simply unfolded the recursion in    $V$   for the ﬁrst    $m$  cycles. As    $m\to\infty$  the te    $V_{\gamma}^{\pi^{\mu}\mu}(a x_{1:m})\,\rightarrow\,0$  0. Thus, if we take the limit  $m\to\infty$  above we can drop  V  without aﬀecting th esult. It follows then that the action taken by the optimal policy    $\pi^{\mu}$    in the  t  $t^{t h}$    cycle is,  

$$
\pi_{t}^{\mu}\,:=\,\arg\operatorname*{max}_{a_{t}}\operatorname*{lim}_{m\to\infty}\sum_{x_{t}}\,\operatorname*{max}_{a_{t+1}}\sum_{x_{t+1}}\cdots\operatorname*{max}_{a_{m}}\sum_{x_{m}}\left[\gamma_{t}r_{t}+\cdot\cdot\cdot+\gamma_{m}r_{m}\right]\mu(a x_{<t}a\underline{{x}}_{t:m})\,.
$$  

If there is more than one maximising action in cycle    $t$  , we simply select one of these in an arbitrary way.  

Intuitively, in the above equation we can see that the optimal agent takes the distribution    $\mu$  , and in eﬀect does a brute force search through all possible futures looking for the action in the current cycle that maximises the expected future discounted reward. The agent knows that the environment will always respond according to the distribution    $\mu$  , thus in each cycle it takes the expec- tation by summing over all the possible observations    $x$   and weighting these by their probability according to    $\mu$  . Furthermore, as the agent always follows an optimal strategy, its own future actions are just a series of value maximising actions.  

# 2.10. Universal AIXI agent  

Although the agent    $\pi^{\mu}$    performs optimally in environment   , this does not  $\mu$  meet the requirements for intelligence according to our deﬁnition adopted in Chapter 1. What we require is a general agent that works well in many diﬀerent environments. Such an agent must learn about its environment by interacting with it, and then modify its behaviour accordingly in order to optimise per- formance. Only then will the agent have the kind of adaptability to diﬀerent environments that we require of an intelligent agent.  

This problem with the optimal agent    $\pi^{\mu}$    is similar to the one we encountered in Section 2.7. There the optimal sequence predictor was based on the distri- bution    $\mu$   that was actually generating the sequence. Of course, for inductive inference    $\mu$   is unknown and must be inferred by observing the sequence. Thus, basing a predictor on    $\mu$   might be “optimal” in terms of prediction performance, but it is in some sense cheating. Moreover, it is certainly not general as the predictor is designed for just one environment.  

Solomonoﬀ’s solution was to replace the unknown    $\mu$   in the optimal pre- dictor with a universal prior distribution, such as    $\xi$  . This produced an ex- tremely powerful universal predictor that rapidly converged to optimal pre- dictions whenever the distribution    $\mu$   was computable (Theorem 2.7.1). In this way Solomonoﬀsolved, at least in theory, the problem of predicting sequences from unknown distributions. Hutter’s innovation was to do essentially the same trick for active agents: he took the optimal active agent    $\pi^{\mu}$  , described in the previous section, and replaced the unknown    $\mu$   with a generalised universal prior distribution    $\xi$  . This produced    $\pi^{\xi}$  , also known as  AIXI , which will be described in this section.  

In order to construct    $\pi^{\xi}$  , the ﬁrst thing to do is to generalise    $\xi$   in Deﬁni- tion 2.5.4 from sequences to active environments. As we saw earlier, an active environment    $\mu$   is an enumerable semi-measure conditioned, in chronological order, on a sequence of actions from an agent. The presence of these actions causes no problems, indeed the development of a universal distribution over ac- tive environments is virtually identical to what we did for sequence prediction in Section 2.5.  

It can be shown that the space of all enumerable chronological semi-measures can be eﬀectively enumerated. Let    $E:=\{\mu_{1},\mu_{2},.\,.\,.\}$   be such an enumeration. Deﬁne the Kolmogorov complexity of one of these environments to be the length of the shortest program that computes the environment’s index: just as we did for distributions over sequences in Deﬁnition 2.5.2. This gives us the universal prior probability  of a chronological environment    $\nu\in E$  ,  

$$
P_{E}(\nu)\,:=\,2^{-K(\nu)}.
$$  

From this prior over environments we can construct a prior over the agent’s observations in a given interaction history by taking a mixture over the envi- ronments,  

$$
\xi\bigl(\mathop{a\!{\underline{{x}}}}_{1:n}\bigr)\;:=\;\sum_{\nu\in E}2^{-K(\nu)}\,\nu\bigl(\mathop{a\!{\underline{{x}}}}_{1:n}\bigr).
$$  

It is easy to see that    $\xi$   is enumerable because   $2^{-K(\nu)}$    is enumerable, as is each    $\nu$   in the sum. Furthermore, this sum of chronological semi-measures is itself a chronological semi-measure. Thus, we see that    $\xi\,\in\,E$  . As was the case for sequences, the dominance property for    $\xi$   can easily be seen by taking one element from the sum corresponding to the semi-measure to be dominated. Note that we are reusing the symbol    $\xi$  . Whether we are talking about    $\xi$   deﬁned over sequences, or over chronological environments, will always be clear from the context.  

To construct the  AIXI  agent    $\pi^{\xi}$  , simply take    $\pi^{\mu}$    and replace    with    $\xi$  ,  $\mu$  

$$
\pi_{t}^{\xi}\;:=\;\arg\operatorname*{max}_{a_{t}}\operatorname*{lim}_{m\rightarrow\infty}\sum_{x_{t}}\operatorname*{max}_{a_{t+1}}\sum_{x_{t+1}}\cdot\cdot\cdot\operatorname*{max}_{a_{m}}\sum_{x_{m}}\big[\gamma_{t}r_{t}+\cdot\cdot\cdot+\gamma_{m}r_{m}\big]\xi(a x_{<t}a\underline{{x}}_{t:m})
$$  

This gives us an agent that does not depend on the unknown    $\mu$  . Replacing the true distribution    in the optimal agent    $\pi^{\mu}$    with the universal prior distri-  $\mu$  bution    $\xi$   is essentially the same as what we did in Section 2.7 when deﬁning Solomonoﬀ’s universal predictor. Of course now we are working in the more general setting of chronological environments rather than just sequences.  

Given that Solomon o prediction works so well for sequence prediction, we might expect the agent    $\pi^{\xi}$    deﬁned above to be similarly powerful in chrono- logical environments. To some extent this is the case, however analysing the performance of universal agents in chronological environments turns out to be signiﬁcantly more complex.  

Perhaps the most elementary question concerns whether our generalised    $\xi$  converges to the true environment    $\mu$  . It turns out that convergence results can be proven, including a result similar to Solomonoﬀ’s convergence result generalised to interaction histories. More precisely, it can be proven that the total    $\mu$  -expected squared diﬀerence between    $\mu$   and    $\xi$   is ﬁnite for interaction histories sampled from    $\pi^{\mu}$    interacting with a computable environment   . Un-  $\mu$  fortunately, when the interaction history comes from    $\mu$   interacting with    $\pi^{\xi}$  , rather than    $\pi^{\mu}$  , we run into trouble. This problem is well illustrated by the Heaven and Hell environment from Section 5.3.2 of (Hutter, 2005):  

2.10.1 Example. (Heaven and Hell)  Imagine an environment where in the ﬁrst cycle the agent is faced with two unmarked doors, one of which must be opened. One of these doors leads to “heaven” where the agent receives plentiful rewards, and the other leads to “hell” where the agent never gets any reward. Once a door is chosen there is no way to go back, the agent is stuck in either heaven or hell forever.  

This is no problem for    $\pi^{\mu}$    as it knows    and so it knows which door to take  $\mu$  to get to heaven. Thus it always achieves maximal future discounted reward. The agent    $\pi^{\xi}$  , on the other hand, must learn through experience. Of course once it has the necessary experience to make the right choice, it may already be too late. All  $\pi^{\xi}$    can do is to guess which door to take and hope for the best. Obviously the expected performance of    $\pi^{\xi}$    will be far below that of    $\pi^{\mu}$  .  $\diamondsuit$  

This example does not expose a design ﬂaw in    $\pi^{\xi}$  , in the sense that no general agent is able to consistently behave optimally in such environments. For example, consider an environment    $\mu^{\prime}$    with the two doors switched. Agent

  $\pi^{\mu}$    would always go to hell in this environment. We could deﬁne an agent

  $\pi^{\mu^{\prime}}$    which would be optimal in    $\mu^{\prime}$  , however it would always go to hell in    $\mu$  . Clearly, no agent could behave optimally in both environments without being told what the true environment was in advance. Matching the performance of optimal agents in each of their respective environments is thus an impossible task for any one agent. As such, we need to think carefully about what it is that we want to prove if we are to show that    $\pi^{\xi}$    is indeed a very powerful and general agent.  

We have already seen in Section 2.7 that the above problem does not occur in the sequence prediction setting. This is because in sequence prediction an agent’s predictions do not aﬀect the future observed sequence and thus mis- takes have no consequences beyond the current cycle. This allows Solomonoﬀ’s prediction system to be able to learn to perform optimally across the entire space of computable sequence prediction problems. Such optimising behaviour is possible in certain other classes of environments. What this suggests then is that we should focus on classes of environments, such as sequence prediction, in which it is at least possible for a general agent to learn to behave optimally.  

We begin by generalising the  AIXI  model to diﬀerent classes of environ- ments. Let    $\mathcal{E}$   be a non-strict subset of the enumeration    $E$   of all e umerable chronological semi-measures. Deﬁne the mixture distribution over  E ,  

$$
\zeta\bigl(\mathop{a\!{\underline{{x}}}}_{1:n}\bigr)\;:=\;\sum_{\nu\in{\mathcal{E}}}2^{-K(\nu)}\,\nu\bigl(\mathop{a\!{\underline{{x}}}}_{1:n}\bigr).
$$  

Now deﬁne the agent  $\pi^{\zeta}$    based on    $\zeta$  , just as we deﬁned    $\pi^{\xi}$    based on    $\xi$  . Note that while    $\pi^{\xi}$    is a single agent, the agent    $\pi^{\zeta}$    depends on which class of environments  $\mathcal{E}$   we are c idering. If    $\mathcal{E}=E$   then    $\zeta=\xi$   and so    $\pi^{\zeta}=\pi^{\xi}$  . In this sense    $\pi^{\zeta}$  generalises  π  $\pi^{\xi}$  .  

Perhaps the most elementary property that an optimal general agent must have is that there should not exist any other agent that is strictly superior. More precisely:  

2.10.2 Deﬁnition. An agent    $\pi$   is  Pareto optimal  if there is no other agent

  $\rho$   such that    $\forall\mu\in\mathcal E$  ,  

$$
V_{\gamma}^{\rho\mu}(a x_{<t})~\geq~V_{\gamma}^{\pi\mu}(a x_{<t})~~~~\forall a x_{<t},
$$  

with strict inequality for at least one    $\mu$  .  

Note that Pareto optimality does not rule out the possibility that some other agent exists which performs better in some environment in    $\mathcal{E}$  . It simply means that no other agent exists which is at least as goo in  all  environments in    $\mathcal{E}$  , and strictly better in at least one. For the agent  π  $\pi^{\zeta}$    the following optimality result can be proven (Section 5.5 of Hutter, 2005):  

2.10.3 Theorem.  For any    ${\mathcal{E}}\subseteq E$  , the agent    $\pi^{\zeta}$    is Pareto optimal.  

As this holds for any    $\mathcal{E}$  , it also holds for  $\mathcal{E}=E$  . Thus the  AIXI  agent    $\pi^{\xi}$    is a Pareto optimal agent over the space of environments  E . Note that for any space of environments  $\mathcal{E}$   many Pareto optimal a nts may exist.  

A stronger result can be proven showing that  π  $\pi^{\zeta}$    is also  balanced Pareto op- timal  (Hutter, 2005). Essentially, this means that any increase in performance in some environment due to switching to another agent, is compensated for by an equal or greater decrease in performance in some other environment.  

While these Pareto optimality results are very general, they only succeed in showing that    $\pi^{\zeta}$    is superior, or at least equal, to other general agents over the same class of environments. The result does not rule out the possibility that  all  general agents, including    $\pi^{\zeta}$  , typically perform poorly. What we need is to show that    $\pi^{\zeta}$    does indeed learn to perform well in many environments. The complication, as we saw in Example 2.10.1 above, is that in some types of environments it is impossible for general agents to perform well. Thus we somehow need to characterise those types of environments in which it is at least possible for a general agent to perform well. Furthermore, even when optimal performance is possible for a general agent, we cannot expect such an agent to perform optimally immediately. We need a performance measure that gives the agent time to learn about the structure of    $\mu$   through interaction. One way to formalise the concept of optimal performance after a period of learning is the following:  

2.10.4 Deﬁnition. An agent    $\pi$   is said to be  self-optimising  in an environ- ment    $\mu$   if,  

$$
\frac{1}{\Gamma_{t}}\,V_{\gamma}^{\pi\mu}\bigl(a x_{<t}\bigr)\;\to\;\frac{1}{\Gamma_{t}}\,V_{\gamma}^{\pi^{\mu}\mu}\bigl(\dot{a}\dot{x}_{<t}\bigr)\,
$$  

with    probability 1 as    $t\rightarrow\infty$  . Here    is an interaction history sampled  $\mu$   $\ a x_{<t}$  from    $\pi$   interacting with    $\mu$  , and   $\dot{a}\dot{x}_{<t}$   is an interaction history sampled from  $\pi^{\mu}$    interacting with   . The norm lisation factor, deﬁned   $\textstyle\Gamma_{t}:=\sum_{i=t}^{\infty}\gamma_{i}$  , is the  $\mu$  total discount remaining at time  t .  

Essentially this says that with high probability the performance of the agent  $\pi$   converges to the performance of the optimal agent. The normalisation is necessary because the un-normalised expected future discounted reward always converges to zero. Thus without it convergence would trivially hold for any agent.  

We say that    $\pi$   is  self-optimising for the set of environments    $\mathcal{E}$   if it is self- optimising for every environment in  E . Furthermore, we say that a set of environments    $\mathcal{E}$   adm s self-optimising agents  if there exists an agent    $\pi$   that is self-optimising for  E . We also extend the result to non-stationary agents by saying that a series of agents    $\pi_{1},\pi_{2},.\cdot\cdot.$   is self-optimising if the above result holds with    $\pi$   replaced by    $\pi_{t}$  . That is, in the    $t^{t h}$    cycle agent    $\pi_{t}$   is applied.  

The following powerful self-optimising result can be proven (Hutter, 2005):  

2.10.5 Theorem.  If there exists a sequence of self-optimising agents    $\pi_{m}$   for a class of environments  $\mathcal{E}$  , then the agent    $\pi^{\zeta}$    is also self-optimising for  $\mathcal{E}$  .  

Intuitively, this result says that the performance of    $\pi^{\zeta}$    will converge to op- timal performance in any class of environments where this is possible for a single agent, even if the agent is non-stationary. This is the minimal require- ment possible, in the sense that if no self-optimising agent existed for some class of environments, then trivially    $\pi^{\zeta}$    cannot be self-optimising in the class either.  

Although this is a strong optimality result, it does have two limitations. Firstly, while the result shows that    $\pi^{\xi}$    converges to optimal performance when- ever this is possible in a class of environments, it does not tell us how fast the convergence is. In Solomonoﬀ’s convergence theorem we saw that the con- vergence of the universal predictor was extremely rapid. Ideally we would like a similar result for active environments. Unfortunately, such a result is impossible in general:  

2.10.6 Example. (Needle in a haystack)  Imagine an environment with  $N$   buttons, one of which generates a reward of 1 in every cycle when pressed, and all the rest produce no reward. The location of the correct button would take roughly   $\log_{2}N$   bits to encode, thus for most values of    $N$   we have    ${\cal K}(\mu)=$   $O(\log_{2}N)$  . As    $\pi^{\xi}$    is not informed prior to the start of the game as to which button generates reward, the best it can do is to press the buttons one at a time. Thus the expected number of incorrect choices that    $\pi^{\zeta}$    will make is  $O(N)=O\big(2^{K(\mu)}\big)$  . 3  

Compare this to Theorem 2.7.1 for sequence prediction which bounds the total squared diﬀerence in prediction error by    $O(K(\mu))$  . Here in the active case the best possible bound for any general agent is exponentially worse at  $O\big(2^{K(\mu)}\big)$     . This is not a design ﬂaw in    $\pi^{\xi}$    as the above limit applies to any gen- eral agent. Clearly then, bounds showing rapid convergence are not possible in general. We can only hope to prove convergence speed bounds for speciﬁc classes of environments. Unfortunately, results in this direction currently exist only in very simple settings.  

This is a signiﬁcant weakness in the theory of universal agents. Until further results are proven it is hard to say just how fast, or slow, convergence to optimal behaviour is in diﬀerent classes of environments. As it appears that results in this direction will be diﬃcult, a more elementary result is to establish which classes of environments at least admit self-optimising agents, and under what conditions. Once we have established this, by Theorem 2.10.5 it would then follow that    $\pi^{\zeta}$    is also self-optimising in these environments. If we can show this for many classes of environments, it then follows that  $\pi^{\zeta}$    is able to perform well in a wide range of environments, at least in the limit.  

Although the required analysis is not particularly diﬃcult for many of the basic classes of environments, sorting out all the deﬁnitions, the relation- ships between them and the conditions under which they admit self-optimising agents is lengthy and requires some care. This is the subject of the next chap- ter.  

# 3. Taxonomy of Environments  

In the previous chapter we introduced the  AIXI  agent    $\pi^{\xi}$  , and its generalisation to arbitrary spaces of environments,    $\pi^{\zeta}$  . Of particular importance was Theo- rem 2.10.5 which roughly said: For any class of environments for which there exists a self-optimising agent, the agent    $\pi^{\zeta}$    deﬁned over this class is also self- optimising. Thus, in order to understand the performance of    $\pi^{\zeta}$    across a wide range of environments, we need to understand which classes of environments admit self-optimising agents, and which do not. In this chapter we present a partial answer to this question by showing that many well known classes of environments admit self-optimising agents under reasonable conditions.  

We begin by formalising some common classes of environments. To do this we examine the environments’ measures and in particular the way in which they condition on the interaction history. In this way we characterise and relate many well known classes of environments, such as  Bernoulli schemes, Markov chains , and  Markov decision processes ( MDP s) . Some interesting new classes of environments naturally arise from the analysis. We then take some important classes of problems studied in artiﬁcial intelligence, such as  sequence prediction  and  classiﬁcation , and express these too in terms of the structure of their measures. This formalisation in terms of chronological measures reveals that many classes of environments are special cases of other classes, that is, a hierarchy of classes of environments exists. Studying this more closely we see that many of these classes are in fact reducible to the class of  ergodic  MDP s . Putting all these relationships together produces a taxonomy of classes of environments.  

It is known that certain machine learning algorithms, such as    $Q$  -learning, are self-optimising in the class of ergodic  MDP s. As many of the classes in our taxonomy are reducible to ergodic  MDP s, it follows that these classes also admit self-optimising agents. Thus, from Theorem 2.10.5, we see that    $\pi^{\zeta}$  converges to optimal behaviour in these classes of environments. In this way, we clarify our earlier claim that universal agents can learn to behave optimally in a wide range of environments, as required by our deﬁnition of intelligence.  

# 3.1. Passive environments  

The ﬁrst class of environments we will consider is the class of  passive environ- ments . Loosely speaking, such environments are not aﬀected by the agent’s actions. We will be more interested in the active environments to be described in later sections.  

3.1.1 Deﬁnition. A  Bernoulli scheme  is an environment   $(\mathcal{A},\mathcal{X},\mu)$   such that  $\forall a x_{1:k}$  ,  

$$
\begin{array}{r}{\mu({a}\!x_{<k}{a}\!\underline{{x}}_{k})=\mu(\underline{{x}}_{k}).}\end{array}
$$  

As the random variables    $x_{1},x_{2},.\cdot\cdot.$   are independent and identically distributed (i.i.d.), one may think of a Bernoulli scheme as being an i.i.d. process.  

The above deﬁnition involves some slight abuse of notation. Essentially, what we are showing is that an equivalent measure with the same name (on the right hand side) can be deﬁned over a reduced parameter space by dropping the parameters that have no eﬀect on the value of the original measure (on the left hand side). In other words, the equation above indicates that the distribution  $\mu$   over    $x_{k}$   can be deﬁned in a way that it is completely independent of the history    $\boldsymbol{a x}_{<k}\boldsymbol{a}_{k}$  .  

Note also that we have written   $(\mathcal{A},\mathcal{X},\mu)$   in order to specify the action and perception spaces associated with the measure. This will be necessary in this chapter as we will often need to consider relationships between environments that diﬀer in their action and perception spaces.  

3.1.2 Example. Many simple stochastic processes can be described as Bernoulli schemes. Imagine a game where a 6 sided die is thrown repetitively. The agent receives a reward of 1 whenever a 6 is thrown, and 0 otherwise. There are no actions that the agent can take. Formally,    ${\mathcal A}\;:=\;\{\epsilon\}$  ,    $\mathcal{O}_{{}}:=$   $\{1,2,3,4,5,6\}$   and    $\mathcal{R}:=\mathbb{B}$  . The measure is deﬁned    $\forall a x_{1:k}$  ,  

$$
\begin{array}{r l}&{\mu(\underline{{x}}_{k}):=\left\{\begin{array}{l l}{\frac{1}{6}}&{\mathrm{for~}x_{k}=o_{k}r_{k}\in\{(1,0),(2,0),(3,0),(4,0),(5,0),(6,1)\},}\\ {0}&{\mathrm{otherwise}.}\end{array}\right.}\end{array}
$$  

Other than perhaps a constant environment, Bernoulli schemes are about the simplest environments possible. Despite their simplicity, they are impor- tant in statistics where sets of i.i.d. random variables play an important role. For example, sampling from a population should ideally produce individuals that are both independent of each other, and come from the same underlying distribution.  

A natural generalisation of Bernoulli schemes is to allow the next percep- tion to depend on the previous observation. This gives us a richer class of environments where the distribution over perceptions can change with time:  

3.1.3 Deﬁnition. A  Markov chain   environment   $(\mathcal{A},\mathcal{X},\mu)$   that is a Bernoulli scheme  $\forall a x_{1}$  , and  $\forall a x_{1:k}$   with  k >  1,  

$$
\mu(a x_{<k}a\underline{{{x}}}_{k})=\mu(o_{k-1}\underline{{{x}}}_{k}).
$$  

For a Markov chain the last observation completely deﬁnes the system’s state and so these outputs are usually referred to as states. In more general classes of environments this is not the case, so for consistency we will use our usual terminology of observations and perceptions. Note that we treat the ﬁrst cycle as a special case as there is no previous perception to condition on. By requiring the system to be a Bernoulli scheme in the ﬁrst cycle we ensure that the ﬁrst action has no eﬀect.  

3.1.4 Example. Imagine a game where we have a ring shaped playing board that has been divided into 20 diﬀerent cells. There is a pebble that starts in cell 1 and moves around the board as follows: On each turn a standard six sided die is thrown to decide how many positions the pebble will be moved clockwise around the board. In cells 5 and 15 the agent receives a reward of

 1. Otherwise the reward is   $0$  .  

We can model this system as a Markov chain. Let    ${\mathcal A}\ :=\ \{\epsilon\}$  ,    $\mathcal{O}_{{}}:=

$   $\{0,1,2,.\,.\,.\,,19\}$  ,    $\mathcal{R}:=\mathbb{B}$  . For    $k=1$   the pebble is in cell 1 and thus    $\forall a x_{1}$  ,  

$$
\mu(\underline{{{x}}}_{1}):={\left\{\begin{array}{l l}{1}&{{\mathrm{for~}}o_{1}=1\,\land\,r_{1}=0,}\\ {0}&{{\mathrm{otherwise}}.}\end{array}\right.}
$$  

Now deﬁne    $\forall a x_{1:k}$   with    $k>1$  ,  

$$
\begin{array}{r}{\mu(o_{k-1}\underline{{x}}_{k}):=\left\{\begin{array}{l l}{\frac{1}{6}}&{\mathrm{for~}o_{k}\in\big\{\left(o_{k-1}+1\right)\mathrm{mod~20,~}.\leq.\,,\,\left(o_{k-1}+6\right)\mathrm{mod~20}\big\}}\\ &{\mathrm{~\land~}r_{k}=\delta_{5,o_{k}}+\delta_{15,o_{k}},}\\ {0}&{\mathrm{otherwise}.}\end{array}\right.}\end{array}
$$  

In this game there is a    $\frac{1}{6}$    chance of obtaining reward if the pebble is currently in one of the six cells before either cell 5 or cell 15. 3  

From the deﬁnitions of Bernoulli schemes and Markov chains it is clear that in both of these classes an agent is “passive”, in the sense that its actions have no eﬀect on the environment’s behaviour. The diﬀerence between the two classes is the size of the history that is relevant to determining the next per- ception. Increasing the length of this history to the full history of observations gives us the most general class of completely passive environments:  

3.1.5 Deﬁnition. A  Totally Passive Environment  is an environment

  $(\mathcal{A},\mathcal{X},\mu)$   such that  $\forall a x_{1:k}$  ,  

$$
\begin{array}{r}{\mu(a x_{<k}a\underline{{x}}_{k})=\mu(o_{<k}\underline{{x}}_{k}).}\end{array}
$$  

By construction, this class of environments is a superset of the classes of environments deﬁned thus far. We could deﬁne a more limited version of this class where the next perception only depends on the last    $n$   observations, rather than the full history. However, such an environment is mathematically equiv- alent to a standard Markov chain where for each history of length    $n$   we create a unique observation in an enlarged observation space. In this new space the environment can then be represented by a ﬁrst order Markov chain. This re- duction technique will be used in a more general setting to prove Lemma 3.2.6.  

While totally passive environments are useful in modelling some systems, in terms of artiﬁcial intelligence they are relatively uninteresting because the agent cannot do anything. We can relax this constraint just a little by only requiring that the agent cannot aﬀect future observations:  

3.1.6 Deﬁnition. A  Passive Environment  is an environment   $(\mathcal{A},\mathcal{X},\mu)$  such that  $\forall a x_{<k}a\underline{{o}}_{k}$  ,  

$$
\begin{array}{r}{\mu(a x_{<k}a\underline{{o}}_{k})=\mu(o_{<k}\underline{{o}}_{k}).}\end{array}
$$  

Note that there are no restrictions on the rewards; the environment is free to reward or punish the agent in any way. Totally passive environments are clearly a special case of passive environments. For more on  AIXI  in passive environments see Section 5.3.2 of (Hutter, 2005).  

Another important special case is the class of problems where the agent is rewarded for correctly predicting a sequence that it cannot inﬂuence:  

3.1.7 Deﬁnition. A  Sequence Prediction Problem  is a passive environ- ment   $(\mathcal{A},\mathcal{X},\mu)$   such that    $\forall a x_{1:k}$  ,  

$$
\mu(a x_{<k}a o r_{k})=\mu(a o r_{k}).
$$  

That is, the reward in each cycle depends entirely on the action and the observation that immediately follows it. As sequence prediction environments are passive the observations do not depend on the agent’s actions, however there is no limit on how long the relevant observation history can be. The above deﬁnition makes precise what we meant in previous chapters where sequence prediction problems were referred to as being “passive”. For more on how  AIXI  deals with sequence prediction problems see Section 6.2 of (Hutter, 2005).  

3.1.8 Example. Let    $\mathcal{A}=\mathcal{O}:=\{0,1,.\,.\,.\,,9\}$  . Deﬁne    $\forall a x_{1:k}$  ,  

$$
\begin{array}{r}{\mu(o_{<k}\underline{o}_{k}):=\left\{\begin{array}{l l}{1}&{\mathrm{if~}o_{k}\mathrm{~the~}k^{t h}\mathrm{~digit~in~}3.141592\dots,}\\ {0}&{\mathrm{otherwise},}\end{array}\right.}\end{array}
$$  

and  

$$
\begin{array}{r}{\mu(a o_{-k}):=\left\{\begin{array}{l l}{1}&{\mathrm{if}\,\,r_{k}=1-\frac{1}{9}|o_{k}-a_{k}|,}\\ {0}&{\mathrm{otherwise}.}\end{array}\right.}\end{array}
$$  

Thus, in order to maximise reward, the agent must generate successive digits of the mathematical constant    $\pi$  . A correct digit gets a reward of 1, while an incorrect digit gets a lesser reward proportional to the diﬀerence between the correct digit and the guess. 3  

# 3.2. Active environments  

The simplest active environment is one where the next perception    $x_{k}$   depends on only the last action    $a_{k}$  :  

3.2.1 Deﬁnition. A  Bandit  is an environment   $(\mathcal{A},\mathcal{X},\mu)$   such that    $\forall a x_{1:k}$  ,  

$$
\mu(a x_{<k}a\underline{{{x}}}_{k})=\mu(a\underline{{{x}}}_{k}).
$$  

This class of environments is named after the bandit machines found in casi- nos around the world, although the relation to real bandit machines is tenuous. Bandit environments are weaker than Markov chains as future perceptions do not depend on past observations. However, they do have the ability to re- act to the last action, and so in this respect they are more powerful. Even though bandit problems are conceptually simple, solving them optimally is surprisingly involved (Berry and Fristedt, 1985; Gittins, 1989).  

3.2.2 Example. Imagine a machine that has    $n$   diﬀerent levers, or arms, that the agent can pull. Each arm has a diﬀerent but ﬁxed probability of generating a reward. For this example, let the reward be either 0 or 1. The agent’s task is to ﬁgure out which arm to pull so that it maximises its expected reward. One approach might be to spend time pulling diﬀerent arms and collecting statistics in order to estimate which produces the most reward.  

We can formally deﬁne this bandit as follows: Let    $\mathcal{O}:=\,\{\epsilon\}$  ,    $\mathcal{R}:=\mathbb{B}$   and let    $\mathcal{A}:=\{1,2,3,\cdot\cdot\,\cdot\,,n\}$   represent the    $n$   diﬀerent arms that the agent can pull. Let    $\beta_{1},\beta_{2},.\ldots,\beta_{n}$   be the respective probabilities of obtaining a reward of 1 after pulling the corresponding arm. Now deﬁne  $\forall a x_{1:k}$  ,  

$$
\begin{array}{r}{\mu(a\underline{{x}}_{k}):=\left\{\begin{array}{l l}{\beta_{a_{k}}}&{\mathrm{for}\ r_{k}=1,}\\ {1-\beta_{a_{k}}}&{\mathrm{for}\ r_{k}=0.}\end{array}\right.}\end{array}
$$  

A natural extension to the class of bandits is to allow the next perception to depend on both the last observation and the last action. This produces a much more powerful class of environments that has been intensively studied and has many theoretical and practical applications:  

3.2.3 Deﬁnition. A (stationary)  Markov Decision Process ( MDP ) environment   $(\mathcal{A},\mathcal{X},\mu)$   that is a Bernoulli scheme    $\forall a x_{1}$  , and    $\forall a x_{1:k}$   with  $k>1$   1,  

$$
\mu(a x_{<k}a\underline{{{x}}}_{k})=\mu(o_{k-1}a\underline{{{x}}}_{k}).
$$  

Usually  MDP s are deﬁned in such a way that the agent does not act before the ﬁrst perception. However, in our deﬁnition the ﬁrst cycle is a Bernoulli scheme and so the ﬁrst action has no eﬀect anyway. Aside from this detail, our deﬁnition is equivalent to the standard deﬁnition (Bellman, 1957). It is immediately clear that this class generalises Bernoulli schemes, bandits and Markov chains.  

What we have deﬁned above is a  stationary  MDP . This is because the prob- ability of a perception given the current action and the last perception does not change, that is, it is independent of    $k$  . In some deﬁnitions the measure    $\mu$  is allowed to vary over time. These  non-stationary  MDP s can be modelled as POMDP s, which will be deﬁned shortly.  

3.2.4 Example. Consider again the simple Markov chain in Example 3.1.4. This can be extended to an  MDP  by allowing the agent to decide whether to mov ise or anticlockwise around the board. Let    $\mathcal{O}\,:=\,\{0,1,.\,.\,.\,,19\}$  and  R  $\mathcal{R}:=\,\mathbb{B}$   B  as before, b w    ${\mathcal{A}}:=\{{\mathcal{C}},{\mathcal{C}}\}$   as the agent can select which direction to move in. For  k  = 1 the pebble is in cell 1 and thus    $\forall a x_{1}$  ,  

$$
\mu({a}\underline{{{x}}}_{1}):=\left\{\begin{array}{l l}{1}&{\mathrm{for}\;o_{1}=1\,\land\,r_{1}=0,}\\ {0}&{\mathrm{otherwise}.}\end{array}\right.
$$  

Now deﬁne    $\forall a x_{1:k}$   with    $k>1$  ,  

$$
\begin{array}{r}{\mu(o_{k-1}a\underline{{x}}_{k}):=\left\{\begin{array}{l l}{\frac{1}{6}}&{\mathrm{for~}o_{k}\in\{(o_{k-1}+1)\mathrm{~mod~}20,\ldots,(o_{k-1}+6)\mathrm{~mod~}20\}}\\ &{\mathrm{~\land~}r_{k}=\delta_{5,o_{k}}+\delta_{15,o_{k}}\mathrm{~\land~}a_{k}=\odot,}\\ {\frac{1}{6}}&{\mathrm{for~}o_{k}\in\{(o_{k-1}-1)\mathrm{~mod~}20,\ldots,(o_{k-1}-6)\mathrm{~mod~}20\}}\\ &{\mathrm{~\land~}r_{k}=\delta_{5,o_{k}}+\delta_{15,o_{k}}\mathrm{~\land~}a_{k}=\odot,}\\ {0}&{\mathrm{otherwise}.}\end{array}\right.}\end{array}
$$  

A natural way to generalise the class of  MDP s is to allow the next perception to depend on the last    $n$   observations and actions:  

3.2.5 Deﬁnition. A (stationary)    $n^{\mathbf{th}}$    order Markov Decision Process is an environment   $(\mathcal{A},\mathcal{X},\mu)$   that is a Bernoulli scheme    $\forall a x_{1}$  , and    $\forall a x_{1:k}$   with  $k>1$  ,  

$$
\begin{array}{r}{\mu(a x_{<k}a\underline{{x}}_{k})=\mu(o_{k-m}a o_{k-m+1:k-1}a\underline{{x}}_{k}),}\end{array}
$$  

where    $m:=\operatorname*{min}\{n,k\}$  .  

Immediately from the deﬁnition we can see that a standard  MDP  is an    $n^{t h}$  order  MDP  where  $n=1$  . The added complication of the variable    $_m$   is to allow for the situation where the current history length    $k$   is less than    $n$  .  

It might appear that    $n^{t h}$    order  MDP s are more general than standard  MDP s, however it turns out that any    $n^{t h}$    order  MDP  can be converted into an equiva- lent  MDP . This is done by extending the observation space and appropriately modifying the measure. The proof follows the same pattern as the reduction of    $n^{t h}$    order Markov chains to standard Markov chains, except that now we have to deal with the complication of having actions in the history.  

3.2.6 Lemma.    $n^{t h}$    order  MDP s can be reduced to  MDP s.  

Proof. Let   $(\mathcal{A},\mathcal{X}=\mathcal{O}\times\mathcal{R},\mu)$  ) be an    $n^{t h}$    order  MDP . To prove the result we will deﬁne an equivalent ﬁrst order  MDP  (  $\mathcal{A},\mathcal{Z}=\mathcal{Q}\times\mathcal{R},\tilde{\mu}_{\mathcal{A}}$  ). We begin by deﬁning the new observation space,  

$$
\mathcal{Q}:=\bigcup_{i=1}^{n}\mathcal{O}\times(\mathcal{A}\times\mathcal{O})^{i-1}.
$$  

Every interaction history that the    $n^{t h}$    order  MDP  conditions on is thus uniquely represented in    $\mathcal{Q}$  . Although it complicates things, we need to include histories of length less than    $n$   to accommodate the ﬁrst    $n-1$   cyc s of the system.  

Next we deﬁne a measure  µ  over the perc tion space  Z , that is equivalent to the measure    $\mu$   over the perception space  X . We begin by dealing with the ﬁrst interaction cycle:    $\forall a z_{1}\in\mathcal{A}\times\mathcal{Z}$   deﬁne,  

$$
\begin{array}{r}{\tilde{\mu}(a\underline{{z}}_{1}):=\left\{\begin{array}{l l}{\mu(a\underline{{z}}_{1})}&{\mathrm{for}\ z_{1}\in\mathcal{X},}\\ {0}&{\mathrm{otherwise}.}\end{array}\right.}\end{array}
$$  

This makes the processes equivalent in the ﬁrst cycle. Next, for interaction cycles    $k>1$  , deﬁne  $\forall q_{k-1}a z_{k}\in\mathcal{Q}\times\mathcal{A}\times\mathcal{Z}$  ,  

$$
\check{\iota}(q_{k-1}\underline{{\alpha}}_{k}):=\left\{\begin{array}{l l}{\mu(o_{k-m}a o_{k-m+1:k-1}a\underline{{x}}_{k})}&{\mathrm{if~}q_{k-1}=o_{k-m}a o_{k-m+1:k-1}}\\ &{\mathrm{~\land~}z_{k}=q_{k}r_{k}}\\ &{\mathrm{~\land~}q_{k}=o_{k-m+1}a o_{k-m+2:k-1}a o_{k}}\\ {0}&{\mathrm{otherwise.}}\end{array}\right.
$$  

That is, if the transition    $q_{k-1}a z_{k}$   is possible when represented in the original environment, then this transition is given the same probability by  µ  in the new environment. Any transition    $q_{k-1}a z_{k}$   which is impossible in the original environment, for example because the two histories represented by    $q_{k-1}$   and  $q_{k}$   are inconsistent, is given a transition probability of zero by  µ . Thus, the two environments have equivalent structure and dynamics. 2  

As the proof illustrates, writing down the equation for a higher order  MDP and working with it is cumbersome. Thus, the above result is useful as it means that we only have to deal with ﬁrst order  MDP s in our analysis. Nevertheless, conceptually it is often more natural to think of certain problems as being    $n^{t h}$  order  MDP s.  

Rather than increasing the history that the measure conditions on, another extension is to assume that the agent cannot properly observe the  MDP ’s outputs:  

3.2.7 Deﬁnition. A  Partially Observable Markov Decision Pro- s an environment   $({\mathcal{A}},{\mathcal{X}}={\mathcal{O}}\times{\mathcal{R}},\mu)$  )  : Let

  $({\mathcal{A}},{\tilde{\mathcal{X}}}={\tilde{\mathcal{O}}}\times{\mathcal{R}},{\tilde{\mu}})$  A X O × R ) be an  MDP  called the  core  MDP . Let  $\phi:\tilde{\mathcal{X}}\times\mathcal{X}\to[0,1]$  X ×X →  1] be a conditional probability measure of the form    $\phi(\tilde{x}\underline{{x}})$  ) which expresses the proba- bility of perceiving    $x$   when the core  MDP  outputs  x . Deﬁne  $\forall a x_{1:k}\in({\mathcal{A}}\times{\mathcal{X}})^{k}$  ,  

$$
\mu(a\underline{{x}}_{1:k})\,:=\,\sum_{\tilde{x}_{1:k}\,\in\,\tilde{\mathcal{X}}^{k}}\tilde{\mu}\!\left(a\underline{{\tilde{x}}}_{1}\right)\phi(\tilde{x}_{1}\underline{{x}}_{1})\,\tilde{\mu}\!\left(\tilde{o}_{1}a\underline{{\tilde{x}}}_{2}\right)\phi\!\left(\tilde{x}_{2}\underline{{x}}_{2}\right)\,\cdot\cdot\,\tilde{\mu}\!\left(\tilde{o}_{k-1}a\underline{{\tilde{x}}}_{k}\right)\phi\!\left(\tilde{x}_{k}\underline{{x}}_{k}\right)
$$  

The nature of  POMDP s is perhaps best illustrated by example.  

3.2.8 Example. Let the core  MDP    $(\mathcal{A},\ddot{\mathcal{X}},\tilde{\mu})$  X ) be the  MDP  deﬁned in Ex- ample 3.2.4. Now imagine that the agent cannot reliably observe which cell the pebble is in. To do this, let    $\mathcal{X}:=\tilde{\mathcal{X}}$  X  and deﬁne the observation function  $\forall\tilde{x},x\in\mathcal{X}$  ,  

$$
\begin{array}{r}{\phi(\tilde{x}\underline{{x}}):=\left\{\begin{array}{l l}{\frac{61}{100}\quad\quad}&{\mathrm{~if~}\tilde{x}=x,}\\ {\frac{1}{100}\quad}&{\mathrm{~otherwise.}}\end{array}\right.}\end{array}
$$  

Thus, with probability 0.61 the agent observes the true output of the core MDP . The rest of the time it observes some other randomly chosen output. The probabilities add up as    $|\mathcal{X}|=|\mathcal{O}|\times|\mathcal{R}|=20\times2=40$  . Thus,    $x$   can take 39 values other than  x .  $\diamondsuit$  

The fact that  POMDP s generalise  MDP s can be seen by letting    $\mathcal{X}=\tilde{\mathcal{X}}$  X  and  $\phi(\tilde{x}\underline{{x}}):=\delta_{\tilde{x},x}$  , in which case it follows that    $\mu=\tilde{\mu}$  . That is, the  POMDP  reduces to being its core  MDP . Furthermore, as    $n^{t h}$    order  MDP s can be reduced to ﬁrst order  MDP s, it follows that  POMDP s also generalise higher order  MDP s.  

To see that  POMDP s can deﬁne non-stationary  MDP s, consider a  POMDP where    $\mathcal{X}$   is a strict subset of X . Now use this extra internal information in the core  MDP  to keep a parameter that varies over time and that aﬀects the core  MDP ’s behaviour. To the external agent who cannot observe this extra information, it appears that the environment is non-stationary.  

Both theoretically and practically this class of environments is diﬃcult to work with. However, it does encompass a huge variety of possibilities, includ- ing all of the environments considered in this chapter, and many real world problems.  

# 3.3. Some common problem classes  

Many of the problems considered in artiﬁcial intelligence can be expressed as classes of environments using our measure notation. In this section we will formalise some of them.  

3.3.1 Deﬁnition. A  Function Maximisation Problem  is an environment

  $(\mathcal{A},\mathcal{X},\mu)$   such that    $\mathcal{O}=\mathcal{R}$   and for some  objective function    $f:\mathcal{A}\rightarrow\mathcal{R}$   we have

  $\forall a x_{1:k}$  ,  

$$
\mu(a x_{<k}a\underline{{x}}_{k}):=\left\{\begin{array}{l l}{1}&{\mathrm{if}\,\,o_{k}=f(a_{k})\,\wedge\,r_{k}=\operatorname*{max}\{o_{1},.\,.\,,o_{k}\},}\\ {0}&{\mathrm{otherwise}.}\end{array}\right.
$$  

Essentially, the agent’s actions are interpreted as input to some function, and in each cycle the result of the function is returned as an observation. We do not simply return the current value of the function as the reward as this dis- courages the agent from exploring once a good value has been found. Rather we return the reward associated with the best value found so far. Obviously, maximisation problems with diﬀerent ranges, or minimisation problems, can be expressed by applying a simple transformation to the original objective function. For more on how  AIXI  deals with various types of function optimi- sation problems see Section 6.4 of (Hutter, 2005).  

3.3.2 Example. Let    $\textstyle f(a):=1-(a-{\frac{1}{4}})^{2}$  . To maximise reward the agent must generate the action that maximises    $f$  , that is,    $\textstyle a={\frac{1}{4}}$  .  $\diamondsuit$  

Artiﬁcial intelligence often considers environments that consist of some kind of a game that is repetitively played by the agent. Games such as chess, various card games, tic-tac-toe and others belong to this class, so long as at the end of each match a new match is started. This can be formalised as follows:  

3.3.3 Deﬁnition. A  Repeated Strategic Game  is an environment

  $(\mathcal{A},\mathcal{X},\mu)$   where    $\exists l\in\mathbb{N}$   such that    $\forall a x_{1:k}$  ,  

$$
\mu(a x_{<k}a\underline{{x}}_{k})=\mu(a o_{l m:k-1}a\underline{{x}}_{k})
$$  

where    $m:=\lfloor k/l\rfloor$  is the number of the  episode  when in cycle    $k$  , and    $l$   is the episode length .  

Clearly, Bernoulli schemes and bandits are repeated strategic games. If we want to allow games to ﬁnish before the episode ﬁnishes we can pad the re- maining cycles, and perhaps also reward the system for padded cycles following a victory in order to encourage rapid wins. For more on how  AIXI  deals with strategic games see Section 6.3 of (Hutter, 2005).  

Another common type of problem considered in artiﬁcial intelligence is clas- siﬁcatio . A classiﬁcation problem consists of a domain space    $\mathcal{W}$   and a set of classes  Z  and some function    $f:\mathcal{W}\to\mathcal{Z}$  . The agent must try to learn this mapping based on examples. When given cases where the class is missing it has to correctly guess the class in order to obtain reward. More formally:  

3.3.4 Deﬁnition. A   assiﬁ tion Problem  is an environment   $(\mathcal{A},\mathcal{X},\mu)$  set up as follows. Let  W  and  Z   e two sets called the  attribute space and the  class space  respectively.  Z  includes a special symbol “?” used to indicate whether the agent needs to guess the class. Let    $\mathcal{O}\subset\mathcal{W}\times\mathcal{Z}$   and let  $f:\mathcal{W}\to\mathcal{Z}\setminus\{\mathfrak{V}\}$   where    $\forall a x_{1:k}$   such that    $x_{k-1}=w z r_{k-1}$   and    $x_{k}=w z r_{k}$  ,  

$$
\mu(a x_{<k}a\underline{{w}}_{k})=\mu(\underline{{w}}_{k}),
$$  

and for    $\alpha\in(0,1)$  ,  

$$
\mu(w z_{k})=\left\{\begin{array}{l l}{\alpha}&{\mathrm{if}\,\,z_{k}=f(w_{k}),}\\ {1-\alpha}&{\mathrm{if}\,\,z_{k}=\mathrm{even},}\\ {0}&{\mathrm{otherwise},}\end{array}\right.
$$  

and  

$$
\begin{array}{r}{\mu(z_{k-1}a_{k}\underline{{r}}_{k})=\left\{\begin{array}{l l}{1}&{z_{k-1}=\mathrm{\"?\"}\wedge a_{k}=f(w_{k-1})\wedge r_{k}=1,}\\ {1}&{z_{k-1}\neq\mathrm{\"?\"}\wedge r_{k}=0,}\\ {0}&{\mathrm{otherwise}.}\end{array}\right.}\end{array}
$$  

The ﬁrst condition says that the distribution of the points in the attribute space is independent of the system’s history. In other words, this part of    $x_{k}$   is a Bernoulli scheme. The second condition says that in each cycle the system must either provide a training instance or ask for the agent to classify based on the attribute vector. The parameter    $\alpha$   controls how often the agent is asked to guess the class. The third condition says that reward is given when the system asks for a classiﬁcation and the agent guesses it correctly. It is easy to see that classiﬁcation problems are passive  MDP s.  

3.3.5 Example. t each element of    $\mathcal{W}$   be a vector of medical measurements for a patient, and  Z  some list of diseases. When provided with a list of patients’ statistics and their diseases, the agent’s job is to learn a function that determines which disease is present given a patient’s medical data.  $\diamondsuit$  

For more on how  AIXI  deals with supervised learning problems see Sec- tion 6.5 of (Hutter, 2005).  

# 3.4. Ergodic MDPs  

Intuitively, a Markov chain is  ergodic  if the current observation cannot impose any long term constraints on future observations. Given that the reward in a Markov chain only depends on the last observation, being ergodic also implies that the current observation does not place any long term constraints on fu- ture rewards either. Although the ergodic property is typically studied in the context of Markov chains, in this section we will extend the notion to  MDP s. To illustrate the idea, we begin by considering some Markov chains that are not  ergodic.  

3.4.1 Example. Imagin a Markov chain with    $\mathcal{O}\;:=\;\{A,B,C\}$  . The chain starts with observation  A  and then transitions to either  B  or  C . In all subsequent cycles the observation remains the same. Thus, once observation  $B$   has been generated, observation    $C$   will never occur. This is a long term constraint on future observations, and thus the Markov chain is not ergodic.    $\diamondsuit$  

It is as if the agent has gone through a one-way door into a part of the environment that it can never return from. This is similar to the Heaven and Hell environment in Example 2.10.1, except that in the above example the agent was unable to choose where to go as the environment was passive. Consider now a slightly more subtle example of non-ergodic behaviour.  

3.4.2 Example. Imagine a arkov chain with    $\mathcal{O}:=\{A,B\}$  . The envir - ment starts with observation  A  and then in the ﬁrst cycle transitions to  B . In the following cycle it transitions back to state    $A$  , and then in the next it returns to    $B$  . In this way the system alternates between the two observations.  

This environment might seem ergodic as both possible observations continue to occur forever and so the agent clearly has not become conﬁned to just one part of the environment. However, if the current observation is    $A$  , it must be the case that two time steps into the future the observation will again be  $A$  . In fact, for any even number of time steps into the future the observation will always be    $A$  . As this is a long term constraint on future observations the environment is not ergodic.  $\diamondsuit$  

The above example can be modiﬁed so that it is ergodic: after outputting observation    $A$   make it so that there is a 0.1 probability of generating this again, and a 0.9 probability of outputting    $B$  . Thus, no matter what the current observation is, after three time steps the environment could output either observation.  

We now formalise the proceeding concepts. We say that two observations communicate  if it is possible to go from one observation to the other and return after some ﬁnite number of steps. A  communicating class  is a set of observations that all communicate with each other, and do not communicate with any observations outside this set. If all observations of the Markov chain belong to the same communicating class, we say that the Markov chain is irreducible . An observation has  period    $k$   if any return to the observation must occur in some multiple of    $k$   time steps and    $k$   is the largest number with this property. For example, if it is only possible to return to an observation in an even number of steps, then this observation has period 2. If an observation has period 1 then it is  aperiodic , and if all observations are aperiodic we say that the Markov chain is aperiodic. We can now formally deﬁne what it means for a Markov chain to be ergodic:  

3.4.3 Deﬁnition. A Markov chain environment   $(\mathcal{A},\mathcal{X},\mu)$   is  ergodic  if and only if it is irreducible and aperiodic.  

Consider again the two examples above. The technical reason the Markov chain in Example 3.4.1 was not ergodic was because the observations    $A$   and  $B$   were not communicating and thus the Markov chain was not irreducible. In Example 3.4.2 the problem was that both observations had period 2 and thus the Markov chain was not aperiodic.  

To extend the concept of being ergodic to cover  MDP s consider again the relationship between Markov chains and  MDP s. Let    $\mu$   be an  MDP  environment, and    $\pi$   an agent that is conditioned on only the last observation. That is,  $\forall a x_{<k}a_{k}$   with    $k>1$   we have,  

$$
\pi(a x_{<k}\underline{{{a}}}_{k})\;=\;\pi(o_{k-1}\underline{{{a}}}_{k}).
$$  

Now consider the measure    $\underset{\mu}{\pi}$    that describes how the above environment and agent interact. In each cycle we have,  

$$
\begin{array}{r c l}{{\frac{\pi}{\mu}\!\left(a x_{<k}a\underline{{{x}}}_{k}\right)}}&{{:=}}&{{\displaystyle\sum_{a_{k}\in\mathcal{A}}\pi(o_{k-1}\underline{{{a}}}_{k})\,\mu(o_{k-1}a\underline{{{x}}}_{k})}}\\ {{}}&{{=}}&{{\displaystyle\overset{\pi}{\mu}\!\left(o_{k-1}\underline{{{x}}}_{k}\right)\!.}}\end{array}
$$  

Thus, if    $\pi$   has the form above and is ﬁxed, the distribution over the next perception depends on only the last observation. That is,  $\underset{\mu}{\pi}$    deﬁnes a Markov chain. In other words, an  MDP  can be thought of as a Markov chain with the addition of actions that allow an agent to inﬂuence future observations. We can remove this by taking an appropriate agent and building it into the  MDP . The resulting system no long has free actions to be chosen, and reverts back to being a Markov chain.  

Using this relationship, we can now deﬁne ergodic  MDP s in the natural way:  

3.4.4 Deﬁnition. An  MDP  environment   $(\mathcal{A},\mathcal{X},\mu)$   is  ergodic  if and only if there exists an agent   $({\mathcal{A}},{\mathcal{X}},\pi)$   such that    $\overset{\triangledown}{\mu}$    deﬁnes an ergodic Markov chain.  

As higher order  MDP s are reducible to  MDP s, we will say that a higher order MDP  is ergodic if it is reducible to an ergodic  MDP .  

Consider again the Heaven and Hell environment deﬁned in Example 2.10.1. After the ﬁrst interaction cycle the agent is always in either heaven or hell. Furthermore, no matter what the agent does, it cannot switch between being in heaven or being in hell, it is stuck in its current location for all eternity. Thus, no matter what agent we select we cannot create a Markov chain such that the observations of being in heaven communicate with the observations of being in hell, and so this  MDP  environment is not ergodic.  

The importance of ergodic  MDP s for us will be their relationship to learn- ing. In an  MDP  environment only the current observation and action have any impact on future perceptions. When the  MDP  is ergodic, the current ob- servation also has no long term impact on future perceptions that cannot be overcome by taking the right actions. This means that in an ergodic  MDP environment no matter what mistakes an agent might make, there is always a way to recover from these. Obviously this is a signiﬁcant property for learning agents, and indeed the following important result can be proven:  

3.4.5 Theorem.  Ergodic  MDP s admit self-optimising agents.  

For references and other details see Appendix B. As ergodic  MDP s admit self-optimising agents it follows by Theorem 2.10.5 that the universal agent    $\pi^{\zeta}$  deﬁned over the class of ergodic  MDP s is also self-optimising. What remains to be shown is that many important classes of environments are in fact special cases of ergodic  MDP s. This is the topic of the next section.  

# 3.5. Environments that admit self-optimising agents  

In this section we will prove that some of the environments we have deﬁned are in fact ergodic  MDP s. Thus, by the results in the last section,    $\pi^{\zeta}$    is self-optimising in these environments. Before we begin we ﬁrst need to intro- duce one extra property: we need to assume that environments are  accessible , meaning that there are no observations that have zero probability of ever being observed.  

3.5.1 Deﬁnition. A chronological environment   $(\mathcal{A},\mathcal{X},\mu)$   is  accessible  if

  $\forall o_{k},\exists a x_{<k}a_{k}$   such that    $\mu(a x_{<k}a_{k}\underline{{o}}_{k})>0$  .  

It is reasonable to assume this because if it is impossible to ﬁnd any ﬁnite interaction history that gives some observation a non-zero probability then we can simply remove it from the observation space. This produces an equivalent environment that is accessible. In particular, this does not interfere with the property that ergodic  MDP s admit self-optimising agents. These unused extra observations play no role.  

3.5.2 Lemma.  Bernoulli schemes are ergodic  MDP s.  

Proof. Consider a Bernoulli scheme   $(\mathcal{A},\mathcal{X},\mu)$  . From the deﬁnition of a Bernoulli scheme we immediately see that they are a special case of the  MDP deﬁnition. As the environment is accessible,    $\forall o_{k}$  ,    $\exists a x_{<k}a_{k}:\mu(a x_{<k}a_{k}\underline{{o}}_{k})>0$  . Applying the deﬁnition of a Bernoulli scheme, this reduces to    $\forall o_{k}:\mu(\underline{o}_{k})>0$  . Thus, as the next observation does not depend on observations prior to    $O_{k-1}$  , nor does it depend on the actions or rewards, the agent and environment together deﬁne a Markov chain. As all observations are possible at every point in time it follows that all observations belong to the same communicating class and are aperiodic. That is, the Markov chain is ergodic. 2  

Note that the above result holds independent of the agent as the environment is passive. The same is true for classiﬁcation problems:  

3.5.3 Lemma.  Classiﬁcation problems are ergodic  MDP s.  

Proof. From Deﬁnition 3.3.4 we see that the distribution over the attribute space    $\mathcal{W}$   is not  pendent on the interaction history, and the distribution over the class space  Z  depends only on the attribute in the current cycle and so it too is independent of anything in prior cycles. More formally,  $\forall a x_{<k}a_{k}o_{k}:$   $\mu(a x_{<k}a_{k}\underline{{o}}_{k})=\mu(\underline{{w}}_{k})\,\mu(w_{k}\underline{{z}}_{k})$   where    $o_{k}:=w_{k}z_{k}$  . Thus the distribution over observations is completely independent of prior cycles.  

Again from the deﬁnition of classiﬁcation problems we see that the distribu- tion over rewards depends on only the observation in the previous interaction cycle and the action in the current cycle. It follows then that in each cycle the perception (consisting of an attribute, class and reward) depends on only the action in the current cycle and the observation in the previous cycle. Thus, classiﬁcation problems are  MDP s.  

As the environment is accessible,    $\forall o_{k}$  ,    $\exists a x_{<k}a_{k}:\mu(a x_{<k}a_{k}\underline{{o}}_{k})>0$  . Because the distribution over observations is independent of previous interaction cy- cles this immediately reduces to    $\forall o_{k}\,:\,\mu(\underline{{o}}_{k})\,>\,0$   and so the environment is ergodic. 2  

As classiﬁcation problems are passive, in the above proof it was again pos- sible to construct an ergodic Markov chain without specifying the agent. In active environments, such as bandits, we must deﬁne an appropriate agent:  

3.5.4 Lemma. Bandits are ergodic  MDP s.  

Proof. Consider a bandit   $(\mathcal{A},\mathcal{X},\mu)$  . By deﬁnition it is trivially an  MDP . As the environment is accessible    $\forall o_{k},\exists a x_{<k}a_{k}:\mu(a x_{<k}a\underline{{o}}_{k})>0$   0. Applying the deﬁnition of a Bandit this reduces to,  

$$
\forall o_{k},\exists a_{k}:\mu(a\underline{{o}}_{k})>0.
$$  

Next we need to show that there exists an agent under which the agent interacting with the environment deﬁnes an ergodic Markov chain. If we deﬁne an agent    $\begin{array}{r}{\forall a_{k}:\pi(\underline{{a}}_{k}):=\frac{1}{|\mathcal{A}|}}\end{array}$    it follows that    $\forall a x_{<k}a o_{k}$  ,  

$$
\pi_{\mu}\big(a x_{<k}a\underline{{o}}_{k}\big)\;:=\;\sum_{a_{k}\in\mathcal{A}}\pi\big(\underline{{a}}_{k}\big)\mu\big(a\underline{{o}}_{k}\big)\;=\;\frac{1}{|\mathcal{A}|}\sum_{a_{k}\in\mathcal{A}}\mu\big(a\underline{{o}}_{k}\big)\;=:\;\pi_{\mu}\big(\underline{{o}}_{k}\big).
$$  

From Equation 3.1 it then follows that for each    $o_{k}$   at least one of the terms in the above sum is non-zero. Thus,    $\forall o_{k}:\,_{\mu}^{\pi}(\underline{o}_{k})\,>\,0$   0 and so    $\overset{\triangledown}{\mu}$    is an ergodic Markov chain and therefore    $\mu$   is an ergodic  MDP . 2  

Unfortunately, repeated strategic games are not ergodic  MDP s. The problem is that there may be observations which can only occur at certain points in each episode, for example at the start or the end. Clearly then one cannot deﬁne an agent such that these observations have period 1, making it impossible to construct an ergodic Markov chain. Nevertheless, through a change of action and perception spaces a repeated strategic game can be converted into an equivalent system which is a bandit. Bandits, as we saw above, are ergodic MDP s. For our purposes such a conversion is suﬃcient as it allows these environments to admit self-optimising agents.  

3.5.5 Lemma.  Repeated strategic games are reducible to ergodic  MDP s.  

Proof. Let   $(\mathcal{A},\mathcal{X},\mu)$   be a rep ategic game with episode length . Now deﬁne a new action space   $\dot{\mathcal{A}}:=\mathcal{A}^{l}$   $l$  A  A . In this new action space every combination of actions that an agent can take in a single episode of the game is represented by a single action. Similarly, deﬁne a new perception space that represents each episode,  $\ddot{\mathcal{X}}:=\mathcal{X}^{l}$  X  X , and now deﬁne a chronological measure  µ over the new spaces such that  $\forall\tilde{a}\tilde{x}_{1:k}$  ,  

$$
\tilde{\mu}\big(\tilde{a}\tilde{x}_{<k}\tilde{a}\tilde{\underline{{x}}}_{k}\big)\;=\;\tilde{\mu}\big(\tilde{a}\tilde{\underline{{x}}}_{k}\big)\;:=\;\mu\big(a\underline{{x}}_{(k-1)l+1:(k-1)l+l}\big).
$$  

By construction the environment   $(\ddot{A},\ddot{X},\tilde{\mu})$  ) is a bandit and thus by Lemma 3.5.4 it is an ergodic  MDP . 2  

The above results show that Bernoulli schemes, classiﬁcation problems, ban- dits and repeated strategic games are either ergodic  MDP s or can be reduced to one. As such, they all admit self-optimising policies and thus an appropriately deﬁned universal agent    $\pi^{\zeta}$    is self-optimising in these classes of environments. As the rewards received in totally passive environments are independent of the agent’s behaviour, these trivially admit self-optimising agents, indeed all agents are equally “optimal”. What about function optimisation and sequence prediction problems?  

Unfortunately, the above approach does not work for function optimisation problems as we have deﬁned them. The problem is that they are not  MDP s as the reward signal depends on more than just the current action and the last observation. They can be modelled as a  POMDP  by including the last reward in the core  MDP  and making this unobservable. In any case, this is not a problem because any agent that enumerates the action space will eventually hit upon the optimal action and thus is self-optimising. What this highlights is that being self-optimising only tells us something about performance in the limit, it says nothing about how quickly an agent will learn to perform well.  

Sequence prediction problems are more problematic. In general, no agent can be self-optimising over the class of all sequence prediction problems. To see this, simply consider that for any prediction agent there exists a sequence where the next observation is always the observation which the agent predicted would be the least likely (for a formal statement of this see Lemma 5.2.4). This is true even for incomputable agents. If we restrict the sequences to have com- putable distributions, but still allow the agent to be incomputable, then we have seen that Solomonoﬀ’s predictor has a bounded total expected predic- tion error. As the prediction error converges to zero, the reward converges to optimal and so Solomonoﬀ’s predictor is self-optimising. Given that the uni- versal agent was built upon the same foundations as Solomonoﬀ’s predictor, we might then expect the same result to hold. At present nobody has been able to prove this, though it is conjectured to be true. Currently the best bound is exponentially worse and holds only for deterministic computable sequence prediction (Section 6.2.2 of Hutter, 2005). For our purposes an exponentially worse bound is still ﬁnite and thus it follows that a universal agent deﬁned over the space of computable sequences is self-optimising.  

# 3.6. Conclusion  

In this chapter we have deﬁned a range of classes of environments and shown that many of these are either special cases of other more general classes, or are at least reducible to more elementary classes through a change of action and perception spaces. This hierarchy of classes deﬁnes a kind of taxonomy of environments. To the best of our knowledge this analysis has not been done before. Figure 3.1 summarises these relationships. The most general and powerful classes of environments are at the top and the most limited and speciﬁc classes at the bottom. As we can see, all of the more concrete classes at the bottom of the hierarchy admit self-optimising agents. By Theorem 2.10.5 it then follows that a universal agent deﬁned over one of these classes is also self-optimising. This supports our earlier claim that universal agents are able to perform well in a wide range of environments, as required by our informal deﬁnition of intelligence.  

![](images/c71b1bbf590e66c38cf548781eb54489fa044f7ff7fede3518e162002de5e8cf.jpg)  

Figure 3.1.: Taxonomy of environments. Downward arrows indicate that the class below is a special case of the class above. Dotted horizontal lines indicate that two classes of environments are reducible to each other. The greyed area contains the classes of environments that admit self-optimising agents, that is, the environments in which a universal agent will learn to behave optimally.  

![](images/6ec73ce7edcb7e90c5f32aa6268f8411bd526401fbd8de59c5009a45dfd8e8bc.jpg)  

# 4. Universal Intelligence Measure  

“. . . we need a deﬁnition of intelligence that is applicable to ma- chines as well as humans or even dogs. Further, it would be helpful to have a relative measure of intelligence, that would enable us to judge one program more or less intelligent than another, rather than identify some absolute criterion. Then it will be possible to assess whether progress is being made . . . ” Johnson (1992)  

In Chapter 1 we explored the concept of intelligence and proposed an in- formal deﬁnition of intelligence. In Chapter 2 we introduced universal agents, and in Chapter 3 we detailed some of the classes of environments in which their behaviour converges to optimal. This shows that universal agents are highly intelligent with respect to the deﬁnition of intelligence that we have adopted. One could argue that the universal agent deﬁned over the space of all enumerable chronological environments, that is  AIXI , is in some sense an optimal machine intelligence.  

In this chapter we turn this idea on its head: Instead of using the theory of universal artiﬁcial intelligence to deﬁne powerful agents, we use it instead to formally deﬁne intelligence itself. One approach is to take  AIXI  and to math- ematically deﬁne a performance measure under which  AIXI  is the maximal agent by construction. This is the approach taken by the  Intelligence Order Relation  (see Section 5.1.4 in Hutter, 2005). Although this produces a very general relation for comparing the relative performance of agents, in order to justify calling this a formal deﬁnition of “intelligence” one must carefully ex- amine the way in which intelligence is deﬁned, and then show how this relates to the equation.  

In this chapter we bridge this gap by proceeding in the opposite direction. We begin with our informal deﬁnition of intelligence from Chapter 1 that was based on a range of standard deﬁnitions given by psychologists and artiﬁcial intelligence researchers. We then formalise this deﬁnition, borrowing ideas from reinforcement learning, Kolmogorov complexity, Solomon o induction and universal artiﬁcial intelligence theory as necessary. The result is an equa- tion for intelligence that is strongly related to existing deﬁnitions, and with respect to which highly intelligent agents can be proven to have powerful op- timality properties. We then look at some of this deﬁnition’s properties and compare it to other tests and deﬁnitions of machine intelligence.  

# 4.1. A formal deﬁnition of machine intelligence  

Consider again our informal deﬁnition of intelligence:  

Intelligence measures an agent’s ability to achieve goals in a wide range of environments.  

This deﬁnition contains three essential components: An agent, environments and goals. Clearly, the agent and the environment must be able to interact with each other, speciﬁcally, the agent needs to be able to send signals to the environment and also receive signals being sent from the environment. Similarly, the environment must be able to send and receive signals. In our terminology we will adopt the agent’s perspective on these communications and refer to the signals sent from the agent to the environment as  actions , and the signals sent from the environment to the agent as  perceptions .  

Our deﬁnition of an agent’s intelligence also requires there to be some kind of goal for the agent to try to achieve. Perhaps an agent could be intelligent, in an abstract sense, without having any objective to apply its intelligence to. Or perhaps the agent has no desire to exercise its intelligence in a way that aﬀects its environment. In either case, the agent’s intelligence would be unobservable and, more importantly, of no practical consequence. Intelligence then, at least the concrete kind that interests us, comes into eﬀect when the agent has an objective or goal that it actively pursues by interacting with its environment.  

The existence of a goal raises the problem of how the agent knows what the goal is. One possibility would be for the goal to be known in advance and for this knowledge to be built into the agent. The problem with this is that it limits each agent to just one goal. We need to allow agents that are more ﬂexible, speciﬁcally, we need to be able to inform the agent of what the goal is. For humans this is easily done using language. In general however, the possession of a suﬃciently high level of language is too strong an assumption to make about the agent. Indeed, even for something as intelligent as a dog or a cat, direct explanation is not very eﬀective.  

Fortunately there is another possibility which is, in some sense, a blend of the above two. We deﬁne an additional communication channel with the sim- plest possible semantics: a signal that indicates how good the agent’s current situation is. We will call this signal the  reward . The agent simply has to maximise the amount of reward it receives, which is a function of the goal. In a complex setting the agent might be rewarded for winning a game or solving a puzzle. If the agent is to succeed in its environment, that is, receive a lot of reward, it must learn about the structure of the environment and in particular what it needs to do in order to get reward.  

This system of an agent interacting with an environment and trying to achieve some goal is the reinforcement learning agent-environment framework from Section 2.8. That this framework ﬁts well with our informal deﬁnition of intelligence is not surprising given how simple and general it is. Indeed, it is not only used in artiﬁcial intelligence, in control theory it is known as the plant-controller framework. For example, the plant could be a nuclear power plant, and the controller a system designed to keep the reactor within safe operating guidelines. Even the way in which you might train your dog to perform tricks by rewarding certain behaviours ﬁts into this very general framework.  

As in Section 2.8, we will include the reward signal as a part of the perception generated by the environment. The perceptions also contain a non-reward part, which we will refer to as  observations . The goal is implicitly deﬁned by the environment as this is what controls when rewards are generated. Thus, in the framework as we have deﬁned it, to test an agent in any given way it is suﬃcient to fully deﬁne the environment.  

Unfortunately, maximising reward is not suﬃcient to deﬁne how the agent should behave over time. We have to deﬁne some kind of a temporal preference that describes how much the agent should value near term rewards verses rewards further into the future. As we saw in Section 2.9, a general approach is to weight, or discount, each reward in a way that depends on which cycle it occurs in. Let  $\gamma_{1},\gamma_{2},.\cdot\cdot.$   be the discounts we apply to the reward in each successive cycle, where    $\forall i:\gamma_{i}\geq0$  , and    $\textstyle\sum_{i=1}^{\infty}\gamma_{i}<\infty$   ∞ in order to avoid inﬁnite weighted sums. Now deﬁne the  expected future discounted reward  for agent    $\pi$  interacting with environment  µ  to be,  

$$
V_{\gamma}^{\pi\mu}:=\mathrm{\bf~E}\Bigg(\sum_{i=t}^{\infty}\gamma_{i}r_{i}\Bigg)\,.
$$  

It is this value function that incorporates our temporal preferences that the agent must optimise. Although this is very general, the discounting parame- ters    $\gamma_{1},\gamma_{2},.\cdot\cdot.$   are nevertheless free parameters. In order to make our formal measure unique we want to remove these parameters, and of course we must do so in a way that is still completely general.  

If we look at the value function above, we see that discounting plays two roles. Firstly, it normalises rewards received so that their sum is always ﬁnite. Secondly, it weights the rewards at diﬀerent points in the future which in eﬀect deﬁnes a temporal preference. A direct way to solve both of these problems, without needing an external parameter, is to simply require the total reward returned by the environment to be bounded. Without loss of generality, we set the bound to be 1. We denote this set of  reward-summable environments by    $\mathbb{E}$  . For any    $\mu\in\mathbb{E}$  , it follows that the expected value of the sum of rewards is also ﬁnite and thus discounting is no longer required,  

$$
V_{\mu}^{\pi}:=\,\mathbf{E}\left(\sum_{i=1}^{\infty}r_{i}\right)\leq1.
$$  

One way of viewing this is that the rewards returned by the environment now have the temporal preference already factored in. Indeed, because every reward summable environment is included, in eﬀect every possible temporal preference is represented in the space of environments. The cost is that this is an additional condition that we place on the space of environments. Previously we required that each reward signal was in a subset of   $[0,1]\cap\mathbb{Q}$  , now we have the additional constraint that the reward sum is always bounded.  

Next we need to quantify what we mean by “goals in a  wide  range of environ- ments.” As we have argued previously, intelligence is not simply the ability to perform well at a narrowly deﬁned task; it is much broader. An intelli- gent agent is able to adapt and learn to deal with many diﬀerent situations, kinds of problems and types of environments. In our informal deﬁnition this was described as the agent’s general ability to perform well in a “wide range of environments.” This ﬂexibility is a deﬁning characteristic and one of the most important diﬀerences between humans and many current AI systems: while Gary Kasparov would still be a formidable player if we were to change the rules of chess, IBM’s Deep Blue chess super computer would be rendered useless without signiﬁcant human intervention.  

As we want our deﬁnition to be as broad and encompassing as possible, the space of environments used should be as large as possible. As the environment is a probability measure with a certain structure, an obvious possibility would be to consider the space of  all  probability measures of this form. Unfortu- nately, this extremely broad class of environments causes serious problems. As the space of all probability measures is uncountably inﬁnite, some environ- ments cannot be described in a ﬁnite way and so are incomputable. This would make it impossible, by deﬁnition, to test an agent in such an environment us- ing a computer. Further, most environments would be inﬁnitely complex and have little structure for the agent to learn from.  

The solution is to require the environmental probability measures to be computable. Not only is this condition necessary if we are to have an eﬀective measure of intelligence, it is also not as restrictive as it might ﬁrst appear. There are still an inﬁnite number of environments with no upper bound on their maximal complexity. Also, although the measures that describe the en- vironments are computable, this does not mean that the environments are de- terministic. For example, although a typical sequence of   $1$  ’s and   $0$  ’s generated by ﬂipping a coin is not computable, the probability measure that describes this distribution is computable and thus it is included in our space of pos- sible environments. Indeed, there is currently no evidence that the physical universe cannot be simulated by a Turing machine in the above sense (for fur- ther discussion of this point see Section 4.4). This appears to be the largest reasonable space of environments.  

We have now formalised all the elements of our informal deﬁnition. The next problem is how to bring these together in order to deﬁne an overall measure of performance; we need to ﬁnd a way to combine an agent’s performance in many diﬀerent environments into a single overall measure. As there are an inﬁnite number of environments, we cannot simply take a uniform distribution over them. Mathematically, we must weight some environments higher than others. But how?  

Consider the agent’s perspective on this situation: there exists a probabil- ity measure that describes the true environment, however this measure is not known to the agent. The only information the agent has are some past ob- servations of the environment. From these, the agent can construct a list of probability measures that are consistent with the observations. We call these potential explanations of the true environment hypotheses. As the number of observations increases, the set of hypotheses shrinks and hopefully the remain- ing hypotheses become increasingly accurate at modelling the environment.  

The problem is that in any given situation there will likely be a large num- ber of hypotheses that are consistent with the current set of observations. The agent must keep these in accordance with Epicurus’ principle of multiple explanations, as we saw in Section 2.1. Because they are all consistent with the current observations, if the agent is going to estimate which hypotheses are the most likely to be correct it must resort to something other than this observational information. This is a frequently occurring problem in inductive inference for which the most common approach is to invoke the principle of Occam’s razor, which we also met in Section 2.1:  

Given multiple hypotheses that are consistent with the data, the simplest should be preferred.  

This is generally considered the rational and intelligent thing to do (Wallace, 2005). Indeed, as noted in Section 1.5, standard IQ tests implicitly test an individual’s ability to use Occam’s razor. In some cases we may even consider the correct use of Occam’s razor to be a more important demonstration of intelligence than achieving a successful outcome. Consider, for example, the following game:  

4.1.1 Example. (Dumb luck game)  A questioner lays twenty  $\S10$   notes out on a table before you and then points to the ﬁrst one and asks “Yes or No?”. If you answer “Yes” he hands you the money. If you answer “No” he takes it from the table and puts it in his pocket. He then points to the next  $\S10$   note on the table and asks the same question. Although you, as an intelligent agent, might experiment with answering both “Yes” and “No” a few times, by the   $13^{t h}$    round you would have decided that the best choice seems to be “Yes” each time. However what you do not know is that if you answer “Yes” in the   $13^{t h}$    round then the questioner will pull out a gun and shoot you! Thus, although answering “Yes” in the   $13^{t h}$    round is the most intelligent choice, given what you know, it is not the most successful one. An exceptionally dim individual may have failed to notice the obvious relationship between answers and getting the money, and thus might answer “No” in the  $13^{t h}$    round, thereby saving his life due to what could truly be called “dumb luck”. 3  

What is important then, is not that an intelligent agent succeeds in any given situation, but rather that it takes actions that we would expect to be the most likely ones to lead to success. Given adequate experience this might be clear, however experience is often not suﬃcient and one must fall back on good prior assumptions about the world, such as Occam’s razor. It is important then that we test the agents in such a way that they are, at least on average, rewarded for correctly applying Occam’s razor, even if in some cases this leads to failure.  

Note that this does not necessarily mean always following the simplest hy- pothesis that is consistent with the observations. It is just that simpler hy- potheses are considered to be more likely to be correct. Thus, if there is a simple hypothesis suggesting one thing, and a large number of slightly more complex hypotheses suggesting something else, the latter may be considered the most likely.  

There is another subtlety that needs to be pointed out. Often intelligence is thought of as the ability to deal with complexity. Or in the words of one psy- chologist, “. . . [intelligence] is the ability to deal with cognitive complexity — in particular, with complex information processing.”(Gottfredson, 1997b) It is tempting then to equate the diﬃcultly of an environment with its complex- ity. Unfortunately, things are not so straightforward. Consider the following environment:  

4.1.2 Example. Imagine a very complex environment with a rich set of relationships between the agent’s actions and observations. The measure that describes this will have a high complexity. However, also imagine that the re- ward signal is always maximal no matter what the agent does. Thus, although this is a very complex environment in which the agent is unlikely to be able to predict what it will observe next, it is also an easy environment in the sense that all agents are optimal, even very simple ones that do nothing at all. The environment contains a lot of structure that is irrelevant to the goal that the agent is trying to achieve. 3  

From this perspective, a problem is thought of as being diﬃcult if the sim- plest good solution to the problem is complex. Easy problems on the other hand are those that have simple solutions. This is a very natural way to think about the diﬃculty of problems, or in our terminology, environments.  

Fortunately, this distinction does not aﬀect our use of Occam’s razor. This is because Occam’s razor assigns to each hypothesis a prior probability of it being the correct model according to its complexity. It says nothing about how relevant or useful that hypothesis might be to the agent’s goals. For example, according to Occam’s razor a simple environment that always gives the agent maximal reward would be more likely than a complex environment that also always gives the agent maximal reward, even though the two environments are equally easy to succeed in. Of course from an agent’s perspective, an incorrect hypothesis that fails to model much of the environment may be a good one if the parts of the environment that the hypothesis fails to model are not relevant to receiving reward. Nevertheless, if we want to reward agents on average for correctly using Occam’s razor, we must weight the environments according to their complexity, not their diﬃculty.  

Although we have chosen to follow a fairly strict interpretation of Occam’s razor, the idea of weighting according to the complexity of the simplest good solution may have some merit. For example, if we weight the diﬀerent “ex- perts” in a  prediction with expert advice  algorithm according to their complex- ity, we are in eﬀect applying this alternate principle. In practice, this can work well.  

Our remaining problem now is to measure the complexity of environments. This is a problem that we have already solved for sequences in Section 2.5, and then generalised to active environments in Section 2.10: for any environment  $\mu\in\mathbb{E}$   we deﬁne its complexity to be  $K(\mu)$  , that is, its Kolmogorov complexity which is essentially just the length of the shortest program that describes    $\mu$  .  

Bringing all these pieces together, we can now deﬁne our formal measure of intelligence:  

4.1.3 Deﬁnition. The  universal intelligence  of an agent    $\pi$   is its expected performance with respect to the universal distribution   $2^{-K(\mu)}$    over the space of all computable reward-summable environments    $\mathbb{E}$  , that is,  

$$
\Upsilon(\pi)\;:=\;\sum_{\mu\in\mathbb{E}}\,2^{-K(\mu)}\,V_{\mu}^{\pi}\;=\;V_{\xi}^{\pi}.
$$  

The ﬁnal equality above follows from the linearity of    $V$   and the deﬁnition of    $\xi$  as a weighted mixture of environments. It shows that the universal intelligence of an agent is simply its expected performance with respect to the universal distribution.  

Consider how this equation corresponds to our informal deﬁnition. We need to measure an agent’s ability to achieve goals in a wide range of environments. Clearly present in the equation is the agent    $\pi$  , the environment    $\mu$   and, implicit in the environment, a goal. The agent’s “ability to achieve” is represented by the value function    $V_{\mu}^{\pi}$    . By a “wide range of environments” we have taken the space of all computable reward-summable environments, where these environ- ments have been characterised as computable chronological measures in the set    $\mathbb{E}$  . Occam’s razor is given by the term   $2^{-K(\mu)}$    which weights the agent’s performance in each environment in a way that decreases according to its com- plexity. The deﬁnition is very general in terms of which sensors or actuators the agent might have, as all information exchanged between the agent and the environment takes place over very general communication channels. Finally, the formal deﬁnition places no limits on the internal workings of the agent. Thus, we can apply the deﬁnition to any system that is able to receive and generate information with a view to achieving goals.  

The main drawback is that the Kolmogorov complexity function    $K$   is not computable and can only be approximated. This is acceptable as our aim has simply been to deﬁne the concept of intelligence in the most general, powerful and elegant way. In future research we will explore ways to approximate this ideal with a practical test. Naturally, the process of estimation will introduce weaknesses and ﬂaws that the current deﬁnition does not have. For example, while the deﬁnition considers the general performance of an agent over all computable environments with bounded reward sum, in practice a test could only ever estimate this by testing the agent on a ﬁnite sample of environments.  

This situation is similar to the deﬁnition of randomness for sequences: In- formally, an inﬁnite sequence is said to be  Martin-L¨ of random  when it has no signiﬁcant regularity (Martin-L¨ of, 1966). This lack of regularity is equivalent to saying that the sequence cannot be compressed in any signiﬁcant way, and thus we can characterise randomness using Kolmogorov complexity. Naturally, we cannot test a sequence for every possible regularity, which is equivalent to saying that we cannot compute its Kolmogorov complexity. We can however test sequences for randomness by checking them for a large number of sta- tistical regularities; indeed, this is what is done in practice. Of course, just because a sequence passes all our tests does not mean that it must be random. There could always be some deeper structure to the sequence that our tests were not able to detect. All we can say is that the sequence seems random with respect to our ability to detect patterns.  

Some might argue that the deﬁnition of something should not just capture the concept, it should also be practical. For example, the deﬁnition of intel- ligence should be such that intelligence can be easily measured. The above example, however, illustrates why this approach is sometimes ﬂawed: if we were to  deﬁne  randomness with respect to a particular set of tests, then one could speciﬁcally construct a sequence that followed a regular pattern in such a way that it passed all of our randomness tests. This would completely under- mine our deﬁnition of randomness. A better approach is to deﬁne the concept in the strongest and cleanest way possible, and then to accept that our ability to test for this ideal has limitations. In other words, our task is to ﬁnd better and more eﬀective tests, not to redeﬁne what it is that we are testing for. This is the attitude we have taken here, though in this thesis our focus is on the ﬁrst part, that is, establishing a strong theoretical deﬁnition of machine intelligence.  

# 4.2. Universal intelligence of various agents  

In order to gain some intuition for our deﬁnition of intelligence, in this sec- tion we will consider a range of diﬀerent agents and their relative degrees of universal intelligence.  

A random agent. The agent with the lowest intelligence, at least among those that are not actively trying to perform badly, would be one that makes uniformly random actions. We will call this    $\pi^{\tt r a n d}$  . Although this is clearly a weak agent, we cannot simply conclude that the value of    $V_{\mu}^{\pi^{\tt r a n d}}$  will always be low as some environments will generate high reward no matter what the agent does. Nevertheless, in general such an agent will not be very successful as it will fail to exploit any regularities in the environment, even trivial ones. It follows then that the values of    $V_{\mu}^{\pi^{\tt r a n d}}$  will typically be low compared to other agents, and thus   $\Upsilon(\pi^{\tt r a n d})$   will be low. Conversely, if   $\Upsilon(\pi^{\tt r a n d})$   is very low, then the equation for   $\Upsilon$   implies that for simple environments, and many complex environments, the value of  $V_{\mu}^{\pi^{\tt r a n d}}$  must also be relatively low. This kind of poor performance in general is what we would expect of an unintelligent agent.  

A very specialised agent. From the equation for   $\Upsilon$  , we see that an agent could have very low universal intelligence but still perform extremely well at a few very speciﬁc and complex tasks. Consider, for example, IBM’s Deep Blue chess supercomputer, which we will represent by    $\pi^{\mathtt{d b1u e}}$  . When    $\mu$  chess describes the game of chess,    $V_{\mu^{\tt c h e s s}}^{\pi^{\tt d b l u e}}$   is very high. However   $2^{-K(\mu^{\tt c r e s s})}$   is small, and for    $\boldsymbol{\mu}\neq\boldsymbol{\mu}^{\mathtt{c h e s s}}$    the value function will be low as    $\pi^{\mathtt{d b1u e}}$    only plays chess. Therefore, the value of   $\Upsilon(\pi^{\tt d b l u e})$   will be very low. Intuitively, this is because Deep Blue is too inﬂexible and narrow to have general intelligence. Current artiﬁcial intelligence systems fall into this category: powerful in some domain, but not general and adaptable enough to be truly intelligent.  

Interestingly, universal intelligence becomes somewhat counter intuitive when we use it to compare very specialised agents. Consider an agent    $\pi^{\tt s i m p l e}$  which is only able to learn to predict sequences of the form   $0000\ldots$  .  and 1111  . . . . Obviously this agent will fail in most environments and thus will have a low universal intelligence, as we would expect. However, environments of this form will have short programs and thus are much more likely than environments which describe, for example, chess. As    $\pi^{\mathtt{d b1u e}}$    can  only  play chess, it cannot learn to predict these simple sequences, it follows then that  $\Upsilon(\pi^{\tt d b l u e})<\Upsilon(\pi^{\tt s i m p l e})$  . Intuitively we would expect the reverse to be true.  

What this shows is that the universal intelligence measure strongly empha- sises the ability to solve simple problems. If any system cannot do this, even if it can do something relatively complex like play chess, then it is considered to have very little intelligence. Of course extreme cases such the one above only occur with artiﬁcial constructions such as chess playing machines. Any human able to play chess would easily be able to learn to predict trivial patterns such as   $0000\ldots$  .  

With the above in mind, it is interesting to consider the progress of artiﬁcial intelligence as a ﬁeld from the perspective of universal intelligence. In the early days of artiﬁcial intelligence there was a lot of emphasis on developing machines that were able to do simple reasoning and pattern matching etc. Extending the power of these general systems was diﬃcult and over time the ﬁeld become increasingly concerned with very narrow systems that were able to solve quite speciﬁc problems. This has lead some people to complain that while we now have impressive systems for some speciﬁc things, we have not progressed much towards true intelligence, meaning artiﬁcial general intelligence. Indeed, from the perspective of universal intelligence, by focusing on increasingly specialised systems we have in fact have gone backwards.  

A general but simple agent. Imagine an agent that performs very basic learning by building up a table of observation and action pairs and keeping statistics on the rewards that follow. Each time an observation that it has seen before occurs, the agent takes the action with highest estimated expected reward in the next cycle with 0.9 probability, or a random action with 0.1 probability. We will call this agent  $\pi^{\tt b a s i c}$  . It is clear that many environments, both complex and very simple, will have at least some structure that such an agent would take advantage of. Thus, for almost all    $\mu$   we will have    $V_{\mu}^{\pi^{\tt b a s c}}>$   $V_{\mu}^{\pi^{\mathrm{rand}}}.$  and so   $\Upsilon(\pi^{\tt b a s i c})>\Upsilon(\pi^{\tt r a n d})$  . Intuitively, this is what we would expect as    $\pi^{\tt b a s1c}$  , while very simplistic, is surely more intelligent than    $\pi^{\tt r a n d}$  .  

Similarly, as    $\pi^{\mathtt{d b1u e}}$    will fail to take advantage of even trivial regularities in some of the most basic environments,   $\Upsilon(\pi^{\tt b a s i c})>\Upsilon(\pi^{\tt d b l u e})$  . This is reason- able as our aim is to measure a machine’s level of general intelligence. Thus an agent that can take advantage of basic regularities in a wide range of envi- ronments should rate more highly than a specialised machine that fails outside of a very limited domain.  

A simple agent with more history. The ﬁrst order structure of    $\pi^{\tt b a s1c}$  , while very general, will miss many simple exploitable regularities. Consider the following environment  $\mu^{\mathtt{a l t}}$  . Let    ${\mathcal{A}}\,=\,\{{\mathfrak{u p}},{\mathfrak{d o w n}}\}$   and    ${\mathcal{O}}\,=\,\{\varepsilon\}$  . In cycle    $k$  the environment generates a reward of 2  $2^{-k}$    each time the agent’s action is diﬀerent to its previous action. Otherwise the reward is   $0$  . We can deﬁne this environment formally,  

$$
\mu^{\mathtt{a l t}}(a x_{<k}a\underline{{x}}_{k}):=\left\{\begin{array}{l l}{1}&{\mathrm{if}\ a_{k}\neq a_{k-1}\wedge r_{k}=2^{-k},}\\ {1}&{\mathrm{if}\ a_{k}=a_{k-1}\wedge r_{k}=0,}\\ {0}&{\mathrm{otherwise}.}\end{array}\right.
$$  

Clearly the optimal strategy for an agent is simply to alternate between the actions  up  and  down . Even though this is very simple, this strategy requires the agent to correlate its current action with its previous action, something that    $\pi^{\tt b a s i c}$    cannot do. Note that we set the reward in cycle    $k$   to be   $2^{k}$    in order to satisfy our bounded reward sum condition.  

A natural extension of    $\pi^{\tt b a s i c}$    is to use a longer history of actions, ob- servations and rewards in its internal table. Let    $\pi^{2{\tt b a c k}}$    be the agent that builds a table of statistics for the expected reward conditioned on the last two actions, rewards and observations. It is immediately clear that    $\pi^{2{\tt b a c k}}$  will exploit the structure of the  $\mu^{\mathtt{a l t}}$    environment. Furthermore, by deﬁnition

  $\pi^{2{\tt b a c k}}$    is a generalisation of    $\pi^{\tt b a s i c}$    and thus it will adapt to any regularity that

  $\pi^{\tt b a s i c}$     $V_{\mu}^{\pi^{\tt2b a c k}}\,>\,V_{\mu}^{\pi^{\tt b a s i c}}$  can adapt to. It follows then that in general   and so

  $\Upsilon(\pi^{\tt2b a c k})>\Upsilon(\pi^{\tt b a s i c})$  , as we would expect. In the same way we can extend the history that the agent utilises back further and produce even more powerful  

![](images/c9e657225388a04cad790e80d831cc6ab248cf7ecf0e2d132fdbe59caafe9da5.jpg)  
Figure 4.1.: A simple game in which the agent climbs a playground slide and slides back down again. A shortsighted agent will always just rest at the bottom of the slide.  

agents that are able to adapt to more lengthy temporal structures and which will have still higher universal intelligence.  

A simple forward looking agent. In some environments simply trying to maximise the next reward is not suﬃcient, the agent must also take into ac- count the rewards that are likely to follow further into the future, that is, the agent must plan ahead. Consider the following environment    $\mu^{\mathtt{s l i d e}}$  . Let  $\mathcal{A}=\{\mathtt{r e s t},\mathtt{c l i m b}\}$   and    $\mathcal{O}=\{\varepsilon\}$  . Imagine there is a slide such as you would see in a playground. The agent can rest at the bottom of the slide, for which it receives a reward of  $2^{-k-4}$  . The alternative is to climb the slide, which gives a reward of 0. Once at the top of the slide the agent always slides back down no matter what action is taken; this gives a reward of   $2^{-k}$  . This deterministic environment is illustrated in Figure 4.1.  

Because climbing receives a reward of 0, while resting receives a reward of  $2^{-k-4}$  , a very shortsighted agent that only tries to maximise the reward in the next cycle will choose to stay at the bottom of the slide. Both    $\pi^{\tt b a s i c}$    and  $\pi^{2{\tt b a c k}}$    have this problem, even though they also take random actions with 0.1 probability and so will occasionally climb the slide by chance. Clearly this is not optimal in terms of total reward over time.  

We can extend the    $\pi^{2{\tt b a c k}}$    agent again by deﬁning a new agent  $\pi$  2forward   that with 0.9 probability chooses its next action to maximise not just the next reward, but   $\hat{r}_{k}+\hat{r}_{k+1}$  , where   $\hat{r}_{k}$   and   $\hat{r}_{k+1}$   are the agent’s estimates of the next two rewards. As the estimate of   $\hat{r}_{k+1}$   will potentially depend not only on    $a_{k}$  , but also on    $a_{k+1}$  , the agent assumes that    $a_{k+1}$   is chosen to simply maximise the estimated reward  $\hat{r}_{k+1}$  .  

$\pi^{2\mathtt{f o r w a r d}}$    The   agent can see that by missing out on the resting reward of  $2^{-k-4}$    for one cycle and climbing, a greater reward of   $2^{-k}$    will be had when sliding back down the slide in the following cycle. Note that the value of the time index    $k$   will have increased by the time the agent gets to slide down, however this is not enough to change the optimal course of action.  

By deﬁnition  π 2forward   generalises    $\pi^{2{\tt b a c k}}$    in a way that more closely reﬂects the value function    $V$   and thus in general    $V_{\mu}^{\pi^{2\tt f o r w a r d}}\,>\,V_{\mu}^{\pi^{2\tt b a c k}}$  . It then follows that   $\Upsilon(\pi^{2\tt f o r w a r d})\;>\;\Upsilon(\pi^{2\tt b a c k})$   as we would intuitively expect for this more powerful agent.  

In a similar way agents of increasing complexity and adaptability can be deﬁned which will have still greater intelligence. However, with more complex agents it is usually diﬃcult to see whether one agent has more universal intel- ligence than another. Nevertheless, the simple examples above illustrate how the more ﬂexible and powerful an agent is, the higher    $V$   typically is and thus the higher its universal intelligence.  

A very intelligent agent. A very intelligent agent would perform well in simple environments, and reasonably well compared to most other agents in more complex environments. From the equation for universal intelligence this would clearly produce a high value for   $\Upsilon$  . Conversely, if   $\Upsilon$   was high then the equation for   $\Upsilon$   implies that the agent must perform well in most simple environments and reasonably well in many complex ones also. Thus, the agent is able to achieve goals in a wide range of environments, as required by our informal deﬁnition of intelligence.  

A super intelligent agent. Consider what would be required to maximise the value of   $\Upsilon$  . By deﬁnition, a “perfect” agent would always pick the action which had greatest expected future reward. To do this, for every environment  $\mu\in\mathbb{E}$   the agent must take into account how likely it is that it is facing    $\mu$  , given the interaction history so far and the prior probability of    $\mu$  , that is,  $2^{-K(\mu)}$  . It would then consider all possible future interactions that might occur, how likely they are, and from this select the action in the current cycle that maximises the expected future reward.  

Essentially, this is the  AIXI  agent described in Section 2.10. The only dif- ference is that  AIXI  was deﬁned using discount parameters, while universal intelligence avoided these by requiring the total reward from environments to be bounded. If we remove discounting for  AIXI  and deﬁne it to work over reward bounded environments, then the universal intelligence measure is in some sense the dual of the universal agent  AIXI . It follows that agents with very high universal intelligence have powerful performance characteristics.  

With our modiﬁed  AIXI  being the most intelligent agent by construction, we can deﬁne the upper bound on universal intelligence to be,  

$$
\bar{\Upsilon}\;:=\;\operatorname*{max}_{\pi}\Upsilon(\pi)\;=\;\Upsilon\bigl(\pi^{\xi}\bigr).
$$  

This upper bounds the intelligence of all future machines, no matter how powerful their hardware and algorithms might be.  

A human. For simple environments, a human should be able to identify their structure and exploit this to maximise reward. However, for more complex environments it is hard to know how well a human would perform. Much of the human brain is set up to process certain kinds of structured information from the human sense organs, and thus is quite specialised, at least compared to the extremely general setting considered here. Perhaps the amount of universal machine intelligence that a human has is not that high compared to some machine learning algorithms? It is diﬃcult to know without experimental results.  

# 4.3. Properties of universal intelligence  

What we have presented is a deﬁnition of machine intelligence. It is not a practical test of machine intelligence, indeed the value of   $\Upsilon$   is not computable due to the use of Kolmogorov complexity. Although some of the criteria by which we judge practical tests of intelligence are not relevant to a pure def- inition of intelligence, many of the desirable properties are similar. Thus, to understand the strengths and weaknesses of our deﬁnition, consider again the desirable properties for a test of intelligence from Section 1.4.  

Valid. The most important property of any proposed formal deﬁnition of intelligence is that it does indeed describe something that can reasonably be called intelligence. Essentially, this is the core argument of this chapter so far: we have taken a mainstream informal deﬁnition and step by step formalised it. Thus, so long as our informal deﬁnition is acceptable, and our formalisation argument holds, the result can reasonably be described as a formal deﬁnition of intelligence.  

As we saw in the previous section, universal intelligence orders the power and adaptability of simple agents in a natural way. Furthermore, a high value of   $\Upsilon$   implies that the agent performs well on most simple and moderately complex environments. Such an agent would be an impressively powerful and ﬂexible piece of technology, with many potential uses. Clearly then, univer- sal intelligence is inherently meaningful, independent of whether or not one considers it to be a measure of intelligence.  

Informative.  $\Upsilon(\pi)$   assigns to agent    $\pi$   a real value that is independent of the performance of other possible agents. Thus we can make direct comparisons between many diﬀerent agents on a single scale. This property is useful if we want to use this measure to study new algorithms or modiﬁcations to existing algorithms. In comparison, some other tests return only comparative results, i.e. that one algorithm is better than another, or even just a binary pass or fail.  

Wide range. As we saw in the previous section, universal intelligence is able to order the intelligence of even the most basic agents such as    $\pi^{\tt r a n d}$  ,    $\pi^{\tt b a s i c}$  ,  $\pi^{2{\tt b a c k}}$    and    $\pi^{2\tt f o r w a r d}$  . At the other extreme we have the theoretical super in- telligent agent  AIXI  which has maximal   $\Upsilon$   value. Thus, universal intelligence spans trivial learning algorithms right up to incomputable super intelligent agents. This seems to be the widest range possible for a measure of machine intelligence.  

General. As the agent’s performance on all well deﬁned environments is fac- tored into its   $\Upsilon$   value, a broader performance metric is diﬃcult to imagine. Indeed, a well deﬁned measure of intelligence that is broader than universal intelligence would seem to contradict the Church-Turing thesis as it would imply that we could eﬀectively measure an agent’s performance for some well deﬁned problem that was outside of the space of computable measures.  

Dynamic. Universal intelligence includes environments in which the agent has to learn and adapt its behaviour over time in order to maximise reward. As such, it is a so called “dynamic intelligence test” that allows rich interaction between the agent being tested and its environment (see Section 1.4). In comparison, most other intelligence tests are “static”, in the sense that they only require the agent to solve isolated one-oﬀproblems. Such tests cannot directly measure an agent’s ability to learn and adapt over time.  

Unbiased. In a standard intelligence test, an individual’s performance is judged on speciﬁc kinds of problems, and then these scores are combined to produce an overall result. Thus the outcome of the test depends on which types of problems it uses, and how each score is weighted to produce the end result. Unfortunately, how we do this is a product of many things, includ- ing our culture, values and the theoretical perspective on intelligence that we have taken. For example, while one intelligence test might contain many logical puzzle problems, another might be more linguistic in emphasis, while another stresses visual reasoning. Modern intelligence tests like the Stanford- Binet try to minimise this problem by covering the most important areas of human reasoning both verbally and non-verbally. This helps but it is still very anthropocentric as we are only testing those abilities that we think are important for human intelligence.  

For an intelligence measure for machines we have to base the test on some- thing more general and principled: universal Turing computation. As all pro- posed models of computation have thus far been equivalent in their expressive power, the concept of computation appears to be a fundamental theoretical property rather than the product of any speciﬁc culture. Thus, by weighting diﬀerent environments depending on their Kolmogorov complexity, and con- sidering the space of all computable environments, we have avoided having to deﬁne intelligence with respect to any particular culture, species etc.  

Unfortunately, we have not entirely removed the problem. The environmen- tal distribution   $2^{-K(\mu)}$    that we have used is invariant, up to a multiplicative constant, to changes in the reference machine  $\mathcal{U}$  . Although this aﬀords us some protection, the relative intelligence of agents can change if we change our ref- erence machine. One approach to this problem is to limit the complexity of the reference machine, for example by limiting its state-symbol complexity. We expect that for highly intelligent machines that can deal with a wide range of environments of varying complexity, the eﬀect of changing from one simple reference machine to another will be minor. For simple agents, such as those considered in Section 4.2, the ordering of their machine intelligence was also not particularly sensitive to natural choices of reference machine. Recently at- tempts have been made to make algorithmic probability completely unique by identifying which universal Turing machines are, in some sense, the most sim- ple (M¨ uller, 2006). Unfortunately however, an elegant solution to this problem has not yet been found.  

An alternate solution, suggested by Peter Dayan (personal communication), would be to allow the agent to maintain state between diﬀerent test environ- ments. This would mitigate any bias introduced as intelligent agents would then be able to adapt to the test’s reference machine over multiple test trials.  

Fundamental. Universal intelligence is based on Turing computation, infor- mation and complexity. These are fundamental universal concepts that are unlikely to change in the future with changes in technology. It also means that universal intelligence is in no way anthropocentric.  

Formal and objective. As universal intelligence is expressed as a mathemati- cal equation, there is little space for ambiguity in the deﬁnition. In particular, it in no way depends on any subjective criteria, unlike some other intelligence tests and deﬁnitions.  

Fully deﬁned. For a ﬁxed reference machine, the universal intelligence mea- sure is fully deﬁned. In comparison, some tests of machine intelligence have aspects which are currently unspeciﬁed and in need of further research.  

Impractical. In its current form the deﬁnition cannot be directly turned into a test of intelligence as the Kolmogorov complexity function is not computable. Thus, in its pure form, we can only use it to analyse the nature of intelligence and to theoretically examine the intelligence of mathematically deﬁned learn- ing algorithms.  

In order to use universal intelligence more generally we will need to con- struct a workable test that approximates an agent’s   $\Upsilon$   value. The equation for   $\Upsilon$   suggests how we might approach this problem. Essentially, an agent’s universal intelligence is a weighted sum of its performance over the space of all environments. Thus, we could randomly generate programs that describe envi- ronmental probability measures and then test the agent’s performance against each of these environments. After sampling suﬃciently many environments, the agent’s approximate universal intelligence could be estimated by weighting its score in each environment according to the complexity of the environment as given by the length of its program. Another possibility might be to try to approximate the sum by enumerating environmental programs from short to long, as the short ones will make by far the greatest contribution to the sum. However, in this case we will need to be able to reset the state of the agent so that it cannot cheat by learning our environmental enumeration method. In any case, various practical challenges will need to be addressed before univer- sal intelligence can be used to construct an eﬀective intelligence test. As this would be a signiﬁcant project in its own right, here we focus on the theoretical issues surrounding universal intelligence.  

Deﬁnition rather than a test. As it is not practical in its current form, uni- versal intelligence is more of a formal deﬁnition of intelligence than a test of intelligence. Some proposals we have reviewed aim to be just tests of intel- ligence, others aim to be deﬁnitions, and in some cases they are intended to be both. Often the exact classiﬁcation of a proposal as a test or deﬁnition, or both, is somewhat subjective.  

Having covered the key properties of the universal intelligence measure, we now compare these properties with the properties of the other proposed tests and deﬁnitions of machine intelligence surveyed in Section 1.7. Although we have attempted to be as fair as possible, some of our judgements will of course be debatable. Nevertheless, we hope that it provides a rough overview of the relative strengths and weaknesses of the proposals. The summary comparison appears in Table 4.1.  

# 4.4. Response to common criticisms  

Attempting to mathematically deﬁne intelligence is very ambitious and so, not surprisingly, the reactions we get can be interesting. Having presented the essence of this work as posters at several conferences, and also as a 30 minute talk, we now have some idea of what the typical responses are. Most people start out sceptical but end up generally enthusiastic, even if they still have a few reservations. This positive feedback has helped motivate us to continue this direction of research. In this section, however, we will attempt to cover some of the more common criticisms.  

It’s obviously false, there’s nothing in your deﬁnition, just a few equations. Perhaps the most common criticism is also the most vacuous one: It’s obviously  

![](images/bf241d0aa17f00e82d7a6a99136eb7c4fd78234b9bfa0f7148211fd4d2228789.jpg)  
Table 4.1.: In the table    $\bullet$  means “yes”,  •  means “debatable”,  ·  means “no”, and ? means unknown. When something is rated as unknown it is because the test in question is not suﬃciently speciﬁed.  

wrong! These people seem to believe that deﬁning intelligence with an equation is clearly impossible, and thus there must be very large and obvious ﬂaws in our work. Not surprisingly, these people are also the least likely to want to spend 10 minutes having the material explained to them. Unfortunately, none of these people have been able to communicate why the work is so obviously ﬂawed in any concrete way — despite in one instance chasing the poor fellow out of the conference centre and down the street begging for an explanation. If anyone would like to properly explain their position to us in the future, we promise not to chase you down the street!  

It’s obviously correct, indeed everybody already knows this. Curiously, the second most common criticism is the exact opposite: The work is obviously right, and indeed it is already well known. Digging deeper, the heart of this criticism comes from the perception that we have not done much more than just describe reinforcement learning. If you already accept that the reinforcement learning framework is the most general and ﬂexible way to describe artiﬁcial intelligence, and not everybody does, then by mixing in Occam’s razor and a dash of complexity theory the equation for universal intelligence follows in a fairly straightforward way. While this is true, the way in which these things have been brought together is new. Furthermore, simply coming up with an equation is not enough, one must argue that what the equation describes is in fact intelligence in a sense that is reasonable for machines.  

We have addressed this question in three main ways: Firstly, in Chap- ter 1 we explored many expert deﬁnitions of intelligence. Based on these, we adopted our own informal deﬁnition of intelligence in Section 1.2. In the present chapter this informal deﬁnition was piece by piece formalised leading to the equation for   $\Upsilon$  . This chain of argument ties our equation for intelli- gence to existing informal deﬁnitions and ideas on the nature of intelligence. Secondly, in Sections 4.2 and 4.3 we showed that the equation has properties that are consistent with a deﬁnition of intelligence. Finally, in Section 4.2 it was shown that universal intelligence is strongly connected to the theory of universally optimal learning agents, in particular  AIXI . From this it follows that machines with very high universal intelligence have a wide range of pow- erful optimality properties. Clearly then, what we have done goes far beyond merely restating reinforcement learning theory.  

Assuming that the environment is computable is too strong. It is certainly possible that the physical universe is not computable, in the sense that the probability distribution over future events cannot, even in theory, be simulated to an arbitrary precision by a computable process. Some people take this posi- tion on various philosophical grounds, such as the need for freewill. However, in standard physics there is no law of the universe that is not computable in the above sense. Nor is there any evidence showing that such a physical law must exist. This includes quantum theory and chaotic systems, both of which can be extremely diﬃcult to compute for some physical systems, but are not funda- mentally incomputable theories. In the case of quantum computers, they can compute with lower time complexity than classical Turing machines, however they are unable to compute anything that a classical Turing machine cannot, when given enough time. Thus, as there is no hard evidence of incomputable processes in the universe, our assumption that the agent’s environment has a computable distribution is certainly not unreasonable.  

If a physical process was ever discovered that was not Turing computable, then this would likely result in a new extended model of computation. Just as we have based universal intelligence on the Turing model of computation, it might be possible to construct a new deﬁnition of universal intelligence based on this new model in a natural way.  

Finally, even if the universe is not computable, and we do not update our formal deﬁnition of intelligence to take this into account, the fact that every- thing in physics so far is computable means that a computable approximation to our universe would still be extremely accurate over a huge range of situa- tions. In which case, an agent that could deal with a wide range of computable environments would most likely still function well within such a universe.  

Assuming that environments return bounded sum rewards is unrealistic. If an environment    $\mu$   is an artiﬁcial game, like chess, then it seems fairly natural for    $\mu$   to meet any requirements in its deﬁnition, such as having a bounded reward sum. However, if we think of the environment    $\mu$   as being the universe in which the agent lives, then it seems unreasonable to expect that it should be required to respect such a bound.  

Strictly speaking, reward is an interpretation of the state of the environ- ment. In this case the environment is the universe, and clearly the universe does not have any notion of reward for particular agents. In humans this in- terpretation is internal, for example, the pain that is experienced when you touch something hot. In this case, should it be a part of the agent rather than the environment? If we gave the agent complete control over rewards then our framework would become meaningless: the perfect agent could simply give itself constant maximum reward. Perhaps the analogous situation for humans would be taking the “perfect” drug.  

A more accurate framework would consist of an agent, an environment and a separate goal system that interpreted the state of the environment and re- warded the agent appropriately. In such a set up the bounded rewards restric- tion would be a part of the goal system and thus the above problem would not occur. However, for our current purposes, it is suﬃcient just to fold this goal mechanism into the environment and add an easily implemented constraint to how the environment may generate rewards. One simple way to bound an en- vironment’s total rewards would be to use geometric discounting as discussed in Section 2.9.  

How do you respond to Block’s “Blockhead” argument? The approach we have taken is unabashedly functional. Theoretically, we desired to have a formal, simple and very general deﬁnition. This is easier to do if we abstract over the internal workings of the agent and deﬁne intelligence only in terms of external communications. Practically, what matters is  how well something works . By deﬁnition, if an agent has a high value of   $\Upsilon$  , then it must work well over a wide range of environments.  

Block attacks this perspective by describing a machine that appears to be intelligent as it is able to pass the Turing test, but is in fact no more than just a big look-up table of questions and answers (Block, 1981, for a related argument see Gunderson, 1971). Although such a look-up table would be unfeasibly large, the fact that a ﬁnite machine could in theory consistently pass the Turing test, seemingly without any real intelligence, intuitively seems odd. Our formal measure of machine intelligence could be challenged in the same way, as could any test of intelligence that relies only on an agent’s external behaviour.  

Our response to this is very simple: if an agent has a very high value of  $\Upsilon$   then it is, by deﬁnition, able to successfully operate in a wide range of environments. We simply do not care whether the agent is eﬃcient, due to some very clever algorithm, or absurdly ineﬃcient, for example by using an unfeasibly gigantic look-up table of precomputed answers. The important point for us is that the machine has an amazing ability to solve a huge range of problems in a wide variety of environments.  

How do you respond to Searle’s “Chinese room” argument? Searle’s Chi- nese room argument attacks our functional position in a similar way by argu- ing that a system may appear to be intelligent without really understanding anything (Searle, 1980). From our perspective, whether or not an agent un- derstands what it is doing is only important to the extent that it aﬀects the measurable performance of the agent. If the performance is identical, as Searle seems to suggest, then whether or not the room with Searle inside understands the meaning of what is going on is of no practical concern; indeed, it is not even clear to us how to deﬁne understanding if its presence has no measurable eﬀects. So long as the system as a whole has the powerful properties required for universal machine intelligence, then we have the kind of extremely general and powerful machine that we desire. On the other hand, if understanding does have a measurable impact on an agent’s performance in some situations, then it is of interest to us. In which case, because   $\Upsilon$   measures performance in all well deﬁned situations, it follows that   $\Upsilon$   is in part a measure of how much understanding an agent has.  

But you don’t deal with consciousness (or creativity, imagination, freewill, emotion, love, soul, etc.) We apply the same argument to consciousness, emotions, freewill, creativity, the soul and other such things. Our goal is to build powerful and ﬂexible machines and thus these somewhat vague properties are only relevant to our goal to the extent to which they have some measurable eﬀect on performance in some well deﬁned environment. If no such measurable eﬀect exists, then they are not relevant to our objective. Of course this is not the same as saying that these things do not exist. The question is whether they are relevant or not. We would consider understanding, imagination and creativity, appropriately deﬁned, to have a signiﬁcant impact on an agent’s ability to adapt to challenging environments. Perhaps the same is also true of emotions, freewill and other qualities. If one accepts that these properties aﬀect an agent’s performance, then universal intelligence is in part a test for these properties.  

Intelligence is fundamentally an anthropocentric concept. As artiﬁcial in- telligence researchers our goal is not to create an “artiﬁcial human”. We are interested in making machines that are able to process information in pow- erful ways in order to achieve many kinds of goals and solve many kinds of problems. As such, a limited anthropocentric concept of intelligence is not interesting to us. Or at least, if such a deﬁnition were to be adopted, it would simply mean that we are interested in something more general and powerful than this human focused concept of “intelligence”.  

Perhaps this is similar to the development of heavier than air ﬂight. One of the most outspoken sceptics of this was the American astronomer Simon Newcomb. Interestingly, even after planes were regularly “ﬂying” he refused to accept defeat. He accepted that planes existed and moved around at great speed up in the air, however he did not accept that what they were doing was “ﬂying”. To his mind, what birds did was quite diﬀerent. Of course, the rest of the population simply generalised and adapted their concept of ﬂying to reﬂect the new technology.  

We believe that with more progress in artiﬁcial intelligence the same thing will eventually happen to the everyday concept of intelligence. At present, most people think of intelligence in human terms simply because this is the only kind of powerful intelligence they have ever encountered. Indeed, from the perspective of evolutionary psychology, it appears that we may have evolved to expect other intelligent agents to think and act like ourselves.  

Universal intelligence does not agree with some everyday intuitions about the nature of intelligence. Everyday intuitions are not a good guide. People informally use the word “intelligence” to mean a variety of diﬀerent things, and even a single person will use the word in multiple ways that are not consistent with each other. Thus, although our deﬁnition should clearly be related to the everyday concept, it is not necessarily desirable, or even possible, for a precise and self-consistent deﬁnition to always agree with everyday usage.  

Consider a word that has both an everyday meaning and a precise technical meaning. When someone says “It’s a beautiful spring day, I am full of energy and could run up a mountain”, what they mean by the word ‘energy’ is related to the concept of energy in physics, i.e. they need energy in the technical sense to get up the mountain. However, the deﬁnition of energy from physics does not entirely capture what they mean. This does not imply that there is something wrong with the concept of energy in physics. We expect the same in artiﬁcial intelligence: people will continue to use the word “intelligence” in an informal way, however in order to do research we will need to adopt a more precise deﬁnition that may be slightly diﬀerent.  

An agent may be intelligent even if it doesn’t achieve anything in any environments. Consider, for example, that you are quietly sitting in a dark room thinking about a problem you are trying to solve. Even though you are not achieving anything in your environment, some would argue that you are still intelligent due to your internal process of thought. Or imagine perhaps the famous physicist Stephen Hawking disconnected from his motorised wheelchair and talking computer. Although his ability to achieve goals in the environment would be limited, he is still no doubt highly intelligent. A related problem is that an agent may simply lack the motivation to exhibit intelligent behaviour, such as a child who wants to get an IQ test out of the way as soon as possibly in order to go outside and play. In both of these cases the agent’s intelligence seems to be divorced from the intelligence observable in its behaviour.  

The above highlights the fact that when informally considering the measure- ment of intelligence we need to be careful about exactly how the measurement is done. Obviously, it only makes sense to measure an individual’s intelligence if certain essential requirements are met: a purely written test for a blind person is senseless, as are the results of a test where the test subject has no interest in performing well. Informally, we just assume that the test has been conducted in an appropriate way. When we say that an agent is intelligent, what we actually mean is that there exists some reasonable setup such that the agent exhibits a high level of intelligence: a blind person may require an oral test, and a disinterested child some kind of a bribe. Clearly then, if we want to be precise about the measured intelligence of an agent we must specify these details about exactly how the test was conducted.  

When we say that an agent has low intelligence, what we mean is that there does not exist any reasonable test setup such that the agent exhibits intelligent behaviour. Some take the position that an entity could be intelligent even if it has no measurable intelligence under  any  test setup. However, such an agent’s “intelligence” would be rather meaningless as it would be a property that has no measurable eﬀects or consequences. Even the philosophical “brain in a vat” could in theory be interfaced to in order to measure its universal intelligence.  

Such an agent is not intelligent because it cannot choose its goals. In the setup we have deﬁned the agent cannot decide what its primary goal is. It simply tries to maximise the reward signal deﬁned by the environment. In the context of machines this is probably a good idea: we want to be the ones deﬁn- ing what the machine’s primary objective is. However, this does not address the question as to whether such a machine should really be called intelligent, or whether it is just a very powerful and general optimiser. Intelligent humans, after all, can choose their own goals in life. But is this really true?  

Obviously we can decide that we want to become a successful scientist, a teacher, or maybe a rock star. So we certainly have some choice in our goals. But are these things our  primary  motivations? If we want to be a successful scientist or a rock star, perhaps this is due to a deeper biological drive to attain high status because this increases our chances of reproductive success. Perhaps the desire to become a teacher stems from a biological drive to care for children, again because having this drive tends to increase our probability of passing on our genes and memes to future generations. Our interest in mating with an attractive member of the opposite sex, avoiding intense physical pain or the pleasure of eating energy rich foods, all of these things have clear biological motivations. Even a suicide bomber who kills himself and thus destroys his future reproductive potential may be driven by an out-of-control biological motivation that tries to increase his societal status in an eﬀort to improve his chances of reproductive success.  

These ideas appear in areas such as evolutionary psychology and, it must be said, attract a fair amount of controversy. Here we will not attempt to defend them. We simply note that they oﬀer one explanation as to how we are able to choose our goals in life, while at the same time having relatively ﬁxed fundamental motivations.  

Universal intelligence is impossible due to the No-Free-Lunch theorem. Some, such as Edmonds (2006), argue that universal deﬁnitions of intelligence are impossible due to Wolpert’s so called “No Free Lunch” theorem (Wolpert and Macready, 1997). However this theorem, or any of the standard variants on it, cannot be applied to universal intelligence for the simple reason that we have not taken a uniform distribution over the space of environments. Instead we have used a highly non-uniform distribution based on Occam’s razor.  

It is conceivable that there might exist some more general kind of “No Free Lunch” theorem for agents that limits their maximal intelligence according to our deﬁnition. Clearly any such result would have to apply only to computable agents given that the incomputable  AIXI  agent faces no such limit. If such a result were true, it would suggest that our deﬁnition of intelligence is perhaps too broad in its scope. Currently we know of no such result (c.f. Chapter 5).  

# 4.5. Conclusion  

Given the obvious signiﬁcance of formal deﬁnitions of intelligence for research, and calls for more direct measures of machine intelligence to replace the prob- lematic Turing test and other imitation based tests (Johnson, 1992), little work has been done in this area. In this chapter we have attempted to tackle this problem by taking an informal deﬁnition of intelligence modelled on expert deﬁnitions of human intelligence, and then formalising it. We believe that the resulting mathematical deﬁnition captures the concept of machine intelligence in a very powerful and elegant way. Furthermore, we have seen that agents which are extremely intelligent with respect to this deﬁnition, such as  AIXI , can be proven to have powerful optimality properties.  

The central challenge for future work on universal intelligence is to convert this theoretical deﬁnition of machine intelligence into a workable test. The basic structure of such a test is already apparent from the equation for   $\Upsilon$  : the test would work by evaluating the performance of an agent on a large sample of simulated environments, and then combine the agent’s performance in each environment into an overall intelligence value. This would be done by weight- ing the agent’s performance in each environment according the environment’s complexity.  

A theoretical challenge that will need to be dealt with is to ﬁnd a suitable replacement for the incomputable Kolmogorov complexity function. One so- lution could be to use    $K t$   complexity (Levin, 1973), another might be to use the Speed prior (Schmidhuber, 2002). Both of these consider the complexity of an algorithm to be determined by both its minimal description length and running time, thus forcing the complexity measures to be computable. Taking computation time into account also makes reasonable intuitive sense because we would not usually consider a very short algorithm that takes an enormous amount of time to run to be a particularly simple one. The fact that such an approach can be made to work is evidenced by the C-Test (see Section 1.7).  

![](images/8ac7675566c45741fce3faaebeeadbb8f51f036e922b2270aa488bd1a0ba6a3f.jpg)  

# 5. Limits of Computational Agents  

One of the reasons for studying mathematical models such as  AIXI  is to gain insights that may be useful for designing practical artiﬁcial intelligence agents. The question then arises as to how useful all this incomputable theory really is. In this chapter we will explore this question by looking at the theoretical limi- tations faced by computable sequence predictors that attempt to approximate the power and generality of Solomon o induction. As sequence prediction lies at the heart of reinforcement learning, any limitations found here will carry over to general computable agents.  

As we saw in Chapter 2, sequence prediction and compression are intimately related. Indeed, they are two diﬀerent ways of looking at the same problem. Intuitively, if you can accurately predict what is coming next you do not need to use much information to encode what the data actually is, and vice versa. For sequence predictors based on the  Minimum Description Length (MDL) principle  (Rissanen, 1996) or the  Minimum Message Length (MML) principle (Wallace and Boulton, 1968), this connection is especially evident as they predict by attempting to ﬁnd the shortest possible description, or model, of the data. Not surprisingly then, they can be viewed as computable approximations of Solomon o induction (Section 5.5 of Li and Vit´ anyi, 1997). Furthermore, in order to produce a working sequence predictor these methods can easily be combined with general purpose data compressors, such as the Lempel-Ziv algorithm (Feder et al., 1992) or Context Tree Weighting (Willems et al., 1995). Unfortunately, while useful in practice, these real world compressors have their limitations: they are able to ﬁnd some kinds of computable regularities in sequences, but not others. As such, predictors based on them fall short of the power and generality of Solomon o induction. Furthermore, even with ideal compression MDL and MML based predictors can take exponentially longer than Solomon o induction to learn as they use only the shortest description of the data, rather than the set of all possible descriptions (Poland and Hutter, 2004).  

Can we do better than this? Do universal and computable predictors for computable sequences exist? Unfortunately, it is easy to see that they do not: simply consider a sequence where the next bit is always the opposite of what the predictor predicts. This is essentially the same as what Dawid noted when he found that for any statistical forecasting system there exist sequences for which the predictor is not calibrated, and thus cannot be learnt (Dawid, 1985). However, he does not deal with the complexity of the sequences themselves, nor does he make a precise statement in terms of a speciﬁc measure of complexity. The impossibility of forecasting has since been developed in considerably more depth by V’yugin (1998), in particular he proves that there is an eﬃcient randomised procedure producing sequences that cannot be predicted, with high probability, by computable forecasting systems.  

In this chapter we study the prediction of computable sequences from the perspective of Kolmogorov complexity. The central question we look at is the prediction of sequences which have bounded Kolmogorov complexity. This leads us to a new notion of complexity: Rather than the length of the short- est program able to generate a given sequence, in other words standard Kol- mogorov complexity, we take the length of the shortest program able to learn to predict the sequence. This new complexity measure has the same fun- damental invariance property as Kolmogorov complexity, and certain strong relationships between the two measures are proven. Nevertheless, in some cases the two can diverge signiﬁcantly. For example, although a long random string that indeﬁnitely repeats has a very high Kolmogorov complexity, this sequence also has a relatively simple structure that even a simple predictor can learn to predict.  

We then prove that some sequences can only be predicted by very complex predictors. This implies that very general prediction algorithms, in particular those that can learn to predict all sequences up to a given Kolmogorov com- plexity, must themselves be complex. This puts an end to our hope of there being an extremely general and yet relatively simple prediction algorithm. We then use this fact to prove that although very powerful prediction algorithms exist, they cannot be mathematically discovered due to G¨ odel incompleteness. This signiﬁcantly constrains our ability to design and analyse powerful pre- diction algorithms, and indeed powerful artiﬁcial intelligence algorithms in general.  

# 5.1. Preliminaries  

For the basic notation for strings and sequences see Appendix A. In this chapter we will sometimes need to encode a natural number as a string. Using simple encoding techniques it can be shown that there exists a computable injective function    $f:\mathbb{N}\to\mathbb{B}^{*}$  where no string in the range of    $f$   is a preﬁx of any other, and  $\forall n\in\mathbb{N}:\ell(f(n))\leq\log_{2}n+2\log_{2}\log_{2}n+1=O(\log n)$  .  

Of particular interest to us will be the class of sequences which can be generated by an algorithm executed on the following type of machine:  

5.1.1 Deﬁnition. A  monotone universal Turing machine    $\mathcal{U}$   is deﬁned as a universal Turing machine with one unidirectional input tape, one unidi- rectional output tape, and some bidirectional work tapes. Input tapes are read only, output tapes are write only, unidirectional tapes are those where the head can only move from left to right. All tapes are binary (no blank symbol) and the work tapes are initially ﬁlled with zeros. We say that  $\mathcal{U}$   outputs/computes a sequence    on input   , and write    $\mathcal{U}(p)=\omega$  , if    $\mathcal{U}$   reads all of    but no more  $\omega$   $p$   $p$  as it continues to write    $\omega$   to the output tape.  

We ﬁx    $\mathcal{U}$   and deﬁne    $\mathcal{U}(p,x)$   by simply   standard coding technique to encode a program    along with a string  x  $\boldsymbol{x}\in\mathbb{B}^{\ast}$   ∈ ∗ as a single input string for  $p$   $\mathcal{U}$  . For simplicity of notation we will often wr e    $p(x)$   to mean the function computed by the program    $p$   when executed on  U  along with the input string  $x$  , that is,    $p(x)$   is short hand for    $\mathcal{U}(p,x)$  .  

5.1.2 Deﬁnition. A sequence  $\omega\in\mathbb{B}^{\infty}$  is a  computable binary sequence if there exists a program    $q\in\mathbb{B}^{*}$  that writes    $\omega$     a one-way output tape when run on a monotone universal Turing machine  U , t at is,    $\exists q\in\mathbb{B}^{*}:\mathcal{U}(q)=\omega$  . We denote the set of all computable sequences by .  

A similar deﬁnition for strings is not necessary as all strings have ﬁnite length and are therefore trivially computable: all an algorithm has to do is to copy the desired string to the output tape and then halt.  

5.1.3 Deﬁnition. A  computable binary predictor  is a program    $p\in\mathbb{B}^{*}$  that on a universal Turing machine  $\mathcal{U}$   computes a total function    $\mathbb{B}^{\ast}\rightarrow\mathbb{B}$  .  

Having    $x_{1:n}$   as input, the objective of a predictor is for its output, called its  prediction , to match the next symbol in the sequence. Formally we express this by writing    $p(x_{1:n})=x_{n+1}$  .  

Note that this is diﬀerent to earlier chapters where a predictor generated a distribution over the possible symbols that might occur next, rather than outputting the predicted symbol. What we consider here is a special case of probabilistic prediction as it is equivalent to a probabilistic predictor that in each cycle always assigns probability 1 to some symbol.  

As the algorithmic prediction of incomputable sequences, such as the halt- ing sequence, is impossible by deﬁnition, we only consider the problem of predicting computable sequences. To simplify things we will assume that the predictor has an unlimited supply of computation time and storage. We will also make the assumption that the predictor has unlimited data to learn from, that is, we are only concerned with whether or not a predictor can learn to predict in the following sense:  

5.1.4 Deﬁnition. We say that a predictor    $p$   can  learn to predict  a sequence    $\omega\;:=\;x_{1}x_{2}\ldots\;\in\;\mathbb{B}^{\infty}$  if there exists    $m\,\in\,\mathbb{N}$   such that    $\forall n\;\geq\;m\;\geq\;$  :  $p(x_{1:n})=x_{n+1}$  .  

The existence of    $_m$   in the above deﬁnition need not be constructive, that is, we might not know when the predictor will stop making prediction errors for a given sequence, just that this will occur eventually. This is essentially “next value” prediction as characterised by Barzdin (1972), which follows the notion of identiﬁability in the limit for languages from Gold (1967).  

5.1.5 Deﬁnition. Let    $\mathcal{P}(\omega)$   be the s l predictors able to learn to predict    $\omega$  . Similarly for sets of sequences  S  $S\subset\mathbb{B}^{\infty}$   ⊂ , deﬁne    $\begin{array}{r}{\mathcal{P}(S):=\bigcap_{\omega\in S}\mathcal{P}(\omega)}\end{array}$  P ).  

A standard measure of complexity for sequences is the length of the shortest program which generates the sequence:  

5.1.6 Deﬁnition. For any sequence    $\omega\in\mathbb{B}^{\infty}$  the  Kolmogorov complexity of the sequence is,  

$$
K(\omega)\,:=\,\operatorname*{min}_{q\in\mathbb{B}^{*}}\,\{\,\ell(q):\mathcal{U}(q)=\omega\,\},
$$  

where    $\mathcal{U}$   is a monotone universal Turing machine. If no such  exists, we deﬁne  $q$   $K(\omega):=\infty$  .  

In essentially the same way we can deﬁne the Kolmogorov complexity of a string    $x\in\mathbb{B}^{n}$  , written    $K(x)$  , by requiring that    $\mathcal{U}(q)$   halts after generating    $x$  on the output tape.  

It can be shown that Kolmogorov complexity depends on our choice of uni- versal Turing machine    $\mathcal{U}$  , but only up to an additive constant that is inde- pendent of    $\omega$  . This is due to the fact that a universal Turing machine can simulate any other universal Turing machine with a ﬁxed length program. For more explanation see Section 2.4, or for an extensive treatment of Kolmogorov complexity and some of its applications see (Li and Vit´ anyi, 1997) or (Calude, 2002).  

# 5.2. Prediction of computable sequences  

The most elementary result is that every computable sequence can be predicted by at least one predictor, and that this predictor need not be signiﬁcantly more complex than the sequence to be predicted.  

5.2.1 Lemma.    $\forall\omega\in\mathcal{C},\exists p\in\mathcal{P}(\omega):K(p)\stackrel{+}{<}K(\omega).$  

Proof. As the sequence    $\omega$   is computable, there must exist at least one algorithm that generates    $\omega$  . Let    $q$   be the shortest such algorithm and construct an algorithm    $p$   that “predicts”    $\omega$   as follows: Firstly the algorithm    $p$   reads    $x_{1:n}$  to ﬁnd the value of    $n$  , then it runs    $q$   to generate  $x_{1:n+1}$   and returns    $x_{n+1}$   as its prediction. Clearly    $p$   perfectly predicts    $\omega$   and    $\ell(p)<\ell(q)+c$  , for some small constant    $c$   that is independent of    $\omega$   and    $q$  . 2  

Not only can any computable sequence be predicted, there also exist very simple predictors able to predict arbitrarily complex sequences:  

5.2.2 Lemma.  There exists a predictor    $p$   such that    $\forall n\in\mathbb{N},\exists\,\omega\,\in\,\mathcal{C}\,:\,p\,\in

$   $\mathcal{P}(\omega)$   and    $K(\omega)>n$  .  

Proof.  $x$   such that    $K(x)=\ell(x)\geq2n$  , and from this deﬁne a sequence  ω  $\omega:=x0000\,.\,.$  . Clearly    $K(\omega)>n$   and yet a simple predictor    $p$   that always predicts 0 can learn to predict    $\omega$  . 2  

The predictor used in the above proof is very simple and can only “learn” sequences that end with all   $0$  ’s, albeit where the initial string can have ar- bitrarily high Kolmogorov complexity. It may seem that this is due to se- quences that are initially complex but where the “tail complexity”, deﬁned

  $\begin{array}{r}{\operatorname*{lim}\operatorname*{inf}_{i\to\infty}K(\omega_{i:\infty})}\end{array}$  , is zero. This is not the case:  

5.2.3 Lemma.  There exists a predictor    $p$   such that    $\forall n\in\mathbb{N},\exists\,\omega\,\in\,\mathcal{C}\,:\,p\,\in

$   $\mathcal{P}(\omega)$   and  $\begin{array}{r}{\operatorname*{lim}\operatorname*{inf}_{i\to\infty}K(\omega_{i:\infty})>n}\end{array}$  .  

Proof. A predictor    $p$   for eventually periodic sequences can be deﬁned as follows: On input    $\omega_{1:k}$   the predictor goes through the ordered pairs  $(1,1),(1,2),(2,1),(1,3),(2,2),(3,1),(1,4),.\,.$   checking for each pair   $(a,b)$  whether the string    $\omega_{1:k}$   consists of an initial string of length    $a$   followed by a repeating string of length    $b$  . On the ﬁrst match that is found    predicts that  $p$  the repeating string continues, and then    $p$   halts. If    $a+b>k$   before a match is found, then    $p$   outputs a ﬁxed symbol and halts. Clearly    $K(p)$   is a small constant and    $p$   will learn to predict any sequence that is eventually periodic.  

For any   $(m,n)\in\mathbb{N}^{2}$  , let    $\omega:=x(y^{*})$   where    $x\in\mathbb{B}^{m}$  , and    $y\in\mathbb{B}^{n}$    is a random string, that is,    $K(y)=n$  . As  $\omega$   is eventually periodic    $p\in\mathscr{P}(\omega)$   and also we see that  $\begin{array}{r}{\operatorname*{lim}\operatorname*{inf}_{i\to\infty}K(\omega_{i:\infty})=\operatorname*{min}\{K(\omega_{m+1:\infty}),K(\omega_{m+2:\infty}),\ldots,K(\omega_{m+n:\infty})\}}\end{array}$  .  

For any    $k\ \in\ \{1,.\,.\,.\,,n\}$   let    $q_{k}^{*}$    be the shortest program that can gener- ate    $\omega_{m+k:\infty}$  . We can deﬁne a halting program    $q_{k}^{\prime}$    that outputs    $y$   where this program consists of    $q_{k}^{*}$  ,    $n$   and    $k$  . Thus,    $\ell(q_{k}^{\prime})\;=\;\ell(q_{k}^{*})\,+\,O(\log n)\;=$   $K(\omega_{k:\infty})\!+\!O(\log n)$  . As  $n=K(y)\le\ell(q_{k}^{\prime})$  ), we see that    $K(\omega_{k:\infty})>n{-}O(\log n)$  . As    $n$   and  k  are arbitrary the result follows. 2  

Using a more sophisticated version of this proof it can be shown that there exist predictors that can learn to predict arbitrary regular or primitive re- cursive sequences. Thus we might wonder whether there exists a computable predictor able to learn to predict all computable sequences. Unfortunately, no universal predictor exists, indeed for every predictor there exists a sequence which it cannot predict at all:  

5.2.4 Lemma. For any predictor    $p$   there constructively exists a sequence

  $\omega:=x_{1}x_{2}\cdot.\cdot.\in\mathcal{C}$   such that    $\forall n\in\mathbb{N}:p(x_{1:n})\neq x_{n+1}$   and    $K(\omega)\stackrel{+}{<}K(p)$  .  

Proof. For any computable predictor    $p$   there constructively exists a com- putable sequence    $\omega\,=\,x_{1}x_{2}x_{3}\cdot.\,.$   computed by an algorithm    $q$   deﬁned as fol- lows: Se  $x_{1}=1-p(\lambda)$  , then    $x_{2}=1-p(x_{1})$  , then    $x_{3}=1-p(x_{1:2})$   and so on. Clearly  ω  and  $\forall n\in\mathbb{N}:p(x_{1:n})=1-x_{n+1}$  .  

Let    $p^{*}$  be the shortest program that computes the same function as    $p$   and deﬁne a sequence generation algorithm    $q^{*}$  based on    $p^{*}$  using the procedure above. By construction,    $\ell(q^{*})=\ell(p^{*})+c$   for some constant    $c$   that is indepen- dent of  $p^{*}$  . Because  $q^{*}$  generates  $\omega$  , it follows that    $K(\omega)\leq\ell(q^{*})$  . By deﬁnition  $K(p)=\ell(p^{*})$   and so    $K(\omega)\stackrel{+}{<}K(p)$  ). 2  

Allowing the predictor to be probabilistic, as we did in previous chapters, does not fundamentally avoid the problem of Lemma 5.2.4. In each step, rather than generating the opposite to what will be predicted by    $p$  , instead    $q$   attempts to generate the symbol that    $p$   is least likely to predict given    $x_{1:n}$  . To do this  $q$   must simulate    $p$   in order to estimate the probability that    $p(x_{1:n})=1$  . With suﬃcient simulation eﬀort,    $q$   can estimate this probability to any desired ac- curacy for any    $x_{1:n}$  . This produces a computable sequence    $\omega$   such that    $\forall n\in\mathbb{N}$  the probability that    $p(x_{1:n})\,=\,x_{n+1}$   is not signiﬁcantly greater than    $\frac{1}{2}$  , that is, the performance of    $p$   is no better than a predictor that makes completely random predictions. As probabilistic prediction complicates things without avoiding this problem, in this chapter we will consider only deterministic pre- dictors. This will also allow us to see the root of this problem as clearly as possible.  

With the preliminaries covered, we now move on to the central problem: predicting sequences of limited Kolmogorov complexity.  

# 5.3. Prediction of simple computable sequences  

As the computable prediction of any computable sequence is impossible, a weaker goal is to be able to predict all “simple” computable sequences.  

5.3.1 Deﬁnition. For    $n\in\mathbb{N}$  , let    $\mathcal{C}_{n}:=\{\omega\in\mathcal{C}:K(\omega)\leq n\}$  . Further, let

  $\mathcal{P}_{n}:=\mathcal{P}(\mathcal{C}_{n})$   be the set of predictors able to learn to predict all sequences in

  ${\mathcal{C}}_{n}$  .  

Firstly, we establish that prediction algorithms exist that can learn to predict all sequences up to a given complexity, and that these predictors need not be signiﬁcantly more complex than the sequences they can predict:  

5.3.2 Lemma.  $\forall n\in\mathbb{N},\exists p\in\mathcal{P}_{n}:K(p)\stackrel{+}{\leq}n+O(\log n).$  

Proof. Let    $h\in\mathbb N$   be the number of progr ms of length    $n$   or less which generate inﬁnite sequences. Build the value of  h  into a prediction algorithm    $p$  constructed as follows:  

In the  $k^{t h}$    prediction cycle run in parallel all programs of length  $n$   or less until  $h$   of these programs have each produced    $k\!+\!1$   symbols of output. Next predict according to the    $k+1^{t h}$    symbol of the generated string whose ﬁrst    $k$   symbols is consistent with the observed string. If more than one generated string is consistent with the observed sequence, pick the one which was generated by the program that occurs ﬁrst in a lexicographical ordering of the programs. If no generated output is consistent, give up and output a ﬁxed symbol.  

For suﬃciently large    $k$  , only the    $h$   programs which produce inﬁnite sequences will produce output strings of length    $k+1$  . As this set of sequences is ﬁnite, they can be uniquely identiﬁed by ﬁnite initial strings. Thus, for suﬃciently large    $k$  , the predictor    will correctly predict any computable sequence    for  $p$   $\omega$  which    $K(\omega)\leq n$  , that is,    $p\in\mathcal P_{n}$  .  

As there are   $2^{n+1}-1$   possible strings of length    $n$   or less,    $h<2^{n+1}$    and thus we can encode    $h$   with   $\log_{2}h+2\log_{2}\log_{2}h=n+1+2\log_{2}(n$   + 1) bits. Thus,  $K(p)<n+1+2\log_{2}(n+1)+c$   for some constant    $c$   that is independent of  $n$  . 2  

Can we do better than this? Lemmas 5.2.2 and 5.2.3 show us that there exist predictors able to predict at least some sequences vastly more complex than themselves. This suggests that there might exist simple predictors able to predict arbitrary sequences up to a high complexity. Formally, could there exist    $p\,\in\,\mathcal P_{n}$   where    $n\,\gg\,K(p)$  ? Unfortunately, these simple but powerful predictors are not possible:  

5.3.3 Theorem.  $\forall n\in\mathbb{N}:p\in\mathcal{P}_{n}\Rightarrow K(p)\stackrel{+}{>}n$  

Proof. For any    $n\in\mathbb{N}$   let    $p\,\in\,\mathcal P_{n}$  , that is,    $\forall\omega\,\in\,\mathcal{C}_{n}\,:\,p\,\in\,\mathcal{P}(\omega)$  . By Lemma 5.2.4 we know that    $\exists\,\omega^{\prime}\in\mathcal{C}:p\notin\mathcal{P}(\omega^{\prime})$  ∈P ) . As    $p\notin\mathcal{P}(\omega^{\prime})$  ∈P ) it must be the case that    $\omega^{\prime}\notin\mathcal{C}_{n}$  ∈C , that is,    $K(\omega^{\prime})\ge n$  . From Lemma 5.2.4 we also know that  $K(p)\stackrel{+}{>}K(\omega^{\prime})$  ) and so the result follows. 2  

Intuitively the reason for this is as follows: Lemma 5.2.4 guarantees that every simple predictor fails for at least one simple sequence. Thus, if we want a predictor that can learn to predict all sequences up to a moderate level of complexity, then clearly the predictor cannot be simple. Likewise, if we want a predictor that can predict all sequences up to a high level of complexity, then the predictor itself must be very complex. Even though we have made the generous assumption of unlimited computational resources and data to learn from, only very complex algorithms can be truly powerful predictors.  

These results easily generalise to notions of complexity that take computa- tion time into consideration. As sequences are inﬁnite, the appropriate mea- sure of time is the time needed to generate or predict the next symbol in the sequence. Under any reasonable measure of time complexity, the operation of inverting a single output from a binary valued function can be performed with little cost. If    $C$   is any complexity measure with this property, it is trivial to see that the proof of Lemma 5.2.4 still holds for    $C$  . From this, an analogue of Theorem 5.3.3 for    $C$   easily follows.  

With similar arguments these results also generalise, in a straightforward way, to complexity measures that take space or other computational resources into account. Thus, the fact that extremely powerful predictors must be very complex, holds under any measure of complexity for which inverting a single bit is inexpensive.  

# 5.4. Complexity of prediction  

Another way of viewing these results is in terms of an alternate notion of sequence complexity deﬁned as the size of the smallest predictor able to learn to predict the sequence. This allows us to express the results of the previous sections more concisely. Formally, for any sequence    $\omega$   deﬁne the complexity measure,  

$$
\dot{K}(\omega)\,:=\,\operatorname*{min}_{p\in\mathbb{B}^{*}}\,\{\,\ell(p):p\in\mathcal{P}(\omega)\,\},
$$  

and  $\dot{K}(\omega):=\infty$   ∞ if    $\mathcal{P}(\omega)=\emptyset$  . Thus, if  $\dot{K}(\omega)$  ) is high then the sequence    $\omega$   is complex in the sense that only complex prediction algorithms are able to learn to predict it. It can easily be seen that this notion of complexity has the same invariance to the choice of reference universal Turing machine as the standard Kolmogorov complexity measure.  

It may be tempting to conjecture that this deﬁnition simply describes what might be called the “tail complexity” of a sequence, that is,  $\dot{K}(\omega)$  ) is equal to  $\begin{array}{r}{\operatorname*{lim}\operatorname*{inf}_{i\to\infty}K(\omega_{i:\infty})}\end{array}$  . This is not the case. In the proof of Lemma 5.2.3 we saw that there exists a single predictor capable of learning to predict any sequence that consists of a repeating string, and thus for these sequences  $\dot{K}$   is bounded. It was further shown that there exist sequences of this form with arbitrarily high tail complexity. Clearly then tail complexity and  $\dot{K}$   cannot be equal in general.  

Using  $\dot{K}$   we can now rewrite a number of our previous results much more succinctly. From Lemma 5.2.1 it immediately follows that,  

$$
\forall\omega:\,0\,\leq\,\dot{K}(\omega)\,\stackrel{+}{<}\,K(\omega).
$$  

From Lemma 5.2.2 we k w that    $\exists c\in\mathbb{N},\forall n\in\mathbb{N},\exists\,\omega\in\mathcal{C}$   such that  $\dot{K}(\omega)<c$  and    $K(\omega)\,>\,n$  , that is,   $\dot{K}$   can attain the lower bound above within a small constant, no matter how large the value of    $K$   is. The sequences for which the upper bound on  $\dot{K}$   is tight are interesting as they are the ones which demand complex predictors. We prove the existence of these sequences and look at some of their properties in the next section.  

The complexity measure K  can also be generalised to sets of sequences, for  $S\subset\mathbb{B}^{\infty}$  deﬁne  $K(S)\mathrel{\mathop:}=\operatorname*{min}_{p}\left\{\,\ell(p)\,:\,p\,\in\,\mathcal{P}(S)\,\right\}$   {  ∈P  } . This allows us to rewrite Lemma 5.3.2 and Theorem 5.3.3 as simply,  

$$
\forall n\in\mathbb{N}\,:\,n\,\stackrel{+}{<}\,\dot{K}(\mathcal{C}_{n})\,\stackrel{+}{<}\,n+O(\log n).
$$  

This is just a restatement of the fact that the simplest predictor capable of predicting all sequences up to a Kolmogorov complexity of    $n$  , has itself a Kolmogorov complexity of roughly    $n$  .  

Perhaps the most surprising thing about  $\dot{K}$   complexity is that this very nat- ural deﬁnition of the complexity of a sequence, as viewed from the perspective of prediction, does not appear to have been studied before.  

# 5.5. Hard to predict sequences  

We have already seen that some individual sequences, such as the repeating string used in the proof of Lemma 5.2.3, can have arbitrarily high Kolmogorov complexity but nevertheless can be predicted by trivial algorithms. Thus, although these sequences contain a lot of information in the Kolmogorov sense, in a deeper sense their structure is very simple and easily learnt.  

What interests us in this section is the other extreme: individual sequences that can only be predicted by complex predictors. As we are concerned with prediction in the limit, this extra complexity in the predictor must be some kind of special information which cannot be learnt through observing the se- quence. Our ﬁrst task is to show that these hard to predict sequences exist.  

5.5.1 Theorem.  $\forall n\in\mathbb{N},\exists\,\omega\in\mathcal{C}\,:\,n\,\stackrel{+}{<}\,\dot{K}(\omega)\,\stackrel{+}{<}\,K(\omega)\,\stackrel{+}{<}\,n+O(\log n).$  

Proof. For any    $n\in\mathbb N$  , let  $Q_{n}\subset\mathbb{B}^{<n}$    be the set of programs shorter than    $n$  that are  edictors, and let  $x_{1:k}\in\mathbb{B}^{k}$   ∈   be the observed initial string rom the sequence  ω  that is to be predicted. Now construct a meta-predictor ˆ :  

By dovetailing the computations, run in parallel every program of length less than    $n$   on every string in    $\mathbb{B}^{\leq k}$  . Each time a program is found to halt on all of these input strings, add the program to a set of “candidate prediction algorithms”, called  ${\boldsymbol{\bar{Q}}}_{n}^{k}$  . As each element of    $Q_{n}$    is a valid predictor, and thus n halts for all input strings in    ${\mathbb{B}}^{*}$  by deﬁnition, for every    $n$   and    $k$   it eventually will be the case that    $|\tilde{Q}_{n}^{k}|=|Q_{n}|$  |  | | . At this point the simulatio to approximate  $Q_{n}$   terminates. It is clear that for suﬃciently large values of  k  all of the valid predictors, and only the valid predictors, will halt with a single symbol of output on all tested input strings. That is,  $\exists r\in\mathbb{N},\forall k>r:\tilde{Q}_{n}^{k}=Q_{n}$  .  

The second part of the   $\hat{p}$   algorithm uses these candidate prediction algo- rithms to make a prediction. For    $p\in\tilde{Q}_{n}^{k}$     $\begin{array}{r}{d^{k}(p):=\sum_{i=1}^{k-1}|p(x_{1:i})-x_{i+1}|}\end{array}$    − | . Informally,    $d^{k}(p)$   is the number of prediction errors made by  p  so far. Com- pute this for all    $p\in{\tilde{Q}}_{n}^{k}$    and then let  $p_{k}^{*}\in\tilde{Q}_{n}^{k}$    ∈   be the program with minimal n  $d^{k}(p)$  . If there is more than one such program, break the tie by letting    $p_{k}^{*}$    be the le xico graphically ﬁrst of these. Finally,  $\hat{p}$   computes the value of    $p_{k}^{*}(x_{1:k})$  and then returns this as its prediction and halts.  

By Lemma 5.2.4, there exists    $\omega^{\prime}\in\mathcal{C}$   such that  $\hat{p}$   makes a prediction error for every    $k$   when trying to predict  ω . Thus, in each cycle at least one of the ﬁnitely many predictors with minimal    $d^{k}$    makes a prediction error and so    $\forall p\,\in\,Q_{n}\,:\,d^{k}(p)\,\rightarrow\,\infty$  as    $k\,\rightarrow\,\infty$  . Therefore,    $\nexists p\,\in\,Q_{n}\,:\,p\,\in\,\mathcal{P}(\omega^{\prime})$  , that is, no program of length less than    $n$   can learn to predict  ω ′   and so    $n\leq$   $\dot{K}(\omega^{\prime})$  ). Further, from Lemma 5.2.1 we know that  $\dot{K}(\omega^{\prime})\stackrel{+}{<}K(\omega^{\prime})$  ), and from Lemma 5.2.4 again,    $K(\omega^{\prime})\stackrel{+}{<}K(\hat{p})$  ).  

Examining the algorithm for   $\hat{p}$  , we see that it contains some ﬁxed length program code and an encoding of    $|Q_{n}|$  , where    $\left|Q_{n}\right|<2^{n}-1$  . Thus, using a standard encoding method for integers,    $K({\hat{p}})\stackrel{+}{<}n+O(\log n)$  ). Chaining these,  $n\stackrel{+}{<}\dot{K}(\omega^{\prime})\stackrel{+}{<}K(\omega^{\prime})\stackrel{+}{<}K(\hat{p})\stackrel{+}{<}n+O(\log n)$  ), which proves the theorem. 2  

This establishes the existence of sequences with arbitrarily high  $\dot{K}$   com- plexity which also have a similar level of Kolmogorov complexity. Next we establish a fundamental property of high  $\dot{K}$   complexity sequences: they are extremely diﬃcult to compute.  

For an algorithm    that generates    $\omega\in\mathcal C$  ,  ne    $t_{q}(n)$   to be the number of  $q$  computation steps performed by    $q$   before the  n  $n^{t h}$    symbol of    $\omega$   is written to the output tape. For example, if    $q$   is a simple algorithm that outputs the sequence 010101  . . . , then clearly    $t_{q}(n)=O(n)$   and so    $\omega$   can be computed quickly. The following theorem proves that if a sequence can be computed in a reasonable amount of time, then the sequence must have a low  $\dot{K}$   complexity:  

5.5.2 Lemma.  $\mathbf{Lemma.}\,\,\,\forall\omega\in\mathcal{C},\,\,i f\,\exists q:\mathcal{U}(q)=\omega\,\,\,a n d\,\,\exists r\in\mathbb{N},\forall n>r:t_{q}(n)<2^{n},$  then  $\overset{\,_{\mathrm{~\large~.~}}}{K}(\omega)\overset{+}{=}0$  .  

Proof. Construct a prediction algorithm  p  as follows:  

On input    $x_{1:n}$  , run all programs of length    $n$   or less, each for   $2^{n+1}$    steps. In a set    $W_{n}$   collect together all generated strings which are at least    $n+1$   symbols long and where the ﬁrst  $n$   symbols match the observed string  $x_{1:n}$  . Now order the strings in    $W_{n}$   according to a lexicographical ordering of their generating programs. If    $W_{n}=\emptyset$  then just return a prediction of 1 and halt. If    $|W_{n}|>1$  then return the  $n+1^{t h}$    symbol from the ﬁrst sequence in the above ordering.  

Assume that    $\exists q:\mathcal{U}(q)=\omega$   such that    $\exists r\in\mathbb{N},\forall n>r:t_{q}(n)<2^{n}$   $q$  not unique, take    $q$   to be the le xico graphically ﬁrst of these. Clearly  ∀ n > r the initial string from    $\omega$   generated by    $q$   will be in the set    $W_{n}$  . As there is no le xico graphically lower program which can generate    $\omega$   within the time constraint    $t_{q}(n)<2^{n}$    for all  $n>r$  , for suﬃciently large    $n$   the predictor  p  must converge on using    $q$   for each prediction and thus   $\tilde{p}\in\mathcal{P}(\omega)$   ∈P ). As    $\ell(\tilde{p})$  ) is clearly a ﬁxed constant that is independent of    $\omega$  , it follows then that  $\begin{array}{r}{\dot{K}(\omega)<\ell(\tilde{p})\stackrel{+}{=}}\end{array}$   $0$  . 2  

We could replace the   $2^{n}$    bound in the above result with any monotonically growing computable function, for example,   $2^{2^{n}}$  . In any case, this does not change the fundamental result that sequences which have a high K  complexity are practically impossible to compute. However, from our theoretical perspec- tive, these sequences present no problem as they can be predicted, albeit with immense diﬃculty.  

# 5.6. The limits of mathematical analysis  

One way to interpret the results of the previous sections is in terms of con- structive theories of prediction. Essentially, a constructive theory of prediction expressed in some suﬃciently rich formal system    $\mathfrak{F}$  , is in eﬀect a description of a prediction algorithm with respect to a universal Turing machine which implements the required parts of    $\mathfrak{F}$  . Thus, from Theorems 5.3.3 and 5.5.1, it follows that if we want to have a predictor that can learn to predict all sequences up to a high level of Kolmogorov complexity, or even just predict individual sequences which have high  $\dot{K}$   complexity, the constructive theory of prediction that we base our predictor on must be very complex. Elegant and highly general constructive theories of prediction simply do not exist, even if we assume unlimited computational resources. This is in marked contrast to Solomonoﬀ’s highly elegant but non-constructive theory of prediction.  

Naturally, highly complex theories of prediction will be very diﬃcult to mathematically analyse, if not practically impossible. Thus, at some point the development of very general prediction algorithms must become mainly an experimental endeavour due to the diﬃculty of working with the required theory. Interestingly, an even stronger result can be proven showing that beyond some point the mathematical analysis is in fact impossible, even in theory:  

5.6.1 Theorem.  In any consistent formal axiomatic system    $\mathfrak{F}$   that is suﬃ- ciently rich to express statements of the form   ${\mathrm{\'exp}}\,\in\,{\mathcal{P}}_{n}\,{\mathrm{\'}}$  , there exists    $m\in\mathbb{N}$  such that for all    $n\;>\;m$   and for all predictors    $p\ \in\ \mathcal P_{n}$   the true statement “  $\boldsymbol{\mathcal{P}}\in\mathcal{P}_{n}$  ” cannot be proven in    $\mathfrak{F}$  .  

In other words, even though we have proven that very powerful sequence prediction algorithms exist, beyond a certain complexity it is impossible to ﬁnd any of these algorithms using mathematics. The proof has a similar structure to Chaitin’s information theoretic proof (Chaitin, 1982) of G¨ odel’s incompleteness theorem for formal axiomatic systems (G¨ odel, 1931).  

Proof. For each    $n\in\mathbb{N}$   let    $T_{n}$   be the set of statements expressed in the formal system    $\mathfrak{F}$   of the form “  $\cdot_{p}\in\mathcal{P}_{n}{}^{\mathcal{\gamma}}$  , where    $p$   is ﬁlled in with the complete description of some algorithm in each case. As the set of programs is denumer- able,    $T_{n}$   is also denumerable and each element of    $T_{n}$   has ﬁnite length. From Lemma 5.3.2 and Theorem 5.3.3 it follows that each    $T_{n}$   contains inﬁnitely many statements of the form   ${\mathfrak{L}}_{p}\in{\mathcal{P}}_{n}{\mathrm{,}}$   which are true.  

Fix    $n$   and create a search algorithm    $s$   that enumerates all proofs in the formal system    $\mathfrak{F}$   searching for a proof of a statement in the set    $T_{n}$  . As the set  $T_{n}$   is recursive,    $s$   can always recognise a proof of a statement in    $T_{n}$  . If    $s$   ﬁnds any such proof, it outputs the corresponding program    $p$   and then halts.  

By way of contradiction, assume that    $s$   halts, that is, a proof of a theorem in    $T_{n}$   is found and    $p$   such that    $p\,\in\,\mathcal P_{n}$   is generated as output. The size of the algorithm    $s$   is a constant (a description of the formal system    $\mathfrak{F}$   and some proof enumeration code) as well as an    $O(\log n)$   term needed to describe    $n$  . It follows then that    $K(p)\,\stackrel{+}{<}\,O(\log n)$  ). However, from Theorem 5.3.3 we know that    $K(p)\ {\stackrel{+}{>}}\ n$  . Thus, for suﬃciently large    $n$  , we have a contradiction and so our assumption of the existence of a proof must be false. That is, for suﬃciently large    $n$   and for all    $p\in\mathcal P_{n}$  , the true statement   ${\mathfrak{L}}_{p}\in{\mathcal{P}}_{n}{\mathrm{~,~}}$   cannot be proven within the formal system    $\mathfrak{F}$  . 2  

The exact value of    $_m$   depends on our choice of formal system    $\mathfrak{F}$   and which reference machine    $\mathcal{U}$   we measure complexity with respect to. However, for reasonable choices of    $\mathfrak{F}$   and    $\mathcal{U}$   the value of    $_m$   would be in the order of 1000. That is, the bound    $_m$   is certainly not so large as to be vacuous.  

# 5.7. Conclusion  

We have shown that there does not exist an elegant constructive theory of prediction for computable sequences, even if we assume unbounded compu- tational resources, unbounded data and learning time, and place moderate bounds on the Kolmogorov complexity of the sequences to be predicted. Very powerful computable predictors are therefore necessarily complex. We have further shown that the source of this problem is the existence of computable sequences which are extremely expensive to compute. While we have proven that very powerful prediction algorithms which can learn to predict these se- quences exist, we have also proven that, unfortunately, mathematical analysis cannot be used to discover these algorithms due to G¨ odel incompleteness.  

These results can be extended to more general settings, speciﬁcally to those problems which are equivalent to, or depend on, sequence prediction. Consider, for example, a reinforcement learning agent interacting with an environment, as described in Chapters 2 and 3. In each interaction cycle the agent must choose its actions so as to maximise the future rewards that it receives from the environment. Of course the agent cannot know for certain if some action will lead to rewards in the future. Whether explicitly or implicitly, it must somehow predict these. Thus, at the heart of reinforcement learning lies a prediction problem, and so the results for computable predictors presented in this paper also apply to computable reinforcement learners. More speciﬁcally, from Theorem 5.3.3 it follows that very powerful computable reinforcement learners are necessarily complex, and from Theorem 5.6.1 it follows that it is impossible to discover any of these extremely powerful reinforcement learning algorithms mathematically. These relationships are illustrated in Figure 5.1.  

It is reasonable to ask whether the assumptions we have made in our model need to be changed. If we increase the power of the predictors further, for example by providing them with some kind of an oracle, this would make the predictors even more unrealistic than they currently are. This goes against our goal of ﬁnding an elegant, powerful and general prediction theory that is more realistic in its assumptions than Solomonoﬀ’s incomputable model. On the other hand, if we weaken our assumptions about the predictors’ resources to make them more realistic, we are in eﬀect taking a subset of our current class of predictors. As such, all the same limitations and problems will still apply, as well as some new ones.  

It seems then that the way forward is to further restrict the problem space. One possibility would be to bound the amount of computation time needed to generate the next symbol in the sequence. However, if we do this without re- stricting the predictors’ resources then the simple predictor from Lemma 5.5.2 easily learns to predict any such sequence and thus the problem of prediction in the limit has become trivial. Another possibility might be to bound the memory of the machine used to generate the sequence, however this makes the generator a ﬁnite state machine and thus bounds its computation time, again making the problem trivial.  

Perhaps the only reasonable solution would be to add additional restrictions to both the algorithms which generate the sequences to be predicted, and to the predictors. We may also want to consider not just learnability in the limit, but also how quickly the predictor is able to learn. Of course we are then facing a much more diﬃcult analysis problem.  

![](images/381836f63453c8b324ada7f8ad4c27f7d7ae94b3b89b9460c8a27f4f00479730.jpg)  

Figure 5.1.: Theorem 5.3.3 rules out simple but powerful artiﬁcial intelligence algorithms, as indicated by the greyed out region in the upper left. Theorem 5.6.1 upper bounds how powerful an algorithm can be before it can no longer be proven to be a powerful algorithm. This is indicated by the horizontal line separating the region of provable algorithms from the region of G¨ odel incompleteness.  

# 6. Temporal Diﬀerence Updating without a Learning Rate  

In Chapters 2 and 3 we saw how universal agents are able to learn to behave optimally across a wide range of environments. Unfortunately, these agents are incomputable as they are based on incomputable universal prior distribu- tions. Thus, in order to use the theory of universal artiﬁcial intelligence to design and build practical algorithms, we must ﬁrst ﬁnd a way to scale the theory down. In Chapter 5 we investigated some of the constraints faced when attempting this. What we uncovered was a number of fundamental negative results. In short, computable predictors capable of predicting all sequences up to a moderate Kolmogorov complexity are both highly complex and mathe- matically impossible to ﬁnd due to G¨ odel incompleteness. The only way out of this bind, it seems, is to move to a more sophisticated measure of complex- ity that takes not only information content into account, but also time and space. Unfortunately, the theory of resource bounded complexity is notori- ously diﬃcult to work with and has many unsolved fundamental questions. Furthermore, even if the universal prior distribution could be replaced by a suitable computable prior, perhaps something like the  Speed prior  (Schmid- huber, 2002), there still remains the fact that  AIXI ’s search through possible futures requires computation time that is exponential in the depth of the look ahead (see the equation for  AIXI  in Section 2.10).  

Despite these diﬃculties, several attempts at scaling  AIXI  down have been made. The most theoretically founded of these is  AIXI tl  (Chapter 7 of Hutter, 2005). In this model, proof search is used to limit the size and computation time of the algorithm. Unfortunately, although technically computable, the resulting agent still requires impossibly vast computational resources. Another more drastic scaling down of  AIXI  did produce a usable algorithm (Poland and Hutter, 2006). Here the problem domain was limited to games that could be described by   $2\times2$   matrices, and the look ahead was bounded to 8 interaction cycles. The resulting algorithm was able to learn simple game theoretic inter- action strategies. While this proves that some kind of scaling down of  AIXI  is possible, the problem space of   $2\times2$   matrix games falls well short of what is needed for a useful artiﬁcial intelligence algorithm.  

In this chapter we present what started out as another attempt to scale AIXI  down. As we have seen in previous chapters, at the core of the rein- forcement learning problem lies the problem of estimating the expected future discounted reward. We begin by expressing this estimation problem as a loss function, more speciﬁcally, as the squared diﬀerence between the empirical future discounted reward and our estimate. We then derive an equation for this estimator by minimising the loss function. Although the resulting learning equations no longer bear much resemblance to  AIXI , they do strongly resemble the standard equation for  temporal diﬀerence learning with eligibility traces , also known as the   $\operatorname{TD}(\lambda)$   algorithm. Interestingly, while the standard algo- rithm has a free learning rate parameter, in our new equation there is none. In its place there is an equation that automatically sets the learning rate in a way that is speciﬁc to each state transition. We have experimentally tested this new learning rule against   $\operatorname{TD}(\lambda)$   and found that it oﬀers superior performance in various settings. We have also extended the algorithm to reinforcement learn- ing and again found encouraging results. This chapter covers the derivation of this algorithm and our experimental results.  

Note that while the notation used in this chapter is fairly standard for the temporal diﬀerence learning literature, it is a little diﬀerent to what we used to deﬁne  AIXI . For example, we now talk of  states  rather than observations, and index the value function in a new way.  

# 6.1. Temporal diﬀerence learning  

In the ﬁeld of reinforcement learning, perhaps the most popular way to esti- mate the future discounted reward of states is the method of  temporal diﬀer- ence learning . It is unclear who exactly introduced this ﬁrst, however the ﬁrst explicit version of temporal diﬀerence as a learning rule appears to be Witten (1977). The idea is as follows: The  expected future discounted reward  of a state  $s$   is,  

$$
\overline{{{V}}}_{s}:=\mathbf{E}\left\{r_{k}+\gamma r_{k+1}+\gamma^{2}r_{k+2}+\cdot\cdot\cdot|s_{k}=s\right\},
$$  

where the rewards    $r_{k},r_{k+1},\ldots$   are geometrically discounted into the future by  $\gamma<1$  . From this deﬁnition it follows that,  

$$
\overline{{{V}}}_{s}=\mathbf{E}\left\{r_{k}+\gamma\overline{{{V}}}_{s_{k+1}}|s_{k}=s\right\}.
$$  

Our task, at time    $t$  , is to compute an estimate  $V_{s}^{t}$  t   of    $\overline{{V}}_{s}$    for each state    $s$  . The only information we have to base this estimate on is the current history of state transitions,    $s_{1},s_{2},.\cdot\cdot,s_{t}$  , and the current history of observed rewards,  $r_{1},r_{2},.\,.\,.\,,r_{t}$  . Equation (6.1) suggests that at time    $t\!+\!1$   the value of    $r_{t}\!+\!\gamma V_{s_{t+1}}$  provides us with information on what    $V_{s}^{t}$    should be: if it is higher than    $V_{s_{t}}^{t}$  t then perhaps this estimate should be increased, and vice versa. This intuition gives us the following estimation heuristic for state    $s_{t}$  ,  

$$
V_{s_{t}}^{t+1}:=V_{s_{t}}^{t}+\alpha\left(r_{t}+\gamma V_{s_{t+1}}^{t}-V_{s_{t}}^{t}\right),
$$  

where    $\alpha$   is a parameter that controls the rate of learning. This type of temporal diﬀerence learning is known as TD(0).  

One shortcoming of this method is that at each time step the value of only the last state    $s_{t}$   is updated. States before the last state are also aﬀected by  

Initialise    $V(s)$   arbitrarily and  $E(s)=0$   for all   Initialise    $s$  repeat Make state transition and observe    $r$  ,    $s^{\prime}$   $\Delta\leftarrow r+\gamma V(s^{\prime})-V(s)$   $E(s)\gets E(s)+1$  for all    $s$   do  $\begin{array}{l}{{V(s)\leftarrow V(s)+\alpha E(s)\Delta}}\\ {{E(s)\leftarrow\gamma\lambda E(s)}}\end{array}$  end for s  ← s ′ until  end of run  

changes in the last state’s value and thus these could be updated too. This is what happens with so called  temporal diﬀerence learning with eligibility traces , where a history, or trace, is kept of which states have been recently visited. Under this method, when we update the value of a state we also go back through the trace updating the earlier states as well. Formally, for any state  $s$   its eligibility trace is computed by,  

$$
\begin{array}{r}{E_{s}^{t}:=\left\{\begin{array}{l l}{\gamma\lambda E_{s}^{t-1}}&{\mathrm{~if~}s\neq s_{t},}\\ {\gamma\lambda E_{s}^{t-1}+1}&{\mathrm{~if~}s=s_{t},}\end{array}\right.}\end{array}
$$  

where    $\lambda$   is used to control the rate at which the eligibility trace is discounted. The temporal diﬀerence update is then, for  all  states    $s$  ,  

$$
V_{s}^{t+1}:=V_{s}^{t}+\alpha E_{s}^{t}\left(\boldsymbol{r}+\gamma V_{s_{t+1}}^{t}-V_{s_{t}}^{t}\right).
$$  

This more powerful version of temporal diﬀerent learning is known as TD  $(\lambda)

$  (Sutton, 1988). The complete algorithm appears in Algorithm 1.  

Although it has been successfully applied to many simple problems, plain

  $\operatorname{TD}(\lambda)$   has a number of drawbacks. One of these is that the learning rate parameter    $\alpha$   has to be experimentally tuned by hand. Indeed, even this is not always enough, for optimal performance some monotonically decreasing function has to be experimentally found that decreases the learning rate over time at the right rate. If the learning rate decreases too quickly, the system may become stuck at a level of sub-optimal performance, if it decreases too slowly then convergence will be unnecessarily late. The main contribution of this chapter is to solve this problem by deriving a temporal diﬀerence rule from statistical principles that automatically sets its learning rate.  

Perhaps the closest work to ours is the LSTD(  $\lambda$  ) algorithm (Bradtke and Barto, 1996; Boyan, 1999; Lagoudakis and Parr, 2003). LSTD(  $\lambda$  ) is concerned with ﬁnding a least-squares linear function approximation to the true value function. The unknown expected rewards and transition probabilities are re- placed by empirical averages up to current time    $t$  . In contrast, we consider ﬁnite state spaces and no function approximation. We derive a least-squares estimate of the empirical values including future rewards by bootstrapping. The computation time for our update is linear in the number of states, like  $\operatorname{TD}(\lambda)$  , while LSTD is quadratic (even in the case of state-indicator features and no function approximation). Indeed our algorithm exactly coincides with TD/Q/Sarsa( λ ) but with a novel learning rate derived from statistical princi- ples. LSTD has not yet been developed for general    $\lambda$   and   . Our algorithm and  $\gamma$  LSTD both get rid of the learning rate and the necessity to initialise    $V$   . Since LSTD has primarily been developed for linear function approximation and has a much more expensive update rule, we focused our experimental comparison to the algorithms for which we determined the learning rate (ﬁnite state space, linear time TD/Q/Sarsa algorithms). It remains to be seen how our approach generalises to (linear) function approximation.  

# 6.2. Derivation  

The  empirical future discounted reward  of a state    $s_{k}$   is the sum of  actual rewards  following from state    $s_{k}$   in time steps    $k,k+1,\ldots$  , where the rewards are discounted as they go into the future. Formally, the empirical value of state    at time    $k$   for    $k=1,...,t$   is,  $s_{k}$  

$$
v_{k}:=\sum_{u=k}^{\infty}\gamma^{u-k}r_{u},
$$  

where the future rewards  $r_{u}$   are geometrically discounted by    $\gamma<1$  . In practice the exact value of    $v_{k}$   is always unknown to us as it depends not only on rewards that have been already observed, but also on unknown future rewards. Note that if    $s_{m}\,=\,s_{n}$   for    $m\neq n$  , that is, we have visited the same state twice at diﬀerent times    $m$   and    $n$  , this does not imply that    $v_{n}\,=\,v_{m}$   as the observed rewards following the state visit may be diﬀerent each time.  

Our goal is that for each state  $s$   the estimate    $V_{s}^{t}$    should be as close as possible to the true expected future discounted reward    $\overline{{V}}_{s}$  . Thus, for each state    $s$   we would like    $V_{s}$   to be close to    for all    $k$   such that   . Furthermore, in  $v_{k}$   $s\,=\,s_{k}$  non-stationary environments we would like to discount old evidence by some parameter    $\lambda\in(0,1]$  . Formally, we want to minimise the loss function,  

$$
L:=\frac{1}{2}\sum_{k=1}^{t}\lambda^{t-k}\big(v_{k}-V_{s_{k}}^{t}\big)^{2}.
$$  

For stationary environments we may simply set    $\lambda=1$   a priori.  

As we wish to minimise this loss, we take the partial derivative with respect to the value estimate of each state and set to zero,  

$$
\frac{\partial L}{\partial V_{s}^{t}}=-\sum_{k=1}^{t}\lambda^{t-k}\big(v_{k}-V_{s_{k}}^{t}\big)\delta_{s_{k}s}=V_{s}^{t}\sum_{k=1}^{t}\lambda^{t-k}\delta_{s_{k}s}-\sum_{k=1}^{t}\lambda^{t-k}\delta_{s_{k}s}v_{k}=0,
$$  

where we could change    $V_{s_{k}}^{t}$    into    $V_{s}^{t}$    due to the presence of the Kronecker    $\delta_{s_{k}s}$  , k deﬁned    $\delta_{x y}:=1$   if   , and   $0$   otherwise. By deﬁning a discounted state visit  $x=y$  counter    $\begin{array}{r}{N_{s}^{t}:=\sum_{k=1}^{t}\lambda^{t-k}\delta_{s_{k}s}}\end{array}$   we get  

$$
V_{s}^{t}N_{s}^{t}=\sum_{k=1}^{t}\lambda^{t-k}\delta_{s_{k}s}v_{k}.
$$  

Since    $v_{k}$   depends on future rewards    $r_{k}$  , Equation (6.5) can not be used in its current form. Next we note that    $v_{k}$   has a self-consistency property with respect to the rewards. Speciﬁcally, the tail of the future discounted reward sum for each state depends on the empirical value at time    $t$   in the following way,  

$$
v_{k}=\sum_{u=k}^{t-1}\gamma^{u-k}r_{u}+\gamma^{t-k}v_{t}.
$$  

Substituting this into Equation (6.5) and exchanging the order of the double sum,  

$$
\begin{array}{r c l}{{V_{s}^{t}N_{s}^{t}}}&{{=}}&{{\displaystyle\sum_{u=1}^{t-1}\sum_{k=1}^{u}\lambda^{t-k}\delta_{s_{k}s}\gamma^{u-k}r_{u}+\sum_{k=1}^{t}\lambda^{t-k}\delta_{s_{k}s}\gamma^{t-k}v_{t}}}\\ {{}}&{{=}}&{{\displaystyle\sum_{u=1}^{t-1}\lambda^{t-u}\sum_{k=1}^{u}(\lambda\gamma)^{u-k}\delta_{s_{k}s}r_{u}+\sum_{k=1}^{t}(\lambda\gamma)^{t-k}\delta_{s_{k}s}v_{t}}}\\ {{}}&{{=}}&{{R_{s}^{t}+E_{s}^{t}v_{t},}}\end{array}
$$  

where    $\begin{array}{r}{E_{s}^{t}\,:=\,\sum_{k=1}^{t}(\lambda\gamma)^{t-k}\delta_{s_{k}s}}\end{array}$   is the eligibility trace of state    $s$  , and    $R_{s}^{t}:=$   $\textstyle\sum_{u=1}^{t-1}\lambda^{t-u}E_{s}^{u}r_{u}$    is the discounted reward with eligibility.  

P  $E_{s}^{t}$    and  $R_{s}^{t}$    depend only on quantities known at time    $t$  . The only unknown s quantity is    $v_{t}$  , which we have to replace with our current estimate of this value at time    $t$  , which is    $V_{s_{t}}^{t}$  . In other words, we bootstrap our estimates. This gives us,  

$$
V_{s}^{t}N_{s}^{t}=R_{s}^{t}+E_{s}^{t}V_{s_{t}}^{t}.
$$  

For state    $s=s_{t}$  , this simpliﬁes to  

$$
V_{s_{t}}^{t}=\frac{R_{s_{t}}^{t}}{N_{s_{t}}^{t}-E_{s_{t}}^{t}}.
$$  

Substituting this back into Equation (6.6) we obtain,  

$$
V_{s}^{t}N_{s}^{t}=R_{s}^{t}+E_{s}^{t}\frac{R_{s_{t}}^{t}}{N_{s_{t}}^{t}-E_{s_{t}}^{t}}.
$$  

This gives us an explicit expression for our    $V$   estimates. However, from an algorithmic perspective an incremental update rule is more convenient. To derive this we make use of the relations,  

$$
\begin{array}{r l r l}{N_{s}^{t+1}=}&{{}\lambda N_{s}^{t}+\delta_{s_{t+1}s},}&{\quad}&{{}N_{s}^{0}=0,}\\ {E_{s}^{t+1}=}&{{}\lambda\gamma E_{s}^{t}+\delta_{s_{t+1}s},}&{\quad}&{{}E_{s}^{0}=0,}\\ {R_{s}^{t+1}=}&{{}\lambda R_{s}^{t}+\lambda E_{s}^{t}r_{t},}&{\quad}&{{}R_{s}^{0}=0,}\end{array}
$$  

Inserting these into Equation (6.7) with    $t$   replaced by    $t+1$  ,  

$$
\begin{array}{r c l}{V_{s}^{t+1}N_{s}^{t+1}}&{=}&{R_{s}^{t+1}+E_{s}^{t+1}\displaystyle\frac{R_{s_{t+1}}^{t+1}}{N_{s_{t+1}}^{t+1}-E_{s_{t+1}}^{t+1}}}\\ &{=}&{\lambda R_{s}^{t}+\lambda E_{s}^{t}r_{t}+E_{s}^{t+1}\displaystyle\frac{R_{s_{t+1}}^{t}+E_{s_{t+1}}^{t}r_{t}}{N_{s_{t+1}}^{t}-\gamma E_{s_{t+1}}^{t}}.}\end{array}
$$  

By solving Equation (6.6) for    $R_{s}^{t}$    and substituting back in,  

$$
\begin{array}{c}{{_{s}^{\prime t+1}N_{s}^{t+1}=\lambda\big(V_{s}^{t}N_{s}^{t}-E_{s}^{t}V_{s_{t}}^{t}\big)+\lambda E_{s}^{t}r_{t}+E_{s}^{t+1}\displaystyle\frac{N_{s_{t+1}}^{t}V_{s_{t+1}}^{t}-E_{s_{t+1}}^{t}V_{s_{t}}^{t}+E_{s_{t+1}}^{t}r_{t}}{N_{s_{t+1}}^{t}-\gamma E_{s_{t+1}}^{t}}}}\\ {{=\displaystyle\big(\lambda N_{s}^{t}+\delta_{s_{t+1}s}\big)V_{s}^{t}-\delta_{s_{t+1}s}V_{s}^{t}-\lambda E_{s}^{t}V_{s_{t}}^{t}+\lambda E_{s}^{t}r_{t}}}\\ {{{}~~~~~~~~~~~~~~~~~~~~~+\displaystyle E_{s}^{t+1}\displaystyle\frac{N_{s_{t+1}}^{t}V_{s_{t+1}}^{t}-E_{s_{t+1}}^{t}V_{s_{t}}^{t}+E_{s_{t+1}}^{t}r_{t}}{N_{s_{t+1}}^{t}-\gamma E_{s_{t+1}}^{t}}.}}\end{array}
$$  

Dividing through by    $N_{s}^{t+1}(=\lambda N_{s}^{t}+\delta_{s_{t+1}s})$  ),  

$$
\begin{array}{r l}&{V_{s}^{t+1}=V_{s}^{t}+\frac{-\delta_{s_{t+1}s}V_{s}^{t}-\lambda E_{s}^{t}V_{s_{t}}^{t}+\lambda E_{s}^{t}r_{t}}{\lambda N_{s}^{t}+\delta_{s_{t+1}s}}}\\ &{\quad\quad\quad+\frac{\left(\lambda\gamma E_{s}^{t}+\delta_{s_{t+1}s}\right)\left(N_{s_{t+1}}^{t}V_{s_{t+1}}^{t}-E_{s_{t+1}}^{t}V_{s_{t}}^{t}+E_{s_{t+1}}^{t}r_{t}\right)}{\left(N_{s_{t+1}}^{t}-\gamma E_{s_{t+1}}^{t}\right)\left(\lambda N_{s}^{t}+\delta_{s_{t+1}s}\right)}.}\end{array}
$$  

Making the ﬁrst denominator the same as the second, then expanding the numerator,  

$$
V_{s}^{t+1}=V_{s}^{t}+\frac{\lambda E_{s}^{t}r_{t}N_{s_{t+1}}^{t}-\lambda E_{s}^{t}V_{s_{t}}^{t}N_{s_{t+1}}^{t}-\delta_{s_{t+1}s}V_{s}^{t}N_{s_{t+1}}^{t}-\lambda\gamma E_{s_{t+1}}^{t}E_{s}^{t}r_{t}}{(N_{s_{t+1}}^{t}-\gamma E_{s_{t+1}}^{t})(\lambda N_{s}^{t}+\delta_{s_{t+1}s})}
$$  

$$
+\frac{\lambda\gamma E_{s_{t+1}}^{t}E_{s}^{t}V_{s_{t}}^{t}+\gamma E_{s_{t+1}}^{t}V_{s}^{t}\delta_{s_{t+1}s}+\lambda\gamma E_{s}^{t}N_{s_{t+1}}^{t}V_{s_{t+1}}^{t}-\lambda\gamma E_{s}^{t}E_{s_{t+1}}^{t}V_{s_{t}}^{t}}{(N_{s_{t+1}}^{t}-\gamma E_{s_{t+1}}^{t})(\lambda N_{s}^{t}+\delta_{s_{t+1}s})}
$$  

$$
+\frac{\lambda\gamma E_{s}^{t}E_{s_{t+1}}^{t}r_{t}+\delta_{s_{t+1}s}N_{s_{t+1}}^{t}V_{s_{t+1}}^{t}-\delta_{s_{t+1}s}E_{s_{t+1}}^{t}V_{s_{t}}^{t}+\delta_{s_{t+1}s}E_{s_{t+1}}^{t}r_{t}}{\big(N_{s_{t+1}}^{t}-\gamma E_{s_{t+1}}^{t}\big)\big(\lambda N_{s}^{t}+\delta_{s_{t+1}s}\big)}.
$$  

After cancelling equal terms (keeping in mind that in every term with a Kro- necker    $\delta_{x y}$   factor we may assume that    $x=y$   as the term is always zero other- wise), and factoring out    $E_{s}^{t}$    the right hand side becomes,  

$$
V_{s}^{t}+\frac{E_{s}^{t}\big(\lambda r_{t}N_{s_{t+1}}^{t}\!-\!\lambda V_{s_{t}}^{t}N_{s_{t+1}}^{t}\!+\gamma V_{s}^{t}\delta_{s_{t+1}s}\!+\!\lambda\gamma N_{s_{t+1}}^{t}V_{s_{t+1}}^{t}\!-\!\delta_{s_{t+1}s}V_{s_{t}}^{t}\!+\!\delta_{s_{t+1}s}r_{t}\big)}{\big(N_{s_{t+1}}^{t}-\gamma E_{s_{t+1}}^{t}\big)\big(\lambda N_{s}^{t}+\delta_{s_{t+1}s}\big)}
$$  

Finally, by factoring out    $\lambda N_{s_{t+1}}^{t}+\delta_{s_{t+1}s}$    we obtain our update rule,  

$$
V_{s}^{t+1}\,=\,V_{s}^{t}\,+\,E_{s}^{t}\,\beta_{t}(s,s_{t+1})\,\bigl(r_{t}+\gamma V_{s_{t+1}}^{t}-V_{s_{t}}^{t}\bigr),
$$  

where the learning rate is given by,  

$$
\beta_{t}(s,s_{t+1})\;:=\;\frac{1}{N_{s_{t+1}}^{t}-\gamma E_{s_{t+1}}^{t}}\;\;\frac{N_{s_{t+1}}^{t}}{N_{s}^{t}}.
$$  

Examining Equation (6.8), we ﬁnd the usual update equation for temporal diﬀerence learning with eligibility traces (see Equation (6.2)), however the learning rate    $\alpha$   has now been replaced by    $\beta_{t}(s,s_{t+1})$  . This learning rate was derived by minimising the squared loss between the estimated and true state value. In the derivation we have exploited the fact that the latter must be self-consistent and then bootstrapped to get Equation (6.6). This gives us an equation for the learning rate for each state transition at time    $t$  , as opposed to the standard temporal diﬀerence learning where the learning rate    $\alpha$   is either a ﬁxed free parameter for all transitions, or is decreased over time by some monotonically decreasing function. In either case, the learning rate is not automatic and must be experimentally tuned for good performance. The above derivation appears to theoretically solve this problem.  

The ﬁrst term in    $\beta_{t}$   seems to provide some type of normalisation to the learning rate, though the intuition behind this is not clear to us. The meaning of second term however can be understood as follows:    $N_{s}^{t}$    measures how often we have visited state    $s$   in the recent past. Therefore, if    $N_{s}^{t}\,\ll\,N_{s_{t+1}}^{t}$      then state    $s$   has a value estimate based on relatively few samples, while state    $s_{t+1}$  has a value estimate based on relatively many samples. In such a situation, the second term in    $\beta_{t}$   boosts the learning rate so that    $V_{s}^{t+1}$  moves more ag- gressively towards the presumably more accurate  $r_{t}+\gamma V_{s_{t+1}}^{t}$  . In the opposite situation when    $s_{t+1}$   is a less visited state, we see that the reverse occurs and the learning rate is reduced in order to maintain the existing value of    $V_{s}$  .  

# 6.3. Estimating a small Markov process  

For our ﬁrst test we consider a small Markov process with 21 states. In each step the state number is either incremented or decremented by one with equal  

![](images/397b67d5b6f124dd3d08d4329364d1fad14ca9c9668d9f34f3e3a1b349c30d35.jpg)  
Figure 6.1.: 21 state Markov process, Figure 6.2.: 21 state Markov process, average performance over average performance over 10 runs. 100 runs.  

probability, unless the system is in state 0 or 20 in which case it always tran- sitions to state 10 in the following step. When the state transitions from 0 to 10 a reward of 1.0 is generated, and for a transition from 20 to 10 a reward of -1.0 is generated. All other transitions have a reward of 0. We set the discount value    $\gamma=0.9$   and then computed the true discounted value of each state by running a brute force Monte Carlo simulation.  

For our ﬁrst test we ran our algorithm 10 times on the above Markov chain and computed the root mean squared error in the value estimate across the states at each time step averaged across each run. The optimal value of    $\lambda$   for our algorithm, which we will call   $\operatorname{HL}(\lambda)$  , was 1.0. This was to be expected given that the environment is stationary and thus discounting old experience is not helpful. Setting this parameter correctly was important, for example if we reduced the value of    $\lambda$   to 0.98 performance became poor.  

For   $\operatorname{TD}(\lambda)$   the optimal value of    $\lambda$   was about 0.7. This algorithm was much less sensitive to the setting of    $\lambda$  . The other important parameter for TD(  $\lambda$  ) was the learning rate    $\alpha$  . We tested a variety of values, the eﬀect of which is illustrated by the two values    $\alpha=0.07$   and    $\alpha=0.13$   on Figure 6.1.  

When    $\alpha$   was high,   $\operatorname{TD}(\lambda)$   learnt more quickly, as we would expect, but then became unstable as the learning rate was too high for ﬁne tuning the value estimates. With the lower learning rate of 0.07, TD(  $\lambda$  ) learnt more slowly, but eventually achieved a more accurate value estimate before becoming stuck. In fact rather than just becoming stuck, what we see is that the error reaches a minimum at around    $\ensuremath{t}\ =\ 4\small{,}000$   and then actually becomes worse for the remainder of the run. This is a well known undesirable characteristic of TD(  $\lambda$  ) (see for example Section 6.2 of Sutton and Barto, 1998). With a ﬁxed learning rate we also see that the variability in the error estimate does not improve towards the end of the run.  

In comparison,   $\operatorname{HL}(\lambda)$   had very fast learning initially, combined with a more accurate ﬁnal estimate of the discounted state values, and the mean squared error in the second half of the experiment was very stable. This is signiﬁcant given that   $\operatorname{TD}(\lambda)$   required two parameters to be tuned for good performance, while   $\operatorname{HL}(\lambda)$   had just    $\lambda$   which could be set  a priori  to 1.0.  

Figure 6.2 shows the same experiment averaged over 100 runs. This obscures the better stability of   $\operatorname{HL}(\lambda)$  , but more clearly illustrates its faster learning and better convergence.  

# 6.4. A larger Markov process  

In order to understand how well  $\operatorname{HL}(\lambda)$   scales as the number of states increases, we ran the previous experiment again but with 51 states. As the movement through the states is almost entirely a random walk with reward on just two transitions, estimating the value function on this Markov chain is signiﬁcantly more diﬃcult than before, even though the total number of states has not grown all that much. In the new experiment the return state was still in the middle of the chain, i.e. state 25. As most of the state space was a long way from the rewards, we increased the    $\gamma$   value to 0.99 so that states in the middle of the chain would not have values too close to 0. The true discounted value of each state was again computed by running a brute force Monte Carlo simulation.  

We ran our algorithm 10 times on the above Markov chain and computed the root mean squared error in the value estimate across the states at each time step averaged across each run. The optimal value of    $\lambda$   for   $\operatorname{HL}(\lambda)$   was 1.0, which was to be expected given that the environment is stationary and thus discounting old experience is not helpful.  

For   $\operatorname{TD}(\lambda)$   we tried various diﬀerent learning rates and values of    $\lambda$  . We could ﬁnd no settings where   $\operatorname{TD}(\lambda)$   was competitive with   $\operatorname{HL}(\lambda)$  . If the learn- ing rate    $\alpha$   was set too high the system would learn as fast as   $\operatorname{HL}(\lambda)$   brieﬂy before becoming stuck. With a lower learning rate the ﬁnal performance was improved, however the initial performance was now much worse than   $\operatorname{HL}(\lambda)$  . The results of these tests appear in Figure 6.3.  

Similar tests were performed with larger and smaller Markov chains, and with diﬀerent values of    $\gamma$  .   $\operatorname{HL}(\lambda)$   was consistently superior to   $\operatorname{TD}(\lambda)$   across these tests. One wonders whether this may be due to the fact that the im- plicit learning rate that   $\operatorname{HL}(\lambda)$   uses is not ﬁxed. To test this we explored the performance of a number of diﬀerent learning rate functions on the 51 state Markov chain described above. We found that functions of the form    $\frac{\kappa}{t}$    always performed poorly, however good performance was possible by setting    $\kappa$   cor- rectly for functions of the form  $\frac{\kappa}{\sqrt{t}}$    and  $\frac{\kappa}{\sqrt[3]{t}}$  . As the results were much closer, we averaged over 300 runs. These results appear in Figure 6.4.  

With a variable learning rate   $\operatorname{TD}(\lambda)$   is performing much better, however we were still unable to ﬁnd an equation that reduced the learning rate in such  

![](images/f52fd9932529eea2e4c7ead16388b5e1ffd2b981f5dd83273b0554e8a7669b2a.jpg)  
Figure 6.3.: 51 state Markov process Figure 6.4.: 51 state Markov process averaged over 10 runs. averaged over 300 runs. The parameter    $a$   is the learning rate    $\alpha$  .  

a way that   $\operatorname{TD}(\lambda)$   would outperform   $\operatorname{HL}(\lambda)$  . This is evidence that   $\operatorname{HL}(\lambda)$   is adapting the learning rate optimally without the need for manual equation tuning.  

# 6.5. Random Markov process  

To test on a Markov process with a more complex transition structure, we created a random 50 state Markov process. We did this by creating a 50 by 50 transition matrix where each element was set to 0 with probability 0.9, and a uniformly random number in the interval   $[0,1]$  ] otherwise. We then scaled each row to sum to 1. Then to transition between states we interpret the    $i^{t h}$    row as a probability distribution over which state follows state    $i$  . To compute the reward associated with each transition we created a random matrix as above, but without normalising. We set    $\gamma=0.9$   and then ran a brute force Monte Carlo simulation to compute the true discounted value of each state.  

The    $\lambda$   parameter for   $\operatorname{HL}(\lambda)$   was simply set to 1.0 as the environment is stationary. For TD we experimented with a range of parameter settings and learning rate decrease functions. We found that a ﬁxed learning rate of    $\alpha=0.2$  , and a decreasing rate of    $\textstyle{\frac{1.5}{\sqrt[{3}]{t}}}$    performed reasonable well, but never as well as  $\operatorname{HL}(\lambda)$  . The results were generated by averaging over 10 runs, and are shown in Figure 6.5.  

Although the structure of this Markov process is quite diﬀerent to that used in the previous experiment, the results are again similar:   $\operatorname{HL}(\lambda)$   preforms as well or better than   $\operatorname{TD}(\lambda)$   from the beginning to the end of the run. Further- more, stability in the error towards the end of the run is better with   $\operatorname{HL}(\lambda)$  and no manual learning tuning was required for these performance gains.  

![](images/ad614f4b474269ff6dfff352162eddb3e2313bfd8eed12ff7a1474e9915d4009.jpg)  
Figure 6.5.: Random 50 state Markov Figure 6.6.: 21 state non-stationary process. The parameter    $a$  Markov process. is the learning rate    $\alpha$  .  

# 6.6. Non-stationary Markov process  

The    $\lambda$   parameter in   $\operatorname{HL}(\lambda)$  , introduced in Equation (6.4), reduces the impor- tance of old observations when computing the state value estimates. When the environment is stationary this is not useful and so we can set    $\lambda=1.0$  , however in a non-stationary environment we need to reduce this value so that the state values adapt properly to changes in the environment. The more rapidly the environment is changing, the lower we need to make  $\lambda$   in order to more rapidly forget old observations.  

To test   $\operatorname{HL}(\lambda)$   in such a setting we reverted back to the 21 state Markov chain from Section 6.3 in order to speed up convergence. We used this Markov chain for the ﬁrst 5,000 time steps. At that point, we changed the reward when transitioning from the last state to the middle state from -1.0 to be 0.5. At time 10,000 we then switched back to the original Markov chain, and so on alternating between the models of the environment every 5,000 steps. At each switch, we also changed the target state values that the algorithm was trying to estimate to match the current conﬁguration of the environment. For this experiment we set    $\gamma=0.9$  .  

As expected, the optimal value of    $\lambda$   for   $\operatorname{HL}(\lambda)$   fell from 1 down to about 0.9995. This is about what we would expect given that each phase is 5,000 steps long. For   $\operatorname{TD}(\lambda)$   the optimal value of  $\lambda$   was around 0.8 and the optimum learning rate was around 0 . 05. As we would expect, for both algorithms when we pushed    $\lambda$   above its optimal value this caused poor performance in the periods following each switch in the environment (these bad parameter settings are not shown in the results). On the other hand, setting    $\lambda$   too low produced initially fast adaption to each environment switch, but poor performance after that until the next environment change. To get accurate statistics we averaged over 200 runs. The results of these tests appear in Figure 6.6.  

$\mathbf{Albrechtm\2\HLS}(\lambda)$  Initialise    $Q(s,a)=0$  ,    $N(s,a)=1$   and    $E(s,a)=0$   for all    $s$  ,    $a$  Initialise    $s$   and    $a$  repeat Take action    $a$  , observed    $r$  ,    $s^{\prime}$  Choose    $a^{\prime}$    by using  $\epsilon$  -greedy selection on    $Q(s^{\prime},\cdot)$   $\Delta\gets r+\gamma Q(s^{\prime},a^{\prime})-Q(s,a)$   $E(s,a)\gets E(s,a)+1$   $N(s,a)\gets N(s,a)+1$  for all  $s,a$   do  $\begin{array}{r}{\beta((s,a),(s^{\prime},a^{\prime}))\leftarrow\frac{1}{N(s^{\prime},a^{\prime})-\gamma E(s^{\prime},a^{\prime})}\ \frac{N(s^{\prime},a^{\prime})}{N(s,a)}}\end{array}$  end for for all  $s,a$   do  $\begin{array}{r l}&{Q(s,a)\gets Q(s,a)+\beta\big((s,a),(s^{\prime},a^{\prime})\big)E(s,a)\Delta}\\ &{E(s,a)\gets\gamma\lambda E(s,a)}\\ &{N(s,a)\gets\lambda N(s,a)}\end{array}$  end for  $s\gets s^{\prime};a\gets a^{\prime}$  until  end of run  

For some reason   $\mathrm{HL}(0.9995)$   learns faster than TD(0 . 8) in the ﬁrst half of the ﬁrst cycle, but only equally fast at the start of each following cycle. Furthermore, its performance in the second half of the ﬁrst cycle is poor. We are not sure why this is happening. We could improve the initial speed at which   $\operatorname{HL}(\lambda)$   learnt in the last three cycles by reducing    $\lambda$  , however that comes at a performance cost in terms of the lowest mean squared error attained at the end of each cycle. In any case, in this non-stationary situation   $\operatorname{HL}(\lambda)$   again performed well in general.  

# 6.7. Windy Gridworld  

Reinforcement learning algorithms such as Watkins’ version of Q(  $\lambda$  ) (Watkins, 1989) and Sarsa(  $\lambda$  ) (Rummery and Niranjan, 1994; Rummery, 1995) are based on temporal diﬀerence updates. This suggests that new reinforcement learning algorithms based on   $\operatorname{HL}(\lambda)$   should be possible.  

For our ﬁrst experiment we took the standard   $\operatorname{Sarnsa}(\lambda)$   algorithm and mod- iﬁed it in the obvious way to use an HL temporal diﬀerence update. In the presentation of this algorithm we have changed notation slightly to make things more consistent with that typical in reinforcement learning. Speciﬁcally, we have dropped the    $t$   super script as this is implicit in the algorithm speciﬁca- tion, and have deﬁned    $Q(s,a):=V_{(s,a)}$  ,  $E(s,a):=E_{(s,a)}$   and    $N(s,a):=N_{(s,a)}$  . Our new reinforcement learning algorithm, which we call   $\mathrm{HLS}(\lambda)$   is given in Algorithm 2. Essentially the only changes to the standard   $\operatorname{Sarnsa}(\lambda)$   algorithm  

![](images/fccf1f30f6085c3828a977e0d461ec05ff71f1291091149403487756c2348704.jpg)  

Figure 6.7.: [Windy Gridworld] S marks the start state and G the goal state, at which the agent jumps back to S with a reward of 1. Small arrows indicate an upward wind of one row per time step. The large arrows indicate a wind of two rows per time step.  

have been to add code to compute the visit counter    $N(s,a)$  , add a loop to compute the    $\beta$   values, and replace    $\alpha$   with    $\beta$   in the temporal diﬀerence update.  

To test   $\mathrm{HLS}(\lambda)$   against standard Sarsa( λ ) we used the Windy Gridworld environment described on page 146 of (Sutton and Barto, 1998). This world is a grid of 7 by 10 squares that the agent can move through by going either up, down, left or right. If the agent attempts to move oﬀthe grid it simply stays where it is. The agent starts in the   $4^{t h}$    row of the   $1^{s t}$    column and receives a reward of 1 when it ﬁnds its way to the   $4^{t h}$    row of the   $8^{t h}$    column. To make things more diﬃcult, there is a “wind” blowing the agent up 1 row in columns 4, 5, 6, and 9, and a strong wind of 2 rows in columns 7 and 8. This is illustrated in Figure 6.7. Unlike in the original version, we have set up this problem to be a continuing discounted task with an automatic transition from the goal state back to the start state. This is because we have not yet derived an episodic version of our learning rule.  

We set    $\gamma=0.99$   and in each run computed the empirical future discounted reward at each point in time. As this value oscillated we also ran a moving average through these values with a window length of 50. Each run lasted for 50,000 time steps as this allowed us to see at what level each learning algorithm topped out. These results appear on the left of Figure 6.8 and were averaged over 500 runs to get accurate statistics.  

Despite putting considerable eﬀort into tuning the parameters of Sarsa(  $\lambda$  ), we were unable to achieve a ﬁnal future discounted reward above 5.0. The  

![](images/ecfb861301df18489c0eea15b237799b3291b74e4b7eda3800bae89bf01c732e.jpg)  
Figure 6.8.: Windy Gridworld performance tests. The left graph shows  $\mathrm{HLS}(\lambda)$   vs.   $\operatorname{Sarnsa}(\lambda)$  , while the right graph shows   $\mathrm{HLQ}(\lambda)$   vs.  $\operatorname{Q}(\lambda)$  . In both tests the algorithm based on HL learning performed best.  e  represents the exploration parameter    $\epsilon$  , and    $a$   represents the learning rate    $\alpha$  . For both tests the performance was averaged over 500 runs.  

settings shown on the graph represent the best ﬁnal value we could achieve. In comparison   $\mathrm{HLS}(\lambda)$   easily beat this result at the end of the run, while being slightly slower than   $\operatorname{Sarnsa}(\lambda)$   at the start. By setting    $\lambda=0.99$   we were able to achieve the same performance as   $\operatorname{Sarnsa}(\lambda)$   at the start of the run, however the performance at the end of the run was then only slightly better than  $\operatorname{Sarnsa}(\lambda)$  . This combination of superior performance and fewer parameters to tune suggest that the beneﬁts of   $\operatorname{HL}(\lambda)$   carry over into the reinforcement learning setting. In terms of computational cost,   $\operatorname{HL}(\lambda)$   was about 1.7 times slower than Sarsa(  $\lambda$  ) per time step due to the cost of computing the  $\beta$   values.  

Another popular reinforcement learning algorithm is Watkins’   $\mathrm{Q}(\lambda)$  . Similar to   $\operatorname{Sarnsa}(\lambda)$   above, we simply inserted the   $\operatorname{HL}(\lambda)$   temporal diﬀerence update into the usual Q(  $\lambda$  ) algorithm in the obvious way. We call this new algorithm HLQ(  $\lambda$  ) and it is given in Algorithm 3. The test environment was exactly the same as we used with Sarsa(  $\lambda$  ) above.  

The results this time were more competitive and appear on the right hand side of Figure 6.8. Nevertheless, despite spending a considerable amount of time ﬁne tuning the parameters of   $\operatorname{Q}(\lambda)$  , we were unable to beat HLQ(  $\lambda$  ). As the performance advantage was relatively modest, the main beneﬁt of HLQ(  $\lambda$  ) was that it achieved this level of performance without having to manually tune a learning rate.  

Initialise    $Q(s,a)=0$  ,    $N(s,a)=1$   and    $E(s,a)=0$   for all    $s$  ,    $a$  Initialise    $s$   and    $a$  

repeat Take action    $a$  , observed    $r$  ,    $s^{\prime}$  Choose    $a^{\prime}$    by using    $\epsilon$  -greedy selection on    $Q(s^{\prime},\cdot)$   $a^{*}\gets\operatorname{arg\,max}_{b}Q(s^{\prime},b)$   $\Delta\leftarrow r+\gamma Q(s^{\prime},a^{*})-Q(s,a)$   $E(s,a)\gets E(s,a)+1$   $N(s,a)\gets N(s,a)+1$  for all    $s,a$   do  $\begin{array}{r}{\beta((s,a),(s^{\prime},a^{\prime}))\leftarrow\frac{1}{N(s^{\prime},a^{\prime})-\gamma E(s^{\prime},a^{\prime})}\ \frac{N(s^{\prime},a^{\prime})}{N(s,a)}}\end{array}$  end for for all    $s,a$   do  $\begin{array}{r l}&{Q(s,a)\gets Q(s,a)+\beta\big((s,a),(s^{\prime},a^{\prime})\big)E(s,a)\Delta}\\ &{N(s,a)\gets\lambda N(s,a)}\end{array}$  if    $a^{\prime}=a^{*}$  then  $E(s,a)\gets\gamma\lambda E(s,a)$  else  $E(s,a)\gets0$  end if end for  $s\gets s^{\prime};a\gets a^{\prime}$  until  end of run  

# 6.8. Conclusion  

We have derived a new equation for setting the learning rate in temporal dif- ference learning with eligibility traces. The equation replaces the free learning rate parameter    $\alpha$  , which is normally experimentally tuned by hand. In every setting tested, be it stationary Markov chains, non-stationary Markov chains or reinforcement learning, our new method produced superior results.  

To further our theoretical understanding, the next step would be to try to prove that the method converges to correct estimates. This can be done for   $\operatorname{TD}(\lambda)$   under certain assumptions on how the learning rate decreases over time (Dayan, 1992; Peng, 1993). Hopefully, something similar can be proven for our new method. In terms of experimental results, it would be interesting to try diﬀerent types of reinforcement learning problems and to more clearly identify where the ability to set the learning rate diﬀerently for diﬀerent state transition pairs helps performance.  

Many extensions to the algorithm should also be possible. One would be to generalise the learning rule to episodic tasks, another would be to merge our update rule with Peng’s version of Q(  $\lambda$  ) (Peng and Williams, 1996), as we have done with Sarsa(  $\lambda$  ) and Watkins’ version of Q(  $\lambda$  ). Finally, it would be useful to extend the algorithm to work with function approximation methods so that it could deal with larger state spaces.  

# 7. Discussion  

The title of this thesis is deliberately provocative. It asks the reader to consider not just intelligent machines, but the possibility of machines that are super intelligent. Many ﬁnd this idea diﬃcult to take seriously. Among researchers the topic is almost taboo: it belongs in science ﬁction. The most intelligent computer in the world, they assure the public, is perhaps as smart as an ant, and that’s on a good day. True machine intelligence, if it is ever developed, lies in the distant future.  

This was not always the case. In the 1960’s pioneering artiﬁcial intelligence researchers had initial successes in a range of areas. Emboldened, they pre- dicted that more powerful systems were not far oﬀ, and that truly intelligent machines might follow. As the researcher Herbert Simon wrote in 1965, “ma- chines will be capable, within twenty years, of doing any work a man can do.” (quoted in Crevier, 1993) The ﬁeld was alight with ambition and, not surprisingly, attracted plenty of attention.  

Over the decade that followed a series of high proﬁle failures brought this dream crashing back to earth. Although progress was being made, it was far slower than many had expected. Naturally the public, and more impor- tantly the funding agencies, wanted to know where the intelligent machines were. The cuts that ensued marked the beginning of the so called ‘AI win- ter’, although usage of this label varies considerably (Crevier, 1993; Russell and Norvig, 1995). During the early 80’s there was a brief reprieve as expert systems became popular, however by the late 80’s these too had failed to live up to expectations. Over time some areas distanced themselves from the label ‘artiﬁcial intelligence’, emphasising that their work was more practical and limited in scope. Any talk of building machines with human level intelligence was frowned upon.  

Since the early 90’s steady progress has slowly begun to reinvigorate the ﬁeld, particularly since the late 90’s. More powerful algorithms coupled with dramatically improved hardware has produced countless advances in robotics, speech recognition, natural language processing, image processing, clustering, classiﬁcation, prediction, various types of optimisation, and many other areas. As a result the reputation of artiﬁcial intelligence has started to recover and funding has improved, leading some to believe that the ‘AI spring’ may have ﬁnally arrived (Havenstein, 2005).  

With the mood becoming more positive, the grand dream of artiﬁcial in- telligence is starting to make a come back. A number of books predicting the arrival of advanced artiﬁcial intelligence have been published and major conferences have held workshops on ‘Human level AI’. Small conferences on ‘Artiﬁcial General Intelligence’ and on the safety issues surrounding powerful machine intelligence have also appeared. Perhaps over the next few years these ideas will become more mainstream, however for now they are at the fringe. Most researchers remain very sceptical about the idea of truly intelligent ma- chines within their lifetime.  

One goal of this thesis is to promote the idea that intelligent machines, even super intelligent machines, is a topic that is both important and one that can be scientiﬁcally studied, even if just theoretically for now. In Chapters 2 and 3 we described Hutter’s model of an intelligent machine and examined some of its remarkable properties. In Chapter 4 we saw how this model can be used to construct a general measure of machine intelligence that formalises many of the standard perspectives on the nature of intelligence outlined in Chapter 1. In Chapter 5 we then examined some of the theoretical limitations faced by powerful artiﬁcial intelligence algorithms. Thus, although highly intelligent machines do not exist yet, theoretical tools are starting to emerge to allow us to study their properties. While this thesis makes some contributions to this eﬀort, many fundamental questions remain open (see for example the open problems listed in Hutter, 2005).  

None of this theoretical work would be of much importance if intelligent machines were impossible in practice, or exceedingly unlikely in any reasonable time frame. In this last chapter we will argue that this is not the case. Our goal is not to conclusively argue that this will happen, or exactly when it will happen, but simply to argue that the possibility cannot be completely discounted. This is important because if a super intelligent machine ever did exist the implications for humanity would be immense. Thus, if there is even a small probability that intelligent machines could be developed in the foreseeable future, it is important that we start to think seriously about the nature of these machines and what the implications might be.  

# 7.1. Are super intelligent machines possible?  

Many people outside of the ﬁeld are deeply sceptical about the idea that ma- chines, mere physical objects of our construction, could ever have anything resembling real intelligence: machines can only ever be strictly logical; they cannot do anything they were not programed to do; and they certainly could not be superior to their own creator — that would be a paradox! However, as anybody working in the ﬁeld knows, these common beliefs are baseless myths. Artiﬁcial intelligence algorithms regularly ﬁnd solutions to problems using heuristics and forms of reasoning that are not strictly logical. They dis- cover powerful new designs for problems that the system’s programmers had never thought of (Koza et al., 2003). They also learn to play games such as chess (Hsu et al., 1995) and backgammon (Tesauro, 1995) at levels superior to that of any human, let alone the researchers who designed and created the system. Indeed, in the case of checkers, computers are now literally unbeatable as they can play a provably perfect game (Schaeﬀer et al., 2007).  

The persistence of these beliefs seems to be due to a number of things. One is that algorithms from artiﬁcial intelligence are not consumer products: they are hidden in the magic of sophisticated technology. For example, when hand writing the address on a card most people do not know that it will likely be read by a computer rather than a human at the sorting oﬃce. People do not think about the learning algorithms that are monitoring their credit card transactions looking for fraud, ﬁltering spam to their email address, automat- ically trading their retirement savings on international markets, monitoring their behaviour on the internet in order to decide which ads should appear on web pages they view, or even just the vision processing algorithms that graded the apples at the supermarket. The steady progress that artiﬁcial intelligence algorithms are making is out of sight, and thus generally out of mind.  

Another common objection is that we humans have something mysterious and special that makes us tick, something that machines, by deﬁnition, do not have. Perhaps some type of non-physical consciousness or feelings, qualia, or ‘quantum ﬁeld’ etc. Of course it is impossible to rule out mysterious possibili- ties until an intelligent machine has been constructed without needing anything particularly mysterious. Nonetheless, we should view such objections for what they are: a form of vitalism. Throughout history, whenever science could not explain some unusual phenomenon, many people readily assumed that God or magic was at work. Even distinguished scientists have fallen into this, only to be embarrassed once more sceptical and curious scientists worked out what was actually going on. Things ranging from the motion of whole galaxies to the behaviour of sub-atomic particles are now known to follow extremely precise physical laws. To conjecture that our brains are somehow special and diﬀerent in some strange way is to speculate based on nothing but our own feelings of specialness.  

If the human brain is merely a ‘meat machine’, as some have put it, it is certainly not the most powerful intelligence possible. To start with, there is the issue of scale: a typical adult human brain weights about 1.4 kg and consumes just 25 watts of power (Kandel et al., 2000). This is ideal for a mobile intelligence, however an artiﬁcial intelligence need not be mobile and thus could be orders of magnitude larger and more energy intensive. At present a large supercomputer can ﬁll a room twice the size of a basketball court and consume 10 megawatts of power. With a few billion dollars much larger machines could be built. Google, for example, is currently constructing a data centre next to a power station in Oregon that will cover two football ﬁelds and have cooling towers four stories high (Markoﬀand Hansell, 2005). Biology never had the option of building brains on such an enormous scale.  

Another point is that brains use fairly large and slow components. Consider one of the simpler of these, axons: essentially the wiring of the nervous system. These are typically around 1 micrometre wide, carry spike signals at up to 75 metres per second at a frequency of at most a few hundred hertz (Kandel et al., 2000). Compare these characteristics with those of a wire that carries signals on a microchip. Currently these are 45 nanometres wide, propagate signals at 300 million metres per second and can easily operate at 4 billion hertz. Some might debate whether an electrochemical spike travelling down an axon is so directly comparable to an electrical pulse travelling down a wire, however it is well established that at least the primary role of an axon is simply to carry this information. Given that present day technology produces wires which are 20 times thinner, propagate signals 4 million times faster and operate at 20 million times the frequency, it is hard to believe that the performance of axons could not be improved by at least a few orders of magnitude.  

Of course, the above assumes that the brain’s design is what we should replicate. Perhaps the brain’s algorithm is close to optimal for some things, but it certainly is not optimal for all problems. Even the most outstanding sa- vants cannot store information anywhere near as quickly, accurately and in the quantities that are possible for a computer. Also savants’ impressive ability to perform fast mental calculations is insigniﬁcant next to even a basic calculator. Brains are poorly designed for such feats. A machine, however, would have no such limitations: it could employ a range of specialised algorithms for diﬀerent types of problems. Concepts like education become obsolete when knowledge and understanding can simply be copied from one intelligent machine to an- other. It is easy to think up many more advantages.  

Most likely improvements over brains are possible in algorithms, hardware and scale. This is not to take away from the amazing system that the brain is, something that we are still unable to match in many ways. All we wish to point out is that if the brain is essentially just a machine, which appears to be the case, then it certainly is not the most intelligent machine that could exist. This idea is reasonable once you think about it: machines can easily carry more, ﬂy higher, move faster and see further than even the most able animals in each of these categories. Why would human intelligence be any diﬀerent? Of course, just because systems with greater than human intelligence are possible in principle, this does not mean that we will be able to build one. Designing and constructing such an advanced machine could be beyond our capabilities.  

# 7.2. How could intelligent machines be developed?  

There are many ways in which machine intelligence might be developed. Un- fortunately, it is diﬃcult to estimate how likely any of these approaches are to succeed. In this section we speculate on a few of them.  

# Theoretical approaches  

The approach most closely related to this thesis would be to take the  AIXI model and ﬁnd a way to usefully scale it down. A number of attempts to do this have been made: the   $\operatorname{HL}(\lambda)$   algorithm presented in Chapter 6, the AIXI tl  algorithm (Chapter 7 of Hutter, 2005), the  AIXI  based algorithm for repeated matrix games (Poland and Hutter, 2006) and  Fitness Uniform Opti- misation  (Hutter and Legg, 2006) all originate in eﬀorts to scale down  AIXI . Although not deriving from  AIXI , the  Shortest and Fastest  algorithm in (Hut- ter, 2002a), the  Speed prior  (Schmidhuber, 2002), the  Optimal Ordered Prob- lem Solver  (Schmidhuber, 2004), and the  G¨ odel Machine  (Schmidhuber, 2005) come from a related background in algorithmic probability and Kolmogorov complexity theory. While each of these has their strengths, as yet none come close to bringing the power of theoretical models such as  AIXI  into reality.  

The key question is how to make a general and powerful artiﬁcial intelli- gence like  AIXI  work eﬃciently. The results of Chapter 5 oﬀer some hints on the direction that such a project might take. For certain, the prediction of general computable sequences is out of the question (Lemma 5.2.4), as is the prediction of all computable sequences whose Kolmogorov complexity is below some moderate bound. Otherwise serious problems arise with the neces- sary complexity of the prediction algorithms (Theorem 5.3.3), and even worse G¨ odel incompleteness (Theorem 5.6.1). Nevertheless, we know that certain types of complex sequences can be predicted by relatively simple algorithms (Lemma 5.2.3), and that many theoretical problems go away when even weak bounds are placed on the computation time of the sequences to be predicted (Lemma 5.5.2). Thus, what we need to aim for is the ability to eﬃciently pre- dict computable sequences that have certain computational resource bounds. How best to characterise such sequences is an open problem, let alone how to eﬃciently learn to predict them. Perhaps any breakthrough is more likely to come from the opposite direction: somebody discovers a theoretically elegant and very powerful algorithm that is able to eﬃciently predict many kinds of se- quences. The structure of this algorithm and how easily it can model diﬀerent sequences will then implicitly deﬁne a natural measure of resource bounded sequence complexity.  

When a breakthrough in this area might occur is impossible to predict, and the same is true of other theoretical approaches. Perhaps with the right theoretical insight Bayesian networks, prediction with expert advice, artiﬁcial neural networks, reasoning engines, or any one of a dozen other techniques will suddenly advance in a dramatic way. Although some progress in all of these areas is a near certainty, true breakthroughs by their very nature are rare and highly unpredictable. Furthermore, even if a huge breakthrough did occur, whether existing computer hardware would be suﬃcient to support the creation of highly intelligent machines would depend on the nature of this new algorithm. In summary, although we cannot rule out the possibility of a large breakthrough leading to intelligent machines, there is little we can do to estimate how likely this is.  

# Brain simulation  

Rather than taking abstract theories like  AIXI  and trying to scale them down to make them practical, another approach is to proceed in the opposite direction: start by studying the details of how real biological brains work and then try to abstract from this a design for an artiﬁcial intelligence. Although this idea is simple in principle, studying and understanding the brain is very challenging.  

One of the most basic problems is that it is currently impossible to observe a brain in action with suﬃcient spatial and temporal resolution to really see how it works. Methods to study the brain include: PET scans, fMRI scans, arrays of probes, marking neurons with chemicals which cause them to ﬂuoresce when they ﬁre, placing extracted neural tissue on microchips that can sense when neurons ﬁre, and the use of staining and microscopy to study the anatomical structure of the brain. The problem is that none of these methods allows a researcher to take a sizable area of a brain and simultaneously observe exactly which neurons are ﬁring, precisely when they ﬁre, what type of neurons they are, which other neurons they are connected to, the types of these connections, how these connections change over short intervals of time, and so on. Instead, researchers have at their disposal a range of methods, each of which provides only a limited view of what is going on. Indeed, what is known about the brain is largely dictated by the strengths and weaknesses of the diﬀerent methods of study.  

Another major problem is the sheer complexity of the system. A human brain consists of hundreds of billions of neurons, and hundreds of trillions of synapses. There are over a hundred diﬀerent types of neurons, diﬀerent synapses employ diﬀerent combinations of neurotransmitters, connection pat- terns vary from one part of the brain to another, and so on (see any standard text book, for example Kandel et al., 2000). When looking at slices of brain tissue where a small percentage of neurons and their dendritic trees have been stained, the brain’s wiring starts to look about as comprehensible as an ocean of tangled spaghetti. Even worse, all these elements interact with each other in highly dynamic ways. Via some kind of a miracle, from this monstrous cacophony emerges the human mind. With so much complexity, even if the perfect brain scanning technology existed it might still be very diﬃcult to understand how the brain actually works.  

Given the scale of these diﬃculties, is building a brain simulation feasible? Perhaps the ﬁrst point to note is that building a working simulation of some- thing does not require understanding everything about how the system works. What is required is that the basic units which comprise the system can be faithfully reproduced and connected together. If this is done properly, the re- sulting dynamics will be the same as the dynamics in the real system — even if we do not fully understand what these higher level dynamics are, or why they are important to the system’s overall functioning. Indeed, simulations are often constructed for the very purpose of better understanding a system’s emergent dynamics once its low level dynamics are understood. Thus, rather than needing to understand all the mysteries of how the brain works, in order to build a functional simulation it is suﬃcient to understand the nature and or- ganisation of the brain’s elementary units: neurons, dendrites, axons, synapses etc. There is already a large body of knowledge about how these basic units work and how they are wired together in various parts of the brain. Literally thousands of researchers around the world are reﬁning and developing this knowledge.  

Another important point, at least for people interested in developing arti- ﬁcial intelligence, is that much of the brain’s complexity is not relevant. A signiﬁcant part of the human brain is a jumble of diﬀerent subsystems that take care of basic instinctive things like breathing, heart beat, blood pressure, reproduction, hunger, thirst, rhythms such as sleeping and body temperature, ﬁght or ﬂight response and so on. Even one of the largest parts of the brain, the cerebellum which is involved in movement and precision timing, is not needed for an artiﬁcial intelligence: individuals without one are still intellectually and emotionally able. The key, it seems, lies in understanding the neocortex, and its interaction with two smaller structures, namely, the thalamus and the hippocampus. It is known that the neocortex is the part of the brain that is primarily responsible for processing vision, sound, touch, proprioception, understanding and generating language, planning, spatial reasoning, coordi- nating and executing movement, and logical thought (Fuster, 2003). Clearly then, the key to artiﬁcial intelligence via brain simulation lies in understanding the neocortex and related structures.  

As diﬀerent regions of the neocortex perform diﬀerent functions, one might expect that they would have signiﬁcantly diﬀerent anatomical structures. Amazingly, this is not the case. Essentially the whole neocortex has the same six layer structure, or up to 12 layers depending on how you count them. Each layer is characterised by the types of neurons present, where axons from neu- rons in the layer project to, and where axons come from that form synapses on the dendritic trees of these neurons. Besides some thickening and thinning of the layers in diﬀerent regions, and the fact that primary visual cortex actu- ally has an extra layer, this six layer structure is consistent across the whole neocortex (Fuster, 2003; Abeles, 1991). What this suggests is that the same information processing mechanism is being applied across the neocortex, and that the variations in function across diﬀerent regions are actually adaptations to the information passing through each region (Creutzfeldt, 1977; Mountcas- tle, 1978).  

A number of results back up this hypothesis. One is that with increased use the region of cortex responsible for performing some action tends to expand, in the sense that neighbouring cortex is recruited. In extreme cases, such as the congenitally blind, the unused areas of visual cortex start to perform other roles, such as helping touch processing for reading braille. In a more dramatic example, the brain of a ferret was physically altered at birth so that visual neocortex received auditory input and vice versa. Each region of neocortex then learnt to process the input it was receiving (Melchner et al., 2000). Similar experiments with rats have produced consistent results.  

If the hypothesis of a single underlying learning and adaptation dynamic across the neocortex is correct, then the key to understanding much of the in- tellectual capacity of the brain lies in understanding how these six layers work, and the way in which they interact with the thalamus and hippocampus. In recent years this approach to artiﬁcial intelligence has been popularised by Jeﬀ Hawkins (2004), though most of these ideas have been known in neuroscience for some time. The main vehicle for Hawkins’ artiﬁcial intelligence work is his company Numenta, with related neuroscience research being carried out at the Redwood Center for Theoretical Neuroscience, also founded by Hawkins. They are certainly not alone in trying to understand the cortex, indeed it is one of the largest areas of neuroscience research with whole journals dedicated to the topic.  

One group of researchers whose work may be useful to brain modelling is currently cutting a cortical column into 30 nanometre slices and then scan- ning these using both electron and light based microscopes. As this produces enormous quantities of data, machine learning algorithms are being developed to automatically identify the structures in these images. Over the next few years this process should produce an extremely detailed three dimensional anatomical model of the cortex (Singer, 2007).  

A group with more of a simulation emphasis is the BlueBrain project based at EPFL. Using information on the structure and behaviour of cortical columns collected from a wide range of sources, they have built a computer model of a column that they run on an IBM BlueGene supercomputer. To calibrate their model they use segments of rat cortex which they stimulate in diﬀerent ways and then compare the resulting dynamics with what their model predicts. They now claim to have succeeded in accurately modelling the dynamics of a cortical column, and are working on ways to expand this model to be able to simulate groups of columns working together (Graham-Rowe, 2007). Their goal is to eventually be able to simulate an entire human neocortex.  

Another group working at the IBM Almaden Research Lab recently an- nounced that they had simulated a mouse scale neocortex, also on an IBM BlueGene supercomputer. Their model consisted of 8 million neurons and 50 billion synapses and ran at one seventh real time speed. They claim that this simulation produced dynamical properties consistent with what is ob- served in a real mouse brain, including EEG like waves (Anantha narayan an and Modha, 2007). Their aim is also to scale up to a human sized neocortex as more powerful supercomputers become available in the coming years. Unlike the BlueBrain project, their core goals are less neuroscience orientated: as their model starts to do more interesting things, their aim is to extract from this useful algorithms for artiﬁcial intelligence.  

Obviously these simulations are pushing the limits of what is known about the cortex, and also what is possible with current computer technology. Nev- ertheless, the fact that these simulations are being attempted at all illustrates how the gap between the power of supercomputers and brains is perhaps not as large as some people think. At present the world’s fastest machine is the IBM Roadrunner supercomputer at the US department of defence which has an oﬃcial LINPACK benchmark performance of   $10^{15}$    ﬂoating point operations per second (FLOPS). Machines capable of   $10^{16}$    FLOPS are being designed and should appear in a few years. No doubt these will be superseded by even more powerful machines.  

To put these numbers in perspective, a human cortex has on the order of  $10^{10}$    neurons and   $10^{14}$    synapses (Koch, 1999). Given that neurons can ﬁre on the order of 100 Hz, this gives a crude estimate of the computational capacity of the brain:   $10^{16}$    operations per second (Moravec, 1998; Kurzweil, 2000). Of course, until a simulation succeeds in producing intelligence, nobody knows for sure how much computer power will be needed. Researchers working in molecular neuroscience tend to think it is much more, while some working in theoretical neuroscience think it could be less. If the estimate of  $10^{16}$    FLOPS is in fact 100 times too low, then we will have to wait 10 years before a suﬃciently powerful computer exists.  

# Evolution  

Another approach that is becoming more attractive with increasing computer power is artiﬁcial evolution. After all, natural evolution produced the human brain so we know that the approach does work — at least as a planet wide phenomenon over billions of years! No computer in the foreseeable future could hope to simulate evolution on such a scale. Fortunately, evolving an artiﬁcial intelligence via an evolutionary algorithm is a much smaller problem. To begin with, it is not necessary to start from scratch the way nature did. Of the approximately 4 billion years since simple cellular life ﬁrst came into existence, about 3 billion years of this time was required just to get to the level of multicellular life. Only then could more complex organisms start to evolve. We can short circuit this by building a virtual body for the agent. Of course, evolving an intelligence for a complex body might be too diﬃcult, thus we may want to begin with trying to evolve a simple intelligence for a simple body in a simple environment, and then scale up. In any case, all the evolutionary algorithm has to do is to work on the design of the agent’s intelligence: much of the remaining complexity can be taken care of by us.  

Another important point is that natural evolution does not seek to max- imise intelligence: its eﬀect is to optimise agents’ ability to spread their genes. Intelligence is a secondary feature that is more useful in some ecological niches than others. In artiﬁcial evolution this does not need to be the case. So long as we can deﬁne and measure intelligence in a suﬃciently general way, we can use this to evaluate the ﬁtness of individuals. With evolution explicitly directed towards maximising intelligence, progress towards more intelligent agents should be far more rapid.  

The universal intelligence measure described in Chapter 4 lays the founda- tion for such a measure. Essentially, what the universal intelligence measure says is that we should test agents initially over extremely trivial pattern recog- nition and interaction problems as these are the most important. As agents learn to solve these, we should slowly include more complex problems, always ensuring that agents are still able to solve the simple problems that came before. This ensures that the agents’ abilities remain very general as they develop. As the universal intelligence measure is still a theoretical deﬁnition, some further work would be needed to ﬁgure out how best to convert it into a practical test for evolving agents.  

The importance of having a good ﬁtness function cannot be stressed enough: with a ﬁtness function that is reliable, smooth and has a gradient that generally points in the right direction, even high dimensional optimisation problems can become relatively easy. With an unreliable or deceptive ﬁtness function seemingly simple problems can become practically unsolvable. It is currently unknown how well universal intelligence would work as a ﬁtness function for evolving agents with general intelligence.  

Another major issue concerns the model that we use for the agents’ intel- ligence: should the agents be neural networks, programs in some language, or some other kind of object? In theory any Turing equivalent representation should do, however in practice the choice of representation is important as it has the eﬀect of biasing the space of possibilities towards certain types of agents. Often choosing the right representation is critical to making artiﬁcial evolution work. For an intelligent agent there are many possible systems of representation, each with their respective proponents. Unfortunately, nobody really knows which of these is the best. About the only thing we know for sure is that nature was able to evolve intelligence working with networks of spik- ing neurons. On the other hand, with a computer system perhaps something closer to a traditional programing language would be more eﬃcient.  

One important problem faced in large scale artiﬁcial evolution is diversity control. Essentially, if you apply too much selection pressure the population tends to collapse around a small group of individuals that are all related to the ﬁttest individual. At this point evolution becomes stuck due to a lack of genetic diversity. If you reduce the selection pressure this helps, however now the speed at which the population ﬁtness rises is reduced. Even with no selection pressure at all population diversity tends to collapse over time due to the phenomenon of genetic drift. For problems where the evolving individuals are fairly simple this can easily be dealt with by creating a distance metric to evaluate how similar individuals are to each other. This can then be used to ensure that similar individuals tend not to mate with each other, or have reduced ﬁtness (see for example Goldberg and Richardson, 1987; Jong, 1975). In more complex problems, however, it can become very diﬃcult to judge how similar two individuals really are. For example, two neural networks may compute the same function, but have completely diﬀerent weights and topologies. Indeed, due to Rice’s theorem it is impossible in general to decide whether two algorithms compute the same function.  

One solution is to use diversity control methods that do not rely on com- paring the genotypes of the individuals, but rather by comparing properties of their phenotypes. A simple example of this is  Fitness Uniform Optimisa- tion  where the evolutionary algorithm tries to increase the diversity in the population by increasing the diversity of ﬁtness (Hutter and Legg, 2006). In the case of the  Fitness Uniform Selection Scheme  (FUSS) this is achieved through selection pressure (Hutter, 2002b; Legg et al., 2004), while for the Fitness Uniform Deletion Scheme  it is achieved by deletion (Legg and Hutter, 2005a). Although these methods have not yet been applied to the evolution of programs or neural networks, they have proven to be eﬀective on a number of deceptive optimisation problems.  

Another possibility might be to mix biologically and theoretically derived designs with evolution. For example, as a starting point take a model of the neocortex based on biological studies, such as those in the previous section, and then apply artiﬁcial evolution to modify and tune the model. Although we might not get the initial design right due to limitations in our understanding of the cortex, it is reasonable to suppose that in the space of all neural networks this initial design is relatively close to the correct design, or a related design that also works. Starting with individuals that are reasonably close to the target makes the optimisation problem that evolution has to solve orders of magnitude easier.  

Even if each of the above suggestions succeeded in reducing the diﬃculty of evolving intelligence by several orders of magnitude, perhaps the biggest prob- lem is still computer power. Advancing technology over the coming decades will help, but this could still be too little. Perhaps the closest we could come to nature’s planet scale evolution would be to construct a world wide net- work of machines donating computation time. After all, more than 2 million years of computer time have so far been donated to the Search for Extrater- restrial Intelligence (SETI), why not something similar to search for artiﬁcial intelligence?  

# 7.3. Is building intelligent machines a good idea?  

It is impossible to know whether any of the approaches discussed in the pre- vious section, or other approaches, will succeed in producing truly intelligent machines. But this is not the point we want to make: the point is that it is not obvious that they will all fail. This is important, because the impact of this event would be huge. Following the ﬁrst credible demonstration of true general intelligence in a machine, for sure a much larger and more powerful machine will be constructed shortly thereafter. This leads to what I. J. Good referred to as an intelligence explosion:  

“Let an ultra intelligent machine be deﬁned as a machine that can far surpass all the intellectual activities of any man however clever. Since the design of machines is one of these intellectual activi- ties, an ultra intelligent machine could design even better machines; there would then unquestionably be an ‘intelligence explosion,’ and the intelligence of man would be left far behind. Thus the ﬁrst ultra intelligent machine is the last invention that man need ever make.” (Good, 1965)  

The deﬁning characteristic of our species is intelligence. It is not by superior size, strength or speed that we dominate life on earth, but by our intelligence. If our intelligence were to be signiﬁcantly surpassed, it is diﬃcult to imagine what the consequences of this might be. It would certainly be a source of enormous power, and with enormous power comes enormous responsibility.  

Machine intelligence could bring unprecedented wealth and opportunity if used constructively and safely. Alternatively, it could bring about some kind of a nightmare scenario. The latter possibility is certainly well known, being a staple of science ﬁction. Positive ﬁctional depictions are rare, probably because casting the machines as villains is a convenient plot device. Outside of works of ﬁction, however, the implications of powerful machine intelligence are rarely encountered. Indeed, the whole subject of truly intelligent machines is generally avoided by academics, as noted at the start of this chapter.  

If one accepts that the impact of truly intelligent machines is likely to be profound, and that there is at least a small probability of this happening in the foreseeable future, it is only prudent to try to prepare for this in advance. If we wait until it seems very likely that intelligent machines will soon appear, it will be too late to thoroughly discuss and contemplate the issues involved. Historically technology has advanced in leaps and bounds, while social and ethical considerations have developed more slowly, often only as a reaction to the problems created by a technology after it arrived. Even what now seem to be obvious moral principles, such as gender and racial equality, were debated for centuries and are still not accepted in many parts of the world. Given that the implications of powerful machine intelligence are likely to be complex, we cannot expect to ﬁnd good answers quickly. We need to be seriously working on these things now.  

A small but growing number of forward thinking individuals and organ- isations are thinking about these issues. Perhaps the premier organisation dedicated to the safe and beneﬁcial development of powerful artiﬁcial intelli- gence is the Singularity Institute for Artiﬁcial Intelligence ( SIAI ). In 2007  SIAI organised a conference that attracted well known speakers including Rodney Brooks (director of the computer science and AI laboratory at MIT), Bar- ney Pell (AI researcher and CEO of Powerset), Wendell Wallach (bioethics lecturer at Yale), Sam Adams (IBM distinguished engineer), Paul Saﬀo (lec- turer at Sanford), Peter Norvig (director of research at Google), Peter Thiel (founder of Clarium Capital and co-founder of Paypal) and Ray Kurzweil (fu- turologist, inventor and entrepreneur). Although this event was one of the ﬁrst of its kind, the calibre of these speakers makes it clear that issues sur- rounding the development of advanced artiﬁcial intelligence are starting to be taken seriously. Other notable people who have spoken and written about the potential dangers of advanced artiﬁcial intelligence in recent years include Bill Joy (co-founder of Sun Microsystems), Nick Bostrom (director of the Future of Humanity Institute at Oxford), and Sir Martin Rees (professor at Cambridge and president of the Royal Society). Hopefully this trend will continue.  

At the  SIAI  itself the principle research fellow is Eliezer Yudkowsky. He has written a number of documents that deal mostly with safety and ethical issues surrounding the development of powerful artiﬁcial intelligence, as well as ideas on how he thinks so called ‘Friendly AI’ should be developed. Although these can be accessed through the  SIAI  website, none of his writings have yet appeared in mainstream peer reviewed journals. As  AIXI  is currently the only comprehensive mathematical theory of machine super intelligence,  SIAI  follows this work with interest and lists (Hutter, 2007b) among their core readings. PhD candidate Nick Hay, who is associated with  SIAI , is examining whether AIXI  theory can be used to study the safety of intelligent machines. Hopefully the gentle introduction to  AIXI  in Chapter 2 will encourage more people to explore some of these research directions.  

In 1887 Lord Acton famously wrote, “Power tends to corrupt, and absolute power corrupts absolutely.” It is not that power itself is inherently good or evil, rather it grants the ability to be so: power ampliﬁes intention. Although Acton’s quote has a ring of truth to it, perhaps it is excessively pessimistic about human nature. In any case, if there is ever to be something approaching absolute power, a super intelligent machine would come close. By deﬁnition, it would be capable of achieving a vast range of goals in a wide range of environments. If we carefully prepare for this possibility in advance, not only might we avert disaster, we might bring about an age of prosperity unlike anything seen before.  

![](images/4ef650f465f3fec94c7eef941195b55cec47102d86796e1219d77e5c04970223.jpg)  

# A. Notation and Conventions  

When deﬁning a symbol or equation we use :  $,-$   to stress that the item on the left is newly deﬁned. When we just need to assert that two things are equal we use the plain   $-$   symbol. Not equal is    $\neq$  , and approximately equal    $\approx$  . By  $x\gg y$   we mean that    $x$   is much greater than    $y$  , and similarly for  $\ll$  .  

The  cardinality  of a set    $S$   is written    $|S|$  . The  empty set  is written    $\emptyset$  ,  subset with the possbility of equality    $\subseteq$  , and  proper subset    $\subset$  . The  natural numbers are denoted    $\mathbb{N}:=\{1,2,.\,.\,.\}$  , naturals with 0 included    $\mathbb{N}_{0}\,:=\,\{0,1,2,3,\ldots\}$  , the  integers    $\mathbb{Z}:=\{.\,.\,.\,-\,2,-1,0,1,2,.\,.\,.\}$  , the  rational numbers    $\mathbb{Q}:=\;\{\frac{n}{m}\;:$   $n,m\in\mathbb{Z}\}$  , and the  real numbers  R . We use standard notation for intervals on the real line. Speciﬁcally, we deﬁne   $[x,y]\,:=\,\{z\,\in\,\mathbb{R}\,:\,x\,\leq\,z\,\leq\,y\}$  , and  $(x,y)\;:=\;\{z\;\in\;\mathbb{R}\;:\;x\;<\;z\;<\;y\}$  . Intervals such as   $(0,1]$   have the obvious meaning.  

$\log_{a}x$   is the  logarithm  of    $x$   base    $a$  .   $\ln x:=\log_{e}x$   where    $e\,=\,2.71828\ldots$  When the speciﬁc base used makes no diﬀerence we simply write   $\log x$  . The al  of    $n$  , written    $n!$  , is deﬁned   $0!=1$   and    $n!:=n(n-1)(n-2)\cdot\cdot\cdot1$   for  $n\in\mathbb{N}$   ∈ N . The  binomial  is written  $\textstyle{{\binom{n}{r}}:={\frac{n!}{(n-r)!r!}}}$   . Although 0  $0^{0}$   is technically − indeterminate, in derivations we follow the standard convention and take its value to be 1. The  Kronecker delta symbol    $\delta_{a b}$   is deﬁned to be   $1$   if    $a=b$  , and 0 otherwise.  

An  alphabet  is a ﬁnite set of elements which are called  symbols . For example,  $\{a,b,c,.\,.\,.\,,z\}$   is an alphabet, as is  $\{{\mathfrak{u p}},{\mathfrak{d o w n}},{\mathfrak{l e f t}},{\mathfrak{r i g h t}}\}$  . Mostly we use the binary alphabet    $\mathbb{B}:=\left\{0,1\right\}$  , in which context the symbols are known as  bits . A binary  string  is a ﬁnite ordered  $n$  -tuple of bits. This is denoted    $x:=x_{1}x_{2}\cdot\cdot\cdot x_{n}$  where    $\forall i\in\{1,.\,.\,.\,,n\}:x_{i}\,\in\,\mathbb{B}$  , or more succinctly,    $\boldsymbol{x}\,\in\,\mathbb{B}^{n}$  . The 0-tuple is denoted  ǫ  and is called the  null string . The expression  $\mathbb{B}^{\leq n}$    represents the set of bina h  $n$   or  ss, and    $\textstyle\mathbb{B}^{*}:=\bigcup_{n\in\mathbb{N}}\mathbb{B}^{n}$   is the set of all binary ∈ strings. A  substring  of a string  x  is itself a string deﬁned    $x_{j:k}:=x_{j}x_{j+1}\cdot\cdot\cdot x_{k}$  where 1  $1\;\leq\;j\;\leq\;k\;\leq\;n$   ≤ Concatenation is indicated by juxtaposition, for example, if  x  $x=x_{1}x_{2}\in\mathbb{B}^{2}$   ∈   and    $y\,=\,y_{1}y_{2}y_{3}\,\in\,\mathbb{B}^{3}$  , then    $x y=x_{1}x_{2}y_{1}y_{2}y_{3}$  some cases we will also concatenate constants, for example if  x  $\boldsymbol{x}\in\mathbb{B}^{\ast}$   ∈ ∗ then  $x101$  is the string    $x$   with 101 added on the end. By    $\ell(x)$   we mean the length of the string    $x$  , for example, if    $x\in\mathbb{B}^{n}$    then    $\ell(x x)=2\ell(x)=2n$  , and for    $k<n$   we have    $\ell(x_{j:k})=k-j+1$  .  

Unlike strings which always have ﬁnite length, a binary  sequence    $\omega$   is an inﬁnite list of bits, for example    $\omega:=x_{1}x_{2}x_{3}\ldots\in\mathbb{B}^{\infty}$  . For a sequence    $\omega\in\mathbb{B}^{\infty}$  we might be interested in the prediction of the   $(t\!+\!1)^{t h}$    bit, denoted    $\omega_{t+1}$  , given that we have so far observed only the ﬁnite initial string    $\omega_{1:t}\in\mathbb{B}^{t}$  . Obviously a string cannot be concatenated onto the end of a sequence as a sequence has no end, however a sequence can be concateded onto the end of a string.  

Our notation and usage of measure theory in this thesis is non-standard as it makes working with strings and sequences easier. Although the usage is explained in the text, for the mathematically inclined the relationship to standard measure theory is as follows: For a given sample space   $\Omega$  , a  probability measure    $\nu$   is a type of   $[0,1]$   valued function deﬁned over a set of subsets of   $\Omega$  , known as a    $\sigma$  - algebra . For prediction we need to measure the probability of sets of sequences that begin with a given string, thus we need a    $\sigma$  -algebra that contains these sets, the so called  cylinder sets  deﬁned,   $\Gamma_{x}:=\{x\omega:\omega\in\mathbb{B}^{\infty}\}$  for    $\boldsymbol{x}\,\in\,\mathbb{B}^{\ast}$  . Such a    $\sigma$  -algebra can easily be constructed by considering the smallest    $\sigma$  -algebra that contains the cylinder sets. In this thesis we do not need to be able to measure arbitrary sets within this    $\sigma$  -algebra and thus we adopt a simpliﬁed notation for measures by deﬁning    $\nu(\underline{{x}})$   to be shorthand for  $\nu(\Gamma_{x})$  . In other words,  $\nu(\underline{{x}})$   is the probability that a binary sequence sampled according to the distribution    $\nu$   begins with the string    $\boldsymbol{x}\in\mathbb{B}^{\ast}$  . This shorthand does not get us into trouble as it can be proven that measures deﬁned on the set    ${\mathbb{B}}^{*}$  correspond uniquely to measures deﬁned on the full    $\sigma$  -algebra (Calude, 2002). As we are often interested in the probability that a string    $\boldsymbol{x}\in\mathbb{B}^{\ast}$  follows a string    $y\,\in\,\mathbb{B}^{\ast}$  in a sequence sampled according to some distribution    $\mu$  , we further deﬁne the shorthand notation    $\mu(y\underline{{x}}):=\mu(y\underline{{x}})/\mu(\underline{{y}})$  .  

Probability distributions, measures and semi-measures are usually denoted by a lowercase greek letter, for example,    $\mu,\nu,\varrho$  . For an unnamed probabilty distribution over a random variable    $X$  , we write    $P(X)$  .    $\mathbf{E}_{\mu}(X)$   is the expected value of    $X$   with respect to   . When    is the true distribution we can omit  $\mu$   $\mu$  this from the notation for expectations.  

Throughout the thesis we refer to an  agent , usually denoted by the condi- tional measure    $\pi$  , that is interacting with some kind of an  environment , usually denoted by the conditional measure    $\mu$  . This interaction occurs by having  ac- tion  symbols from the alphabet    $\mathcal{A}$   being se by the agent to the environment, and  perception  symbols from the alphabet  X  being sent back in the ot r di- rection. Each perception co ists of an  observation  from the alphabet  O  and a  reward  from the alphabet  R . When referring to the full measure deﬁned by  $\pi$   interacting with    $\mu$   we use the symbol    $\underset{\mu}{\pi}$  .  

To denote symbols being sent we use the lower case variable names    $a$  ,    $o$  ,  $r$   and    $x$   for actions, observations, rewards and perceptions respectively. We index these in the order in which they occur, thus    $a_{1}$   is the agent’s ﬁrst action,  $a_{2}$   is the second action and so on. The agent and the environment take turns at sending symbols, starting with the agent. This produces a history of actions, observations and rewards which can be written,    $a_{1}o_{1}r_{1}a_{2}o_{2}r_{2}a_{3}o_{3}r_{3}\ldots$  . As we refer to interaction histories a lot, we need to be able to represent these compactly. One trick that we use is to squeeze symbols together and then index them as blocks of symbols. Thus for the complete interaction history up to and including cycle    $t$  , we can write    $a x_{1:t}:=a_{1}x_{1}a_{2}x_{2}a_{3}\,.\,.\,.\,a_{t}x_{t}$  . For the history before cycle    $t$   we use    $a x_{<t}:=a x_{1:t-1}$  . Note that    $x_{t}:=o_{t}r_{t}$  .  

Some of our results will have the property of holding within an additive constant that is independent of the variables in the expression. We indicate this by placing a small plus above the equality or inequality symbol. For example,    $f\stackrel{+}{<}g$   means that    $\exists c\in\mathbb{R},\forall x:f(x)<g(x)+c$  . If    $f\,\stackrel{+}{<}\,g\,\stackrel{+}{<}\,f$  , we write    $f\,\stackrel{+}{=}\,g$  . When using standard “big O” notation this is superﬂuous as expressions are already understood to hold within an independent constant, however we will sometimes still use it for consistency of notation. Similarly, we deﬁne    $f\stackrel{\times}{\leq}g$   to mean that    $\exists c\in\mathbb{R},\forall x:f(x)\leq c\cdot g(x)$  , and if    $f\stackrel{\times}{\leq}g\stackrel{\times}{\leq}f$   ≤  we write    $f\stackrel{\times}{=}g$  .  

![](images/add1d0c546baca9f240873cc72d9047cb51b1ee9b8b256befc46e059667d9e50.jpg)  

# B. Ergodic MDPs admit self-optimising agents  

A number of results similar to Theorem 3.4.5 exist, such as the proof that with probability 1 the Q-learning algorithm converges to optimal in an ergodic  MDP environment (Watkins and Dayan, 1992). Unfortunately, in order to establish the necessary chain of results we need something a little diﬀerent: we require the convergence to hold for any history and with an eﬀective horizon that goes to inﬁnity. For a precise statement of the theorem, necessary technical conditions and proof, see Theorem 5.38 in (Hutter, 2005). Although Hutter’s proof is straight forward, it assumes a certain continuity condition on value functions based on estimated transition probabilities for ergodic  MDP s. This is left as a (large!) exercise for the reader (Problem 5.12 in Hutter, 2005). In this appendix we follow the plan of attack suggested in Problem 5.12 to prove the missing continuity result, namely, Theorem B.4.3 that appears at the end of this section.  

# B.1. Basic deﬁnitions  

For deﬁnitions of agents, environments, ergodic and many other things used, see Chapters 2 and 3. Rather than just talking about agents, as we do in the rest of this thesis, here we will use the slightly more reﬁned notion of a  policy . Essentially, a policy is some rule or mode of operation that an agent has. For example, when navigating a maze an agent’s policy might be to always turn left at each intersection. The distinction is useful when we wish to consider agents which have multiple diﬀerent modes of operation. That is, an agent which follows some policy for a while and then, perhaps due to certain conditions such as a lack of success, switches to a diﬀerent policy.  

The proofs in this section will make extensive use of results from linear algebra. The mathematical notation used is fairly standard: we will represent real valued matrices with capital etters, f example    $A\in\mathbb{R}^{n\times m}$  . By    $A_{i j}$   we mean the single scalar element of  A  on the  i  $i^{t h}$    row and    $j^{t h}$    column. By    $A_{*j}$   we mean the    $j^{t h}$    column of    $A$   and similarly for    $A_{i*}$  . We represent vectors with a bold lowercase var le, for example    $\mathbf{a}\in\mathbb{R}^{n}$  . Similar to the case for matrices, by    $\mathbf{a}_{i}$   we mean the  i  $i^{t h}$    element of    $\mathbf{a}$  . In some situations a matrix or vector may already have other indexes, in this case we place square brackets around it and then index so as to avoid confusion. For example,   $[\mathbf{b}_{\pi}]_{i}$   is the    $i^{t h}$    element of the vector    ${\bf b}_{\pi}$  . We represent the classical adjoint of a matrix    $A$   by adj  $(A)$   and the determinant by   $\operatorname*{det}(A)$  .  

In order to express the Markov chains as matrices, we need to be able to index the actions and perceptions with natural numbers. This could be achieved by a simple numbering scheme; here we will just assume that this has already been done. That is, without loss of generality we assume that  $\mathcal{X}:=\{1,\dots,n_{1}\}$   and    $\mathcal{A}:=\{1,\cdot\,\cdot\,,n_{2}\}$   for    $n_{1},n_{2}\in\mathbb{N}$  . The diﬃ ulty with this is that each perception  x  is still associated with an observation  o  and a reward  $r$  . To recover the r ard a ociated with    $x$   we write    $r(x)\in{\mathcal{R}}$  , and similarly for    $o(x)\in\mathcal{O}$  . Both  and  are ﬁnite, as always, but otherwise unspeciﬁed.  

Let    $\pi$   be a stationary policy such that  $\forall a x_{<k}a_{k}:\pi(a x_{<k}\underline{{a}}_{k})=\pi(o(x)_{k-1}\underline{{a}}_{k})$  . That is, under the policy  $\pi$   the distribution of actions depends on only the last observation in a way that is independent of    $k$  . It follows that the equation for the    $k^{t h}$    perception    $x_{k}$   given history    $\scriptstyle a x_{<k}$   is,  

$$
\pi(a x_{<k}\underline{{a}}_{k})\,\mu(a x_{<k}a\underline{{x}}_{k})\,=\,\pi(o(x_{k-1})\underline{{a}}_{k})\,\mu(o(x_{k-1})a\underline{{x}}_{k}).
$$  

Thus, for a given    $\mu$   and    $\pi$   the next perception    $x_{k}$   depends on only the previous observation    $o(x)_{k-1}$   in a way that is independent of    $k$  , that is,  and    together  $\pi$   $\mu$  form a stationary Markov chain. Given that everything is stationary, we can drop the index    $k$   and write    $x$  ,    $a$   and    $x^{\prime}$    for the perception, action and the following perception.  

From the deﬁnition of an  MDP  it is clear that we can represent a (stationary) MDP  as a three dimensional Cartesian tensor    $D\in\mathbb{R}^{n_{1}\times n_{2}\times n_{1}}$    deﬁned  $\forall x,a,x^{\prime}$  ,  

$$
D_{x a x^{\prime}}\,:=\,\mu(o(x)a\underline{{x}}^{\prime}).
$$  

Note that the only part of    $x$   that plays a part in deﬁning the structure of    $D$   is the associated observation    $o(x)$  , as required by our deﬁnition of an  MDP . The reward associated with    $x$  , that is    $r(x)$  , has no role.  

We can now express the interaction of the policy and the environment as a square stochastic matrix    $T\in\mathbb{R}^{n_{1}\times n_{1}}$    deﬁned,  

$$
T_{x x^{\prime}}\,:=\,\sum_{a\in\mathcal{A}}\pi(o(x)\underline{{{a}}})\,\mu(o(x)a\underline{{{x}}}^{\prime})\,=\,\sum_{a\in\mathcal{A}}\pi(o(x)\underline{{{a}}})D_{x a x^{\prime}}.
$$  

It should be noted that this character is ation of the interaction between    $\mu$  and    as a stochastic matrix    $T$   is only possible if    is a stationary  MDP  and  $\pi$   $\mu$   $\pi$   is a stationary policy. Fortunately this is all we will need for optimality, though we will brieﬂy have to consider non-stationary policies in Section B.3 in order to prove this. It is worth keeping in mind that when we see a matrix    $T$  this represents the complete system of an agent and environment interacting, rather than just an environment. That is, it represents the interaction measure  $^{70}$  .  

Deﬁ e a matrix    $R_{x a}\,\in\,\mathbb{R}^{n_{1}\times n_{2}}$    to be the expected rewar hoosing action  a  after perception  x . Further deﬁne the column vector  $\mathbf{r}_{\pi}\in\mathbb{R}^{n_{1}}$    where,  

$$
[\mathbf{r}_{\pi}]_{x}\,:=\,\mathbf{E}(R_{x a}|x)\,=\,\sum_{a\in\mathcal{A}}\pi(x\underline{{a}})R_{x a}.
$$  

The advantage of expressing everything in matrix notation is that the full range of linear algebra techniques is now easy to work with. For example, the probability of transiting between any two perceptions can be easily computed by taking powers of    $T$  : if we have a perception    $i$   then the probability that exactly    $m$   cycles later the perception will be    $j$  , is given by   $[T^{m}]_{i j}$  .  

B.1.1 Deﬁnition. For an environment    $\mu$   and a policy    $\pi$   the  expected average value  in cycles    $k$   to    given history    is deﬁned to be,  $_m$   $\ a x_{<k}$  

$$
V_{k m}^{\pi\mu}(a x_{<k})\,:=\,\frac{1}{m}\sum_{a x_{k:m}}\mathbf{\Psi}_{\mu}^{\pi}(a x_{<k}\underline{{a}}\underline{{x}}_{k:m})\sum_{i=k}^{m}r(x_{i}).
$$  

Additionally we deﬁne the  expected long run average value  to be,  

$$
V_{k\infty}^{\pi\mu}(a x_{<k}):=\operatorname*{lim}_{m\to\infty}V_{k m}^{\pi\mu}(a x_{<k}),
$$  

when this limit exists.  

When    $k\,=\,1$   there is no history, that is,   , the null string. In this  $a x_{<k}\,=\,\epsilon$  case we simplify the notation slightly by deﬁning    $V_{1m}^{\pi\mu}:=V_{1m}^{\pi\mu}(\epsilon)$  ). Similarly for    $V_{1\infty}^{\pi\mu}$  .  

In matrix notation we can express the expected long run average value for each initial perception    $x_{1}\in\mathcal{X}=\{1,\ldots,n_{1}\}$   as a vector of value functions,  

$$
{\bf V}_{1\infty}^{\pi\mu}\,=\,\left(\begin{array}{c}{{V_{1\infty}^{\pi\mu}(1)}}\\ {{\vdots}}\\ {{V_{1\infty}^{\pi\mu}(n_{1})}}\end{array}\right)=\left(\operatorname*{lim}_{m\rightarrow\infty}\frac{1}{m}\sum_{k=0}^{m-1}T^{k}\right){\bf r}_{\pi},
$$  

if the limit exists.  

B.1.2 Deﬁnition. For an environment    the  optimal policy , denoted    $\pi^{\mu}$  ,  $\mu$  is deﬁned as:  

$$
\pi^{\mu}\,:=\,\arg\operatorname*{max}_{\pi}V_{1\infty}^{\pi\mu},
$$  

where the maximum is taken over all policies, including non-stationary ones.  

In some sense the optimal policy is the ideal policy. However, the optimal policy is usually only optimal with respect to the speciﬁc environment for which it was deﬁned. If we do not know the speciﬁc details of the environment that the policy will face in advance, the best we can do is to have a policy which will adapt to the environment based on experience. In such a situation the policy is unlikely to be optimal as it will probably make some non-optimal actions as it learns about the environment it faces. In this situation the following concept is useful:  

B.1.3 Deﬁnition. We say that a policy  $\pi$   is  self-optimising  in an environ- ment  $\mu$   if its expected average value converges to the optimal expected average value as    $m\to\infty$  , that is,  

$$
V_{1m}^{\pi\mu}~\longrightarrow~V_{1m}^{\pi^{\mu}\mu}.
$$  

Intuitively this means that the expected performance of the policy in the long run is as good as an optimal policy which was designed with complete knowledge of the environment in advance. Classes of environments which admit self-optimising policies are important because they are environments in which it is possible for general purpose policies to adapt their behaviour until eventually their actions become optimal.  

# B.2. Analysis of stationary Markov chains  

In this section we will establish some of the properties of Markov chains that we will require. Our ﬁrst lemma shows that the key term   $(I-\alpha T)^{-1}$    can be expanded using a Taylor series. The proof of this lemma and the following lemma and theorem are based on the proof of Proposition 1.1 from Section 4.1 of (Bertsekas, 1995).  

B.2.1 Lemma.  For a s atrix    $T\in\mathbb{R}^{n\times n}$  and scalar    $\alpha\in(0,1)$   there exist stochastic matrices  $T^{*}\in\mathbb{R}^{n\times n}$      and  $H\in\mathbb{R}^{n\times n}$    such that  

$$
(I-\alpha T)^{-1}=(1-\alpha)^{-1}T^{*}+H+O(|1-\alpha|)
$$  

where    $\begin{array}{r}{\operatorname*{lim}_{\alpha\to1}O(|1-\alpha|)=0}\end{array}$  .  

Proof. Deﬁne the    $n\times n$   matrix,  

$$
M(\alpha):=(1-\alpha)(I-\alpha T)^{-1}.
$$  

Applying the matrix inversion formula we see that  

$$
M(\alpha)=(1-\alpha)\frac{\mathrm{adj}(I-\alpha T)}{\mathrm{det}(I-\alpha T)},
$$  

where the determinant   $\operatorname*{det}(I-\alpha T)$   is an    $n^{t h}$    order polynomial in    $\alpha$   and the classical adjoint a  $\mathrm{dj}(I-\alpha T)$   is an    $n\times n$   matrix of    $n{-}1^{t h}$    order polynomials in  $\alpha$  . Therefore    $M(\alpha)$   can be expressed as an    $n\times n$   matrix where each element is either zero or a fraction of two polynomials in  $\alpha$   that have no common factors.  

We know that the denominator polynomials of    $M(\alpha)$   cannot have 1 as a s this would imply that the corresponding element of    $M(\alpha)\,\rightarrow\,\infty$  as  $\alpha\rightarrow1$   → 1. This cannot happen because,  

$$
(1-\alpha)^{-1}M(\alpha)\mathbf{r}_{\pi}=(I-\alpha T)^{-1}\mathbf{r}_{\pi}
$$  

where  ∀  $\forall i\in\left\{1,\dots,n\right\}:\left|\left[(I-\alpha T)^{-1}\mathbf{r}_{\pi}\right]_{i}\right|\leq(1-\alpha)^{-1}\operatorname*{max}_{k}\left|[\mathbf{r}_{\pi}]_{k}\right|$    . Clearly then the absolute value of the elements of  $M(\alpha)\mathbf{r}_{\pi}$   are bounded by max  $\operatorname*{max}_{k}\left|[\mathbf{r}_{\pi}]_{k}\right|$  for    $\alpha<1$  . Therefore we can express the  $i j^{t h}$    element of  $M(\alpha)$  ) as,  

$$
M_{i j}(\alpha)=\frac{\gamma(\alpha-\phi_{1})\cdot\cdot\cdot(\alpha-\phi_{p})}{(\alpha-\psi_{1})\cdot\cdot\cdot(\alpha-\psi_{q})}
$$  

where    $\gamma,\phi_{i},\psi_{j}\in\mathbb{R}$   for all    $i\in\{1,.\cdot\cdot p\}$   and    $j\in\{1,\ldots q\}$  .  

Using this expression we can take a Taylor expansion of    $M(\alpha)$   about   $1$   as follows. Firstly, deﬁne the matrix    $T^{*}\in\mathbb{R}^{n\times n}$    as,  

$$
T^{*}:=\operatorname*{lim}_{\alpha\to1}M(\alpha)
$$  

and the matrix    $H\in\mathbb{R}^{n\times n}$    as  

$$
H_{i j}:=\left.-\frac{\partial}{\partial\alpha}M_{i j}(\alpha)\right]\Bigr|_{\alpha=1}\,.
$$  

That is,    $H$   is a matrix having a  $i j^{t h}$    element the ﬁrst derivative of  $-M_{i j}(\alpha)$  with respect to    $\alpha$   evaluated at  α  = 1.  

From the equation for a ﬁrst order Taylor expansion,  

$$
M(\alpha)=T^{*}+(1-\alpha)H+O((1-\alpha)^{2})
$$  

where    $O((1-\alpha)^{2})$   is an    $\alpha$  -dependent matrix such that  

$$
\operatorname*{lim}_{\alpha\to1}{\frac{O((1-\alpha)^{2})}{1-\alpha}}=0.
$$  

Dividing through by   $(1-\alpha)$   we get  

$$
(1-\alpha)^{-1}M(\alpha)=(1-\alpha)^{-1}T^{*}+H+O(|1-\alpha|)
$$  

where   $\begin{array}{r}{\operatorname*{lim}_{\alpha\to1}O(|1-\alpha|)=0}\end{array}$  . The result then follows as   $(I-\alpha T)^{-1}=(1-$   $\alpha)^{-1}M(\alpha)$   by deﬁnition. 2  

We will soon show that    $T^{*}$  as deﬁned above plays a signiﬁcant role in the analysis. Before looking at this more closely, we will ﬁrstly prove some useful identities.  

B.2.2 Lemma.  It follows from the deﬁnitions of    $T^{*}$  and    $M(\alpha)$   that,  

$$
T^{*}=T^{*}T=T T^{*}=T^{*}T^{*}
$$  

and for  k N  

$$
(T-T^{*})^{k}=T^{k}-T^{*}.
$$  

Proof. By subtracting the identity    $\alpha I\,=\,\alpha(I\,-\,\alpha T)(I\,-\,\alpha T)^{-1}$    from the identity    $I=(I-\alpha T)(I-\alpha T)^{-1}$    we see that,  

$$
(1-\alpha)I=(I-\alpha T)(1-\alpha)(I-\alpha T)^{-1}
$$  

and thus,  

$$
\alpha T(1-\alpha)(I-\alpha T)^{-1}=(1-\alpha)(I-\alpha T)^{-1}+(\alpha-1)I.
$$  

Taking    $\alpha\rightarrow1$   gives,  

$$
\operatorname*{lim}_{\alpha\to1}\alpha T\cdot\operatorname*{lim}_{\alpha\to1}(1-\alpha)(I-\alpha T)^{-1}=\operatorname*{lim}_{\alpha\to1}(1-\alpha)(I-\alpha T)^{-1}+\operatorname*{lim}_{\alpha\to1}(\alpha-1)I
$$  

which, using the deﬁnition of    $M(\alpha)$  , becomes,  

$$
T\cdot\operatorname*{lim}_{\alpha\rightarrow1}M(\alpha)=\operatorname*{lim}_{\alpha\rightarrow1}M(\alpha).
$$  

Finally using the deﬁnition of    $T^{*}$  this reduces to just,  

$$
T T^{*}=T^{*}.
$$  

Using essentially the same argument it can also be shown that    $T^{*}T=T^{*}$  . It then immediately fol  $\forall k\in\mathbb{N}:T^{k}T^{*}=T^{*}T^{k}=T^{*}$  .  

From the relation  TT  $T T^{*}=T^{*}$    ∗   ∗ it follows that    $T^{*}-\alpha T T^{*}=T^{*}-\alpha T^{*}$  and so  $(I-\alpha T)T^{*}=(1-\alpha)T^{*}$  and thus,  

$$
T^{*}=(1-\alpha)(I-\alpha T)^{-1}T^{*}.
$$  

Taking    $\alpha\to\infty$  gives,  

$$
\operatorname*{lim}_{\alpha\to1}T^{*}=\operatorname*{lim}_{\alpha\to1}(1-\alpha)(I-\alpha T)^{-1}\cdot\operatorname*{lim}_{\alpha\to1}T^{*},
$$  

which by the deﬁnition of    $T^{*}$  is just,  

$$
T^{*}=T^{*}T^{*}.
$$  

This establishes the ﬁrst result.  

The second result will be proven by induction. Trivially   $(T\!-\!T^{*})^{1}=T^{1}\!-\!T^{*}$  which establishes the case    $k=1$  . Now assume that the induction hypothesis holds for the    $k^{t h}$    case and consider the   $(k+1)^{t h}$    case:  

$$
\begin{array}{r c l}{{(T-T^{*})^{k+1}}}&{{=}}&{{(T-T^{*})^{k}(T-T^{*})}}\\ {{}}&{{=}}&{{(T^{k}-T^{*})(T-T^{*})}}\\ {{}}&{{=}}&{{T^{k+1}-T^{k}T^{*}-T^{*}T^{k}+T^{*}T^{*}}}\\ {{}}&{{=}}&{{T^{k+1}-T^{*}.}}\end{array}
$$  

The second line follows from the induction assumption and the ﬁnal line from the results above. 2  

We will use these simple relations frequently in the proofs that follow without further comment. Now we can prove a key result about the structure of    $T^{*}$  : it is the limiting average distribution for the matrix    $T$  .  

B.2.3 Theorem.  For a stochastic matrix    $T\in\mathbb{R}^{n\times n}$    and  $\forall m\in\mathbb{N}$  ,  

$$
T^{*}=\frac{1}{m}\sum_{k=0}^{m-1}T^{k}+\frac{1}{m}(T^{m}-I)H.
$$  

where    $H\in\mathbb{R}^{n\times n}$    is the matrix that satisﬁes Lemma B.2.1.  

Proof. As    $T$   is a stochastic matrix, from Lemma B.2.1 we see that there exist matrices    $H\in\mathbb{R}^{n\times n}$    and    $T^{*}\in\mathbb{R}^{n\times n}$    such that,  

$$
H=(I-\alpha T)^{-1}-(1-\alpha)^{-1}T^{*}-O(|1-\alpha|)
$$  

where    $\alpha\in(0,1)$   and   $\begin{array}{r}{\operatorname*{lim}_{\alpha\to1}O(|1-\alpha|)=0}\end{array}$  . However from the geometric series equations it follows that,  

$$
\begin{array}{l c l}{\displaystyle(I-\alpha T)^{-1}-(1-\alpha)^{-1}T^{*}}&{=}&{\displaystyle\sum_{k=0}^{\infty}\alpha^{k}T^{k}-T^{*}\sum_{k=0}^{\infty}\alpha^{k}=\sum_{k=0}^{\infty}\alpha^{k}(T^{k}-T^{*})}\\ &&{=}&{\displaystyle I-T^{*}+\sum_{k=1}^{\infty}\left(\alpha(T-T^{*})\right)^{k}}\\ &&{=}&{\displaystyle I-T^{*}+\frac{\alpha(T-T^{*})}{I-\alpha(T-T^{*})}}\\ &&{=}&{\displaystyle(I-\alpha(T-T^{*}))^{-1}-T^{*}.}\end{array}
$$  

Substituting this result into Equation (B.4) and taking    $\alpha\rightarrow1$  ,  

$$
\begin{array}{r c l}{{{\cal H}}}&{{=}}&{{\displaystyle\operatorname*{lim}_{\alpha\rightarrow1}\left[(I-\alpha(T-T^{*}))^{-1}-T^{*}-{\cal O}(|1-\alpha|)\right]}}\\ {{}}&{{=}}&{{\displaystyle(I-T+T^{*})^{-1}-T^{*}.}}\end{array}
$$  

Multiplying by   $\left(I-T+T^{*}\right)$   and then    $T^{*}$  we see that,  

$$
\begin{array}{r c l}{{(I-T+T^{*})H}}&{{=}}&{{I-(I-T+T^{*})T^{*}}}\\ {{H-T H-T^{*}H}}&{{=}}&{{I-T^{*}+T T^{*}-T^{*}T^{*}=I-T^{*}}}\\ {{T^{*}H-T^{*}H-T^{*}H}}&{{=}}&{{T^{*}-T^{*}}}\\ {{T^{*}H}}&{{=}}&{{0.}}\end{array}
$$  

It now also follow hat  $H-T H=I-T^{*}$  and so    $T^{*}+H=I+T H$  . Multiplying by  T on the left for    $k\in\ensuremath{\mathbb{N}}_{0}$   now gives,  

$$
T^{*}+T^{k}H=T^{k}+T^{k+1}H.
$$  

Summing over    $k\,=\,0,1,.\,.\,.\,,m-1$   and cancelling equal terms and dividing through by    $m$   produces,  

$$
\begin{array}{r c l}{m T^{*}+\displaystyle\sum_{k=0}^{m-1}T^{k}H}&{=}&{\displaystyle\sum_{k=0}^{m-1}T^{k}+\displaystyle\sum_{k=0}^{m-1}T^{k+1}H}\\ {m T^{*}+H}&{=}&{\displaystyle\sum_{k=0}^{m-1}T^{k}+T^{m}H}\end{array}
$$  

from which the result follows as    $m\neq0$  .  

This establishes bounds on the convergence of  $\textstyle{\frac{1}{m}}\sum_{k=0}^{m-1}T^{k}$  P  to    $T^{*}$  that we will need. As    $H$   is bounded, simply taking    $m\to\infty$  yields the following:  

B.2.4 Corollary.  For a stochastic matrix    $T\in\mathbb{R}^{n\times n}$  ,  

$$
T^{*}=\operatorname*{lim}_{m\to\infty}\frac{1}{m}\sum_{k=0}^{m-1}T^{k}.
$$  

By applying this result to Equation (B.2) we can now express the expected long run average value very simply in terms of    $T^{*}$  ,  

$$
\mathbf{V}_{1\infty}^{\pi\mu}:=\left(\operatorname*{lim}_{m\to\infty}\frac{1}{m}\sum_{k=0}^{m-1}T^{k}\right)\mathbf{r}_{\pi}=T^{*}\mathbf{r}_{\pi}.
$$  

Thus by the existence of    $T^{*}$  we can infer that the expected long run average value also exists in this case.  

B.2.5 Corollary.  Let    $\mu$   be a stationary  MDP  environment and    $\pi$   a stationary policy.  $\forall m\in\mathbb{N}$  ,  

$$
|V_{1\infty}^{\pi\mu}-V_{1m}^{\pi\mu}|=O\left(\frac{1}{m}\right).
$$  

Proof. Let    $T\in\mathbb{R}^{n\times n}$    represent the Markov rmed by the interaction of    and   . From Theorem B.2.3 we see that  $\forall m\in\mathbb{N}$  ,  $\mu$   $\pi$  

$$
T^{*}-\frac{1}{m}\sum_{k=0}^{m-1}T^{k}=\frac{1}{m}(T^{m}-I)H.
$$  

Multiplying by    $\mathbf{r}_{\pi}$   on the right gives  

$$
T^{*}\mathbf{r}_{\pi}-\left({\frac{1}{m}}\sum_{k=0}^{m-1}T^{k}\right)\mathbf{r}_{\pi}={\frac{1}{m}}\left(T^{m}-I\right)H\mathbf{r}_{\pi},
$$  

and thus the result follows as the elements of both    $T^{m}$    and    $H$   are bounded.  2  

Of course this result is not surprising as we would expect the expected average value to converge to its limit in a reasonable way when both the environment and policy are stationary.  

Finally let us note some technical results on the relationship between    $T$   and  $T^{*}$  .  

B. 6 Lemma.  For an ergodic stochastic matrix    $T\in\mathbb{R}^{n\times n}$    the ow vectors of  T  $T^{*}$    ∗ are all the same and deﬁne a stationary distribution under  T .  

This is a standard result in the theory of ergodic Markov chains. See for ex- ample Chapter V of (Doob, 1953) or any book on discrete stochastic processes for a proof.  

The following result shows that the limiting matrix    $T^{*}$  is in some sense continuous with respect to small changes in  $T$  . This will be important because it means that if we have an estimate of    $T$   that converges in the limit then our estimate of    $T^{*}$  will also converge.  

B.2.7 Theorem. For an erg dic stochastic matrix    ${\cal T}\ \in\ \mathbb{R}^{n\times n}$  atrix  $T^{*}\;\in\;\mathbb{R}^{n\times n}$    is continuous in  T  in the following sense: If    ${\hat{T}}\,\in\,\mathbb{R}^{n\times n}$   ∈   is a stochastic matrix where    $\mathrm{max}_{i j}\left|T_{i j}-\hat{T}_{i j}\right|$   −  is small, then    $\exists c_{T}>0$   which depends on    $T$  , such that  $\begin{array}{r}{\operatorname*{max}_{i j}\left|T_{i j}^{*}-\hat{T}_{i j}^{*}\right|\leq c_{T}\operatorname*{max}_{i j}\left|T_{i j}-\hat{T}_{i j}\right|}\end{array}$      − .  

Proof. For an ergodic square matrix  T  $T\in\mathbb{R}^{n\times n}$   ∈   the row vectors o  $T^{*}$  l the same and correspond to the stationary distribution row vector  t  $\mathbf{t}^{*}\in\mathbb{R}^{1\times n}$  ∈ . That is,  

$$
T^{*}=\left(\begin{array}{c}{{\bf t^{*}}}\\ {{\bf\vdots}}\\ {{\bf t^{*}}}\end{array}\right)
$$  

wher  $\mathbf{t}^{\ast}T=\mathbf{t}^{\ast}$  and for all distrib ion vectors    $\mathbf{t}\in\mathbb{R}^{1\times n}$    we have    $\mathbf{t}T^{*}=\mathbf{t}^{*}$  . Thus  T  has an eigenvalue of 1 with  t  $\mathbf{t}^{*}$  ∗ being the corresponding left eigenvector.  

From linear algebra we know that  $\forall T\in\mathbb{R}^{n\times n}$  ,  

$$
\operatorname{adj}(I-T)\left(I-T\right)=\operatorname*{det}(I-T)I.
$$  

However as    $T$   has an eigenvalue of   $1$  ,   $\operatorname*{det}(I-T)=0$   and thus,  

$$
\operatorname{adj}(I-T)\,T=\operatorname{adj}(I-T),
$$  

or equivalently,    $\forall i\in\{1,\dots,n\}$  ,  

$$
[\mathrm{adj}(I-T)]_{i*}T=[\mathrm{adj}(I-T)]_{i*}.
$$  

Because    $\{\mathbf{t}^{*}\}$   is a basis for the eigenspace corresponding to the eigenvalue 1,  $[\mathrm{adj}(I-T)]_{i*}$  must be in this eigenspace. That is,    $\forall i\in\{1,\dots,n\}$  ,    $\exists c_{i}\in\mathbb{R}$  :  

$$
[\mathrm{adj}(I-T)]_{i*}=\left(\mathrm{cof}_{1i}(I-T),.\,.\,,\mathrm{cof}_{n i}(I-T)\right)=c_{i}\mathbf{t}^{*}
$$  

where the cofactor is deﬁned   $\mathrm{cof}_{j i}(I-T):=(-1)^{j+i}\operatorname*{det}(\mathrm{minor}_{j i}(I-T))$  .  

As    $T$   has an eigenvalue of   $1$   with geometric multiplicity   $1$   it follows that  $I-T$   has an eige value of 0 also with geometric multiplicity 1. Thus the nullity of    $I-T$   is 1 and so   $\operatorname{rank}(I-T)=n-1$  . While we deﬁne the rank of a matrix to be the dimension of its column or row space, it also can be deﬁned as the size of the largest non-zero minor and the two deﬁntions can be proven to be equivalent. As the adjoint is composed of order    $n-1$   minors it immediately follows that adj  $(I-T)\neq0$   and thus    $\exists k$  , which depends on    $T$  , such that    $c_{k}>0$  .  

As minor  $_{j k}(I{-}T)$   is an   $(n\!-\!1)\!\times\!(n\!-\!1)$   sub-matrix f   $(I{-}T)$   the determinant of this is an orde  $n{-}1$  olynomial in the elements of  T . Thus, by the continuity of polynom ls,  ∃  $\exists c^{\prime}>0$     0 such that f a suﬃciently small    $\varepsilon>0$   change in any element of  T  we will get at most a  c  $c^{\prime}\varepsilon$  ε  change in each   $\operatorname{cof}_{j k}(I-T)$  . However we know that    $\begin{array}{r}{\mathbf{t}^{*}=\frac{1}{c_{k}}[\mathrm{adj}(I-T)]_{k*}}\end{array}$  , and so an    $\varepsilon$   change in the elements of  $T$   results in at most a  $\frac{c^{\prime}}{c_{k}}\varepsilon$   change in the elements of    $\mathbf{t}^{*}$  and thus    $T^{*}$  . Deﬁne  $\textstyle c_{T}:={\frac{c^{\prime}}{c_{k}}}$    to indicate that this constant depends on    $T$   and we are done. 2  

# B.3. An optimal stationary policy  

We now turn our attention to optimal policies. While our analysis so far has only dealt with stationary policies, in general optimal policies need not be stationary. As non-stationary policies are more diﬃcult to analyse our preference is to deal with only stationary policies if possible. In this section we prove that for the class of ergodic ﬁnite stationary  MDP  environments an optimal policy can indeed be chosen so that it is stationary. This will simplify our analysis in later sections. However, in order to show this result we will need to brieﬂy consider policies which are potentially non-stationary. The proofs in this section follow those of Section 4.2 in (Bertsekas, 1995).  

Let us assume that the policy    $\pi$   is deterministic but not necessarily station- ary, that is,    $\pi\,:=\,\{\pi_{1},\pi_{2},.\,.\,.\}$  . Thus in the    $k^{t h}$    cycle we apply    $\pi_{k}$  . Deﬁne  $p_{i}(x)\;:=\;\arg\operatorname*{max}_{y\in\mathcal{A}}\pi_{i}(x\underline{{{a}}})$   to be the action chosen by policy    $\pi$   in cycle    $i$  . Clearly this is unique for deterministic    $\pi$  .  

In order to make some of the equations that follow more manageable we need to deﬁne the following two mappings. For any function    $f\,:\,\mathcal{X}\,\rightarrow\,\mathbb{R}$  etermi olicy    $\pi:=\{\pi_{1},\pi_{2},.\,.\,.\}$   we deﬁne the mapping    $B_{\pi_{k}}$   for any  $k\in\mathbb N$  N  to be  $\forall x\in\mathcal{X}$  ,  

$$
(B_{\pi_{k}}f)(x):=R_{x p_{k}(x)}+\sum_{x^{\prime}\in\mathcal{X}}\mu(\,x\,p_{k}(x)\,{\underline{{x}}}^{\prime}\,)f(x^{\prime}).
$$  

Of interest will be the policy that simply selects the action which maximises this expression in each cycle for any given    $x$  . For this we deﬁne for any function  $f:\mathcal{X}\to\mathbb{R}$   and    $\forall x\in\mathcal{X}$  ,  

$$
(B f)(x):=\operatorname*{max}_{a\in\mathcal{A}}\left[R_{x a}+\sum_{x^{\prime}\in\mathcal{X}}\mu(x a\underline{{x}}^{\prime})f(x^{\prime})\right].
$$  

Clearly this policy is stationary as the maximising    $a$   depends only on    $x$   and is independent of which cycle the system is in. By  $(B^{2}f)(x)$   we mean   $(B(B f))(x)$  and similarly higher powers such as   $(B^{i}f)(x)$   and   $(B_{\pi_{k}}^{i}f)(x)$  ). The equation  $B f=f$   is the well known Bellman equation (Bellman, 1957).  

An elementary property of the mappings    $B_{\pi_{k}}$   and    $B$   is their monotonicity in the following sense.  

B.3.1 Lemma.  For any    $f,f^{\prime}:\mathcal{X}\to\mathbb{R}$   such that    $\forall x\in\mathcal{X}:f(x)\leq f^{\prime}(x)$   and  possibly non-stationary deterministic policy    $\pi:=\{\pi_{1},\pi_{2},.\,.\,.\}$  , we have  $\forall x\in\mathcal{X}$  ,  $\forall i,k\in\mathbb{N}$  ,  

$$
(B_{\pi_{k}}^{i}f)(x)\leq(B_{\pi_{k}}^{i}f^{\prime})(x)
$$  

and  

$$
(B^{i}f)(x)\leq(B^{i}f^{\prime})(x).
$$  

Proof. Clearly the cases    $B^{1}$    and    $B_{\pi_{k}}^{1}$    are true from their deﬁnitions. A simple induction argument establishes the general result. 2  

Deﬁne the column vector    $\mathbf{e}:=\,(1,\.\,.\,,1)^{\mathrm{t}}\,\in\,\mathbb{R}^{n\times1}$  . Using these mappings we can now prove that the optimal policy can be chosen stationary if certain conditions hold.  

B.3.2 Theo    $\mu$   be a ﬁnite stationary  MDP  environment. If    $\lambda\in\mathbb{R}$   is a scalar and  $\mathbf{h}\in\mathbb{R}^{n\times1}$    a column vector such that  $\forall x\in\mathcal{X}$  ,  

$$
\lambda+[\mathbf{h}]_{x}=\operatorname*{max}_{a\in\mathcal{A}}\left[R_{x a}+\sum_{x^{\prime}\in\mathcal{X}}\mu(x a\underline{{x}}^{\prime})[\mathbf{h}]_{x^{\prime}}\right]
$$  

or equivalently,  

$$
\lambda\mathbf{e}+\mathbf{h}=B\mathbf{h},
$$  

then  

$$
\lambda=V_{1\infty}^{*\mu}:=\operatorname*{max}_{\pi}V_{1\infty}^{\pi\mu}.
$$  

Furthermore, if a stationary policy    $\pi^{\mu}$    attains the maximum in Equation   $(B.7)$  for each    $x$   then this policy is optimal, that is,    $V_{1\infty}^{\pi^{\mu}\mu}=\lambda$  .  

Proof. We have    $\lambda\in\mathbb R$   and    $\mathbf{h}\,\in\,\mathbb{R}^{n\times1}$    s at for any (possibly non- stationary) policy    $\pi=\{\pi_{1},\pi_{2},.\,.\,.\}$   and cycle  m  $m\in\mathbb{N}$   ∈ N  and  $\forall x_{m}\in\mathcal{X}$  ,  

$$
\lambda+[\mathbf{h}]_{x_{m}}\geq R_{x_{m}p_{m}(x_{m})}+\sum_{x_{m+1}\in\mathcal{X}}\mu\big(\,x_{m}\,p_{m}(x_{m})\,x_{m+1}\,\big)[\mathbf{h}]_{x_{m+1}}.
$$  

Furthermore, if  $\pi_{m}$   attains  maximum i ion (B.7) for each    $x_{m}\in\mathcal{X}$  then equality holds in the  m  $m^{t h}$    cycle and  p  $p_{m}(x_{m})$  ) is optimal for this single cycle. The main idea of this proof is to extend this result so that we get a policy which is optimal across all cycles.  

Using the mapping    $B_{\pi_{m}}$   we can express the above equation more compactly as,  

$$
\lambda\mathbf{e}+\mathbf{h}\geq B_{\pi_{m}}\mathbf{h}.
$$  

Applying now    $B_{\pi_{m-1}}$   to both sides and using the monotonicity property from Lemma B.3.1 we see that,  

$$
\lambda\mathbf{e}+B_{\pi_{m-1}}\mathbf{h}\geq B_{\pi_{m-1}}B_{\pi_{m}}\mathbf{h}.
$$  

However we also know that    $\lambda\mathbf{e}+\mathbf{h}\geq B_{\pi_{m-1}}\mathbf{h}$   and so it follows that,  

$$
2\lambda\mathbf{e}+\mathbf{h}\geq B_{\pi_{m-1}}B_{\pi_{m}}\mathbf{h}.
$$  

Repeating this    $_m$   times we get  

$$
m\lambda\mathbf{e}+\mathbf{h}\geq B_{\pi_{1}}B_{\pi_{2}}\cdot\cdot\cdot B_{\pi_{m}}\mathbf{h},
$$  

where equality continues to hold in the case where    $\pi_{k}$   attains the maximum in Equation (B.7) in each cycle    $k\in\{1,\ldots,m\}$  . When this is the case we see that    $\pi$   is optimal for the cycles 1 to    $m$  .  

From the deﬁnition of    $B_{\pi_{k}}$   we see that,  

$$
[B_{\pi_{1}}B_{\pi_{2}}\cdot\cdot\cdot B_{\pi_{m}}\mathbf{h}]_{x_{1}}=\mathbf{E}\left\{[\mathbf{h}]_{x_{m+1}}+\sum_{k=1}^{m}R_{x_{k}p_{k}(x_{k})}\bigg\vert x_{1},\pi,\mu\right\}
$$  

is the total expected reward over    $m$   cycles from the initial perception    $x_{1}$   to the ﬁnal perception    $x_{m+1}$   under policy    $\pi$   and environment    $\mu$  . Thus    $\forall x_{1}\in\mathcal{X}$  ,  

$$
m\lambda+[\mathbf{h}]_{x_{1}}\geq\mathbf{E}\left\{[\mathbf{h}]_{x_{m+1}}+\sum_{k=1}^{m}R_{x_{k}p_{k}(x_{k})}\bigg|x_{1},\pi,\mu\right\}
$$  

where equality holds if    $\pi_{k}$   attains the maximum in Equation (B.7) in each cycle.  

Dividing by    $_m$  , gives  $\forall x_{1}\in\mathcal{X}$  ,  

$$
\lambda+\frac{1}{m}[\mathbf{h}]_{x_{1}}\geq\frac{1}{m}\mathbf{E}\left\{[\mathbf{h}]_{x_{m+1}}|x_{1},\pi,\mu\right\}+\frac{1}{m}\mathbf{E}\left\{\sum_{k=1}^{m}R_{x_{k}p_{k}(x_{k})}\bigg|x_{1},\pi,\mu\right\}.
$$  

Taking    $m\to\infty$  this reduces to  $\forall x_{1}\in\mathcal{X}$  ,  

$$
\lambda\geq\operatorname*{lim}_{m\to\infty}{\frac{1}{m}}\mathbf{E}\left\{\sum_{k=1}^{m}R_{x_{k}p_{k}(x_{k})}{\bigg|}x_{1},\pi,\mu\right\},
$$  

or equivalently,  

$$
\lambda\geq V_{1\infty}^{\pi\mu},
$$  

where equality holds if    $\pi_{k}$   attains the maximum in Equation (B.7) for each cycle. When this is the case,    $\pi$   is optimal and thus    $\begin{array}{r}{V_{1\infty}^{\pi\mu}\,=\,\operatorname*{max}_{\pi}V_{1\infty}^{\pi\mu}\,=\,\lambda}\end{array}$  . ∞ ∞ Furthermore, we see that this optimal policy is stationary because in Equation (B.7) the action  $a$   only depends on the current perception    $x$   and is independent of the cycle number. We call this optimal stationary policy    $\pi^{\mu}$  . 2  

The above result only guarantees the existence of an optimal stationary policy    $\pi^{\mu}$    for a stationary  MDP  environment    $\mu$   in the case where there is a solution to the Bellman equation    $\lambda\mathbf{e}+\mathbf{h}\,=\,B\mathbf{h}$  . Fortunately for ergodic MDP s it can be shown that such a solution always exists (our deﬁnition of ergodicity implies condition (2) of Proposition 2.6 in (Bertsekas, 1995) where the existence of a solution is proven). It now follows that:  

B.3.3 Theorem.  For any ergodic ﬁnite stationary  MDP  environment  µ  there exists an optimal stationary policy    $\pi^{\mu}$  .  

This is a useful result because the interaction between a stationary  MDP environment and a stationary policy is much simpler to analyse than the non- stationary case. We will refer back to this result a number of times when we need to assert the existence of an optimal stationary policy.  

One thing that we have not shown is that the optimal policy with respect to a given  MDP  can be computed. Given that our  MDP  is ﬁnite and therefore the number of possible stationary deterministic policies is also ﬁnite we might expect that this problem should be solvable. Indeed, it can be shown that the Policy Iteration algorithm is able to compute an optimal stationary policy in this situation (see Section 4.3 of Bertsekas, 1995).  

# B.4. Convergence of expected average value  

Our goal is to ﬁnd a good policy for an unknown stationary  MDP    $\mu$  . Because we do not know the structure of the  MDP , that is    $\mu$  , we create an estimate µ  and then ﬁnd the optimal policy with respect to this estimate, which we will call    $\pi^{\hat{\mu}}$  . Our hope is that if our estimate  µ  is suﬃciently close to    $\mu$  , then  $\pi^{\hat{\mu}}$   will perform well compared to the true optimal policy    $\pi^{\mu}$  . Speciﬁcally we would like  $\left|V_{1\infty}^{\pi^{\hat{\mu}}\mu}-V_{1\infty}^{\pi^{\mu}\mu}\right|=0$  

In the analysis that follows we will need to be careful about whether we are  talking about the true environment    $\mu$  , our estimate of this  µ , or various combi- nations of environments interacting with various policies. Sometimes policies will be optimal with respect to the environment that they are interacting with, sometimes they will only be optimal with respect to an estimate of the envi- ronment that they are actually interacting with, and in some cases the policy may be arbitrary. Needless to say that care is required to avoid mixing things up.  

As d ly, let    $D\in\mathbb{R}^{\mathcal{X}\times\mathcal{A}\times\mathcal{X}}$    represent the chronological system  and   $\hat{D}\,\in\,\mathbb{R}^{\mathcal{X}\times\mathcal{A}\times\mathcal{X}}$     $\mu$   ∈ logical system  µ . From Equation (B.1) we know that the matrix  T  $T\in\mathbb{R}^{\mathcal{X}\times\mathcal{X}}$   ∈   representing the Markov chain formed by a policy    $\pi$   interacting with    $\mu$   is deﬁned by,  

$$
T_{x x^{\prime}}:=\sum_{a\in\mathcal{A}}\pi(x\underline{{a}})D_{x a x^{\prime}}.
$$  

We can similarly deﬁne  $\hat{T}$   from    $\pi$   and D . It now follows that if  $\hat{D}$   is close to    $D$  , in the sense that  $\begin{array}{r}{\varepsilon:=\operatorname*{max}_{x a x^{\prime}}|D_{x a x^{\prime}}-\hat{D}_{x a x^{\prime}}|}\end{array}$  |  is small, then for  any  stationary policy    $\pi$   the associated matrices    $T$   and   $\hat{T}$   are close:  

$$
\begin{array}{r c l}{\underset{x x^{\prime}}{\operatorname*{max}}\,\Big|T_{x x^{\prime}}-\hat{T}_{x x^{\prime}}\Big|}&{=}&{\underset{x x^{\prime}}{\operatorname*{max}}\,\Bigg|\displaystyle\sum_{a\in\mathcal{A}}\pi(x\underline{{a}})\big({D}_{x a x^{\prime}}-\hat{D}_{x a x^{\prime}}\big)\Bigg|}\\ &{\leq}&{\underset{x}{\operatorname*{max}}\,\Bigg|\displaystyle\sum_{a\in\mathcal{A}}\pi(x\underline{{a}})\varepsilon\Bigg|=\varepsilon.}\end{array}
$$  

This is important as it means that we can take bounds on the accuracy of our estimate of the true  MDP  and imply from this bounds on the accuracy of the estimate  $\hat{T}$   for any stationary policy.  

For any given stationary policy this bound also carries over to the associated expected long run average value functions in a straightforward way:  

B.4.1 Lemma.  For stationary ﬁnite  MDP s such that    $\varepsilon:=\mathrm{max}_{x a x^{\prime}}\,|D_{x a x^{\prime}}-$   $\hat{D}_{x a x^{\prime}}|$  |  it follows that for any stationary policy    $\pi$  ,  

$$
\left|V_{1\infty}^{\pi\mu}-V_{1\infty}^{\pi\hat{\mu}}\right|=O(\varepsilon).
$$  

Proof. Let    $T$   and   $\hat{T}$   be the Markov chains deﬁned by    $D$   and  $\hat{D}$   interacting with a stationary policy    $\pi$  . By the argument above we see that max ′    $|T_{x x^{\prime}}-$   $x x^{\prime}

$   $\hat{T}_{x x^{\prime}}|\,\leq\,\varepsilon$  | ≤ . Thus by Theorem B.2.7 we know th t there exists    $c_{T}$   such that

  $\begin{array}{r}{\operatorname*{max}_{x x^{\prime}}|T_{x x^{\prime}}^{*}-\hat{T}_{x x^{\prime}}^{*}|\leq c_{T}\varepsilon}\end{array}$   − | ≤ , where    $c_{T}$   depends on  T . By Equation (B.5) we see that,  

$$
\left|V_{1\infty}^{\pi\mu}-V_{1\infty}^{\pi\hat{\mu}}\right|=\left|(T^{*}-\hat{T}^{*})\mathbf{r}_{\pi}\right|=O(\varepsilon).
$$  

From this lemma we can show that the optimal policies with respect to    $\mu$  and  µ  are bounded:  

B.4.2 Theorem.  For a stationary ﬁnite  MDP  such that    $\begin{array}{r}{\varepsilon:=\operatorname*{max}_{x a x^{\prime}}|D_{x a x^{\prime}}-}\end{array}$   $\hat{D}_{x a x^{\prime}}|$   it follows that,  

$$
\left|V_{1\infty}^{\pi^{\mu}\mu}-V_{1\infty}^{\pi^{\hat{\mu}}\hat{\mu}}\right|=O(\varepsilon),
$$  

where    $\pi^{\mu}$    and    $\pi^{\hat{\mu}}$  µ  are optimal policies that are also stationary.  

Proof. For any two functions    $f,f^{\prime}:\mathcal{D}\rightarrow\mathbb{R}$   such that    $\forall x\,\in\,\mathcal{D}\,:\,|f(x)-$   $f^{\prime}(x)|\leq\delta$   it follows that    $\begin{array}{r}{|\operatorname*{max}_{x\in\mathcal{D}}f(x)-\operatorname*{max}_{x^{\prime}\in\mathcal{D}}f^{\prime}(x^{\prime})|\leq\delta}\end{array}$  . From Lemma B.4.1 it then follows that,  

$$
\left|\operatorname*{max}_{\pi}V_{1\infty}^{\pi\mu}-\operatorname*{max}_{\pi^{\prime}}V_{1\infty}^{\pi^{\prime}\hat{\mu}}\right|=O(\varepsilon)
$$  

where    $\pi$   and    $\pi^{\prime}$    belong to the set of stationary policies. However by Theorem B.3.3 we know that the optimal policies for    $\mu$   and  µ  can be chosen stationary and thus the result follows. 2  

We now have all the necessary results to show that if   $\hat{\mu}$   is a good estimate of    $\mu$   then our policy    $\pi^{\hat{\mu}}$   that is based on   $\hat{\mu}$   will perform near optimally with respect to the true environment in the limit.  

B.4.3 Theorem.  Let    $\mu$   and    $\hat{\mu}$   be two ergodic stationary ﬁnite  MDP  environ- ments that are close in t se that    $\begin{array}{r}{\varepsilon:=\operatorname*{max}_{x a x^{\prime}}|D_{x a x^{\prime}}-\hat{D}_{x a x^{\prime}}|}\end{array}$  |  is small. It can be shown that for  m  $m\in\mathbb{N}$   ∈ ,  

$$
\left|V_{1m}^{\pi^{\hat{\mu}}\mu}-V_{1m}^{\pi^{\mu}\mu}\right|=O\left(\frac{1}{m}\right)+O(\varepsilon)
$$  

where    $\pi^{\mu}$    is an optimal policy for the true distribution   , and    $\pi^{\hat{\mu}}$   is an optimal  $\mu$  policy with respect to the estimate of the true distribution    $\hat{\mu}$  .  

# Proof.  

From Theorem B.3.3 we see that    $\pi^{\hat{\mu}}$   can be chosen stationary. From the triangle inequality and the results of Corollary B.2.5 (with    $\pi\,\sim\!\pi^{\hat{\mu}}$  ), Lemma B.4.1 (with    $\pi\sim\pi^{\hat{\mu}}$  ) and Theorem B.4.2 we see that,  

$$
\begin{array}{r c l}{{\left|V_{1m}^{\pi^{\hat{\mu}}\mu}-V_{1\infty}^{\pi^{\hat{\mu}}\mu}\right|}}&{{=}}&{{\left|V_{1m}^{\pi^{\hat{\mu}}\mu}-V_{1\infty}^{\pi^{\hat{\mu}}\mu}+V_{1\infty}^{\pi^{\hat{\mu}}\mu}-V_{1\infty}^{\pi^{\hat{\mu}}\hat{\mu}}+V_{1\infty}^{\pi^{\hat{\mu}}\hat{\mu}}-V_{1\infty}^{\pi^{\mu}\mu}\right|}}\\ {{}}&{{\leq}}&{{\left|V_{1m}^{\pi^{\hat{\mu}}\mu}-V_{1\infty}^{\pi^{\hat{\mu}}\mu}\right|+\left|V_{1\infty}^{\pi^{\hat{\mu}}\mu}-V_{1\infty}^{\pi^{\hat{\mu}}\hat{\mu}}\right|+\left|V_{1\infty}^{\pi^{\hat{\mu}}\hat{\mu}}-V_{1\infty}^{\pi^{\mu}\mu}\right|}}\\ {{}}&{{=}}&{{{\cal O}\left(\displaystyle\frac{1}{m}\right)+{\cal O}(\varepsilon).}}\end{array}
$$  

Now from Corollary B.2.5 (with    $\pi\sim\pi^{\mu}$  ) and the triangle inequality again,  

$$
\begin{array}{r c l}{{\left|V_{1m}^{\pi^{\hat{\mu}}\mu}-V_{1m}^{\pi^{\mu}\mu}\right|}}&{{=}}&{{\left|V_{1m}^{\pi^{\hat{\mu}}\mu}-V_{1\infty}^{\pi^{\mu}\mu}+V_{1\infty}^{\pi^{\mu}\mu}-V_{1m}^{\pi^{\mu}\mu}\right|}}\\ {{}}&{{\leq}}&{{\left|V_{1m}^{\pi^{\hat{\mu}}\mu}-V_{1\infty}^{\pi^{\mu}\mu}\right|+\left|V_{1\infty}^{\pi^{\mu}\mu}-V_{1m}^{\pi^{\mu}\mu}\right|=O\left(\displaystyle\frac{1}{m}\right)+O(\varepsilon).}}\end{array}
$$  

![](images/1574c11f6f6b781c1de6a8a1a4fc492d611e9267a9eaa8394595bd5166cccee6.jpg)  

# C. Deﬁnitions of Intelligence  

“Viewed narrowly, there seem to be almost as many deﬁnitions of intelligence as there were experts asked to deﬁne it.” R. J. Stern- berg quoted in (Gregory, 1998)  

Despite a long history of research and debate, there is still no standard deﬁnition of intelligence. This has lead some to believe that intelligence may be approximately described, but cannot be fully deﬁned. We believe that this degree of pessimism is too strong. Although there is no single standard deﬁnition, if one surveys the many deﬁnitions that have been proposed, strong similarities between many of the deﬁnitions quickly become obvious.  

Here we take the opportunity to present the many informal deﬁnitions that we have collected over the years. Naturally, compiling a complete list would be impossible as many deﬁnitions of intelligence are buried deep inside articles and books. Nevertheless, the 70 odd deﬁnitions presented below are, to the best of our knowledge, the largest and most well referenced collection there is.  

# C.1. Collective deﬁnitions  

In this section we present deﬁnitions that have been proposed by groups or organisations. In many cases deﬁnitions of intelligence given in encyclopedias have been either contributed by an individual psychologist or quote an earlier deﬁnition given by a psychologist. In these cases we have chosen to attribute the quote to the psychologist, and have placed it in the next section. In this section we only list those deﬁnitions that either cannot be attributed to speciﬁc individuals, or represent a collective deﬁnition agreed upon by many individ- uals. As many dictionaries source their deﬁnitions from other dictionaries, we have endeavoured to always list the original source.  

1. “The ability to use memory, knowledge, experience, understanding, rea- soning, imagination and judgement in order to solve problems and adapt to new situations.” AllWords Dictionary, 2006 2. “The capacity to acquire and apply knowledge.” The American Heritage Dictionary, fourth edition, 2000 3. “Individuals diﬀer from one another in their ability to understand com- plex ideas, to adapt eﬀectively to the environment, to learn from expe- rience, to engage in various forms of reasoning, to overcome obstacles  

by taking thought.” American Psychological Association (Neisser et al.,  

1996)

 4. “The ability to learn, understand and make judgments or have opinions that are based on reason” Cambridge Advance Learner’s Dictionary, 2006

 5. “Intelligence is a very general mental capability that, among other things, involves the ability to reason, plan, solve problems, think abstractly, comprehend complex ideas, learn quickly and learn from experience.” Common statement with 52 expert signatories (Gottfredson, 1997a)

 6. “The ability to learn facts and skills and apply them, especially when this ability is highly developed.” Encarta World English Dictionary, 2006

 7. “. . . ability to adapt eﬀectively to the environment, either by making a change in oneself or by changing the environment or ﬁnding a new one . . . intelligence is not a single mental process, but rather a combination of many mental processes directed toward eﬀective adaptation to the environment.” Encyclopedia Britannica, 2006

 8. “the general mental ability involved in calculating, reasoning, perceiving relationships and analogies, learning quickly, storing and retrieving infor- mation, using language ﬂuently, classifying, generalizing, and adjusting to new situations.” Columbia Encyclopedia, sixth edition, 2006

 9. “Capacity for learning, reasoning, understanding, and similar forms of mental activity; aptitude in grasping truths, relationships, facts, mean- ings, etc.” Random House Unabridged Dictionary, 2006

 10. “The ability to learn, understand, and think about things.” Longman Dictionary or Contemporary English, 2006

 11. “: the ability to learn or understand or to deal with new or trying situa- tions : . . . the skilled use of reason (2) : the ability to apply knowledge to manipulate one’s environment or to think abstractly as measured by objective criteria (as tests)” Merriam-Webster Online Dictionary, 2006

 12. “The ability to acquire and apply knowledge and skills.” Compact Ox- ford English Dictionary, 2006

 13. “. . . the ability to adapt to the environment.” World Book Encyclopedia, 2006

 14. “Intelligence is a property of mind that encompasses many related mental abilities, such as the capacities to reason, plan, solve problems, think abstractly, comprehend ideas and language, and learn.” Wikipedia, 4 October, 2006  

15. “Capacity of mind, especially to understand principles, truths, facts or meanings, acquire knowledge, and apply it to practise; the ability to learn and comprehend.” Wiktionary, 4 October, 2006 16. “The ability to learn and understand or to deal with problems.” Word Central Student Dictionary, 2006 17. “The ability to comprehend; to understand and proﬁt from experience.” Wordnet 2.1, 2006 18. “The capacity to learn, reason, and understand.” Wordsmyth Dictio- nary, 2006  

# C.2. Psychologist deﬁnitions  

This section contains deﬁnitions from psychologists. In some cases we have not yet managed to locate the exact reference and would appreciate any help in doing so.  

1. “Intelligence is not a single, unitary ability, but rather a composite of sev- eral functions. The term denotes that combination of abilities required for survival and advancement within a particular culture.” Anastasi (1992) 2. “. . . that facet of mind underlying our capacity to think, to solve novel problems, to reason and to have knowledge of the world.” Anderson (2006) 3. “It seems to us that in intelligence there is a fundamental faculty, the alteration or the lack of which, is of the utmost importance for practi- cal life. This faculty is judgement, otherwise called good sense, practi- cal sense, initiative, the faculty of adapting ones self to circumstances.” Binet and Simon (1905) 4. “We shall use the term ‘intelligence’ to mean the ability of an organism to solve new problems . . . ” Bingham (1937) 5. “Intelligence is what is measured by intelligence tests.” Boring (1923) 6. “. . . a quality that is intellectual and not emotional or moral: in measur- ing it we try to rule out the eﬀects of the child’s zeal, interest, industry, and the like. Secondly, it denotes a general capacity, a capacity that enters into everything the child says or does or thinks; any want of ‘in- telligence’ will therefore be revealed to some degree in almost all that he attempts;” Burt (1957)  

7. “A person possesses intelligence insofar as he has learned, or can learn, to adjust himself to his environment.” S. S. Colvin quoted in (Sternberg, 2000)

 8. “. . . the ability to plan and structure one’s behavior with an end in view.” J. P. Das

 9. “The capacity to learn or to proﬁt by experience.” W. F. Dearborn quoted in (Sternberg, 2000)

 10. “. . . in its lowest terms intelligence is present where the individual an- imal, or human being, is aware, however dimly, of the relevance of his behaviour to an objective. Many deﬁnitions of what is indeﬁnable have been attempted by psychologists, of which the least unsatisfactory are 1. the capacity to meet novel situations, or to learn to do so, by new adap- tive responses and 2. the ability to perform tests or tasks, involving the grasping of relationships, the degree of intelligence being proportional to the complexity, or the abstractness, or both, of the relationship.” Drever (1952)

 11. “Intelligence A: the biological substrate of mental ability, the brain’s neuroanatomy and physiology; Intelligence B: the manifestation of in- telligence A, and everything that inﬂuences its expression in real life behavior; Intelligence C: the level of performance on psychometric tests of cognitive ability.” H. J. Eysenck.

 12. “Sensory capacity, capacity for perceptual recognition, quickness, range or ﬂexibility or association, facility and imagination, span of attention, quickness or alertness in response.” F. N. Freeman quoted in (Sternberg, 2000)

 13. “. . . adjustment or adaptation of the individual to his total environment, or limited aspects thereof . . . the capacity to reorganize one’s behavior patterns so as to act more eﬀectively and more appropriately in novel situations . . . the ability to learn . . . the extent to which a person is edu- cable . . . the ability to carry on abstract thinking . . . the eﬀective use of concepts and symbols in dealing with a problem to be solved . . . ” W. Freeman

 14. “An intelligence is the ability to solve problems, or to create products, that are valued within one or more cultural settings.” Gardner (1993)

 15. “. . . performing an operation on a speciﬁc type of content to produce a particular product.” J. P. Guilford

 16. “Sensation, perception, association, memory, imagination, discrimina- tion, judgement and reasoning.” N. E. Haggerty quoted in (Sternberg, 2000)  

17. “The capacity for knowledge, and knowledge possessed.” Henmon (1921)  

18. “. . . cognitive ability.” Herrnstein and Murray (1996)  

19. “. . . the resultant of the process of acquiring, storing in memory, retriev- ing, combining, comparing, and using in new contexts information and conceptual skills.” Humphreys

 20. “Intelligence is the ability to learn, exercise judgment, and be imagina- tive.” J. Huarte

 21. “Intelligence is a general factor that runs through all types of perfor- mance.” A. Jensen

 22. “Intelligence is assimilation to the extent that it incorporates all the given data of experience within its framework . . . There can be no doubt either, that mental life is also accommodation to the environment. Assimilation can never be pure because by incorporating new elements into its earlier schemata the intelligence constantly modiﬁes the latter in order to adjust them to new elements.” Piaget (1963)

 23. “Ability to adapt oneself adequately to relatively new situations in life.” R. Pinter quoted in (Sternberg, 2000)

 24. “A biological mechanism by which the eﬀects of a complexity of stimuli are brought together and given a somewhat uniﬁed eﬀect in behavior.” J. Peterson quoted in (Sternberg, 2000)

 25. “. . . certain set of cognitive capacities that enable an individual to adapt and thrive in any given environment they ﬁnd themselves in, and those cognitive capacities include things like memory and retrieval, and prob- lem solving and so forth. There’s a cluster of cognitive abilities that lead to successful adaptation to a wide range of environments.” Simonton (2003)

 26. “Intelligence is part of the internal environment that shows through at the interface between person and external environment as a function of cognitive task demands.” R. E. Snow quoted in (Slatter, 2001)

 27. “. . . I prefer to refer to it as ‘successful intelligence.’ And the reason is that the emphasis is on the use of your intelligence to achieve success in your life. So I deﬁne it as your skill in achieving whatever it is you want to attain in your life within your sociocultural context — meaning that people have diﬀerent goals for themselves, and for some it’s to get very good grades in school and to do well on tests, and for others it might be to become a very good basketball player or actress or musician.” Sternberg (2003)  

28. “. . . the ability to undertake activities that are characterized by (1) diﬃ- culty, (2) complexity, (3) abstractness, (4) economy, (5) adaptedness to goal, (6) social value, and (7) the emergence of originals, and to maintain such activities under conditions that demand a concentration of energy and a resistance to emotional forces.” Stoddard 29. “The ability to carry on abstract thinking.” L. M. Terman quoted in (Sternberg, 2000) 30. “Intelligence, considered as a mental trait, is the capacity to make im- pulses focal at their early, unﬁnished stage of formation. Intelligence is therefore the capacity for abstraction, which is an inhibitory process.” Thurstone (1924) 31. “The capacity to inhibit an instinctive adjustment, the capacity to rede- ﬁne the inhibited instinctive adjustment in the light of imaginally experi- enced trial and error, and the capacity to realise the modiﬁed instinctive adjustment in overt behavior to the advantage of the individual as a social animal.” L. L. Thurstone quoted in (Sternberg, 2000) 32. “A global concept that involves an individual’s ability to act purposefully, think rationally, and deal eﬀectively with the environment.” Wechsler (1958) 33. “The capacity to acquire capacity.” H. Woodrow quoted in (Sternberg, 2000) 34. “. . . the term intelligence designates a complexly interrelated assemblage of functions, no one of which is completely or accurately known in man . . . ” Yerkes and Yerkes (1929) 35. “. . . that faculty of mind by which order is perceived in a situation previ- ously considered disordered.” R. W. Young quoted in (Kurzweil, 2000)  

# C.3. AI researcher deﬁnitions  

This section lists deﬁnitions from researchers in artiﬁcial intelligence.  

1. “. . . the ability of a system to act appropriately in an uncertain environ- ment, where appropriate action is that which increases the probability of success, and success is the achievement of behavioral subgoals that support the system’s ultimate goal.” Albus (1991) 2. “Any system . . . that generates adaptive behviour to meet goals in a range of environments can be said to be intelligent.” Fogel (1995) 3. “Achieving complex goals in complex environments.” Goertzel (2006)  

4. “Intelligent systems are expected to work, and work well, in many dif- ferent environments. Their property of intelligence allows them to max- imize the probability of success even if full knowledge of the situation is not available. Functioning of intelligent systems cannot be considered separately from the environment and the concrete situation including the goal.” Gudwin (2000)

 5. “[Performance intelligence is] the successful (i.e., goal-achieving) perfor- mance of the system in a complicated environment.” Horst (2002)

 6. “Intelligence is the ability to use optimally limited resources – including time – to achieve goals.” Kurzweil (2000)

 7. “Intelligence is the power to rapidly ﬁnd an adequate solution in what appears  a priori  (to observers) to be an immense search space.” Lenat and Feigenbaum (1991)

 8. “Intelligence measures an agent’s ability to achieve goals in a wide range of environments.” Legg and Hutter (2006)

 9. “. . . doing well at a broad range of tasks is an empirical deﬁnition of ‘intelligence’ ” Masum et al. (2002)

 10. “Intelligence is the computational part of the ability to achieve goals in the world. Varying kinds and degrees of intelligence occur in people, many animals and some machines.” McCarthy (2004)

 11. “. . . the ability to solve hard problems.” Minsky (1985)

 12. “Intelligence is the ability to process information properly in a complex environment. The criteria of properness are not predeﬁned and hence not available beforehand. They are acquired as a result of the information processing.” Nakashima (1999)

 13. “. . . in any real situation behavior appropriate to the ends of the system and adaptive to the demands of the environment can occur, within some limits of speed and complexity.” Newell and Simon (1976)

 14. “[An intelligent agent does what] is appropriate for its circumstances and its goal, it is ﬂexible to changing environments and changing goals, it learns from experience, and it makes appropriate choices given per- ceptual limitations and ﬁnite computation.” Poole et al. (1998)

 15. “Intelligence means getting better over time.” Schank (1991)

 16. “. . . the essential, domain-independent skills necessary for acquiring a wide range of domain-speciﬁc knowledge – the ability to learn anything. Achieving this with ‘artiﬁcial general intelligence’ (AGI) requires a highly adaptive, general-purpose system that can autonomously acquire an ex- tremely wide range of speciﬁc knowledge and skills and can improve its own cognitive ability through self-directed learning.” (Voss, 2005)  

17. “Intelligence is the ability for an information processing system to adapt to its environment with insuﬃcient knowledge and resources.” Wang (1995)

 18. “. . . the mental ability to sustain successful life.” K. Warwick quoted  

# Bibliography  

Abeles, M. (1991).  Corticonics: Neural circuits of the cerebral cortex . Cam- bridge University Press.  

Albus, J. S. (1991). Outline for a theory of intelligence.  IEEE Trans. Systems, Man and Cybernetics ,  21 , 473–509.  

Alvarado, N., Adams, S., Burbeck, S., and Latta, C. (2002). Beyond the Turing test: Performance metrics for evaluating a computer simulation of the human mind.  Performance Metrics for Intelligent Systems Workshop . Gaithersburg, MD, USA: North-Holland.  

Anantha narayan an, R., and Modha, D. S. (2007). Anatomy of a cortical sim- ulator.  ACM/IEEE SC2007 Conference on High Performance Networking and Computing . Reno, NV, USA.  

Anastasi, A. (1992). What counselors should know about the use and inter- pretation of psychological tests.  Journal of Counseling and Development , 70 , 610–615.  

Anderson, M. (2006). Intelligence.  MS Encarta online encyclopedia .  

Asohan, A. (2003). Leading humanity forward.  The Star .  

Barzdin, J. M. (1972). Prognostication of automata and functions.  Information Processing ,  71 , 81–84.  

Bell, T. C., Cleary, J. G., and Witten, I. H. (1990).  Text compression . Prentice Hall.  

Bellman, R. (1957).  Dynamic programming . New Jersey: Princeton University Press.  

Berry, D. A., and Fristedt, B. (1985).  Bandit problems: Sequential allocation of experiments . London: Chapman and Hall.  

Bertsekas, D. P. (1995).  Dynamic programming and optimal control, vol. (ii) . Belmont, Massachusetts: Athena Scientiﬁc.  

Binet, A. (1911).  Les idees modernes sur les enfants . Paris: Flammarion.  

Binet, A., and Simon, T. (1905). Methodes nouvelles por le diagnostic du niveai intellectuel des anormaux.  L’Ann´ ee Psychologique ,  11 , 191–244.  

Bingham, W. V. (1937).  Aptitudes and aptitude testing . New York: Harper & Brothers.  

Block, N. (1981). Psychologism and behaviorism.  Philosophical Review ,  90 , 5–43.  

Boring, E. G. (1923). Intelligence as the tests test it.  New Republic ,  35 , 35–37.  

Boyan, J. A. (1999). Least-squares temporal diﬀerence learning.  Proc. 16th International Conf. on Machine Learning  (pp. 49–56). Morgan Kaufmann, San Francisco, CA.  

Bradtke, S. J., and Barto, A. G. (1996). Linear least-squares algorithms for temporal diﬀerence learning.  Machine Learning ,  22 , 33–57.  

Bringsjord, S., and Schimanski, B. (2003). What is artiﬁcial intelligence? Psychometric AI as an answer.  Eighteenth International Joint Conference on Artiﬁcial Intelligence ,  18 , 887–893.  

Burt, C. L. (1957).  The causes and treatments of backwardness . University of London press.  

Calude, C. S. (2002).  Information and randomness . Berlin: Springer. 2nd edition.  

Calude, C. S., J¨ urgensen, H., and Legg, S. (2000). Solving ﬁnitely refutable mathematical problems. In C. S. Calude and G. Puaun (Eds.),  Finite ver- sus inﬁnite: Contributions to an eternal dilemma , 39–52. Springer-Verlag, London.  

Carroll, J. B. (1993).  Human cognitive abilities: A survey of factor-analytic studies . New York: Cambridge University Press.  

Cattell, R. B. (1987).  Intelligence: Its structure, growth, and action . New York: Elsevier.  

Chaitin, G. J. (1982). G¨ odel’s theorem and information.  International Journal of Theoretical Physics ,  22 , 941–954.  

Cilibrasi, R., and Vit´ anyi, P. M. B. (2005). Clustering by compression.  IEEE Trans. Information Theory ,  51 , 1523–1545.  

Cleary, J. G., Legg, S., and Witten, I. H. (1996). An MDL estimate of the signiﬁcance of rules.  Information, Statistics and Induction in Science  (pp. 43–45).  

Creutzfeldt, O. D. (1977). Generality of the functional structure of the neo- cortex.  Naturwissenschaften ,  64 , 507–517.  

Crevier, D. (1993).  AI: The tumultuous search for artiﬁcial intelligence . New York: BasicBooks.  

Dawid, A. P. (1985). Comment on The impossibility of inductive inference.  

Journal of the American Statistical Association ,  80 , 340–341. Dawkins, R. (2006).  The god delusion . Bantam Books. Dayan, P. (1992). The convergence of TD(  $\lambda$  ) for general  λ .  Machine learning , 8, 341–362.Doob, J. L. (1953).  Stochastic processes . New York: John Wiley & Sons. Dowe, D. L., and Hajek, A. R. (1998). A non-behavioural, computational extension to the Turing test.  International Conference on Computational Intelligence & Multimedia Applications (ICCIMA ’98)  (pp. 101–106). Gipp- sland, Australia. Drever, J. (1952). A dictionary of psychology . Harmondsworth: Penguin Books. Edmonds, B. (2006). The social embedding of intelligence – Towards produc- ing a machine that could pass the turing test. In  The turing test sourcebook: Philosophical and methodological issues in the quest for the thinking com- puter . Kluwer. Eisner, J. (1991). Cognitive science and the search for intelligence.  Invited paper presented to the Socratic Society, University of Cape Town . Feder, M., Merhav, N., and Gutman, M. (1992). Universal prediction of indi- vidual sequences.  IEEE Trans. on Information Theory ,  38 , 1258–1270. Fi´ evet, C. (2005). Mesurer l’intelligence d’une machine. Le Monde de l’intelligence  (pp. 42–45). Paris: Mondeo publishing. Fogel, D. B. (1995). Review of computational intelligence: Imitating life.  Proc. of the IEEE ,  83 . Ford, K. M., and Hayes, P. J. (1998). On computational wings: Rethinking the goals of artiﬁcial intelligence.  Scientiﬁc American ,  Special Edition . French, R. M. (1990). Subcognition and the limits of the Turing test.  Mind , 99 , 53–65. Fuster, J. M. (2003).  Cortex and mind: Unifying cognition . Oxford University Press.Gardner, H. (1993).  Frames of mind: Theory of multiple intelligences . Fontana Press.  

Gittins, J. C. (1989).  Multi-armed bandit allocation indices . John Wiley & Sons.  

G¨ odel, K. (1931).  Uber formal unentscheidbare S¨ atze der Principia Mathemat- ica und verwandter Systeme I.  Monatshefte f¨ ur Matematik und Physik ,  38 , 173–198. [English translation by E. Mendelsohn: “On undecidable proposi- tions of formal mathematical systems”. In M. Davis, editor,  The undecidable , pages 39–71, New York, 1965. Raven Press, Hewlitt].  

Goertzel, B. (2006).  The hidden pattern . Brown Walker Press.  

Gold, E. M. (1967). Language identiﬁcation in the limit.  Information and Control ,  10 , 447–474.  

Goldberg, D. E., and Richardson, J. (1987). Genetic algorithms with sharing for multi-modal function optimization.  Proc. 2nd International Conference on Genetic Algorithms and their Applications  (pp. 41–49). Cambridge, MA: Lawrence Erlbaum Associates.  

Good, I. J. (1965). Speculations concerning the ﬁrst ultra intelligent machine. Advances in Computers ,  6 , 31–88.  

Gottfredson, L. S. (1997a). Mainstream science on intelligence: An editorial with 52 signatories, history, and bibliography.  Intelligence ,  24 , 13–23.  

Gottfredson, L. S. (1997b). Why g matters: The complexity of everyday life. Intelligence ,  24 , 79–132.  

Gottfredson, L. S. (2002). g: Highly general and highly practical.  The general factor of intelligence: How general is it?  (pp. 331–380). Erlbaum.  

Gould, S. J. (1981).  The mismeasure of man . W. W. Norton & Company.  

Graham-Rowe, D. (2005). Spotting the bots with brains.  New Scientist mag- azine ,  2512 , 27.  

Graham-Rowe, D. (2007). A working brain model.  Technology Review . 28 November.  

Gregory, R. L. (1998). The Oxford companion to the mind . Oxford, UK: Oxford University Press.  

Gudwin, R. R. (2000). Evaluating intelligence: A computational semiotics perspective.  IEEE International conference on systems, man and cybernetics (pp. 2080–2085). Nashville, Tenessee, USA.  

Guilford, J. P. (1967).  The nature of human intelligence . New York: McGraw- Hill.  

Gunderson, K. (1971).  Mentality and machines . Garden City, New York, USA: Doubleday and company. Harnad, S. (1989). Minds, machines and Searle.  Journal of Theoretical and Experimental Artiﬁcial Intelligence ,  1 , 5–25. Havenstein, H. (2005). Spring comes to AI winter. Computer World . 14 February. Hawkins, J., and Blakeslee, S. (2004).  On intelligence . New York: Owl books. Henmon, V. A. C. (1921). The measurement of intelligence.  School and Society , 13 , 151–158. Herman, L. M., and Pack, A. A. (1994). Animal intelligence: Historical per- spectives and contemporary approaches. In R. Sternberg (Ed.),  Encyclopedia of human intelligence , 86–96. New York: Macmillan. Hern´ andez-Orallo, J. (2000a). Beyond the Turing test. Journal of Logic, Language and Information ,  9 , 447–466. Hern´ andez-Orallo, J. (2000b). On the computational measurement of intelli- gence factors.  Performance Metrics for Intelligent Systems Workshop  (pp. 1–8). Gaithersburg, MD, USA. Hern´ andez-Orallo, J., and Minaya-Collado, N. (1998). A formal deﬁnition of intelligence based on an intensional variant of Kolmogorov complexity. Proceedings of the International Symposium of Engineering of Intelligent Systems (EIS’98)  (pp. 146–163). ICSC Press. Herrnstein, R. J., and Murray, C. (1996).  The bell curve: Intelligence and class structure in American life . Free Press. Horn, J. (1970). Organization of data on life-span development of human abilities.  Life-span developmental psychology: Research and theory . New York: Academic Press. Horst, J. (2002). A native intelligence metric for artiﬁcial systems.  Perfor- mance Metrics for Intelligent Systems Workshop . Gaithersburg, MD, USA. Hsu, F. H., Campbell, M. S., and Hoane, A. J. (1995). Deep Blue system overview.  Proceedings of the 1995 International Conference on Supercom- puting  (pp. 240–244). Hutchens, J. L. (1996). How to pass the Turing test by cheating. www.cs.umbc.edu/471/current/papers/hutchens.pdf . Hutter, M. (2001). Convergence and error bounds for universal prediction of nonbinary sequences.  Proc. 12th Eurpean Conference on Machine Learning Hutter, M. (2002a). The fastest and shortest algorithm for all well-deﬁned problems.  International Journal of Foundations of Computer Science ,  13 , 431–443. Hutter, M. (2002b). Fitness uniform selection to preserve genetic diversity. Proc. 2002 Congress on Evolutionary Computation (CEC-2002)  (pp. 783– 788). Washington D.C, USA: IEEE. Hutter, M. (2005). Universal artiﬁcial intelligence: Sequential deci- sions based on algorithmic probability . Berlin: Springer. 300 pages, http://www.hutter1.net/ai/uaibook.htm. Hutter, M. (2006). The Human knowledge compression prize. http://prize.hutter1.net . Hutter, M. (2007a). On universal prediction and Bayesian conﬁrmation.  The- oretical Computer Science ,  384 , 33–48. Hutter, M. (2007b). Universal algorithmic intelligence: A mathematical top  $\rightarrow$  down approach. In  Artiﬁcial general intelligence , 227–290. Berlin: Springer. Hutter, M., and Legg, S. (2006). Fitness uniform optimization.  IEEE Trans- actions on Evolutionary Computation ,  10 , 568–589. Hutter, M., and Legg, S. (2007). Temporal diﬀerence updating without a learning rate.  Neural Information Processing Systems (NIPS ’07) . Hutter, M., Legg, S., and Vit´ anyi, P. M. B. (2007). Algorithmic probability. Scholarpedia . Johnson, W. L. (1992). Needed: A new test of intelligence. SIGARTN: SIGART Newsletter (ACM Special Interest Group on Artiﬁcial Intelligence) , 3 . Johnson-Laird, P. N., and Wason, P. C. (1977). A theoretical analysis of insight into a reasoning task. In P. N. Johnson-Laird and P. C. Wason (Eds.), Thinking: Readings in cognitive science , 143–157. Cambridge University Press.Jong, K. (1975). An analysis of the behavior of a class of genetic adaptive systems.  Dissertation Abstracts International ,  36 . Kandel, E. R., Schwartz, J. H., and Jessell, T. M. (2000).  Principles of neural science . New York: McGraw-Hill. 4nd edition. Kaufman, A. S. (2000). Tests of intelligence. In R. J. Sternberg (Ed.),  Hand- book of intelligence . Cambridge University Press.  

neurons . New York: Oxford University Press. Koza, J. R., Keane, M. A., Streeter, M. J., Mydlowec, W., Yu, J., and Lanza, G. (2003).  Genetic programming IV: Routine human-competitive machine intelligence . Kluwer Academic. Kurzweil, R. (2000).  The age of spiritual machines: When computers exceed human intelligence . Penguin. Lagoudakis, M. G., and Parr, R. (2003). Least-squares policy iteration.  Journal of Machine Learning Research ,  4 , 1107–1149. Legg, S. (1996). Minimum information estimation of linear regression models. Information, Statistics and Induction in Science  (pp. 103–111). Legg, S. (1997).  Solomon o induction  (Technical Report CDMTCS-030). Cen- tre of Discrete Mathematics and Theoretical Computer Science, University of Auckland. Legg, S. (2006a). Incompleteness and artiﬁcial intelligence.  Collegium Logicum of the Kurt G¨ odel Society . Vienna. Legg, S. (2006b). Is there an elegant universal theory of prediction? Proc. Algorithmic Learning Theory (ALT 06) . Barcelona, Spain. Legg, S., and Hutter, M. (2004).  A taxonomy for abstract environments  (Tech- nical Report IDSIA-20-04). IDSIA. Legg, S., and Hutter, M. (2005a). Fitness uniform deletion for robust optimiza- tion.  Proc. Genetic and Evolutionary Computation Conference (GECCO’05) (pp. 1271–1278). Washington, OR: ACM SigEvo. Legg, S., and Hutter, M. (2005b). A universal measure of intelligence for arti- ﬁcial agents.  Proc. 21st International Joint Conf. on Artiﬁcial Intelligence (IJCAI-2005)  (pp. 1509–1510). Edinburgh. Legg, S., and Hutter, M. (2006). A formal measure of machine intelligence.  An- nual Machine Learning Conference of Belgium and The Netherlands (Bene- learn’06)  (pp. 73–80). Ghent. Legg, S., and Hutter, M. (2007a). A collection of deﬁnitions of intelligence. Advances in Artiﬁcial General Intelligence: Concepts, Architectures and Algorithms  (pp. 17–24). Amsterdam, NL: IOS Press. Online version: www.vetta.org/shane/intelligence.html. Legg, S., and Hutter, M. (2007b). Tests of machine intelligence.  50 Years of Artiﬁcial Intelligence  (pp. 232–242). Monte Verit\` a, Switzerland.  

Legg, S., and Hutter, M. (2007c). Universal intelligence: A deﬁnition of ma- chine intelligence.  Minds and Machines ,  17 , 391–444. Legg, S., Hutter, M., and Kumar, A. (2004). Tournament versus ﬁtness uni- form selection.  Proc. 2004 Congress on Evolutionary Computation (CEC’04) (pp. 2144–2151). Portland, OR: IEEE. Legg, S., Poland, J., and Zeugmann, T. (2008). On the limits of learning with computational models.  Knowledge Media Science . To appear. Lenat, D., and Feigenbaum, E. (1991). On the thresholds of knowledge.  Arti- ﬁcial Intelligence ,  47 , 185–250. Levin, L. A. (1973). Universal sequential search problems.  Problems of Infor- mation Transmission ,  9 , 265–266. Levin, L. A. (1974). Laws of information conservation (non-growth) and aspects of the foundation of probability theory.  Problems of Information Transmission ,  10 , 206–210. Li, M., and Vit´ anyi, P. M. B. (1997).  An introduction to Kolmogorov complex- ity and its applications . Springer. 2nd edition. Loebner, H. G. (1990). The Loebner prize — The ﬁrst Turing test. http://www.loebner.net/Prizef/loebner-prize.html . Macphail, E. M. (1985). Vertebrate intelligence: The null hypothesis. In L. Weiskrantz (Ed.),  Animal intelligence , 37–50. Oxford: Clarendon. Mahoney, M. V. (1999). Text compression as a test for artiﬁcial intelligence. AAAI/IAAI  (p. 970). Markoﬀ, J., and Hansell, S. (2005). Hiding in plain sight, google seeks more power.  New York Times . 14 June. Martin-L¨ of, P. (1966). The deﬁnition of random sequences.  Information and Control ,  9 , 602–619. Masum, H., Christensen, S., and Oppacher, F. (2002). The Turing ratio: Metrics for open-ended tasks.  GECCO 2002: Proceedings of the Genetic and Evolutionary Computation Conference  (pp. 973–980). New York: Morgan Kaufmann Publishers. McCarthy, J. (2004). What is artiﬁcial intelligence? www-formal.stanford.edu/jmc/whatisai/whatisai.html . Melchner, L., Pallas, S. L., and Sur, M. (2000). Visual behaviour mediated by retinal projections directed to the auditory pathway.  Nature ,  404 , 871–876.  

Minsky, M. (1985).  The society of mind . New York: Simon and Schuster.  

Moravec, H. (1998). When will computer hardware match the human brain? Journal of Transhumanism ,  1 . Mountcastle, V. B. (1978). An organizing principle for cerebral function: The unit model and the distributed system. In  The mindful brain . MIT Press. M¨ uller, M. (2006).  Stationary algorithmic probability  (Technical Report). TU Berlin, Berlin. http://arXiv.org/abs/cs/0608095. Nakashima, H. (1999). AI as complex information processing. Minds and machines ,  9 , 57–80. Neisser, U., Boodoo, G., Bouchard, Jr., T. J., Boykin, A. W., Brody, N., Ceci, S. J., Halpern, D. F., Loehlin, J. C., Perloﬀ, R., Sternberg, R. J., and Urbina, S. (1996). Intelligence: Knowns and unknowns.  American Psychologist ,  51 , 77–101. Newell, A., and Simon, H. A. (1976). Computer science as empirical enquiry: Symbols and search.  Communications of the ACM 19 ,  3 , 113–126. Peng, J. (1993).  Eﬃcient dynamic programming-based learning for control . Doctoral dissertation, Northeastern University, Boston, MA. Peng, J., and Williams, R. J. (1996). Increamental multi-step Q-learning. Machine Learning ,  22 , 283–290. Piaget, J. (1963).  The psychology of intelligence . New York: Routledge. Poland, J., and Hutter, M. (2004). Convergence of discrete MDL for sequential prediction.  Proc. 17th Annual Conf. on Learning Theory (COLT’04)  (pp. 300–314). Banﬀ: Springer, Berlin. Poland, J., and Hutter, M. (2006). Universal learning of repeated matrix games. Proc. 15th Annual Machine Learning Conf. of Belgium and The Netherlands (Benelearn’06)  (pp. 7–14). Ghent. Poole, D., Mackworth, A., and Goebel, R. (1998).  Computational intelligence: A logical approach . New York, NY, USA: Oxford University Press. Raven, J. (2000). The Raven’s progressive matrices: Change and stability over culture and time.  Cognitive Psychology ,  41 , 1–48. Reznikova, Z. I. (2007).  Animal intelligence . Cambridge University Press. Reznikova, Z. I., and Ryabko, B. (1986). Analysis of the language of ants by information-theoretic methods.  Problems Inform. Transmission ,  22 , 245– 249. Rissanen, J. J. (1996). Fisher Information and Stochastic Complexity.  IEEE Trans. on Information Theory ,  42 , 40–47.  

Rummery, G. A. (1995).  Problem solving with reinforcement learning . Doctoral dissertation, Cambridge University.  

Rummery, G. A., and Niranjan, M. (1994).  On-line Q-learning using connec- tionist systems.  Technial Report CUED/F-INFENG/TR 166). Engineering Department, Cambridge University.  

Russell, S. J., and Norvig, P. (1995).  Artiﬁcial intelligence. A modern ap- proach . Englewood Cliﬀs: Prentice-Hall.  

Sanghi, P., and Dowe, D. L. (2003). A computer program capable of passing I.Q. tests.  Proc. 4th ICCS International Conference on Cognitive Science (ICCS’03)  (pp. 570–575). Sydney, NSW, Australia.  

Saygin, A., Cicekli, I., and Akman, V. (2000). Turing test: 50 years later. Minds and Machines ,  10 .  

Schaeﬀer, J., Burch, N., Bj¨ ornsson, Y., Kishimoto, A., M¨ uller, M., Lake, R., Lu, P., and Sutphen, S. (2007). Checkers is solved.  Science . 19 July.  

Schank, R. (1991). Where’s the AI?  AI magazine ,  12 , 38–49.  

Schmidhuber, J. (2002). The Speed Prior: a new simplicity measure yield- ing near-optimal computable predictions. Proc. 15th Annual Conference on Computational Learning Theory (COLT 2002)  (pp. 216–228). Sydney, Australia: Springer.  

Schmidhuber, J. (2004). Optimal ordered problem solver.  Machine Learning , 54 , 211–254.  

Schmidhuber, J. (2005). G¨ odel machines: Self-referential universal problem solvers making provably optimal self-improvements. In  Artiﬁcial general intelligence . Springer, in press.  

Schweizer, P. (1998). The truly total Turing test.  Minds and Machines ,  8 , 263–272.  

Searle, J. (1980). Minds, brains, and programs.  Behavioral & Brain Sciences , 3 , 417–458.  

Shieber, S. (1994). Lessons from a restricted Turing test.  CACM: Communi- cations of the ACM ,  37 .  

Simonton, D. K. (2003). An interview with Dr. Simonton. In J. A. Plucker (Ed.),  Human intelligence: Historical inﬂuences, current controversies, teaching resources . http://www.indiana.edu/  $\sim$  intell.  

Singer, E. (2007). A wiring diagram of the brain.  Technology Review . 19 November.  

Slatter, J. (2001).  Assessment of children: Cognitive applications . San Diego: Jermone M. Satler Publisher Inc. 4th edition. Slotnick, B. M., and Katz, H. M. (1974). Olfactory learning-set formation in rats.  Science ,  185 , 796–798. Smith, T. C., Witten, I. H., Cleary, J. G., and Legg, S. (1994). Objective evaluation of inferred context-free grammars.  Australian and New Zealand Conference on Intelligent Information Systems  (pp. 393–396). Smith, W. D. (2006). Mathematical deﬁnition of “intelligence” (and conse- quences).  http://math.temple.edu/ ∼ wds/homepage/works.html . Solomonoﬀ, R. J. (1964). A formal theory of inductive inference: Part 1 and 2.  Inform. Control ,  7 , 1–22, 224–254. Solomonoﬀ, R. J. (1978). Complexity-based induction systems: comparisons and convergence theorems.  IEEE Trans. Information Theory ,  IT-24 , 422– 432. Spearman, C. E. (1927).  The abilities of man, their nature and measurement . New York: Macmillan. Stern, W. L. (1912). Psychologischen Methoden der Intelligenz-Pr¨ ufung . Leipzig: Barth. Sternberg, R. J. (1985).  Beyond IQ: A triarchic theory of human intelligence . New York: Cambridge University Press. Sternberg, R. J. (Ed.). (2000).  Handbook of intelligence . Cambridge University Press.Sternberg, R. J. (2003). An interview with Dr. Sternberg. In J. A. Plucker (Ed.),  Human intelligence: Historical inﬂuences, current controversies, teaching resources . http://www.indiana.edu/  $\sim$  intell. Sternberg, R. J., and Berg, C. A. (1986). Quantitative integration: Deﬁnitions of intelligence: A comparison of the 1921 and 1986 symposia. What is intelligence? Contemporary wiewpoints on its nature and deﬁnition  (pp. 155–162). Norwood, NJ: Ablex. Sternberg, R. J., and Grigorenko, E. L. (Eds.). (2002).  Dynamic testing: The nature and measurement of learning potential . Cambridge University Press. Sutton, R., and Barto, A. (1998).  Reinforcement learning: An introduction . Cambridge, MA, MIT Press.Sutton, R. S. (1988). Learning to predict by the methods of temporal diﬀer- ences.  Machine Learning ,  3 , 9–44.  

Terman, L. M., and Merrill, M. A. (1950).  The Stanford-Binet intelligence scale . Boston: Houghton Miﬄin.  

Tesauro, G. (1995). Temporal diﬀerence learning and TD-Gammon.  Commu- nications of the ACM ,  39 , 58–68.  

Thurstone, L. L. (1924).  The nature of intelligence . London: Routledge.  

Thurstone, L. L. (1938).  Primary mental abilities . Chicago: University of Chicago Press.  

Treister-Goren, A., Dunietz, J., and Hutchens, J. L. (2000). The developmen- tal approach to evaluating artiﬁcial intelligence – a proposal.  Performance Metrics for Intelligence Systems .  

Treister-Goren, A., and Hutchens, J. L. (2001). Creating AI: A unique inter- play between the development of learning algorithms and their education. Proceeding of the First International Workshop on Epigenetic Robotics .  

Turing, A. M. (1950). Computing machinery and intelligence.  Mind .  

Voss, P. (2005). Essentials of general intelligence: The direct path to AGI. Artiﬁcial General Intelligence . Springer-Verlag.  

V’yugin, V. V. (1998). Non-stochastic inﬁnite and ﬁnite sequences.  Theoretical computer science ,  207 , 363–382.  

Wallace, C. S. (2005).  Statistical and inductive inference by minimum message length . Berlin: Springer.  

Wallace, C. S., and Boulton, D. M. (1968). An information measure for clas- siﬁcation.  Computer Jrnl. ,  11 , 185–194.  

Wang, P. (1995). On the working deﬁnition of intelligence  (Technical Re- port 94). Center for Research on Concepts and Cognition, Indiana Univer- sity.  

Watkins, C. (1989).  Learning from delayed rewards . Doctoral dissertation, King’s College, Oxford.  

Watkins, C. J. C. H., and Dayan, P. (1992). Q-learning.  Machine Learning , 8 , 279–292.  

Watt, S. (1996). Naive psychology and the inverted Turing test.  Psycoloquy , 7 .  

Wechsler, D. (1958). The measurement and appraisal of adult intelligence . Baltimore: Williams & Wilkinds. 4 edition.  

Willems, F., Shtarkov, Y., and Tjalkens, T. (1995). The context-tree weighting method: Basic properties.  IEEE Transactions on Information Theory ,  41 , 653–664. Witten, I. H. (1977). An adaptive optimal controller for discrete-time Markov environments.  Information and Control ,  34 , 286–295. Wolpert, D. H., and Macready, W. G. (1997). No free lunch theorems for optimization.  IEEE Trans. on Evolutionary Computation ,  1 , 67–82. Yerkes, R. M., and Yerkes, A. W. (1929).  The great apes: A study of anthropoid life . New Haven: Yale University Press. Zentall, T. R. (1997). Animal memory: The role of instructions.  Learning and Motivation ,  28 , 248–267. Zentall, T. R. (2000). Animal intelligence. In R. J. Sternberg (Ed.),  Handbook of intelligence . Cambridge University Press. Zvonkin, A. K., and Levin, L. A. (1970). The complexity of ﬁnite objects and the development of the concepts of information and randomness by means of the theory of algorithms.  Russian Mathematical Surveys ,  25 , 83–124.  

![](images/9da93e5cf164da1a160b98086a45140b57588b6cc266ee98de8bf9ce1de31ec9.jpg)  

# Index  

abstract thinking, 6 action space, 41 actions, 40, 72 Acton, Lord, 137 Adams, S., 136 Adaptive Intelligence Inc., 19 agent active, 39 AIXI, 48, 82, 93, 109, 137 choose own goals, 92 formal, 42 human, 83 informal, 40 optimal, 45 passive, 39 random, 78 self-optimising, 50, 64 simple, 80 specialised, 79 super intelligent, 82 agent-environment model, 39 AI winter, 125 Albus, J. S., 9 aliens, 135 alphabet, 41 Anastasi, A., 7 aperiodic, 63 Artiﬁcial general intelligence (AGI), 126 axons, 127  

backgammon, 126 Bayes’ rule, 25 Bayes, T., 25 Bellman equation, 46 Binet, A., 4, 13  

Bingham, W. V., 5 Block, N., 16 Blockhead argument, 89 BlueBrain project, 132 BlueGene supercomputer, 132 Boring, E. G., 9 brain, 128 brain simulation, 130 Bringsjord, S., 19  

Brooks, R., 136 C-Test, 20 Carroll, J. B., 4 Cattell, R. B., 4, 10 Chaitin, G., 20, 105 checkers, 127 chess, 2, 40, 126 Chinese room argument, 89 coenumerable function, 33 Colvin, 5 communicating class, 63 competitive games, 19 complexity Kolmogorov, 20, 31, 34, 98 Levin, 20, 93 of environments, 76 of prediction, 102 time, 21, 104 computable binary sequence, 97 computable function, 33 consciousness, 90, 127 control theory, 41, 73 cortex, 131 creativity, 90  

Dearborn, 5  

diversity control, 134 Dowe, D., 17 Drever, J., 7 dumb luck, 75  

eligibility trace, 111 emotion, 90 empirical future discounted re- ward, 112 enumerable function, 33 environment, 6, 40 accessible, 65 active, 57 Bandit, 57 bandit, 66 Bernoulli scheme, 54, 65 Classiﬁcation problem, 65 classiﬁcation problem, 61 computable, 74 ergodic, 64 formal, 42 function optimisation, 60, 67 higher order MDP, 58 Markov chain, 54, 62 Markov decision process (MDP), 57 Partially Observable MDP (POMDP), 59 passive, 56 repeated strategic game, 61, 66 reward summable, 73, 88 sequence prediction, 56, 67 totally passive, 55 wide range of, 74 Epicurus, principle of multiple ex- planations, 24 evidence, 26 evolution, 133 expected future discounted reward, 44, 73, 110  

ﬁtness function, 134 ﬁtness uniform optimisation, 135 Fogel, D. B., 9  

freewill, 90 g-factor, 4, 14 G¨ odel incompleteness, 105, 129 G¨ odel machine, 129 Galton, F., 13 Gardner, H., 3 genetic algorithm, 133 geometric temporal discounting, 45 Goertzel, B., 10 Good, I. J., 135 Google, 127 Gottfredson, L. S., 1, 6, 20 Gudwin, R. R., 9 Guilford, P. J., 3 HAL project, 18 Hawkins, J., 132 Hay, N., 137 Henmon, 7 Hernandez-Orallo, J., 20 HL( λ ) algorithm, 115 Horst, J., 10 human, 83 Hutter prize, 18 Hutter, M., 18, 21 imagination, 90 indiﬀerence principle, 36 inductive inference, 23 intelligence animal, 15 crystallized, 4 cultural bias, 12 deﬁnitions, 4, 9 dynamic tests, 13 eﬃciency, 11 everyday meaning, 91 explosion, 135 ﬂuid, 4 informal thesis deﬁnition, 6 IQ, 11, 14, 20 machine, 16 order relation, 71 static tests, 12  

tests, 11 theories, 3 universal, 77, 133 interaction history, 42, 44  

Jensen, A., 8 Joshua blue project, 18 Joy, B., 137  

knowledge, 7 Kronecker delta, 30, 115 Kurzweil, R., 10, 133, 137  

learning, 6 Levin, L., 32 linguistic complexity, 18 Loebner prize, 17 love, 90 lower semi-computable, 34 LSTD(  $\lambda$  ) algorithm, 111  

Mahoney, M., 17 Masum, H., 9, 19 maximum a posteriori (MAP), 39 maximum likelihood (ML), 39 McCarthy, J., 9 measure computable,    $\mathcal{M}_{c}$  , 33 enumerable,    $\mathcal{M}_{e}$  , 33 probability, 28 meat machine, 127 minimum description length (MDL), 39, 95 minimum message length (MML), 39, 95 Minsky, M., 10 mixture distribution, 29, 49 Moravec, H., 133 Neisser, U., 5 Newell, A., 10 No free lunch theorem, 93 non-deterministic polynomial time, NP, 22 Norvig, P., 136  

observations, 40, 73 Occam’s razor, 14, 25, 35, 75 Ockham, W., 25 Optimal ordered problem solver (OOPS), 129  

Pareto optimal, 49 Pell, B., 136 perception space, 41 perceptions, 40, 72 Pinter, 5 planning, 6 polynomial time, P, 21 Poole, D., 10 posterior distribution, 26 prediction with expert advice, 77 predictor computable, 95, 97 Solomonoﬀ, 38 prior distribution, 26 algorithmic, 34 dominant, 35 improper, 27 Solomonoﬀ, 30 Solomonoﬀ-Levin, 32, 35 Speed, 93, 109, 129 universal, 36 universal over environments, 47 prosperity, 137 psychometric tests, 19 Q( λ ), 122 qualia, 127 R. J. Sternberg, 4 randomness, Martin-L¨ of, 78 Raven, J., 14 reasoning, 6 Rees, M, 137 reference machine, 32 reinforcement learning, 41 reward, 40, 72 reward space, 41 Roadrunner supercomputer, 133  

Saﬀo, P., 136 Sarsa(  $\lambda$  ), 121 Search for Extraterrestrial Intelli- gence (SETI), 135 Searle, J., 16 self-optimising, 50 semi-measure, 32 sequence prediction, 27, 56 sequence prediction test, 21 Simon, H., 125 Simonton, D. K., 5 simple computable sequences, 100 Singularity Institute for Artiﬁcial Intelligence (SIAI), 136 Smith’s test, 21 Smith, W. D., 21 Snow, 5 Solomon o induction, 38 Solomonoﬀ, R. J., 30 soul, 90 Standford-Binet, 13 Sternberg, R. J., 5 supercomputer, 132 symbols, 41  

Wechsler adult intelligence scale, 14 Wechsler, D., 5, 13 Wikipedia, 18 Woodrow, H., 8 working memory, 14  

Yudkowsky, E., 137  

temporal diﬀerence learning, 110 temporal preferences, 44 Terman, L. M., 7, 13 text compression, 17 Thiel, P., 136 Thurstone, L. L., 3 Turing machine monotone universal, 96 preﬁx universal, 30 Turing test, 16 fail, 17  

universal inference, 36  

vitalism, 127 Voss, P., 19  

Wallach, W, 136 Wang, P., 11 Warwick, K., 9  