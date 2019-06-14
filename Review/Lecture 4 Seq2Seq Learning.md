# Lecture 4 Seq2Seq Learning

## Overview

1. Problems abstraction

   1. N-to-1: sentiment analysis,topic classification
   2. N-to-N: pos tagging
   3. N-to-path: parsing
   4. N-to-M: dialog, translation

2. Application

   1. Speech recognition

      — input: speech signal

      — output: text

   2. Movie frame labelling

      — input: video frame

      — output: scene labels

   3. PoS tagging

      — input: text

      — output: part of speech

   4. Arithmetic calculation

      Math expression -> Numbers

   5. Machine Translation

      Eng. Text -> Chinese Text

   6. Sentence Completion

      Partial sentence -> partial sentence

   7. Coversational Modelling

      Utterance -> utterance

## Seq2Seq with DL

### Recurrent Neural Network (RNN=Neural Network + Memory)

==Memory==*: retention of the information over time to influence the future action*

#### Model Structure

— Solve the limitation of CNN for that input and output has to be the same length

— Process sequence of vectors $X$ by applying ==*recurrent function*== at every time steps  

— The RNN state consists of a single hidden vector $h$

- ​    $h_t$: current state cal. by the last state and the current input
- ​    $h_{t-1}$ : the state of the last time step 
- ​    $x_t​$: the input at the current time step

***Note.****every categorical inputs are named as 'time step'*

![Screen Shot 2019-05-08 at 3.00.21 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 3.00.21 pm.png)

— The $ tanh$ function to project the linear output to the non-linear interval $[-1,1]​$

— Share the weights with all inputs

![Screen Shot 2019-05-08 at 3.17.39 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 3.17.39 pm.png)

#### Backward propagation through time (BPTT)

— Forward through entire sequence to compute loss

— Backward through entire sequence to compute gradients

![Screen Shot 2019-05-08 at 3.21.50 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 3.21.50 pm.png)

#### Truncated Backpropagation through time (TBPTT)	

— Run forward and backward through chucks of sequences instead of whole sequence

— Carry hidden state in time forever 

— Only backpropagate for some smaller number of steps

![Screen Shot 2019-05-08 at 3.26.34 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 3.26.34 pm.png)

#### Limitation of vanilla RNN

— ==Gradients Exploding==: if using active function $Relu$

$Relu: \phi(x)=max(0,x)$ 

$\frac{\alpha \phi(x)}{\alpha x} =1 (x >0) \ or \ 0(x\le0)$

![Screen Shot 2019-05-08 at 4.45.28 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 4.45.28 pm.png)





— ==Vanishing gradients reason==:  The $taugh$ function output a lot of values under $1$, therefore, according to the chain rule,  the value of a gradient rate are too small and the model stops learning or takes too long to learn (equation 2)

The $taugh$ function will project all weighted outputs to values between $[-1,1]$, when using BPTT to calculate the gradients thus minimizing $loss$:

- loss function: cross entropy $E(y,\hat{y})=\sum_tE(y_t,\hat{y}_t)=-\sum_ty_tlog(\hat{y}_t)$
-  Cal. Gradients: $\frac{\alpha E}{\alpha W}=\sum_t \frac{\alpha E_t}{\alpha W}$
- $h_t=taugh(w_{hh}h_{t-1}+w_{ht}x_t)…………………………………….(1)​$
- $\sum_t \frac{\alpha E_t}{\alpha W}=\sum_t\frac{\alpha E_t}{\alpha \hat{y}_t}\frac{\alpha \hat{y}_t}{\alpha h_t}\frac{\alpha h_t}{\alpha h_{t-1}}\frac{\alpha h_{t-1}}{\alpha h_{t-k}}...\frac{\alpha h_{t-k}}{\alpha W}……………………………(2)$

— ==Long term dependency==

![Screen Shot 2019-05-08 at 3.34.23 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 3.34.23 pm.png)

### Long Short-term Memory (LSTM)

#### LSTM cell internal structure

— Finall output state at the current time step: $c_t=f_t*c_{t-1}+i_t*\tilde{c_t}$

