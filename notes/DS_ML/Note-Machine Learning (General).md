# ​Machine Learning (General)

## Machine learning project check list

### 1. Frame the problem & look at the big picture

- Supervised/unsupervised/reinforcement problem?
- Regression/classification/clustering etc.?
- Select a performance measure
  - Regression: RMSE/MAE
  - Classification: Precision/recall; sensitivity/specificity; log-loss; Gini; entropy

### 2. EDA

- Visualizing the data
  - distribution
  - correlation
  - feature engineering (can also be a step in next process)
- Missing data

### 3. Data preparation

- Cleaning: missing data, label encoding
- Handling categorical data: one-hot encoding
- Feature scaling
- Feature engineering: create new features

### 4. Select & train a model

- Use cross-validation as the mean to evaluate the overall performance of a model

  ```python
  from sklearn.model-selection import cross-val-score
  ```

- Recommend to save both the hyperparameters & the trained parameters as well as the CV scores, and the predictions using `joblib` from external module

### 5. Fine tune the model

- Grid search: `GridSearchCV` in `model-selection` module
- Random search: `RandomizedSearchCV`
- Ensemble methods: combine the models that perform best
- Analyze the best model and their error
- Evaluate the system on the test data

## Model performance metrics vs model's cost function

- Cost function: used in estimating the model parameters, the process of minimizing the cost/loss function leads to the optimized model specification given the data; e.g. MSE, gini impurity measure
- Performance measures: used in evaluating the predictions performance from a model identified from the data; e.g. AUC, log-loss, F1 score.
- Different ML models use different cost/loss function for parameter estimation, it's the "built-in" feature of a specific ML algorithm. When models are trained, their predictive capabilities are compared under the same performance measure/metric.

### Model performance metrics: classification

#### log-loss vs. AUC vs. F1 score

- AUC computed w.r.t. binary classification with a varying decision threshold 
- Log-loss takes "certainty" of classification into account' it conceptually goes beyond AUC and is especially relevant in imbalanced data or unequally distributed error cost
  
  $$-(y\log(p) + (1-y)\log(1-p)) \leftarrow \text{ well calibrated}$$ 
  
  - a measure of accuracy that incorporates the idea of probabilistic confidence
  
  $$F_1 = \left.( \frac{2}{\text{recall}^{-1} + \text{precision}^{-1}}\right.),$$
  
  where recall = sensitivity (true positive rate, TPR) and precision = positive predicted value(PPV, i.e. true positive/predicted positive)

- If care about absolute probabilistic difference, go with log-loss
- If only care about final class prediction, and don't want to tune threshold, go with AUC
- F1 is sensitive to threshold
- If data is imbalanced
  - Go with F1 if have less positive cases, e.g. fraud detection
  - Go with AUC if don't care about the actual class

## Multiclass vs. multilabel vs. mutioutcome classification

- Multiclass 

  - OVA (one vs all): $K$ model if there are $K$ classes; preferred for most binary classifier
  - OVO (one vs one): $\frac{K(K-1)}{2}$ models if there are total $K$ classes; faster to train, leads to many small sets

  Random forest can directly classify instances into multiple classes

- Multilabel: each instance have more than one outcome to predict, e.g. a photo with multiple people in it

- Multioutcome: multiclass + multilabel, i.e. each outcome, among multiple outcomes, is a multiclass perdition task

## Gini impurity vs. entropy

- Gini impurity for node $i$:

  $$G_i = 1 - \sum_{k=1}^K p_{i, k}^2,$$ 
  
  $k$ is the class index

- Entropy for node $i$:

  $$H_i = -\sum_{k=1, p_{i,k}\ne0}^K p_{i, k}\log(p_{i, k})$$

- Most of the times, $G_i$ or $H_i$ don't make big difference, while $G_i$ is slightly faster to compute

- Gini trends to isolate the most frequent class in its own branch, while entropy trends to be more balanced

## Data leakage in ML

- ***Data leakage***: training data contains info about the target, but similar data won't be available when the model is used for prediction.
  - **Target leakage**: timing or chronological order of x and y
  - **Train-test leakage**
- Target leakage: some "predictor" or x may actually be collected/updated after y(outcome) is observed in the training data, such variables should be used in model training
- Train-test contamination: when validation data affects the preprocessing behavior
  - e.g. run a preprocessing (like fitting an imputer for missing values) before calling `train_test_split()`
  - If the validation is based on a simple train-test split, exclude the validation data from any type of fitting, including the fitting of preprocessing steps. When using CV, it's even more critical that you do your preprocessing inside then pipeline!

