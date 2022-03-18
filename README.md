# Ames Housing Data and Kaggle Challenge
### A method for predicting the sale price of houses


## Problem Statement
What is the sale value of a house? In capitalist societies there is no exogenous price setter, but rather the price of a house is simply whatever someone is willing to pair for it. This poses a significant challenge for both buyers and sellers in the housing market. House appraisal is a valuable service, and accurate assessment of a house's value provides leverage in negotations, and a competitve advantage for investors. However, accurately evaluating a house's value is difficult undertaking, and so real estate appraisal has historically been an expert domain.

In the last several decades, the rapid expansion of digitization and the consequent arrival of 'Big Data' have open new possibilities for housing appraisal. Machine learning can be a powerful tool in this, but there remains difficultly in calibrating high-accuracy predictive models.

This paper attempts to design an accurate model predicting house price using linear regression, and as a point of reference a K Nearest Neighbour model is also implemented. It is hoped that in designing these models I can gain insight into the optimal model design for predicting the final sale price of houses.


## Data
This report uses the Ames housing dataset. The full dictionary is available here: http://jse.amstat.org/v19n3/decock/DataDocumentation.txt

The variable names are preserved throughout this report, although they are reformatted to remove uppercase, and spaces are replaced with underscores for consistency and ease of use. In addition, there is some feature engineering, for example, each house's construction year and sale year are used to compute the home's age at time of sale, and this is used as a continuous variable for training the models. There are also interaction terms added at several points in this report, but in all cases the features are named in a consistent manner, allowing the reader to refer back to the above data dictionary for clarification.


## Results
|Model Number #| Description | Parameters | CV score | Train score | Test score |
| --- | --- | --- | --- | --- | --- |
|1| **`Basic Multiple Regression - Numeric`** |na|0.82|0.83|0.81|
|2| **`Log(y) Multiple Regression - Numeric`** |na|0.85|0.86|0.84|
|3| **`Multiple Regression with Polynomial Features - Numeric`** |na|0.86|0.88|0.85|
|4| **`Multiple Regression with Selected Interaction Terms - Numeric &  Non-Numeric Features`** |na|0.90|0.91|0.91|
|5| **`Ridge Regression`** |α= 1.4696843861124473e-20|0.90|0.91|0.90|
|6| **`Lasso Regression`** |α= 1.6681005372000558e-10|0.90|0.91|0.91|
|7| **`K Nearest Neighbours`** |'manhattan'; k=7|0.84|0.91|0.87|


## Conclusions
The difference in performance between models using y or log(y) raises some interesting points. The reader may recall from the EDA that there does appear to be some non-linearity in sale price with respect to sqf (variance in saleprice increases as saleprice and sqf get larger), and so this may explain why model #2 performs better than model #1. Using log(y) only hurts model performance after interactions terms are included, which may be because some of the non-linearity is captured by those polynomial features, meaning that taking the log(y) doesn't have the same impact.

Another valuable takeaway comes from the calibration of model #4, through the trial-and-error assessment of different dummified categorical and ordinal features. In the EDA I discussed how the attribute 'neighborhood' would likely capture much of the information on density and urbanity/rurality that other attributes would, and conjectured that 'neighborhood' could serve as a proxy for many other variables that could be excluded. In practice, though, 'neighborhood' tended to hurt model performance, and my final model #4 showed significant improved only after I removed it, and added in several other features that I thought might express urbanity/rurality. In part, this may simply be because there were too many unique neighbourhoods in Ames, and so including a dummy for each may have pushed the model up against its complexity limit (that is, the number of features is constrained by sample size), which meant that too many other important variables had to be excluded in order to make room. Another possibility is simply that neighbourhood isn't, in the end, all that good an indicator of value. This can happen when there is significant diversity within neighbourhoods, and not only between them. In such cases, the fact that a house is sold in some neighbourhood may not tell very much at all about the home, because there are a wide range of income levels within the neighbourhood. In some cases, municipal boundaries can even contain starkly contrasting urban and rural areas.

While neighbourhood ended up hurting my models in this particular project, it doesn't mean that it will never be useful. Again, the sample size proved a constraint here, as some rejected versions of model 4 had even better performance than my final versions, but used too many features to be able to execute k-fold cross-validation. My recommendation for future study would be to collect a larger dataset, if only to allow for more features to be used in modelling sale price. It remains possible that neighbourhood could serve as a valuable attribute in a regression with perhaps a hundred features or more.
