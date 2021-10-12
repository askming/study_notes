# Boosting

## The Idea of Additive Training
- In boosting, we use an additive strategy: fix what we have learned, and add one new tree at a time. Let the prediction at step $t$ as $\hat{y}_i^{(t)}$, then

$$
\begin{array}{ll}
\hat{y}_i^{(0)} &= 0\\
\hat{y}_i^{(1)} &= f_1(x_i) = \hat{y}_i^{(0)} + f_1(x_i)\\
\hat{y}_i^{(2)} &= f_1(x_i) + f_2(x_i)= \hat{y}_i^{(1)} + f_2(x_i)\\
&\dots\\
\hat{y}_i^{(t)} &= \sum_{k=1}^t f_k(x_i)= \hat{y}_i^{(t-1)} + f_t(x_i)
\end{array}
$$

- Or more generally, Forward stagewise additive modeling (FSAM)
  
  $$
  f_{t} = \sum_{i=1}^{t}v_ih_i = f_{t-1} + v_ih_t
  $$


- Depending of the format of the loss function, it leads to different boosting methods
    - $L^2$ Boosting: squared loss
  - 
      $$l(y, \hat{y}) = \sum_i\left[y_i - (f_{t-1}(x_i) + vh_t(x_i))\right]^2$$

    - AdaBoost: exponential loss
      
      $$l(y, f(x)) = \exp(-yf(x))$$

    - BinomialBoost: logistic loss
      
      $$l(y, f(x)) = \log(1 + e^{-yf(x)})$$
      
## Gradient Boosting
- Gradient boosting isn't about the format of the loss function, instead, it refers to using the gradient method to find the optimal solution in boosting models. It can be used in "any" loss function

- To choose the next model, we optimize the following objective function at step $t$
  
  $$
  \begin{array}{ll}
  \text{obj}^{(t)} & = \sum_{i=1}^n l(y_i, \hat{y}_i^{(t)})  \\
            & = \sum_{i=1}^n l(y_i, \hat{y}_i^{(t-1)} + f_t(x_i)) 
  \end{array}
  $$


  - In general, we take the *Taylor expansion* of the loss function up to the second order:
    
    $$\text{obj}^{(t)} = \sum_{i=1}^n [l(y_i, \hat{y}_i^{(t-1)}) + g_i f_t(x_i) + \frac{1}{2} h_i f_t^2(x_i)] $$
    
    where 
    
    $$
    \begin{array}{ll}
    g_i &= \partial_{\hat{y}_i^{(t-1)}} l(y_i, \hat{y}_i^{(t-1)})\\
    h_i &= \partial_{\hat{y}_i^{(t-1)}}^2 l(y_i, \hat{y}_i^{(t-1)})
    \end{array}    
    $$
  
  - After removing all the constants, the specific objective function at step $t$ becoms
    
    $$
    \sum_{i=1}^n [g_i f_t(x_i) + \frac{1}{2} h_i f_t^2(x_i)]
    $$
    
    - the value of the objective function only depends on $g_i$ and $h_i$. This is how gradient Boost supports custom loss functions. We can optimize every loss function, including logistic regression and pairwise ranking, using exactly the same solver that takes $g_i$ and $h_i$ as input!

### Tree gradient boosting
- Learning tree structure is much harder than traditional optimization problem where you can simply take the gradient. It is intractable to learn all the trees at once.
- To apply gradient boosting algorithm to decision tree model, first we need to mathematically defined a tree 
  
  $$
  f_t(x) = w_{q(x)}, w \in R^T, q:R^d\rightarrow \{1,2,\cdots,T\}.
  $$
    
    - $w$ is the vector of scores on leaves
    - $q$ is a function assigning each data point to the corresponding leaf, i.e. the actual splitting of leaves 
    - $T$ is the number of leaves 

#### Loss function for XGBoost
- This is how XGBoost different from other boosting models: in addition to minimize the loss, XGBoost also penalizes the model complexity 

