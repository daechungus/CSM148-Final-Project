CS M148 Final Project Report
By Nathan Chen, Jessica Yao, Dae Hoon Chung, Angelina Yang, Huy Nguyen

Main Document

i. Dataset
We are using the dataset ‘covertype’ from the UC Irvine Machine Learning Repository. The data consists of 54 numerical features and 1 categorical target feature, forest cover type, with 7 levels. The predictor variables are cartographic variables only, which means that the variables are only characteristics of the location itself and not the vegetation. Data is raw and unscaled, with binary columns of data for qualitative independent variables such as wilderness areas and soil types. We only have measurements inferred from a map rather than sensor data, from satellite or otherwise.

The study area includes four wilderness areas located in the Roosevelt National Forest of northern Colorado: Neota (Area 2), Rawah (Area 1), Comanche Peak (Area 3), and Cache la Poudre (Area 4). As for the primary cover types in these areas, we have spruce/fir (Type 1), lodgepole pine (Type 2), aspen (Type 5), Ponderosa pine (Type 3), Douglass-fir (Type 6), cottonwood/willow (Type 4), and Krummholz (Type 7).

ii. Problem Overview
We want to predict the forest cover type, of which there are 7 categories: Spruce/Fir, Lodgepole Pine, Ponderosa Pine, Cottonwood/Willow, Aspen, Douglas-fir, Krummholz. We aim to investigate the relationships between the cartographic features and the cover types that are associated with them.

iii. Methodology
We first take to cleaning the data for any missing or extreme values. The data did not contain any missing values so we proceeded to exploratory data analysis (EDA) to identify data trends and potential areas of feature engineering.

In the EDA phase, we analyzed trends in the data and investigated whether some features may be telling signs of a distinct cover type. Starting with the data in its tabular form, we generate visualizations that help to understand form and summary statistics of each feature. Using these insights, we can construct a plan for feature selection/engineering and construct the best model for classification. Some features were already one-hot encoded, such as soil type and wilderness area; the sheer quantity of soil type and wilderness type-related features are a reflection of the ecological diversity of the national park and the high-cardinality of the data collected from the park. This presents categorical data in a more robust format that allows us to better handle categories in fitting and testing multiple models with the best classification accuracy.

EDA allowed us to make initial hypotheses about which features may prove more or less useful in classifying cover types. For example, plotting wilderness areas presented results that suggested certain cover types occurred more often in specific wilderness areas; therefore, wilderness areas became a potential feature to use in fitting our classification model. However, soil type-related features were very imbalanced in their binary values, and some were sparse to the point of almost relatively nonexistent. Analyses such as these suggest that we consider the removal of these features as we worked to build a high accuracy cover type classification model.

We were tasked with fitting multiple types of models on the data to get the highest classification accuracy. This involved testing and optimizing our hyperparameters and features of choice that would perform the best on test data over a 5-fold cross validation. Our findings, after testing linear regression, logistic regression, principle components analysis and clustering, random forest, and neural network, is that the random forest has the best performance in using the features to classify tree cover types.

iv. Results
We initially fit a Random Forest Classifier with 10 estimators and the gini criterion, trained using all the available features. The training data had higher accuracy than the testing data, which indicates that the model may be overfitting the training data and not generalizing as well as it could to unseen data. However, the accuracy for the testing data is very high at 94% accuracy, so we are not too worried about overfitting having a large negative effect on our predictive accuracy.

Through analyzing feature importances of our initial model, we discover that the soil type variables, per our guess in EDA, are the least important features. The most important features appear to be Elevation, Horizontal Distance to Hydrology, and Vertical Distance to Hydrology. We used this information to create a more efficient model that removes all soil type features.

Other metrics we used to evaluate our initial model were precision, recall, f1-score, and a confusion matrix. The confusion matrix showed that the majority of values were predicted correctly, with high values along the diagonal. We saw high misclassifications for data points between cover type 0 and 1. This is related to the high number of data points in these two classes, which may be causing the model to misclassify them more often. Other misclassifications include actual class 4 being predicted as class 1, and the actual class 5 being predicted as class 2. The precision, recall, and f1-scores are all very high across all classes, indicating strong model performance. The model is weakest at predicting class 5, with an f1-score of 0.81. The lowest precision score of 0.91 was with class 4, and the lowest recall score was with class 5 at 0.72. We can see that generally, the model is predicting well with accuracy at 94% and weighted average f1-score of 0.94.

In our second iteration of the model, we removed all soil types as predictors because they are not important features. Again, the model uses 10 estimators with gini criterion. Training accuracy was 0.998 and testing accuracy was 0.939. Again, we see that training accuracy is higher than testing accuracy, but the testing accuracy is very high at 0.94. Thus, removing the soil features did not significantly impact model predictions and make them less accurate.

We calculated the feature importances for this model again (without soil features), and we can see that the strongest predictors are still elevation, horizontal distance to roadways, and horizontal distance to fire points. The weakest predictors are the wilderness area one-hot encoded variables, along with slope.

We see the same pattern in the confusion matrix with this model as the original model. The diagonal values are the highest, which means that the model is good at prediction. The most commonly misclassified tree cover types are 0 and 1. The next most misclassified are the actual value 4 being classified as 1 and the actual value 5 being classified as 2. In the classification report containing precision, recall, and f1-score, we see that the evaluation metrics are still pretty good, even with the soil types removed. The lowest precision score is tied between class 3 and class 6 at 0.91, while the lowest recall score is still class 5 at 0.67. The lowest f1-score is also still class 5 at 0.87. The accuracy and weighted average f1-score have not decreased significantly, which means that the model is still predicting well.

Additionally, we wanted to see if our model would improve with more estimators, so we fit another Random Forest Classifier to 100 estimators and the gini criterion. The model did not improve significantly with more estimators. The accuracies are nearly identical to the previous model. The accuracy is less than a percent higher for both training and testing data. However, the classification report shows that the precision, recall, and f1-scores have improved significantly across the classes. Specifically, the lowest precision, recall, and f1-scores have improved over the previous model, with a 0.04 increase in f1-score for class 5 and a 0.05 increase in recall score for class 5. The accuracy and weighted average f1-score are pretty similar from the previous model. Therefore, we can conclude that the model with only 10 estimators is the best for our needs, since the predictive power is pretty similar but the model is faster to train and much more computationally efficient to use.

v. How to use our code
Clone our GitHub repository: https://github.com/daechungus/CSM148-Final-Project/tree/main
Run CS_M148_group_project.ipynb

Citation:
J. Blackard. "Covertype," UCI Machine Learning Repository, 1998. [Online]. Available: https://doi.org/10.24432/C50K5N.