![preview](https://pic2.zhimg.com/v2-e1cb116af01ef77826cd55bc1f8e5dd9_r.jpg)

![preview](https://pic2.zhimg.com/v2-d7fc6f5ee5dd07d2662bceca25488fe5_r.jpg)

![image-20190508170428742](/Users/jessicaxu/Library/Application Support/typora-user-images/image-20190508170428742.png)

#### Forget gate

—$f_t$ is the forget gate to decide how much information should be discarded

— $h_{t-1}$ and $x_t$ are inputs for $f_t$

— $sigmoid$ function to covert values in [0,1]: $\alpha(z)=\frac{1}{1+exp(-z)}$

![preview](https://pic4.zhimg.com/v2-89ddea95073d6cb76623af1e33fbd3c3_r.jpg)

####  Cell state

—$\tilde{c_t}: $ the update of the cell state 

![Screen Shot 2019-05-08 at 5.37.31 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 5.37.31 pm.png)

#### Input gate

— $i_t:$ input gate to control how much indormation from $\tilde{c_t}$ will be used fo

![preview](https://pic1.zhimg.com/v2-9f2646898e9d3f82fd022735b8ec6f80_r.jpg)

#### Output gate

— calculate the output of the hidden state $h_t$

— output the $y_t$ and the input for the next time step $c_t$

![preview](https://pic2.zhimg.com/v2-3edbdda4409cb1774c03bb459fa4a6e5_r.jpg)



### Gated Recurrent Unit (GRU)

— A simlified LSTM cell,only have two gate: update gate($z_t$) and reset gate($r_t$)

![Screen Shot 2019-05-09 at 11.50.09 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-09 at 11.50.09 am.png)

#### Update gate:

1. controlling how much information from the last state will be carried on in this state.
2. the larger the value of $z_t$ is, the more information is carried on
3. useful to capture the long-term memory 

#### Reset gate:

1. combining the forget gate and input gate
2. controlling how much information from the last state will be discard
3. the smaller the value of $r_t$ is, the more information is discarded
4. useful to capture the shor-term memory

#### GRU Forward feed

- reset gate: $$r_t=sigmoid(W_r*[h_{t-1},x_t]+b_r)​$$
- update gate: $z_t=sigmoid(W_z*[h_{t-1},x_t]+b_z)$
- candidate hidden state at current time step: $\tilde{h_t}=tanh(W_{\tilde{h_t}}*[r_t*h_{t-1},x_t]+b_h)$
- hidden state at current time step: $h_t=z_t*h_{t-1}+(1-z_t)* \tilde{h_t}$
- $y_t=sigmoid(W_o*h_t)​$

#### GRU BPTT: Learning Parameter

$W_r=W_{rx}+W_{rh}$

$W_z=W_{zx}+W_{zh}$

$W_{\tilde{h}}=W_{\tilde{h}x}+W_{\tilde{h}h}$

Input for the output layer: $y_t^i=W_oh$

Output for the output layer:$y_t^o=sigmoid(y_t^i)$

The MSE loss at the t time step: $E_t=\frac{1}{2}(y_d-y_t^o)^2$, total: $E=\sum_{t=1}^TE_t$

The error $\delta $: $\frac {\alpha E}{\alpha W_o}=\delta_{y_t} h_t$  

​			$W_r​$ : $\frac {\alpha E}{\alpha W_{zx}}=\delta_{z_t} x_t​$ , $\frac {\alpha E}{\alpha W_{zh}}=\delta_{z_t} h_{t-1}​$

​			$W_z​$ : $\frac {\alpha E}{\alpha W_{\tilde{h}x}}=\delta_{t} x_t​$, $\frac {\alpha E}{\alpha W_{\tilde{h}h}}=\delta_{t} (r_t,h_{t-1})​$

​			$W_{\tilde{h}}​$ : $\frac {\alpha E}{\alpha W_{rx}}=\delta_{r_t} x_t​$, $\frac {\alpha E}{\alpha W_{\tilde{h}h}}=\delta_{r_t} h_{t-1}​$

#### Comparison (LSTM vs GRU)

![Screen Shot 2019-05-09 at 11.59.40 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-09 at 11.59.40 am.png)

![Screen Shot 2019-05-09 at 12.00.15 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-09 at 12.00.15 pm.png)

### Application: POS Tagging

![Screen Shot 2019-05-09 at 12.04.50 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-09 at 12.04.50 pm.png)

## Seq2Seq Encoding and Decoding

 ![Screen Shot 2019-05-09 at 1.01.27 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-09 at 1.01.27 pm.png)



