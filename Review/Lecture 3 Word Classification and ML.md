# Lecture 3 Word Classification and ML

## Deep Learning with NLP

### A neuron: Function,Parameter,Cost, Optimiser, and Gradient

*— Input: x=number of apple given by Lisa*

*— Output: y=number of banana received by Lisa*

*— Parameter: Need to be estimated*

1. Function

   — data: input,output

   — model: $Y=WX+b$

2. Parameter

   — estimate the $w$, $b$ to optimise objective function

3. Cost

   — Sqare loss: $C(w,b)=\sum(y_n-\hat{y}_n)^2$

4. Optimiser

   — $arg min \ C(w,b)$

   — $w,b \in [-\infty,\infty ]$

5. Gradient

   — $\hat{w}=w-lr*grads​$

   —  $\hat{w} '=\hat{w}-lr*(current \ y-desired \ y)*grads(current)*existing \ input$

   ![Screen Shot 2019-05-08 at 11.13.25 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 11.13.25 am.png)

   ![Screen Shot 2019-05-08 at 11.04.09 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 11.04.09 am.png)

### Parapeters vs Hyper-parameter

1. Parameters:
   - tunable components of model
   - learnt from training data
   - Eg. probabilities, feature weights  
2. Hyper-parameters:
   - Variables that controls how parameters are learnt
   - Chosen a priori or tuned using held-out data
   - Eg. Model size(depth,complexity), learning rate

### Non-linear Neural Network

#### Multilayer Perceptron

1. Loss function:

   - $S(y)=O(wx+b)$

   - $O(t)= \frac{1}{1+e^{-t}}$

     ![Screen Shot 2019-05-08 at 11.39.47 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 11.39.47 am.png)

2. Objective function

   ***layer 1=input features***

   ***layer 2=add and or combinations***

   $y=(w_1x+b_1)S_1+(w_2x+b_2)S_2+(w_3x+b_3)S_3$

   1. Layer 1: $S_1=O(w_4x+b_4)​$

   2. Layer 2: $S_2=O(w_5S_1+w_6S3+b_5)$

   3. Layer 1: $S_3=O(w_7x+b_6)$

      ![Screen Shot 2019-05-08 at 11.56.59 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 11.56.59 am.png)

      ![Screen Shot 2019-05-08 at 11.57.13 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 11.57.13 am.png)

      ![Screen Shot 2019-05-08 at 11.57.31 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 11.57.31 am.png)

      ![Screen Shot 2019-05-08 at 12.04.26 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 12.04.26 pm.png)

### Evaluation

![Screen Shot 2019-05-08 at 12.09.01 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 12.09.01 pm.png)

![Screen Shot 2019-05-08 at 12.10.56 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 12.10.56 pm.png)

### Score nomalization and Cost function 

![Screen Shot 2019-05-08 at 12.19.57 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 12.19.57 pm.png)

### Parameter update

1. Backpropagation ![Screen Shot 2019-05-08 at 11.13.25 am](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 11.13.25 am.png)

2. Gradient(=slope)

   ![Screen Shot 2019-05-08 at 11.04.09 am](/Users/jessicaxu/Desktop/Screen%20Shot%202019-05-08%20at%2011.04.09%20am.png)

3. Gradient decent optimisation

### Summary

![Screen Shot 2019-05-08 at 12.24.37 pm](/Users/jessicaxu/Desktop/Screen Shot 2019-05-08 at 12.24.37 pm.png)



