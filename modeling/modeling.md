# Modeling
The modeling process took on several steps:
* Data Preprocessing
* Feature Selection (with cross-validation)
* Model Building and Test Error Estimation
* Model Evaluation

## Preprocessing
The `conference` columns were temporarily dropped for ease of modeling with simply numerical features. These can be added again after using Label Encoding or One Hot Encoding, if the curse of dimensionality does not befall us after doing so. These processes are iterative so our model could certainly be improved if we add it! :smiley: The `winner` column was then One-Hot encoded and the `away_winner` column dropped to produce our target variable.

## Feature Selection
To reduce our dimensions from approximately 70 to 15, `sklearn`'s `selectKBest` functionality was used during cross-validation. This was done in accordance to Tibshirani's discussion of ***The Right and Wrong way to do Cross-Validation*** in Chapter 7.10.2 of *Elements of Statistical Learning*:
1. Randomly divide the training data into K (10) cross-validation folds
2. For each fold *k = 1, 2, ..., K*:
    * Use SelectKBest to calculate the 15 highest ANOVA scores among the predictors
    * Record which variables appear among the top 15
3. Tally the 15 features with the highest occurences 

The following graph showcases the features that occured the most often in the top 15 of 10 iterations of KFold Cross Validation:

![](/images/feature_selection.png)

Now our predictors don't have an unfair advantage of being chosen on the basis of *all* samples, but on folds in Cross-Validation

## Model Building and Selection
3 multivariate classifiers were fit on training data and compared via ROC AUC scores in KFold cross-validation: 
* Random Forest
* Logistic Regression
* K Nearest Neighbors

![](/images/auc_scores.png)

The Logistic Regression model performed the best, with an AUC score of approximately 0.81 (a perfect classifier would have an AUC of 1.0):

![](/images/log_reg_auc.png)

## Model Evaluation
We can see from the plot below that on our test data, the model has an AUC of approximately 0.84, which is better than our estimate of prediction error from cross-validation. This is an interesting phenomenon that I will certainly look into and read about. It's possible that our training data was simply 'harder' than our test data and the model has a good fit.
![](/images/roc_auc_TEST.png)
