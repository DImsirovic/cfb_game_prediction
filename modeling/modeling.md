## Modeling
The modeling process took on several steps:
* Data Preprocessing
* Feature Selection (with cross-validation)
* Model Building and Test Error Estimation
* Model Evaluation

TODO: Fix blank graphs and increase size

### Preprocessing
The `conference` columns were temporarily dropped for ease of modeling with simply numerical features. These can be added again after using Label Encoding or One Hot Encoding, if the curse of dimensionality does not befall us after doing so. These processes are iterative so our model could certainly be improved if we add it! :smiley: The `winner` column was then One-Hot encoded and the `away_winner` column dropped to produce our target variable.

### Feature Selection
To reduce our dimensions from approximately 70 to 15, `sklearn`'s `selectKBest` functionality was used during cross-validation. This was done in accordance to Tibshirani's discussion of ***The Right and Wrong way to do Cross-Validation*** in Chapter 7.10.2 of *Elements of Statistical Learning*. Details can be found in the source code.
(Feature selection graph)
![](/images/feature_selection.png)


### Model Building and Selection
3 multivariate classifiers were trained and compared via ROC AUC scores: 
* Random Forest
* Logistic Regression
* K Nearest Neighbors

![](/images/auc_scores.png)

The Logistic Regression model performed the best, with an AUC score of approximately 0.81 (a perfect classifier would have an AUC of 1.0):

![](/images/log_reg_auc.png)

### Model Evaluation
TODO: Calculate the AUC of the logistic regression model on our test data
