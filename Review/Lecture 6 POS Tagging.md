# Lecture 6 POS Tagging

## POS overview

***Note 1. In English Tagset, larger and mode fine-grained tagsets are preferred***

***Note 2. lingustic structure: syntactic word classes***

![Screen Shot 2019-05-09 at 4.56.02 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-09 at 4.56.02 pm.png)

### Criteria for POS tagging

- Distrbutional

  — where can the words occur?

  — e.g. nouns can appear with possession, like "her car","his car"

  — e.g. different types of verbs have different distributional properties, "to jump","he jumps"

- Morphological

  — what for does the word have?

  — what affixes can it take?

  — e.g ness-, -tion,-ity, and -ance tend to indicate nouns

  — e.g. -ate, -ize tend to be verbs; -ing tend to be the present participle 

- Notional( or Semantic)

  — what sort of concept does the word refer to?

  — e.g nouns generally refer to living things, places, non-living thighs, or concepts

  —e.g. verbs refer to actions

### Purpose of POS tagging

- Useful in and of itself

  — TTS

  — Lemmatization

  — Linguistically motivation word clustering

- Useful as a pre-processing step for parsing
- Useful as features to downstream systems

### Issue of Pos tagging

- Ambiguous to identify the correct POS for word with more than one 

## Baseline Approaches

### Lexicon-based POS Tagging

- Look up the word in the dictionary and look up the POS tag
- Annotate word with tag without considering informatino
- Indentify words present on lexicon
- Aggregate result
- ==cannot handle== unknown words

### Rule-based POS Tagging

- Basic idea (old POS taggers):

  — identify possible POS tags for each word in the sentence based on lexicon.

  — use set of hand-craft rules to select a POS from each tag list for each word.

- Approach

  — Assign each token all their possible tags

  — Apply rules that eliminate all tags that are in consistent with its context 

  — Assign unknow word tokens a tag (the most frequent tag) that is consistent with its context

- Disadvantages:

  — used a large set of hand-crafted context-sensitive rules

  — ==cannot== eliminate all POS ambiguity

### N-gram with POS tagging

- Approaches:

  — take n-gram tokens as the input

  — calculate the frequency matrix of the POS given the POS of the preceding word

  — given a new text, tag the words from left to right with the most likely tag given the preceding one

- Problems:

  — Frequency matrix, will quicly get very large and sparse

  — One incorrect tagging choice might have unintended effects

  — No lookahead: choosing the "most probable" tag at one stage,

  might lead to highly imporbalble choice later.

  — As we are looking for the overall most likely tagging sequence given the bigram frequencies 

## Probabilistic Approaches

### **Hidden Markiv Model**

### Markove Chain

- A stochastic model used to model randomly changing system to 

  —  assumes the markov property

- A stochastic process has the Markov property, if

  — conditional propability of future state of the process

  — depends ==only on== the present state

  — ==not== on the previous states

#### *1. Markov Model(MM): Markov Property*

- Assumption: last k states are sufficient

- Process:

  — (r=1) first-order Markov process: $P(S_t|S_{t-1},…,S_0)=P(S_t|S_{t-1})​$

  — (r=2) second-order MP: $P(S_t|S_{t-1},…,S_0)=P(S_t|S_{t-1},S_{t-2})$

  ![Screen Shot 2019-05-09 at 5.56.32 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-09 at 5.56.32 pm.png)

- ==Example: P28-P31==: ***REVIEW!!!!***

![Screen Shot 2019-05-09 at 6.00.06 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-09 at 6.00.06 pm.png)

![Screen Shot 2019-05-09 at 6.00.24 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-09 at 6.00.24 pm.png)

![Screen Shot 2019-05-09 at 6.00.35 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-09 at 6.00.35 pm.png)

![Screen Shot 2019-05-09 at 6.03.02 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-09 at 6.03.02 pm.png)

#### *2. Hidden Markov Model (HMM)*

##### — Concept :

- A class of probabilistic graphical model

- Predict a sequence of unknown (hidden) varibales based a set of observed variables

  ![Screen Shot 2019-05-13 at 4.01.33 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-13 at 4.01.33 pm.png)

##### — Cal.: Joint Probs of a sequence of hidden states

- Initial state information (prior probability)

  — the initial probability of transitioning to a hidden state

- Transition data

  — the probability of transitioning to a new state conditioned on a present state

- Emission data

  — the probability of transitioning to an observed state conditioned on a hidden state

![Screen Shot 2019-05-13 at 4.18.30 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-13 at 4.18.30 pm.png)

##### — Evaluation

*What is ==the probability== that the observations were generated by a given model?*

— Observation sequence $X=x_1x_2x_3…x_t$

— Model $\lambda =(A,B,\pi)$

— $\alpha_t(i)$ : the $t^{th}$  observation is on $i$ state

— $b_i(x_t)$: the probability for $x_t$ under the $i$ state 

