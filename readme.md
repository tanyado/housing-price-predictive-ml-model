
# Ames, Iowa Properties


## Baseline Models:
The folowing two model were use as a baseline and a psuedo baseline model:

Null/Baseline Model
SLR

The null model consists of the prediction being solely the mean of the property sale prices. This is the model "to beat" as I create and further refine our predictive model. The null model (also known as the dummy regression) serves only to have *some* kind of baseline in which to compare our actual model against. It is only slightly better than a random guess. The null model is include for comparison purposes.



The Simple Linear Regression (SLR) model is merely a jump off point as I build out our more complex Mutiple Linear Regression (MLR) model. It serves as a pared down structure start refining our predictive model.

## Feature Engineering & Cleaning:
Initally, the features (x-variables) chosen for the MLR was chosen solely because they were numerical value. You can see in the notebook that Ichose to list all feature in a separate python variable and notebook cell for ease of refinement later. 

Later the features were revised to features that appear to have a strong correlation on sale price, based on both intuition and on the seaborn pairplot in the EDA phase.

The vast majority of cleaning revolved around dropping our unexamined features, ordinal mapping for some categories, and dummifying other categories. And ensuring that both the training and test datasets were cleaned in the same exact manner. 

Note: the df_test does not have any properties that contain 'Utilities_NoSeWa' value, so Ineeded to provide this column with '0' values for the testing data set. 

 
## Our Predictive Model (MLR + Lasso Regression)


Through several cycles of refinement somethings I did are below:

I did a Standard Scaler in our preprocessing phase so that I can measure the features against each other fairly.

Through one of the intial cycles I did both a plain train-test-split and a k-fold cross validation. Both scores showed that this version of the model was very overfitted. The TTS model had a training score of 83% and a validation score of 69%. Showing high variation. The Cross Validation model has a high score (MSE) showing high error in our loss function.

```
score_tr:  0.82890749060673
score_test:  0.6904955294972794
```

Due to a misjudegement, I actually did a polynomial fit and quickly realized that made the model evern worse, due to increasing the "wiggliness" of the curve and overfitting the model even more!

```
score_tr:  0.9165158143139792
score_test:  0.3333213067923606
```

Eventually, I did both a Lasso and Ridge Regression. Both did a decent job of regularizing the model. With the training and the test scores being fairly similar across both regularization techniques. I chose to move forward with Lasso Regression due to the marginal improvement to the model.

```
X_train LASSO Score:  0.8275763304912442
X_val LASSO Score:  0.7048586534656807

```

I do want to note that there is a difference between a good statistical model (based purely on scores) vs a good predictive model (that procduces accurate preditions in Kaggle). As this model is fairly good, however did very poorly when fed never-before-seen serving data from Kaggle. 


Below are the coeffient values that informed the final business decision presented in the presentation.
```
0       Lot Frontage   2639.233617
1           Lot Area   5498.048102
2   Utilities_AllPub    636.411575
3   Utilities_NoSeWa     -0.000000
4   Utilities_NoSewr     -0.000000
5     Bldg Type_1Fam   3063.281062
6   Bldg Type_2fmCon      0.000000
7   Bldg Type_Duplex      0.000000
8    Bldg Type_Twnhs   -791.574410
9   Bldg Type_TwnhsE   -305.383363
10      Overall Qual  23343.934536
11      Overall Cond   5986.121939
12        Year Built  10736.421540
13    Year Remod/Add   2348.395078
14        Exter Qual  12540.318394
15        Exter Cond  -1447.984001
16       Central Air      0.000000
17        1st Flr SF  30372.612799
18        2nd Flr SF  20787.653227
19     Bedroom AbvGr  -3985.131605
20     Kitchen AbvGr  -2322.574298
21        Fireplaces   4691.953860
22      Fireplace Qu    890.535003
23     Garage Yr Blt  -3249.100161
24       Garage Area  10094.794476
25         Pool Area   -318.712767
26          Misc Val  -8788.827422
```


Here is the singled out coeffients:

```

1           Lot Area   5498.048102
5     Bldg Type_1Fam   3063.281062
17        1st Flr SF  30372.612799


24       Garage Area  10094.794476
2   Utilities_AllPub    636.411575 medium effecgt, convience. 
10      Overall Qual  23343.934536
14        Exter Qual  12540.318394
```
