

# Lecture 8: Language model and Natural Language Generation

## Language Model

— Task of predicting the word coming to the next based on the given word

— a probabilistic model which predicts the probability that a sequence of tokens belongs to a language

- Language Modeling in Natural Language Generation

  — Conditional Language Modelling: 

  the task of predicting the next  word, given the words so far, and also some other input **x**:

  — Natural Language Generation

  - Dialogue (chit chat and task-based)

    x=dialogue history, y=next utterance

  - Abstractive Summarisation

    x=input text, y=summarized text

  - Machine Translation

    x=source sentence, y=target sentence

- Tips for using LM

  — collect and learn the model with the corpus that includes documents about the domain that your system/application will be used

## Traditional LM

##### Statistical Language Model (SLM): $p(A|B)=\frac{p(A \cap B)}{p(B)}=\frac{p(A.B)}{p(A)}$

- Conditional probability 

  $p(A|B)=\frac{p(A \cap B)}{p(B)}=\frac{p(A.B)}{p(A)}​$

- Conditional Language Modeling:

  — Predicting the next word, given the words so far, and also some other input x

  ![image-20190612105937012](/Users/jessicaxu/Library/Application Support/typora-user-images/image-20190612105937012.png)

##### N-gram LM

- N-gram: a sequence of N words=a chunk if n consecutive words

- N-gram model predicts the probability of a given N-gram within any sequence of words in the language

- Assumption: the next word, $x^{(t+1)}$, depends only on the preceding n-1 words

  ![image-20190612113406377](/Users/jessicaxu/Library/Application Support/typora-user-images/image-20190612113406377.png)

  ![image-20190612113514289](/Users/jessicaxu/Library/Application Support/typora-user-images/image-20190612113514289.png)

- Limitations

  - Trade-off Issue

    ![Screen Shot 2019-06-12 at 11.49.20 am](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 11.49.20 am.png)

    — OOV (small $n$) or Model Size(big $n$)

    — Optimal $n$

  - Zero count issue:

    **P(w|is spreading) =**Count(***is spreading w***) / Count(***is spreading***)

    1.  ***'is speeding w'*** never occur in the coupurs:

       — Consequence: the probability will be 0

       — Solution: Smoothing (add small value to the count for every $w$ in the courpus)

    2. ***'is spreding'***  never occur in the corpus 

       — Consequence: impossible to calculate the porbability for any $w$

       — Solution: Backoff (condition on ***'spreading'***)

## Neural Language model

##### Window-based NLM

![Screen Shot 2019-06-12 at 11.50.57 am](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 11.50.57 am.png)

- Pros

  — No Trade-off issue

- Cons

  — window size selection issue : incresing window size elarge $W$

  — Input vectors are multiplied by completely different wieghts in $W​$: No symmetry in how the inputs are processed

##### RNN-based LM

***Neural Network + Memory= Recurrent Neural Network***

- Basic architecture 

![Screen Shot 2019-06-12 at 11.57.10 am](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 11.57.10 am.png)

- Types of RNN

![Screen Shot 2019-06-12 at 12.00.05 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 12.00.05 pm.png)

- Example of prediction

  ![Screen Shot 2019-06-12 at 12.42.40 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 12.42.40 pm.png)

- Pros

  — can process any length input (sequence 2 sequence structure)

  — can use information from many step back (hidden state)

  — model size does not increase (sequence padding)

  — same weights applied on every time step (symmetry structure: share parameters)

- Cons

  — Slow computation (Neural nets)

  — Difficult to access information from many step back (coverage sequence no.)

##### Seq2Seq Model with trained language Model

- Teacher reforming: feed the gold target sentence into the decoder, regardess of what the decoder predicts

  ![Screen Shot 2019-06-12 at 12.45.43 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 12.45.43 pm.png)

## Natural Language Generation

##### Decoding Algorithm

***Greedin Decoding*** 

- Generate the sentence by taking argmax on each step of the decoder
  - Take the most probable word on each step
- Use that as the previous argmax ouput as the next word and feed it as input on the next step
- Keep going until you produce <EOS> (end token)
- ==LIMITATION==: cannot backtracking (no way to undo decisions)-> ungrammatical and unnatural
  - Solution: Exhaustive search decoding —> computing all possible sequences