- ###### Foward Algorithm

  — Span a lattice of N states and T times 

  — Keep the sum of probabilities of all the paths coming to each state i at time t.

  —Forward Probability

  ***note. $S_j$ is a given state sequence***

  $P(O,Q|\lambda)=P(O|Q, \lambda)*P(Q| \lambda)$

  $P(O|Q,\lambda)=\Pi_{t=q}^Tp(x_t|q_t, \lambda)=b_{q_1}(x_1)b_{q_2}(x_2)…b_{q_T}(x_T)$

  $P(Q|\lambda)=\pi_{q_1}b_{q_1}(x_1)a_{q_1q_2}b_{q_2}(x_2)a_{q_2q_3}b_{q_3}(x_3)…a_{q_{(T-1)}q_t}b_{q_T}(x_T)$

   $\alpha(j)=P(x_1x_2…x_t,q_t=S_j | \lambda)​$

  ​	     $= \sum_{Q_t}P(x_1x_2…x_t,Q_t=q_1…q_t | \lambda)​$

  ​            $= \sum _{i=1}\alpha_{t-1}(i)a_{ij}b_j(x_t)​$

  - Initialiser: $\alpha_1(i)=\pi_ib_i(x_1)……..1\le i \le N​$

  - Induction

    $\alpha_t(j)=\sum_{i=1}\alpha_{t-1}(i)a_{ij}b_{j}(x_t)…1\le j\le N,t=2,3..,T​$

  - Termination

    $P(X|\lambda)=\sum \alpha_T(i)$

  ![Screen Shot 2019-05-13 at 4.37.21 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-13 at 4.37.21 pm.png)

- ###### Backward Algorithm

  — Initialization

  $\beta_T(i)=1……………………….1 \le i \le N​$

  — Induction

  $\beta_t(i)=\sum a_{ij}b_{j}(x_{t+1})\beta_{t+1}(j)……………..1 \le i \le N,t=T-1,T-2,…,1$

  ![Screen Shot 2019-05-13 at 5.22.07 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-13 at 5.22.07 pm.png)

  

##### — Decoding

*What is ==the most likely state observation==, given a model and a sequence of observation?*

*— Given a model $\lambda$*

*— Observation sequence: $X =x_1,x_2,…,x_T$*

*— Calculation: $P(X,Q|\lambda)=?$*                         		$Q^*=armax_QP(X,Q|\lambda)=argmax_QP(X|Q,\lambda)P(Q| \lambda)$ 

— A path or state sequence: $Q=q_1,L,q_T$

- ###### Viterbi Path

  — Span a lattice of N states and T times

  — Keep the probability and the previous node of the most probable path coming to each state i at time t

  — Recursive path selection

  1. Path probability 
  2. Path node

  ![Screen Shot 2019-05-13 at 5.57.44 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-13 at 5.57.44 pm.png)

- ###### Viterbi Algorithm

  ![Screen Shot 2019-05-13 at 5.58.32 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-13 at 5.58.32 pm.png)

#### 4. HMM with POS Tagging

- Transition

  ![Screen Shot 2019-05-14 at 9.50.37 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-14 at 9.50.37 am.png)

- Emission

  ![Screen Shot 2019-05-14 at 9.50.44 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-14 at 9.50.44 am.png)

- Trellis

  ![Screen Shot 2019-05-14 at 9.50.51 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-14 at 9.50.51 am.png)

#### 5. HMM Tagger Issue

- Unknown Words (OOV)

  — Solution 1: Use n-gram predict the correct tag

  — Solution 2: Use morphology (prefixes, suffixes ) or hypenation

- Independency Problem

  — Only depend on every state and its corresponding observed object

  — Sequence labelling: having relation to individual words, also to the observed sequence length, word context and others

### **Maximum Entropy Markov Models**(MEMM)

- Better expresion ability

  — Considering the dependencies between neighboring state and the entire observed sequence

- Reducing workload and keep consitency

  — Dosen't consider $P(X)$

  — Reducing the modeling workload

  — Learning the consistency between the target and the estimated function

  ![Screen Shot 2019-05-14 at 10.44.22 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-14 at 10.44.22 am.png)

- Label Bias Issue

  ![Screen Shot 2019-05-14 at 10.51.50 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-14 at 10.51.50 am.png)

### **Conditional Radonm Field**

#### Attribute Comparison (HMM, MEMM, CRF)

- CRF addressed the labeling bias issue and eliminated unreasonable hypothesis in HMM
- MEMM adopts local variate normalization
- CRF adopts global variance normalisation 

#### Model Comparison

- Generative Model

  — a model generate observed data randomly

  — model the joint probability p(x,y)

- Discriminative model (CRF)

  — directly estimate the posterior probability p(x|y)

  — aim at modeling the" discrimination" between different outputs

- Topological structure

  — HMM and MEMM are a directed graph

  — CRF is an undirected graph

#### Global optimum or local optimum comparison

- HMM

  — directly models the transition probability 

  — directly models phenotype probability (emission)

  — cal. the probability of co-occurence 

- MEMM

  — establish the probability of co-occurrence based on the transition probability and the phenotype probability

  — cal. the conditional probability and only adopts the local variance normalization, making it easy to fall into a local optimum

  

- CRF

  — cal. the normalization probability in the global scope

  — optimal global solution and resolves the labeling bias issue

#### CRF Advantages and disadvantage

- with HMM

  — no strict independence assumptions

  — can accommodate any context information

- with MEMM

  — computs the conditinoal probabilities of global output nodes

  — overcoms the drawbacks of label bias in MEMM

- disadv.

  — computationally complex at the training stage of the algorithm 

  — very difficult to re-train the model with updated new data

## Deep learning approaches

![Screen Shot 2019-05-14 at 2.32.34 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-14 at 2.32.34 pm.png)

![Screen Shot 2019-05-14 at 2.53.13 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-14 at 2.53.13 pm.png)

![Screen Shot 2019-05-14 at 2.51.51 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-14 at 2.51.51 pm.png)