$$\text{obj}^{(t)} = \sum_{i=1}^n l(y_i, \hat{y}_i^{(t)}) + \sum_{i=1}^t\Omega(f_i)$$

#### Model Complexity in XGBoost
- In XGBoost, the complexity of a tree $f_t(x)$ is defined as 
  
  $$
  \Omega(f) = \gamma T + \frac{1}{2}\lambda \sum_{j=1}^T w_j^2
  $$
  
#### Final loss function for XGBoosted trees
- By plugging in the model complexity term, the loss function for XGBoosted tree becomes

$$
\begin{array}{ll}
\text{obj}^{(t)} &\approx \sum_{i=1}^n [g_i w_{q(x_i)} + \frac{1}{2} h_i w_{q(x_i)}^2] + \gamma T + \frac{1}{2}\lambda \sum_{j=1}^T w_j^2\\
&= \sum^T_{j=1} [(\sum_{i\in I_j} g_i) w_j + \frac{1}{2} (\sum_{i\in I_j} h_i + \lambda) w_j^2 ] + \gamma T
\end{array}
$$

- With futher simiplification:
  
  $$
  \text{obj}^{(t)} = \sum^T_{j=1} [G_jw_j + \frac{1}{2} (H_j+\lambda) w_j^2] +\gamma T
  $$

  where
  - $G_j = \sum_{i\in I_j} g_i$
  - $H_j = \sum_{i\in I_j} h_i$
  - $I_j = \{i|q(x_i)=j\}$ is the set of indices of data points assigned to the $j$-th leaf.

- $w_j$ are independent with respect to each other and since the above loss function is a quadratic function of $w_j$, it can be solved for the "best" $w_j$ for a given structure $q(x)$ can be solved as 
  
  $$ w_j^\ast = -\frac{G_j}{H_j+\lambda} $$

- And the objective reduction we can get is 
  
  $$
  \text{obj}^\ast = -\frac{1}{2} \sum_{j=1}^T \frac{G_j^2}{H_j+\lambda} + \gamma T
  $$

#### Learn the tree structure
- Exploring all possible next tree is intractble, in practice we try to optimize one level of the tree at a time. Specifically we try to split a leaf into two leaves, and the score it gains is

  $$
  Gain = \frac{1}{2} \left[\frac{G_L^2}{H_L+\lambda}+\frac{G_R^2}{H_R+\lambda}-\frac{(G_L+G_R)^2}{H_L+H_R+\lambda}\right] - \gamma
  $$

  - the score on the new left leaf
  - the score on the new right leaf
  - the score on the original leaf
  - regularization on the additional leaf

- If the gain is smaller than $\gamma$, we would do better not to add that branch.This is exactly the **pruning** techniques in tree based models! By using the principles of supervised learning, we can naturally come up with the reason these techniques work

- For real valued data, we usually want to search for an optimal split. To efficiently do so, we place all the instances in sorted order and search for the best split

#### Some notes on XGBoost
- The name `xgboost` actually refers to the engineering goal to push the limit of computation resources for boosted tree algorithms. Which is the reason why many people use xgboost. For model, it might be more suitable to be called it as regularized gradient boosting.

- Both `xgboost` and `gbm`(gradient boosting machines) follows the principle of gradient boosting.  There are however, the difference in modeling details. Specifically, `xgboost` used a more regularized model formalization to control over-fitting, which gives it better performance.

- XGBoost is normally used to train gradient-boosted decision trees and other gradient boosted models. Random forests use the same model representation and inference, as gradient-boosted decision trees, but a different training algorithm. 


### Reference
[Introduction to Boosted Trees — xgboost 1.0.0](https://xgboost.readthedocs.io/en/latest/tutorials/model.html)
[Introduction to Boosted Trees by Tianqi Chen](http://homes.cs.washington.edu/~tqchen/pdf/BoostedTree.pdf)