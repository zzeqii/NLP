# Lecture 2  Word Embedding and Representation

## Word Meaning based Representation

1. Definition: Meaning

   the ideas that it represents and which can be explained using other words

   the thoughts and ideas that are intended to be expressed by it 

2. Resources: WordNet(NLTK)

   **calculating the similar words based on lemmas**

   `from nltk.corpus import wordnet as wn`

   `pose={'n':'noun','v':'verb','s':'adj(s)','a':'adj','r':'adv'}`

   `for synset in wn.synsets("good"):` 

   `print("{}:{}".format(pose[synset[synset.pos()]],", ".join([l.name() for l in synset.lemmas()])))`

   ![Screen Shot 2019-05-06 at 10.02.20 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-06 at 10.02.20 am.png)

   **Calculating similar synsets based  on hypernyms**

   `from nltk.corpus import wordnet as wn`

   `panda=wn.synset("panda.n.01")`

   `hyper=lambda s:s.hypernyms()`

   `list(panda.closure(hyper))`

   ![Screen Shot 2019-05-06 at 10.06.31 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-06 at 10.06.31 am.png)

3. Problems

   1. missing nuance: synonyms is not correct all the time
   2. missing new meanings of words: impossible to keep up-to-date
   3. subjective
   4. need human labor to create and adapt
   5. difficult to compute accurate word similarity

## Count based Word representation 

***Sparse representation: high-dimensional features***

### One-hot Encoding

1. every word is represented by a binary vector with index 0 and 1

2. vecotor dimention =(1,len(vocaubulary) )

   ![Screen Shot 2019-05-06 at 11.39.06 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-06 at 11.39.06 am.png)

3. *Manual Coding one-hot*

   ![Screen Shot 2019-05-06 at 11.57.14 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-06 at 11.57.14 am.png)

   ![Screen Shot 2019-05-06 at 11.59.14 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-06 at 11.59.14 am.png)

   ![Screen Shot 2019-05-06 at 11.59.23 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-06 at 11.59.23 am.png)

   

   **Problems**

   1. No word similarity: "sydeny hotel" (supposed)->"sydney inn"

   2. Inefficiency : vector dimension =len(vocab)

      ​			 word only be represent with 1, others are 0s

### Bag of words(BOW)

1. *Representation of text*

   The occurrence of the word in the document 

   — a ==vocabulary== of known words

   — a ==measure== of the presence of known words

2. *Discarding the order and the structure of the word*

   Only concerned on the occurence of known words in the document, not the  location of known words in the document 

3. *Problem*

   1. discard the order and structure will ignore the context
   2. ignore the meaning of the word,thus ignore the semantic analysis

### Term Frequencey-Inverse Document Frequency (TF-IDF)

   ***Representing how important a word to a document in a corpus***

1. TF=BOW

   The count of how many times a word occurs in a given document 

2. IDF

   The number of times a word occurs in a corpus of documents 

   **$$\mathbb{w_{i,j} = tf_{i,j} *log(\frac{N}{df_i})}​$$**

   $$w_{i,j}=weight \ of  \ term \ i \ in \ document \ j$$

   $$tf_{i,j}=number \ of  \ the \ occurence \ of \ term \ i \ in \ document \ j$$

   $$\mathbb{N} = total \  number \ of \ documents$$

   $$df_i= number \ of \ document \ contain \ term \ i   $$

3. IDF is the way to scale up the rare word in the document and normalize the  common words in the corpus 

## Prediction Based Word Representation

***Word Embedding: low dimensional,distributed representations***

### Word2Vec: *representing the words similarity*

#### Contimuous Bag of Words (CBOW)

***Predicted center word given (bag of ) context words***

1. Assumption:

   — Predicted the center word by around context

   — Context are selected as input by using sliding window

   ![Screen Shot 2019-05-06 at 3.11.17 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-06 at 3.11.17 pm.png)

