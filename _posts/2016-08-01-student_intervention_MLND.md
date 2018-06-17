---
layout: post
title: Building a Student Intervention Model
meta: "classifier"
category: ml
author: "Jean-Marc Beaujour"
img_post: 20160801_student_intervention.jpg
summary: We will build a model that predicts how likely a student is to pass their high school final exam. The model must be effective while using the least amount of computation costs.
github-link: github.com/jmlb/ML_Nanodegree-Udacity/tree/master/Student_Intervention
---

As education has grown to rely more on technology, vast amounts of data has become available for examination and prediction. Logs of student activities, grades, interactions with teachers and fellow students, and more, are now captured in real time through learning management systems like Canvas and Edmodo. Within all levels of education, there exists a push to help increase the likelihood of student success, without watering down the education or engaging in behaviors that fail to improve the underlying issues. Graduation rates are often the criteria.
A local school district has a goal to reach a 95% graduation rate by the end of the decade by identifying students who need intervention before they drop out of school. We will build a model that predicts how likely a student is to pass their high school final exam: **the model must be effective while using the least amount of computation costs.


# Classification vs. Regression
This is a binary classification problem with a labeled dataset. Our model must output either "1" if the student passed the final exam or "0" otherwise.

# Exploring the Data
Let's begin by investigating the dataset to determine how many students we have information on, and learn about the graduation rate among these students.

<script src="https://gist.github.com/jmlb/9131cd466fd764cd70643368d89bff1e.js"></script>

[+] Total number of students: 395<br>
[+] Number of features: 30<br>
[+] Number of students who passed: 265<br>
[+] Number of students who failed: 130<br>
[+] Graduation rate of the class: 67.09%<br>
<br>
[+] There is more Female than Male students in the dataset: <br>
F: 208 | M: 187 
[+] The number of grad Female students is about the same as that of Male students:<br>
133 Nbr of grad Female | 132 Nbr of grad Male <br>
[+] The Male students have a higher graduation rate: F: 0.64|M: 0.71 <br>
[+] The ratio of students attending G. Pereira versus Mousinho is 7 : 1<br>
[+] Graduation rate at Gabriel Pereira: 0.68 | Mousinho da Silveira 0.63<br>
[+] The graduation rate is higher for students attending Pereira: 0.05 points higher<br>


## Visualization of a few data


<script src="https://gist.github.com/jmlb/a62d063d9ff61dd1a550bdc935d63f42.js"></script>

<img src="/images/201608/student_intervention_output_11_0.png" width="80%">