![Screen Shot 2019-06-12 at 12.53.15 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 12.53.15 pm.png)

***Beam Search***

- A search algorithm which aims to find a high-probability sequence by tracking multiple possible sequences at once
- One each step of decoder, keep track of the ==k most probable partial sequences== (which we call hypotheses)
  - K: beam size (usually 5-10)
- After reaching stopping criterion, ==choose the sequence with the highest probability==(factoring in some adjustment for length)

![Screen Shot 2019-06-12 at 2.45.56 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 2.45.56 pm.png)

- The effect of beam size k

  - Small k: similar problems to greedy decoding

  - Large k: consider more hypothesis

    — solve the issues in greedy decoding

    — Conputationallu expensive

    — open-ended tasls like chit-chat dialogue, large k can make output more generic

    ![Screen Shot 2019-06-12 at 2.50.57 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 2.50.57 pm.png)

##### Sampling-based decoding

***Pure sampling***

— Each step t, randomly sample from the probability distribution $p_t$ to obtain next word. — Like greedy decoding, but using sample instead of argmax

***Top-n sampling***

— Each step t, randomly sample from $P_t$, restricted to just the top-n most probable words

— Like pure sampling, but truncate the probability distribution

— n=1 is greedy search, n=V is pure sampling 

— increase n to get more diverse/risky output

— Decrease n to get more generic/safe output

## Other NLG Approaches

##### Neural based NLG in Dialog: Issue

A naive application of standard seq2seq methods has serious pervasive deficiency

— Either because it's generic

— Or because changing the subject to something unrelated

— Boring response

— Repetition problem

— Lack of consistent persona problem

##### Template-based generation

- The most common approach in spoken natural language generation

- In simplest form, words fill in slots

  ![Screen Shot 2019-06-12 at 4.08.46 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 4.08.46 pm.png)

- Most common NLG used in comeercial system
- Used in conjunction with concatenative TTS to make natural sounding output
- Pros
  - Conceputally simple : No specialized knowledge required to develope
  - Tailored to the domain, so often good quality
- Cons 
  - Lacks generality: Repeatedly encode linguistic rules
  - Little variation in style
  - Difficult to grow/maintain: each utterace must be manually added
- Solution
  - Deeper utterance representations
  - Linguistic rules to manipulate them

##### Rule-based Generation

- Content planning 

  - What information must be communicated?

    — Content selection ordering

- Sentence planning

  - What words and ysntactic constructions will be used for describing the content

    — Aggretation: what elements can be grouped together for more natural-sounding, succinct output

    — Lexicalisation: what word are used to express the various entities?

- Realisation

  - How is it all combined into a sentence that is syntactically and morphologically correct ![Screen Shot 2019-06-12 at 4.40.13 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 4.40.13 pm.png)

- Example:

  ![Screen Shot 2019-06-12 at 4.42.03 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 4.42.03 pm.png)

##### Summarisation: two strategies

***Extractive Summarisation***

- slect parts (sentences) of the orginal text to form a summary

***Abstraction Summarisation***

- Generate new text using natural laguange generation techniques

![Screen Shot 2019-06-12 at 4.45.19 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 4.45.19 pm.png)

![Screen Shot 2019-06-12 at 4.46.18 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 4.46.18 pm.png)

## Language Model and NLG Evaluation

##### Evaluation

- ==Perplexity==

  - only caputre how powerful the model it is, but not the generation

  - $=​$ Exponential of the cross-entropy loss: ==Lower perplexity is better==

    ![Screen Shot 2019-06-12 at 4.48.02 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 4.48.02 pm.png)

    ![Screen Shot 2019-06-12 at 4.50.46 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-06-12 at 4.50.46 pm.png)

- No automatic metrics to adequately capture overall quality

- Some metrics to capture particular aspects of generated text

  - Fluency: compute probability-well-trained LM
  - Correct style: LM tranined on targert corpus
  - Diversity: rare word usage, uniqueness of n-grams
  - Relevance to input: semantic similarity measure
  - Simple things like length and repetition:
  - Task-specific metrics: compression rate for summarization
  - Human evaluation

  

  