2. Neural network archetecture

   — input (context) and output (context) are one-hot vector

   — $$W_i\ : \ V*N$$ weight matrix between input layer and hidden layer

   — $$W_j\ : \ N*V$$ weight matrix between output layer and hidden layer

   **Iuput Layer**

   1. Generate one-hot:  the input one hot  $$x _{(c−m) }, . . . , x _{(c−1)} , x _{(c+1)} , . . . , x _{(c+m)} $$ for the input context of size m, where $$x_c$$ is the center word. We have $$ C(= 2m) $$ one hot word vector of size $[1*V]$. so our input layer size is$ [C * V]$
   2. N-dimension: multiply $W_i​$ ,return embedded words $[1*N]​$
   3. Averaging: take average of these $2m [1 * N] ​$vectors

   ![Screen Shot 2019-05-06 at 3.29.37 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-06 at 3.29.37 pm.png)

   

   **Output Layer**

   1. score vector: calculate the hidden layer output by multiplying hidden layer input with matrix $W_j$. Now we get a score vector of size$[1 * V]$. lets name it $z$.

   2. Classifier: score probability $\hat{y }= softmax(z)$

   3. Evaluation(Cross-entropy): error between output and target is calculated and propagated back to re-adjust the weights

      $$H(\hat{y},y)=- \sum_{j=1}^{|v|}ylog(\hat{y})​$$

   ![Screen Shot 2019-05-06 at 3.31.50 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-06 at 3.31.50 pm.png)

#### Continuous Skip-gram:

**Predicted context( "outside") words (position independent) given a centre word**

![Screen Shot 2019-05-06 at 4.17.40 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-06 at 4.17.40 pm.png)

**Iuput Layer**

1. Generate one-hot:  the input centert word  $x_c$ ,where $$x _{(c−m)} , . . . , x_{ (c−1)} , x _{(c+1) }, . . . , x _{(c+m)} $$ are context words to predict. so our input layer size is $$[1*V]$$
2. N-dimension: multiply $W_i$ ,return embedded words $[1*N]$

**Output Layer**

1. score vector: calculate the hidden layer output by multiplying hidden layer input with matrix $W_j$. Now we get $C(=m) $ score vector of size$[1 * V]$. lets name it $z​$.

2. Classifier: score probability $\hat{y }= softmax(z)$

3. Evaluation: error between output and target is calculated and propagated back to re-adjust the weights![Screen Shot 2019-05-06 at 3.31.50 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-06 at 3.31.50 pm.png)

   ***==Note.== $\hat{y}$ are all same but their target vectors are different so they all give different error vectors and Element-wise sum is taken over all the error vectors to obtain a final error vector.***

#### Word2vec Summary

##### Idaeas

1. Have a large corpus

2. Every word in a fixed vocabulary is represented by a vector

3. Go through each position $t$ in text, which has a center word $c$ and context words $o$

4. Use the similarity of the word vectors for $c$ and $o$ to calculate the probability of $o$ given $c$

5. Keep adjusting the word vectors to maximize this probability

   

##### Problems

1. Cannot cover the morphological similarity

   — every word as an independent vector

2. Hard to conduct embedding for rare words

   — based on the hypothesis distrbution

   — dosen't embed the rare words

3. Cannot handle the OOV (Out-of-Vocabulary)

   — only work if the word is in the vocabulary

##### Comparison 

![Screen Shot 2019-05-08 at 9.41.05 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 9.41.05 am.png)

##### Word Embedding Evaluation

![Screen Shot 2019-05-08 at 10.00.01 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 10.00.01 am.png)

1. Intrisic word vector evaluation

   *Evaluate word vectors by how well their ==cosine distance== after addition captures intuitive semantic and syntactic analogy questions*

   ![Screen Shot 2019-05-08 at 10.14.22 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 10.14.22 am.png)

### FastText

1. Handling the limitation 2 of the Word2Vec

2. Instead of feeding individual words into the neural network, it take breaks words into several n-grams(sub-words)

3. FastText with n-grams

   — breaking down words with n-grams

   — feed n-grams into the neural network (CBOW or Skip-gram)

   — rare words can be properly represented as it's most likely that some of their n-grams also appears in other words

### Global Vectors (CloVe)

1. Handling limtation 1 of Word2Vector
2. Training on global co-occurence counts rather than on separate local context windows 