**More observations**: Most students with age 15 to 21 pass the final exam, with the exception of the 19 yo students (grad rate < 0.5. Note that there is only 1 student of age 21 and 1 student of age 22. There are more Urban than Rural students. The graduation rate of Urban students is higher than that of Rural Students


# Preparing the Data
In this section, we will prepare the data for modeling, training and testing.
It is often the case that the data you obtain contains non-numeric features. This can be a problem, as most machine learning algorithms expect numeric data to perform computations with.

<script src="https://gist.github.com/jmlb/540a45fe60a056cb6358d7f4f7f76ad6.js"></script>

[+] **Feature columns**<br>
['school', 'sex', 'age', 'address', 'famsize', 'Pstatus', 'Medu', 'Fedu', 'Mjob', 'Fjob', 'reason', <br>
'guardian', 'traveltime', 'studytime', 'failures', 'schoolsup', 'famsup', 'paid', 'activities', 'nursery', <br>
'higher', 'internet', 'romantic', 'famrel', 'freetime', 'goout', 'Dalc', 'Walc', 'health', 'absences']
    

<table style="font-size:10px"><tr style="border:1px solid"><td></td><td>school</td><td>sex</td><td>age</td><td>address</td><td>famsize</td><td>Pstatus</td><td>Medu</td><td>Fedu</td><td>Mjob</td><td>Fjob</td><td>higher</td><td>internet</td><td>romantic</td><td>famrel</td><td>freetime</td><td>goout</td><td>Dalc</td><td>Walc</td><td>health</td><td>absences</td></tr>
<tr style="border:1px solid"><td>0</td><td>GP</td><td>F</td><td>18</td><td>U</td><td>GT3</td><td>A</td><td>4</td><td>4</td><td>at_home</td><td>teacher</td><td>yes</td><td>no</td><td>no</td><td>4</td><td>3</td><td>4</td><td>1</td><td>1</td><td>3</td><td>6</td></tr>
<tr style="border:1px solid"><td>1</td><td>GP</td><td>F</td><td>17</td><td>U</td><td>GT3</td><td>T</td><td>1</td><td>1</td><td>at_home</td><td>other</td><td>yes</td><td>yes</td><td>no</td><td>5</td><td>3</td><td>3</td><td>1</td><td>1</td><td>3</td><td>4</td></tr>
<tr style="border:1px solid"><td>2</td><td>GP</td><td>F</td><td>15</td><td>U</td><td>LE3</td><td>T</td><td>1</td><td>1</td><td>at_home</td><td>other</td><td>yes</td><td>yes</td><td>no</td><td>4</td><td>3</td><td>2</td><td>2</td><td>3</td><td>3</td><td>10</td></tr>
<tr style="border:1px solid"><td>3</td><td>GP</td><td>F</td><td>15</td><td>U</td><td>GT3</td><td>T</td><td>4</td><td>2</td><td>health</td><td>services</td><td>yes</td><td>yes</td><td>yes</td><td>3</td><td>2</td><td>2</td><td>1</td><td>1</td><td>5</td><td>2</td></tr>
<tr style="border:1px solid"><td>4</td><td>GP</td><td>F</td><td>16</td><td>U</td><td>GT3</td><td>T</td><td>3</td><td>3</td><td>other</td><td>other</td><td>yes</td><td>no</td><td>no</td><td>4</td><td>3</td><td>2</td><td>1</td><td>2</td><td>5</td><td>4</td></tr>
</table>

[5 rows x 30 columns]
<br>
There are several non-numeric columns that need to be converted! Many of them are simply 'yes'/'no', e.g. 'internet'. These can be reasonably converted into '1'/'0' (binary) values. Other columns, like 'Mjob' and 'Fjob', have more than two values, and are known as **categorical variables**. The recommended way to handle such a column is to create as many columns as possible values (e.g. 'Fjob_teacher', 'Fjob_other', 'Fjob_services', etc.), and assign a '1' to one of them and '0' to all others.
These generated columns are sometimes called **dummy variables**, and we will use the [`pandas.get_dummies()`](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.get_dummies.html?highlight=get_dummies#pandas.get_dummies) function to perform this transformation.

<script src="https://gist.github.com/jmlb/92b688542e24ea0893b01ecff723854c.js"></script>

Processed feature columns (48 total features):
    ['school_GP', 'school_MS', 'sex_F', 'sex_M', 'age', 'address_R', 'address_U', 'famsize_GT3', 'famsize_LE3', 'Pstatus_A', 'Pstatus_T', 'Medu', 'Fedu', 'Mjob_at_home', 'Mjob_health', 'Mjob_other', 'Mjob_services', 'Mjob_teacher', 'Fjob_at_home', 'Fjob_health', 'Fjob_other', 'Fjob_services', 'Fjob_teacher', 'reason_course', 'reason_home', 'reason_other', 'reason_reputation', 'guardian_father', 'guardian_mother', 'guardian_other', 'traveltime', 'studytime', 'failures', 'schoolsup', 'famsup', 'paid', 'activities', 'nursery', 'higher', 'internet', 'romantic', 'famrel', 'freetime', 'goout', 'Dalc', 'Walc', 'health', 'absences']

<table style="font-size:10px"><tr style="border:1px solid"><td></td><td>school</td><td>sex_M</td><td>sex_F</td><td>age</td><td>address_R</td><td>address_U</td><td>famsize_GT3</td><td>famsize_LE3</td><td>Pstatus_A</td><td>higher</td><td>internet</td><td>romantic</td><td>famrel</td><td>freetime</td><td>goout</td><td>Dalc</td><td>Walc</td><td>health</td><td>absences</td></tr>
<tr style="border:1px solid"><td>0.0</td><td>1.0</td><td>1.0</td><td>0.0</td><td>18</td><td>0.0</td><td>1.0</td><td>1.0</td><td>0.0</td><td>1.0</td><td>1.0</td><td>0.0</td><td>0.0</td><td>4.0</td><td>3.0</td><td>4.0</td><td>1.0</td><td>1.0</td><td>3.0</td><td>6.0</td></tr>
<tr style="border:1px solid"><td>1.0</td><td>1.0</td><td>1.0</td><td>0.0</td><td>17</td><td>0.0</td><td>1.0</td><td>1.0</td><td>0.0</td><td>0.0</td><td>1.0</td><td>1.0</td><td>0.0</td><td>5.0</td><td>3.0</td><td>3.0</td><td>1.0</td><td>1.0</td><td>3.0</td><td>4.0</td></tr>
<tr style="border:1px solid"><td>2.0</td><td>1.0</td><td>1.0</td><td>0.0</td><td>15</td><td>0.0</td><td>1.0</td><td>0.0</td><td>1.0</td><td>0.0</td><td>1.0</td><td>1.0</td><td>0.0</td><td>4.0</td><td>3.0</td><td>2.0</td><td>2.0</td><td>3.0</td><td>3.0</td><td>10.0</td></tr>
<tr style="border:1px solid"><td>3.0</td><td>1.</td><td>1.0</td><td>0.0</td><td>15.0</td><td>0.0</td><td>1.0</td><td>1.0</td><td>0.0</td><td>0.0</td><td>1.0</td><td>1.0</td><td>1.0</td><td>3.0</td><td>2.0</td><td>2.0</td><td>1.0</td><td>1.0</td><td>5.0</td><td>2.0</td></tr>
<tr style="border:1px solid"><td>4.0</td><td>1.0</td><td>1.0</td><td>0.0</td><td>16.0</td><td>0.0</td><td>1.0</td><td>1.0</td><td>0.0</td><td>0.0</td><td>1.0</td><td>0.0</td><td>0.0</td><td>4.0</td><td>3.0</td><td>2.0</td><td>1.0</td><td>2.0</td><td>5.0</td><td>4.0</td></tr>
</table>
<br>
[5 rows x 48 columns]


# Implementation: Training and Testing Data Split
Now, we split the data (both features and corresponding labels) into training and test sets. In the following code cell below, you will need to implement the following:<br>
[+] Randomly shuffle and split the data ('X_all', 'y_all') into training and testing subsets.<br>
[+] Use 300 training points (approximately 75%) and 95 testing points (approximately 25%).<br>
[+] Set a 'random_state' for the function(s) you use, if provided.<br>
[+] Store the results in 'X_train', 'X_test', 'y_train', and 'y_test'.<br>

<script src="https://gist.github.com/jmlb/5c1522465185155fc01cc2345c10d14b.js"></script>

This "dummy" model predicts that all the students passed the final exam (i.e a graduation rate of 100%).

<script src="https://gist.github.com/jmlb/9bad917ce036174851af7548d1229e1e.js"></script>

[+] Number of True Positive: 205 <br>
[+] Number of False Positive: 95 <br>
[+] **The F1-score on the training set is 0.81 and on the test set 0.77.** Our supervised Learning Model must perform better if using F1-score as a metric of model performance.

The confusion matrix of the naive model for the training set is shown below (in parenthesis is the count):
<table><tr style="border:1px solid"><td></td><td>Positive predict</td><td>Negative predict</td></tr>
<tr style="border:1px solid"><td>Positive Truth</td><td>tp (=205)</td><td>fn (=0)</td></tr>
<tr style="border:1px solid"><td>Negative Truth</td><td>fp(=95)</td><td>tn(=0) </td></tr>
</table>

The **fp** are false positive, i.e the examples where the model wrongly classified the students as passing the tests.

1. Recall = tp/(tp+fn) = 205/(205+0) = 1
2. Precision = tp/(tp + fp) = 205/(205+95) = 0.68

The F1-score might not be the best metric to evaluate the performance of the model in our problem. 
$$ F1Score = 2 \times \frac{Precision \times Recall}{Precision + Recall} $$

We will express F1-Score in terms of **tp**, **tn**, **fp**, **fn** instead:
$$ F1Score = 2 \times \frac{\frac{tp}{tp + fp} \times \frac{tp}{tp+fn}}{\frac{tp}{tp+fp}+\frac{tp}{tp+fn}} $$

The equation can be simplified to:
$$ F1Score =\frac{2 tp}{2tp + fn + fp} $$

Let's now look at some cases:<br>
1. Case1: **fn**=**fp**=0 (i.e no misclassification), then F1-score=1 as expected <br>
2. Case2: fn=0.5 but fp=0 i.e we have 50% of the students who passed the exam were predicted to fail the exam.
$$F1Score =\frac{2\times 0.5}{2 \times 0.5 + 0 + 0.5} = 0.67$$ <br>
3. Case3: **fn**=0 but **fp**=0.5, i.e the student that failed the exam were predicted to pass the exam.
$$F1Score =\frac{2\times 0.5}{2 \times 0.5 + 0.5 + 0} = 0.67$$ <br>

Case2 and Case3 result in the same F1-score. However, a classifier that misclassifies failing students as graduating students can be considered to be worst than a classifier that misclassifies graduating students as failing students. We might want to use a metric that put more weights on Case3 misclassifications

# Training and Evaluating Models
We choose 3 supervised learning models that are appropriate for this problem.

**Boosted Decision Tree Classifier(modelA)**<br>
A Boosted Decision Tree Classifier (DTC)is a supervised learning algorithm using Ensemble method to improve the accuracy of the learning algorithm. The real-world application of Boosted DTC includes star galaxy classification, assess quality of Financial Products, diagnosis of desease, object recognition and text processing. 
One of the advantages of the Boosted DTC is that it can be easily visualized and explained: given a new example, its classification can be explained by Boolean Logic. A Boosted DTC has few parameters to tune, and it requires little data preparation such as normalization of the features.
One of the disadvantages of a Boosted DTC is that if the weak learners are too complex, it can result in high variance. Typically, Boosted Decision Tree Classifiers use an ensemble of DTC of depth=1, i.e Decision Stumps.

**Naive Bayes Classification (modelB)**<br>
The Naive Bayes Classification (NBC) algorithm is a classification technique based on Bayes Theorem and can be used for binary and multi-class problems. It is used in spam detection, recognizing letters from handwritten texts as well as facial analysis problems. The Naive Bayes Classifier assumes that the features are conditionally independent, that is to say that given the class variable, the value of a particular feature is unrelated to that of any other features. The NBC presents the advantage to have no tuning parameters. It is easy to implement and fast to predict classes of datasets. It is also useful for models that require transparency.
NBC performs well in the case of categorical input variables compared to numerical variables. For the later case, one needs to assume normal distribution which is a strong assumption. Another disadvantage is that: if a category that is not present in the training data is found in the test data, the model will not be able to make a prediction. The assumption of independent predictors adds to the bias of the model. NBC tends to perform poorly due to their simplicity. 

**Logistic Regression (modelC)**<br>
Logistic Regression is a Supervised Learning Algorithm that can be used for binary classification, but also for multi-class when applying the oneVsAll method. Logistic Regression is used in Marketing to predict customer rentention, in Finance to predit stock price, and text classification. Unlike DTC or Naive Bayes Algorithm, Logistic Regression can include interaction terms to investigate potential combined effects of the variables. 
The model coefficients can be interpreted to understand the direction and strength of the relationships between the features and the class. A Logistic regression without polynomial terms can only represent linear/planar boundary. In order to represent more complex boundaries, high order feature terms are required. This comes with the disadvantage of increasing the number of features, with the risk of high variance, and also increases the computing time. Though, regularization can be used to prevent overfitting. Another disadvantage of the Logistic Regression is that it requires additional data processing such as feature normalization.   


We will try out Logistic Regression because it is a simple model that can be used as a **benchmark**. Because we are dealing with a **small dataset**, we also chose to experiment with the Naive Bayes Classifier because it is a high Bias classifier. Even if the features are not entirely independent, Naive Bayes can still perform well. The Model interpretability is also an important factor in the choice of the model. Indeed, those 3 models can be interpreted by the school board and reveal correlation between the student performance and the features. Those models are also cheap and fast to compute. Finally, it is also important that the models achieve high accuracy.

# Setup
We run the code cell below to initialize three helper functions which you can use for training and testing the three supervised learning models. The functions are as follows:<br>
- "train_classifier" - takes as input a classifier and training data and fits the classifier to the data. <br>
- "predict_labels" - takes as input a fit classifier, features, and a target labeling and makes predictions using the F1 score. <br>
- "train_predict" - takes as input a classifier, and the training and testing data, and performs `train_clasifier` and `predict_labels`. <br>
- This function will report the F<sub>1</sub> score for both the training and testing data separately.

<script src="https://gist.github.com/jmlb/08cbfe5e62af15b49269b6821579358e.js"></script>

# Implementation: Model Performance Metrics

<script src="https://gist.github.com/jmlb/ad3f42e67ee663058252031de835cef2.js"></script>

 <p style="font-size:12px; line-height: 1.2;">   
    [+] model A: Decision Tree Classifier with Boosting <br>
    Training a AdaBoostClassifier using a training set size of 100. . . <br>
    Trained model in 0.0072 seconds <br>
    Total observed 64 | predicted Success 64 <br>
    Made predictions in 0.0022 seconds. <br>
    F1 score for training set: 1.0000. <br>
    Total observed 60 | predicted Success 56 <br>
    Made predictions in 0.0012 seconds. <br>
    F1 score for test set: 0.6552. <br> <br>
    <br>
    [+] model B: Bernouilli Naive Bayes <br>
    Training a BernoulliNB using a training set size of 100. . . <br>
    Trained model in 0.0034 seconds <br>
    Total observed 64 | predicted Success 76 <br>
    Made predictions in 0.0009 seconds. <br>
    F1 score for training set: 0.8286. <br>
    Total observed 60 | predicted Success 81 <br>
    Made predictions in 0.0005 seconds. <br>
    F1 score for test set: 0.7943. <br>
    <br>
    [+] model C: Linear Regression <br>
    Training a LogisticRegression using a training set size of 100. . . <br>
    Trained model in 0.0026 seconds <br>
    Total observed 64 | predicted Success 70 <br>
    Made predictions in 0.0003 seconds. <br>
    F1 score for training set: 0.9104. <br>
    Total observed 60 | predicted Success 67 <br>
    Made predictions in 0.0005 seconds. <br>
    F1 score for test set: 0.7087. <br>
    <br>
    [+] model A: Decision Tree Classifier with Boosting  <br>
    Training a AdaBoostClassifier using a training set size of 200. . . <br>
    Trained model in 0.0064 seconds <br>
    Total observed 138 | predicted Success 138 <br>
    Made predictions in 0.0017 seconds. <br>
    F1 score for training set: 1.0000. <br>
    Total observed 60 | predicted Success 67 <br>
    Made predictions in 0.0007 seconds. <br>
    F1 score for test set: 0.7402. <br>
    <br>
    [+] model B: Bernouilli Naive Bayes <br>
    Training a BernoulliNB using a training set size of 200. . . <br>
    Trained model in 0.0015 seconds <br>
    Total observed 138 | predicted Success 162 <br>
    Made predictions in 0.0016 seconds. <br>
    F1 score for training set: 0.8267. <br>
    Total observed 60 | predicted Success 76 <br>
    Made predictions in 0.0017 seconds. <br>
    F1 score for test set: 0.7794. <br>
    <br>
    [+] model C: Linear Regression <br>
    Training a LogisticRegression using a training set size of 200. . . <br>
    Trained model in 0.0091 seconds <br>
    Total observed 138 | predicted Success 147 <br>
    Made predictions in 0.0008 seconds. <br>
    F1 score for training set: 0.8421. <br>
    Total observed 60 | predicted Success 77 <br>
    Made predictions in 0.0003 seconds. <br>
    F1 score for test set: 0.7883. <br>
    <br>
    [+] model A: Decision Tree Classifier with Boosting <br>
    Training a AdaBoostClassifier using a training set size of 300. . . <br>
    Trained model in 0.0034 seconds <br>
    Total observed 205 | predicted Success 205 <br>
    Made predictions in 0.0010 seconds. <br>
    F1 score for training set: 1.0000. <br>
    Total observed 60 | predicted Success 58 <br>
    Made predictions in 0.0005 seconds. <br>
    F1 score for test set: 0.6102. <br>
    <br>
    [+] model B: Bernouilli Naive Bayes <br>
    Training a BernoulliNB using a training set size of 300. . . <br>
    Trained model in 0.0025 seconds <br>
    Total observed 205 | predicted Success 228 <br>
    Made predictions in 0.0019 seconds. <br>
    F1 score for training set: 0.8037. <br>
    Total observed 60 | predicted Success 75 <br>
    Made predictions in 0.0007 seconds. <br>
    F1 score for test set: 0.7556. <br>
    <br>
    [+] model C: Linear Regression <br>
    Training a LogisticRegression using a training set size of 300. . . <br>
    Trained model in 0.0120 seconds <br>
    Total observed 205 | predicted Success 236 <br>
    Made predictions in 0.0028 seconds. <br>
    F1 score for training set: 0.8435. <br>
    Total observed 60 | predicted Success 73 <br>
    Made predictions in 0.0007 seconds. <br>
    F1 score for test set: 0.7820. <br>
</p>

# Tabular Results

** Classifer 1 - Decision Tree Classifier  **

<table>
<tr style="border: 1px solid"><td>Training Set Size</td><td>Prediction Time (test)</td><td>Training Time</td><td>F1 Score (train)</td><td>F1 Score (test)</td></tr>
<tr style="border: 1px solid"><td>100</td><td>0.0059406</td><td>0.0006511</td><td>1.0</td><td> 0.6207 </td></tr>
<tr style="border:1px solid"><td>200</td><td>0.0061325</td><td>0.0005101</td><td>1.0</td><td> 0.7360</td></tr>
<tr style="border: 1px solid"><td>300</td><td>0.0065699</td><td>0.0005306</td><td>1.0</td><td>0.6281</td></tr>
</table>

** Classifer 2 - Naive Bayes **
<table>
<tr style="border:1px solid"><td>Training Set Size</td><td>Prediction Time (test)</td><td>Training Time</td><td>F1 Score (train)</td><td>F1 Score (test)</td></tr>
<tr style="border: 1px solid"><td>100</td><td>0.001072</td><td>0.0004</td><td>0.8206</td><td> 0.7943 </td></tr>
<tr style="border: 1px solid"><td>200</td><td>0.001585</td><td>0.0007</td><td> 0.8267</td><td> 0.7794</td></tr>
<tr style="border: 1px solid"><td>300</td><td>0.001323</td><td>0.0004</td><td>0.8037</td><td>0.7556</td></tr>
</table>

** Classifer 3 - Logistic Regression**  
<table>
<tr style="border: 1px solid"><td>Training Set Size</td><td>Prediction Time (test)</td><td>Training Time</td><td>F1 Score (train)</td><td>F1 Score (test)</td></tr>
<tr style="border: 1px solid"><td>100</td><td>0.002504</td><td>0.000122</td><td>0.9104</td><td> 0.7087 </td></tr>
<tr style="border:1px solid"><td>200</td><td>0.002925</td><td>0.000122</td><td> 0.8421</td><td>  0.7883</td></tr>
<tr style="border: 1px solid"><td>300</td><td>0.003895</td><td>0.000122</td><td>0.8435</td><td>0.7820 </td></tr>
</table>


**The Boosted Decision Tree Classifier (DTC)** is the best performing model, although it is the slowest of the 3 models. The Boosted DTC when using the default settings does not perform better than the Naive Bayes nor the Logistic Regression Classifier. However, the settings can be tweaked (as it will be done later) in order to improve the performance. In particular, it is clear that, as is (i.e with the default settings), our Boosted DTC suffers from overfitting as demonstrated by a F1-score=1 for the training set (300 examples), and a poor F1-score of 0.63 for the test set.

The goal of the model is to predict whether a student will pass the final exam. In order to build our model, we take a sample of students whose the result at the final exam is known. Each student is "defined" by a set of characteristics/attributes: gender, address, the school attended, etc...

The model consists in generating a number of simple rules on the students attributes, with a classification error below better than 0.5, i.e better than . For example, one possible rule could be to split the students depending on the area: Urban or Rural. If all the students with the Urban attribute had passed the exam, and all the students with Rural attribute had failed, then this rule alone would be sufficient to classify students, and predict outcome for future students. But such a simple rule is very likely to make errors. A new rule is then generated and it will pay more attention to the examples that were misclassified by the previous rule. As new rules are generated, they will continue to pay more attention to the examples that have been misclassified by the preceding rule.

After many iterations of generating new rules, the prediction from all those simple rules are combined in the form of a weighted average, which will have much more accuracy than the prediction from a single rule alone.

# Implementation: Model Tuning

<script src="https://gist.github.com/jmlb/6c7395c4bf75962ea8facb2799843c18.js"></script>

The final model has a F1-score of 0.82 for the training set and 0.80 for the test set.
The F1-score of training set is lower for the tuned model: 0.82 versus 1.0 for the untuned model. And the F1-score of the test set is improved after tuning the model.
The results clearly indicate that there is less overfitting in the tuned model. Indeed, ($\Delta$ F1-score), i.e "F1-score of training set" minus "F1-score of test set", is lower for the tuned model.