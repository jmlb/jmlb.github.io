---
layout: post
title: Creating Customer Segmentation
meta: Machine-Learning, Unsupervised Learning, Udacity, ML_NanoDegree, sk-learn
category: ml
author: "Jean-Marc Beaujour"
img_post: 20160819_segmentation.jpg
summary: In this post, we will identify customers segments using data collected from customers of a wholesale distributor in Lisbon (Portugal). The dataset includes the various customers annual spending amounts (reported in monetary units) of diverse product categories for internal structure.
github-link: "na"
---


In this post, we will identify customers segments using data collected from customers of a wholesale distributor in Lisbon (Portugal). The dataset includes the various customers annual spending amounts (reported in monetary units) of diverse product categories for internal structure. The project includes several steps: explore data (determine if any product categories are highly correlated), scale each product category, identify and remove outliers, dimension reduction using PCA, implement a clustering algorithm to segment the customer data and finally compare segmentation. The dataset for this project can be found on the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Wholesale+customers). For the purposes of this project, the features `'Channel'` and `'Region'` will be excluded in the analysis — with focus instead on the six product categories recorded for customers.


Let's load the dataset along with a few of the necessary Python libraries.

<script src="https://gist.github.com/jmlb/bf97bf590d58a0fb00cb8e84ebb04441.js"></script>

Wholesale customers dataset has 440 samples with 6 features each.


# Data Exploration
In this section, we begin exploring the data through visualizations to understand how each feature is related to the others. We will observe a statistical description of the dataset, consider the relevance of each feature, and select a few sample data points from the dataset which you will track through the course of this project.

<script src="https://gist.github.com/jmlb/28b4a87d3c18c2caa1e2734e7b5914d1.js"></script>


<div>
<table border="1" class="dataframe" style="font-size: 11px; border:1px solid;">
    <tr style="text-align: center; border:1px solid;">
      <td></td>
      <td>Fresh</td>
      <td>Milk</td>
      <td>Grocery</td>
      <td>Frozen</td>
      <td>Detergents_Paper</td>
      <td>Delicatessen</td>
    </tr>
    <tr style="text-align: center;border:1px solid;">
      <td>count</td>
      <td>440.000000</td>
      <td>440.000000</td>
      <td>440.000000</td>
      <td>440.000000</td>
      <td>440.000000</td>
      <td>440.000000</td>
    </tr>
    <tr style="text-align: center;border:1px solid;">
      <td>mean</td>
      <td>12000.297727</td>
      <td>5796.265909</td>
      <td>7951.277273</td>
      <td>3071.931818</td>
      <td>2881.493182</td>
      <td>1524.870455</td>
    </tr>
    <tr style="text-align: center;border:1px solid;">
      <td>std</td>
      <td>12647.328865</td>
      <td>7380.377175</td>
      <td>9503.162829</td>
      <td>4854.673333</td>
      <td>4767.854448</td>
      <td>2820.105937</td>
    </tr>
    <tr style="text-align: center;border:1px solid;">
      <td>min</td>
      <td>3.000000</td>
      <td>55.000000</td>
      <td>3.000000</td>
      <td>25.000000</td>
      <td>3.000000</td>
      <td>3.000000</td>
    </tr>
    <tr style="text-align: center;border:1px solid;">
      <td>25%</td>
      <td>3127.750000</td>
      <td>1533.000000</td>
      <td>2153.000000</td>
      <td>742.250000</td>
      <td>256.750000</td>
      <td>408.250000</td>
    </tr>
    <tr style="text-align: center;border:1px solid;">
      <td>50%</td>
      <td>8504.000000</td>
      <td>3627.000000</td>
      <td>4755.500000</td>
      <td>1526.000000</td>
      <td>816.500000</td>
      <td>965.500000</td>
    </tr>
    <tr style="text-align: center;border:1px solid;">
      <td>75%</td>
      <td>16933.750000</td>
      <td>7190.250000</td>
      <td>10655.750000</td>
      <td>3554.250000</td>
      <td>3922.000000</td>
      <td>1820.250000</td>
    </tr>
    <tr style="text-align: center;border:1px solid;">
      <td>max</td>
      <td>112151.000000</td>
      <td>73498.000000</td>
      <td>92780.000000</td>
      <td>60869.000000</td>
      <td>40827.000000</td>
      <td>47943.000000</td>
    </tr>
</table>
</div>    
    
**Product Categories** - Here are possible products that may be purchased within the 6 product categories:<br>
1. Fresh: fruits, vegetables, fish, meat <br>
2. Milk: dairy products i.e Milk, cheese, cream<br>
3. Grocery: flour, rice, cereals, beverages, can food<br>
4. Frozen: frozen vegetables, processed food<br>
5. Detergents_paper: household products, Detergent, hand/floor soap, napkins<br>
6. Delicatessen: fine chocolate, french cheese<br>
The integer represents the annual spending of a customer in monetary unit (mu) for a particular product category.

# Implementation: Selecting Samples
To get a better understanding of the customers and how their data will transform through the analysis, it would be best to select a few sample data points and explore them in more detail. 

<script src="https://gist.github.com/jmlb/79e9cc53468e7c06cf0911b459b921ca.js"></script>

Chosen samples of wholesale customers dataset:
<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center;border:1px solid">
      <td></td>
      <td>Fresh</td>
      <td>Milk</td>
      <td>Grocery</td>
      <td>Frozen</td>
      <td>Detergents_Paper</td>
      <td>Delicatessen</td>
    </tr>
    <tr style="text-align:center; border:1px solid">
      <th>0</th>
      <td>12126</td>
      <td>3199</td>
      <td>6975</td>
      <td>480</td>
      <td>3140</td>
      <td>545</td>
    </tr>
    <tr style="text-align:center; border:1px solid">
      <th>1</th>
      <td>4155</td>
      <td>367</td>
      <td>1390</td>
      <td>2306</td>
      <td>86</td>
      <td>130</td>
    </tr>
    <tr style="text-align:center; border:1px solid">
      <td>2</td>
      <td>17360</td>
      <td>6200</td>
      <td>9694</td>
      <td>1293</td>
      <td>3620</td>
      <td>1721</td>
    </tr>
</table>
</div>



<script src="https://gist.github.com/jmlb/1b68b512ec7cee6f90ccaed79f054887.js"></script>

<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid">
      <td></td>
      <td>Fresh</td>
      <td>Milk</td>
      <td>Grocery</td>
      <td>Frozen</td>
      <td>Detergents_Paper</td>
      <td>Delicatessen</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>0</th>
      <td>126.0</td>
      <td>-2597.0</td>
      <td>-976.0</td>
      <td>-2592.0</td>
      <td>259.0</td>
      <td>-980.0</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>1</th>
      <td>-7845.0</td>
      <td>-5429.0</td>
      <td>-6561.0</td>
      <td>-766.0</td>
      <td>-2795.0</td>
      <td>-1395.0</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>2</th>
      <td>5360.0</td>
      <td>404.0</td>
      <td>1743.0</td>
      <td>-1779.0</td>
      <td>739.0</td>
      <td>196.0</td>
    </tr>
</table>
</div>



<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid">
      <td></td>
      <td>Fresh</td>
      <td>Milk</td>
      <td>Grocery</td>
      <td>Frozen</td>
      <td>Detergents_Paper</td>
      <td>Delicatessen</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>0</th>
      <td>3622.0</td>
      <td>-428.0</td>
      <td>2219.0</td>
      <td>-1046.0</td>
      <td>2324.0</td>
      <td>-421.0</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>1</th>
      <td>-4349.0</td>
      <td>-3260.0</td>
      <td>-3366.0</td>
      <td>780.0</td>
      <td>-730.0</td>
      <td>-836.0</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>2</th>
      <td>8856.0</td>
      <td>2573.0</td>
      <td>4938.0</td>
      <td>-233.0</td>
      <td>2804.0</td>
      <td>755.0</td>
    </tr>
</table>
</div>


[+] customer0's spending on Milk, Frozen and Delicatessen is lower than the median, but it spends more on Fresh, Grocery and Detergents.<br>
[+] customer1's spending is lower than the median and the mean for all categories except for 'Frozen', for which speding is higher than median but lower than the mean. <br>
[+] customer2 spending is higher than the mean and median spending for all categories except for 'Frozen'.<br>

The table below details the total spending of each sample customer and the percentage of spending per product categories ('%cost_')
<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid">
      <td></td>
      <td>Customer</td>
      <td>total cost</td>
      <td>%cost_Fresh</td>
      <td>%cost_Milk</td>
      <td>%cost_Grocery</td>
      <td>%cost_Frozen</td>
      <td>%cost_Detergents_Paper</td>
      <td>%cost_Delicatessen</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>0</th>
      <td>26465</td>
      <td>45.8</td>
      <td>12.1</td>
      <td>26.4</td>
      <td>1.8</td>
      <td>11.9</td>
      <td>2.1</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>1</th>
      <td>8434</td>
      <td>49.3</td>
      <td>4.4</td>
      <td>16.5</td>
      <td>27.3</td>
      <td>1.0</td>
      <td>1.5</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>2</th>
      <td>39888</td>
      <td>43.5</td>
      <td>15.5</td>
      <td>24.3</td>
      <td>3.2</td>
      <td>9.1</td>
      <td>4.3</td>
    </tr>
</table>
</div>

[+] All the 3 customers purchase mainly 'Fresh' products, which accounts for about 50% of their total spending. <br>
[+] Customer0 and Customer2 are likely to be operating in the same business segment, with similar purchase pattern.
Indeed, their 2nd largest expenditure is for 'Grocery' products followed by 'Fresh' and 'Frozen' products. 
Customer2 out-spends Customer0 by about 13000mu: that infers that Customer2 may be a larger business and may need more volume. Customer0 and Customer2 may be in the Retail segment, where food related items (Fresh, Milk, Grocery) are typically available, as well as Household products.<br>
[+] For Customer1, the 2nd largest spending is for 'Frozen' products followed by 'Grocery' products.
Customer1 might be in the Restaurant segment, where the highest percentage of expenditures are for food related items, and the smallest percentage of expenditure for 'Detergents_Paper' products. Note also that Customer1 has the smallest total expenditure compared to the 2 other customers, about 3 to 5 times lower than Customer0 and Customer2 respectively. It is very likely that a Restaurant needs less volume than retailers.

As a side note, it might be useful to look at the volume of the purchase for the different product categories to get more insight on the customer business segment. Indeed, 'Detergents_paper' products are typically more expensive than 'Milk' products for example. Therefore,  a larger percentage of expenditure on 'Detergents_Paper' does not necessarily mean that the customer purchase a higher volume of 'Detergents_Paper' products.

# Implementation: Feature Relevance
One interesting thought to consider is if one (or more) of the six product categories is actually relevant for understanding customer purchasing. That is to say, is it possible to determine whether customers purchasing some amount of one category of products will necessarily purchase some proportional amount of another category of products? We can make this determination quite easily by training a supervised regression learner on a subset of the data with one feature removed, and then score how well that model can predict the removed feature.

<script src="https://gist.github.com/jmlb/4972659f23f22ab0b1c506db32e53b66.js"></script>

    
[+] Deleted feature: Fresh
<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid">
      <td></td>
      <td>Feature importance</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Milk</th>
      <td>0.226778</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Grocery</th>
      <td>0.082826</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Frozen</th>
      <td>0.305044</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Detergents Paper</th>
      <td>0.139340</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Delicatessen</th>
      <td>0.246012</td>
    </tr>
</table>
</div>


[+] Deleted feature: Milk
<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid">
      <td></td>
      <td>Feature importance</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Fresh</th>
      <td>0.138434</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Grocery</th>
      <td>0.219012</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Frozen</th>
      <td>0.031806</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Detergents Paper</th>
      <td>0.465780</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Delicatessen</th>
      <td>0.144969</td>
    </tr>
</table>
</div>

[+] Deleted feature: Grocery
<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid">
      <td></td>
      <td>Feature importance</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Fresh</th>
      <td>0.020446</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Milk</th>
      <td>0.045690<</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Frozen</th>
      <td>0.016514</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Detergents Paper</th>
      <td>0.890934</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Delicatessen</th>
      <td>0.026416</td>
    </tr>
</table>
</div>
 
[+] Deleted feature: Frozen
<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid">
      <td></td>
      <td>Feature importance</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Fresh</th>
      <td>.098081</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Milk</th>
      <td>0.067566</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Grocery</th>
      <td>0.064459</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Detergents Paper</th>
      <td>0.127549</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Delicatessen</th>
      <td>0.642346</td>
    </tr>
</table>
</div>
  
[+]Deleted feature: Detergents_Paper
<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid">
      <td></td>
      <td>Feature importance</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Fresh</th>
      <td>0.040617</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Milk</th>
      <td>0.013689</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Grocery</th>
      <td>0.900611</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Frozen</th>
      <td>0.021572</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Delicatessen</th>
      <td>0.023511</td>
    </tr>
</table>
</div>

[+] Deleted feature: Delicatessen
<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid">
      <td></td>
      <td>Feature importance</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Fresh</th>
      <td>0.148177</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Milk</th>
      <td>0.147138</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Grocery</th>
      <td>0.073654</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Frozen</th>
      <td>0.487107</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>Detergents_Paper</th>
      <td>0.143924</td>
    </tr>
</table>
</div>

<img src="/images/201608/market_segmentation_output_13_12.png" width="90%">

The code above is generalized to study how the score varies when trying to predict any of the 6 attributes. In order to visualize the consistency of the test score values for each case, we run the Decision Tree Regressor multiple times (**nbr_fit_trials=100**) on the training set and calculate the score of the model on the test set. 

The score is defined by:<br>
$$ R^2 = 1 - \frac{\sum (y_{\text{true}} - y_{\text{pred}})^2}{\sum (y_{\text{true}} - \bar{y}_{\text{true}})^2} $$

where $$y_{\text{true}}$$ is the true value of $$y$$, $$y_{\text{pred}}$$ is the predicted value, and $$\bar{y}_{\text{true}}$$ is the mean of the true value.
$$R^2$$ can be positive with best value of 1. It can also be negative if the model fails to fit the data, i.e if  $$\sum (y_{\text{true}} - y_{\text{pred}})^2$$ is large.


We obtain a negative score when attempting to predict the values of the feature 'Milk' using the 5 other features. Although the score values are scattered for the 100 fit trials , it is consistently negative (the red bar in the plot shows the mean value of the score). Therefore, 'Milk' cannot be predicted using the 5 other features and is therefore necessary for identifying customers' spending habits. Similarly, the values of the features 'Frozen' and 'Delicatessen' cannot be predicted. 

The best average score is obtained when predicting 'Grocery' : $$ R^2 \approx 70\% $$. This means that, if given the 5 other features, the values of 'Grocery' can be determined with relatively good accuracy. Therefore, the feature 'Grocery' might not be necessary for identifying patterns in customers' spending. To a lesser extent, the values of 'Detergent_papers' can also be predicted from the remaining features: in this case $$ R^2 $$ is close to 50%.


The parameter **feature_importance** also provides some additional information on the possible correlation between the features, in particular for the 2 cases where $$ R^2 $$ is positive.
>The importance of a feature is computed as the (normalized) total reduction of the criterion brought by that feature. >It is also known as the Gini importance.
[Ref. sklearn documentation](http://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeRegressor.html)

When using 'Grocery' as the label, the most important feature is 'Detergent_papers', with a **Gini importance** of 0.89, and inversely when using 'Detergent_papers' as the label, the feature 'Grocery' becomes the most important with a **Gini importance** of 0.90. The result indicates that there is some correlation between the attribute 'Grocery' and 'Detergent_papers', although we cannot deduce whether it is a positive or a negative correlation.

# Visualize Feature Distributions
To get a better understanding of the dataset, we can construct a scatter matrix of each of the six product features present in the data.

<script src="https://gist.github.com/jmlb/b4e9d5ce2865d81df66e818c2f0dc7f8.js"></script>

<img src="/images/201608/market_segmentation_output_17_0.png" width="90%">

[+] Most features are uncorrelated, except for 'Grocery' and 'Detergents_paper' which are positively correlated.  'Grocery' spending clearly increases with 'Detergent_paper' spending. Thus, the values of the feature 'Grocery' can be predicted given the values of 'Detergents_paper', and vice-versa. There is also evidence of some correlation between other pairs of features: 'Detergent_papers' versus 'Milk', and  'Grocery' versus 'Milk', which are all positive correlation. All the 6 attributes show a lognormal distribution (the data is not normally distributed). The distributions are skewed to the right with a long tail: the bulk of the customers have relatively small spending but a few customers have spending much larger.

# Data Preprocessing
In this section, we will preprocess the data to create a better representation of customers by performing a scaling on the data and detecting (and optionally removing) outliers. 
If data is not normally distributed, especially if the mean and median vary significantly (indicating a large skew), it is most [often appropriate](http://econbrowser.com/archives/2014/02/use-of-logarithms-in-economics) to apply a non-linear scaling — particularly for financial data. One way to achieve this scaling is by using a [Box-Cox test](http://scipy.github.io/devdocs/generated/scipy.stats.boxcox.html), which calculates the best power transformation of the data that reduces skewness. A simpler approach which can work in most cases would be applying the natural logarithm.

<script src="https://gist.github.com/jmlb/a74252d16914e1c16a4a1a9ceef24a0a.js"></script>
<img src="/images/201608/market_segmentation_output_22_0.png" width="90%">

The **Log transformation** is usually performed on skewed data to make it approximately normal, and that make the transformed data more appropriate to be used with models that assume a normal distribution, like Gaussian Mixture Model. After the transformation of the data, we see a clearer correlation between 'Detergent_papers' and 'Grocery', and also between 'Milk' and 'Detergent_papers', and 'Grocery' and 'Milk'.


Run the code below to see how the sample data has changed after having the natural logarithm applied to it.
<script src="https://gist.github.com/jmlb/3bd02e1950d32e490a2083b6bd6efb28.js"></script>

<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicatessen</th>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>0</th>
      <td>9.403107</td>
      <td>8.070594</td>
      <td>8.850088</td>
      <td>6.173786</td>
      <td>8.051978</td>
      <td>6.300786</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>1</th>
      <td>8.332068</td>
      <td>5.905362</td>
      <td>7.237059</td>
      <td>7.743270</td>
      <td>4.454347</td>
      <td>4.867534</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>2</th>
      <td>9.761924</td>
      <td>8.732305</td>
      <td>9.179262</td>
      <td>7.164720</td>
      <td>8.194229</td>
      <td>7.450661</td>
    </tr>
</table>
</div>


# Implementation: Outlier Detection
Detecting outliers in the data is extremely important in the data preprocessing step of any analysis. The presence of outliers can often skew results which take into consideration these data points. There are many "rules of thumb" for what constitutes an outlier in a dataset. Here, we will use [Tukey's Method for identfying outliers](http://datapigtechnologies.com/blog/index.php/highlighting-outliers-in-your-data-with-the-tukey-method/): An **outlier step** is calculated as 1.5 times the interquartile range (IQR). A data point with a feature that is beyond an outlier step outside of the IQR for that feature is considered abnormal.

<script src="https://gist.github.com/jmlb/764097d32e787505e10b7baaad702987.js"></script>

[+] Data points considered outliers for the feature 'Fresh':
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicatessen</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>65</th>
      <td>4.442651</td>
      <td>9.950323</td>
      <td>10.732651</td>
      <td>3.583519</td>
      <td>10.095388</td>
      <td>7.260523</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>66</th>
      <td>2.197225</td>
      <td>7.335634</td>
      <td>8.911530</td>
      <td>5.164786</td>
      <td>8.151333</td>
      <td>3.295837</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>81</th>
      <td>5.389072</td>
      <td>9.163249</td>
      <td>9.575192</td>
      <td>5.645447</td>
      <td>8.964184</td>
      <td>5.049856</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>95</th>
      <td>1.098612</td>
      <td>7.979339</td>
      <td>8.740657</td>
      <td>6.086775</td>
      <td>5.407172</td>
      <td>6.563856</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>96</th>
      <td>3.135494</td>
      <td>7.869402</td>
      <td>9.001839</td>
      <td>4.976734</td>
      <td>8.262043</td>
      <td>5.379897</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>128</th>
      <td>4.941642</td>
      <td>9.087834</td>
      <td>8.248791</td>
      <td>4.955827</td>
      <td>6.967909</td>
      <td>1.098612</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>171</th>
      <td>5.298317</td>
      <td>10.160530</td>
      <td>9.894245</td>
      <td>6.478510</td>
      <td>9.079434</td>
      <td>8.740337</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>193</th>
      <td>5.192957</td>
      <td>8.156223</td>
      <td>9.917982</td>
      <td>6.865891</td>
      <td>8.633731</td>
      <td>6.501290</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>218</th>
      <td>2.890372</td>
      <td>8.923191</td>
      <td>9.629380</td>
      <td>7.158514</td>
      <td>8.475746</td>
      <td>8.759669</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>304</th>
      <td>5.081404</td>
      <td>8.917311</td>
      <td>10.117510</td>
      <td>6.424869</td>
      <td>9.374413</td>
      <td>7.787382</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>305</th>
      <td>5.493061</td>
      <td>9.468001</td>
      <td>9.088399</td>
      <td>6.683361</td>
      <td>8.271037</td>
      <td>5.351858</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>338</th>
      <td>1.098612</td>
      <td>5.808142</td>
      <td>8.856661</td>
      <td>9.655090</td>
      <td>2.708050</td>
      <td>6.309918</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>353</th>
      <td>4.762174</td>
      <td>8.742574</td>
      <td>9.961898</td>
      <td>5.429346</td>
      <td>9.069007</td>
      <td>7.013016</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>355</th>
      <td>5.247024</td>
      <td>6.588926</td>
      <td>7.606885</td>
      <td>5.501258</td>
      <td>5.214936</td>
      <td>4.844187</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>357</th>
      <td>3.610918</td>
      <td>7.150701</td>
      <td>10.011086</td>
      <td>4.919981</td>
      <td>8.816853</td>
      <td>4.700480</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>412</th>
      <td>4.574711</td>
      <td>8.190077</td>
      <td>9.425452</td>
      <td>4.584967</td>
      <td>7.996317</td>
      <td>4.127134</td>
    </tr>
  </tbody>
</table>
</div>

[+]Data points considered outliers for the feature 'Milk':
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicatessen</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>86</th>
      <td>10.039983</td>
      <td>11.205013</td>
      <td>10.377047</td>
      <td>6.894670</td>
      <td>9.906981</td>
      <td>6.805723</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>98</th>
      <td>6.220590</td>
      <td>4.718499</td>
      <td>6.656727</td>
      <td>6.796824</td>
      <td>4.025352</td>
      <td>4.882802</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>154</th>
      <td>6.432940</td>
      <td>4.007333</td>
      <td>4.919981</td>
      <td>4.317488</td>
      <td>1.945910</td>
      <td>2.079442</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>356</th>
      <td>10.029503</td>
      <td>4.897840</td>
      <td>5.384495</td>
      <td>8.057377</td>
      <td>2.197225</td>
      <td>6.306275</td>
    </tr>
  </tbody>
</table>
</div>

[+] Data points considered outliers for the feature 'Grocery':
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicatessen</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>75</th>
      <td>9.923192</td>
      <td>7.036148</td>
      <td>1.098612</td>
      <td>8.390949</td>
      <td>1.098612</td>
      <td>6.882437</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>154</th>
      <td>6.432940</td>
      <td>4.007333</td>
      <td>4.919981</td>
      <td>4.317488</td>
      <td>1.945910</td>
      <td>2.079442</td>
    </tr>
  </tbody>
</table>
</div>

[+] Data points considered outliers for the feature 'Frozen':
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicatessen</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>38</th>
      <td>8.431853</td>
      <td>9.663261</td>
      <td>9.723703</td>
      <td>3.496508</td>
      <td>8.847360</td>
      <td>6.070738</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>57</th>
      <td>8.597297</td>
      <td>9.203618</td>
      <td>9.257892</td>
      <td>3.637586</td>
      <td>8.932213</td>
      <td>7.156177</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>65</th>
      <td>4.442651</td>
      <td>9.950323</td>
      <td>10.732651</td>
      <td>3.583519</td>
      <td>10.095388</td>
      <td>7.260523</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>145</th>
      <td>10.000569</td>
      <td>9.034080</td>
      <td>10.457143</td>
      <td>3.737670</td>
      <td>9.440738</td>
      <td>8.396155</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>175</th>
      <td>7.759187</td>
      <td>8.967632</td>
      <td>9.382106</td>
      <td>3.951244</td>
      <td>8.341887</td>
      <td>7.436617</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>264</th>
      <td>6.978214</td>
      <td>9.177714</td>
      <td>9.645041</td>
      <td>4.110874</td>
      <td>8.696176</td>
      <td>7.142827</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>325</th>
      <td>10.395650</td>
      <td>9.728181</td>
      <td>9.519735</td>
      <td>11.016479</td>
      <td>7.148346</td>
      <td>8.632128</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>420</th>
      <td>8.402007</td>
      <td>8.569026</td>
      <td>9.490015</td>
      <td>3.218876</td>
      <td>8.827321</td>
      <td>7.239215</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>429</th>
      <td>9.060331</td>
      <td>7.467371</td>
      <td>8.183118</td>
      <td>3.850148</td>
      <td>4.430817</td>
      <td>7.824446</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>439</th>
      <td>7.932721</td>
      <td>7.437206</td>
      <td>7.828038</td>
      <td>4.174387</td>
      <td>6.167516</td>
      <td>3.951244</td>
    </tr>
  </tbody>
</table>
</div>

[+] Data points considered outliers for the feature 'Detergents_Paper':
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicatessen</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>75</th>
      <td>9.923192</td>
      <td>7.036148</td>
      <td>1.098612</td>
      <td>8.390949</td>
      <td>1.098612</td>
      <td>6.882437</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>161</th>
      <td>9.428190</td>
      <td>6.291569</td>
      <td>5.645447</td>
      <td>6.995766</td>
      <td>1.098612</td>
      <td>7.711101</td>
    </tr>
  </tbody>
</table>
</div>

[+] Data points considered outliers for the feature 'Delicatessen':
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicatessen</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>66</th>
      <td>2.197225</td>
      <td>7.335634</td>
      <td>8.911530</td>
      <td>5.164786</td>
      <td>8.151333</td>
      <td>3.295837</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>109</th>
      <td>7.248504</td>
      <td>9.724899</td>
      <td>10.274568</td>
      <td>6.511745</td>
      <td>6.728629</td>
      <td>1.098612</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>128</th>
      <td>4.941642</td>
      <td>9.087834</td>
      <td>8.248791</td>
      <td>4.955827</td>
      <td>6.967909</td>
      <td>1.098612</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>137</th>
      <td>8.034955</td>
      <td>8.997147</td>
      <td>9.021840</td>
      <td>6.493754</td>
      <td>6.580639</td>
      <td>3.583519</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>142</th>
      <td>10.519646</td>
      <td>8.875147</td>
      <td>9.018332</td>
      <td>8.004700</td>
      <td>2.995732</td>
      <td>1.098612</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>154</th>
      <td>6.432940</td>
      <td>4.007333</td>
      <td>4.919981</td>
      <td>4.317488</td>
      <td>1.945910</td>
      <td>2.079442</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>183</th>
      <td>10.514529</td>
      <td>10.690808</td>
      <td>9.911952</td>
      <td>10.505999</td>
      <td>5.476464</td>
      <td>10.777768</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>184</th>
      <td>5.789960</td>
      <td>6.822197</td>
      <td>8.457443</td>
      <td>4.304065</td>
      <td>5.811141</td>
      <td>2.397895</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>187</th>
      <td>7.798933</td>
      <td>8.987447</td>
      <td>9.192075</td>
      <td>8.743372</td>
      <td>8.148735</td>
      <td>1.098612</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>203</th>
      <td>6.368187</td>
      <td>6.529419</td>
      <td>7.703459</td>
      <td>6.150603</td>
      <td>6.860664</td>
      <td>2.890372</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>233</th>
      <td>6.871091</td>
      <td>8.513988</td>
      <td>8.106515</td>
      <td>6.842683</td>
      <td>6.013715</td>
      <td>1.945910</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>285</th>
      <td>10.602965</td>
      <td>6.461468</td>
      <td>8.188689</td>
      <td>6.948897</td>
      <td>6.077642</td>
      <td>2.890372</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>289</th>
      <td>10.663966</td>
      <td>5.655992</td>
      <td>6.154858</td>
      <td>7.235619</td>
      <td>3.465736</td>
      <td>3.091042</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>343</th>
      <td>7.431892</td>
      <td>8.848509</td>
      <td>10.177932</td>
      <td>7.283448</td>
      <td>9.646593</td>
      <td>3.610918</td>
    </tr>
  </tbody>
</table>
</div>


[+] Data Point at index=128 is an outlier for 2 features <br>
[+] Data Point at index=65 is an outlier for 2 features <br>
[+] Data Point at index=66 is an outlier for 2 features <br>
[+] Data Point at index=75 is an outlier for 2 features <br>
[+] Data Point at index=154 is an outlier for 3 features <br>

Are there any data points considered outliers for more than one feature based on the definition above? 

<script src="https://gist.github.com/jmlb/4ca665ff03865f4502c2a230133c6e6e.js"></script>

<img src="/images/201608/market_segmentation_output_30_1.png" width="90%">

<script src="https://gist.github.com/jmlb/0d15918566a937360afab564db522108.js"></script>

[+] Data points that are outliers for at least 2 features:
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicatessen</th>
      <th>sum</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>128</th>
      <td>140</td>
      <td>8847</td>
      <td>3823</td>
      <td>142</td>
      <td>1062</td>
      <td>3</td>
      <td>14017</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>65</th>
      <td>85</td>
      <td>20959</td>
      <td>45828</td>
      <td>36</td>
      <td>24231</td>
      <td>1423</td>
      <td>92562</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>66</th>
      <td>9</td>
      <td>1534</td>
      <td>7417</td>
      <td>175</td>
      <td>3468</td>
      <td>27</td>
      <td>12630</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>75</th>
      <td>20398</td>
      <td>1137</td>
      <td>3</td>
      <td>4407</td>
      <td>3</td>
      <td>975</td>
      <td>26923</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>154</th>
      <td>622</td>
      <td>55</td>
      <td>137</td>
      <td>75</td>
      <td>7</td>
      <td>8</td>
      <td>904</td>
    </tr>
  </tbody>
</table>
</div>

[+] Customers with total spending < 4000 mu
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicatessen</th>
      <th>sum</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>97</th>
      <td>403</td>
      <td>254</td>
      <td>610</td>
      <td>774</td>
      <td>54</td>
      <td>63</td>
      <td>2158</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>98</th>
      <td>503</td>
      <td>112</td>
      <td>778</td>
      <td>895</td>
      <td>56</td>
      <td>132</td>
      <td>2476</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>131</th>
      <td>2101</td>
      <td>589</td>
      <td>314</td>
      <td>346</td>
      <td>70</td>
      <td>310</td>
      <td>3730</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>154</th>
      <td>622</td>
      <td>55</td>
      <td>137</td>
      <td>75</td>
      <td>7</td>
      <td>8</td>
      <td>904</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>275</th>
      <td>680</td>
      <td>1610</td>
      <td>223</td>
      <td>862</td>
      <td>96</td>
      <td>379</td>
      <td>3850</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>355</th>
      <td>190</td>
      <td>727</td>
      <td>2012</td>
      <td>245</td>
      <td>184</td>
      <td>127</td>
      <td>3485</td>
    </tr>
  </tbody>
</table>
</div>

There are 5 data points that are outliers for more than 1 feature: index=[ 128, 65, 66, 75, 154]. I decided to include only 1 customer in the list of outliers : **154**. 
First, **154** is an outlier for 3 features. Furthermore if we look at the total spending of this customer, it is the lowest within the full dataset (see table above). This customer does not contribute much to the bottom line of the wholesale distributor, and incorporating that data point might skewed the analysis.
Other data points like **65** are outliers for 2 features. However, the total spending of customer **65** is among the highest (sum=92562) and it might be more important for the company to use a segmentation model that includes this valuable customer.

# Feature Transformation
In this section we will use **principal component analysis (PCA)** to draw conclusions about the underlying structure of the wholesale customer data. Since using PCA on a dataset calculates the dimensions which best maximize variance, we will find which compound combinations of features best describe customers.

<script src="https://gist.github.com/jmlb/014336e1b17817d0fad2d56c37d6bbd9.js"></script>

<img src="/images/201608/market_segmentation_output_35_1.png" width="90%">

[+] How much variance in the data is explained in total by the first and second principal component?<br>
0.7142 (71.42%) variance is explained by the 1st and 2nd PCA Dimension. Note that the 1st component is mainly driven by 'Detergent_papers' feature and to a lesser extent by the 'Milk' and 'Grocery' features. The 2nd PCA dimension is dominated by the triplet: 'Fresh', 'Frozen' and 'Delicatessen' features. The 4 first PCA Dimensions have total explained variance of more than 0.9300. Therefore, using the first 4 components is enough to capture most of the variance of the original data, i.e the dataset can be compressed from 6 dimensions (features) to 4 dimensions without loosing much information. The dimensions of the PCA represent different spending patterns, and are defined by a weighted sum of the original features. For example:<br>

PCA_Dimension1 = 0.78 \* 'Detergents_Paper' + 0.5 \* 'Grocery' + 0.4 \* 'Milk' - 0.2 \* 'Frozen' - 0.2 \* 'Fresh' + 0.2 \* 'Delicatessen'.<br>

PCA_Dimension1 represents a spending pattern with higher spending on 'Detergents_Paper', 'Milk', 'Grocery' and 'Delicatessen' and decreasing spending on 'Frozen' and 'Fresh' products. PCA_Dimension1 is mostly correlated ( positively) to 'Detergents_Paper'.<br>

PCA_Dimension2: all product categories show positive weights but of different amplitudes, and the highest weight being for 'Fresh' and lowest rate for 'Detergents_paper'.<br>

PCA_Dimension3: is mainly correlated to 'Delicatessen' (negative correlation) and 'Fresh' (positive correlation). It reflects the case where customers who buy more 'Fresh' products, would consequently spend less on 'Delicatessen' and 'Frozen' products.<br>

PCA_Dimension4: is mainly correlated to 'Frozen' (positive correlation) and negatively correlated to 'Delicatessen' and 'Fresh'.<br>

<script src="https://gist.github.com/jmlb/04b9c016fa31ecb1fc39d2b4b8eb59e9.js"></script>

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Dimension 1</th>
      <th>Dimension 2</th>
      <th>Dimension 3</th>
      <th>Dimension 4</th>
      <th>Dimension 5</th>
      <th>Dimension 6</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>0</th>
      <td>1.1367</td>
      <td>-0.1357</td>
      <td>1.2913</td>
      <td>-0.6126</td>
      <td>0.4993</td>
      <td>0.0965</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>1</th>
      <td>-3.3629</td>
      <td>-1.7125</td>
      <td>0.3371</td>
      <td>0.7769</td>
      <td>0.3800</td>
      <td>0.6531</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>2</th>
      <td>1.5088</td>
      <td>1.3272</td>
      <td>0.5133</td>
      <td>-0.4019</td>
      <td>0.2411</td>
      <td>0.0286</td>
    </tr>
  </tbody>
</table>
</div>


#Dimensionality Reduction
When using principal component analysis, one of the main goals is to reduce the dimensionality of the data — in effect, reducing the complexity of the problem. Dimensionality reduction comes at a cost: Fewer dimensions used implies less of the total variance in the data is being explained. Because of this, the **cumulative explained variance ratio** is extremely important for knowing how many dimensions are necessary for the problem. Additionally, if a signifiant amount of variance is explained by only two or three dimensions, the reduced data can be visualized afterwards.

<script src="https://gist.github.com/jmlb/2862072559f4fea27dc9d95312cdc103.js"></script>

<script src="https://gist.github.com/jmlb/24aefe2fea62eae6333d495d28fb3b76.js"></script>
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Dimension 1</th>
      <th>Dimension 2</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>0</th>
      <td>1.1367</td>
      <td>-0.1357</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>1</th>
      <td>-3.3629</td>
      <td>-1.7125</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>2</th>
      <td>1.5088</td>
      <td>1.3272</td>
    </tr>
  </tbody>
</table>
</div>

<script src="https://gist.github.com/jmlb/8eced71643b8d913bdad3567901dbeb4.js"></script>

<img src="/images/201608/market_segmentation_output_45_0.png" width="90%">

Above, the scatter matrix is plotted for Dimension1 and Dimension2.
Note that Dimension1 shows a bimodal distribution. This is not unexpected: indeed, Dimension1 gets contributions mainly from 'Milk', 'Grocery' and 'Detergents_Paper' products (the highest weights). And we have seen earlier in this report, that the 2 features 'Grocery and 'Detergents_Paper' are positively correlated.
In contrast, Dimension2 shows a unimodal distribution: in this case, the main contributing features are 'Fresh', 'Frozen', and 'Delicatessen'. Those features are uncorrelated.

# Clustering

In this section, we will choose to use either a K-Means clustering algorithm or a Gaussian Mixture Model clustering algorithm to identify the various customer segments hidden in the data.

[+] **K-means clustering** is one of the simplest algorithm for unsupervised learning. It is fast and scale well with large dataset. Another advantage of the **K-means clustering** algorithm is that it makes no assumption on the distribution of the observations, and performed a hard assignment of the points to a cluster.<br>
[+] The **Gaussian Mixture Model** (GMM) compute to what degree (probability) a data point is assigned to a cluster: **GMM** offers the possibility to relax the assignment threshold depending on the data.

We will use the **GMM** algorithm for the following reasons:<br>
1. with the Log transformation, the data is closer to a normal distribution: we can see uni-modal and bi-modal distribution of some features (ref. to the scatter matrix of the  Log transformed features)<br>
2. our dataset is relatively small, so compute time is not an issue.


#Creating Clusters
Depending on the problem, the number of clusters that we expect to be in the data may already be known. When the number of clusters is not known *a priori*, there is no guarantee that a given number of clusters best segments the data, since it is unclear what structure exists in the data — if any. However, we can quantify the "goodness" of a clustering by calculating each data point's *silhouette coefficient*. The [silhouette coefficient](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html) for a data point measures how similar it is to its assigned cluster from -1 (dissimilar) to 1 (similar). 

<script src="https://gist.github.com/jmlb/f3e818ba7ee5cb55680f90b0b5b445e8.js"></script>
<img src="/images/201608/market_segmentation_output_51_0.png" width="90%">
<script src="https://gist.github.com/jmlb/be480b3e7f1881f364a41c6b9d373c14.js"></script>


We report the silhouette score for several cluster numbers we tried.
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th>Nbr of clusters</th>
      <th>silhouette score</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>2</th>
      <td>0.4</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>3</th>
      <td>0.39</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>4</th>
      <td>0.3</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>5</th>
      <td>0.28</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>6</th>
      <td>0.28</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>7</th>
      <td>0.32</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>8</th>
      <td>0.3</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>9</th>
      <td>0.3</td>
    </tr>
  
  </tbody>
</table>
</div>

[+] The best score is obtained when using 2 clusters. However, there is not much difference between the silhouette score when using 2 clusters or 3 clusters. Beyond 3 clusters, the silouette score drops significantly.


# Cluster Visualization

<script src="https://gist.github.com/jmlb/020c2365a411b19c7fb251500fb1b395.js"></script>
<img src="/images/201608/market_segmentation_output_55_0.png" width="90%">

Each cluster present in the visualization above has a central point. These centers (or means) are not specifically data points from the data, but rather the averages of all the data points predicted in the respective clusters. For the problem of creating customer segments, a cluster's center point corresponds to the average customer of that segment. Since the data is currently reduced in dimension and scaled by a logarithm, we can recover the representative customer spending from these data points by applying the inverse transformations.

<script src="https://gist.github.com/jmlb/90766fe49784dff8de667da1e0ebe3e5.js"></script>

<div>
<table border="1" class="dataframe">
  <thead>
    <tr  style="text-align: center; border:1px solid;">
      <th></th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicatessen</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>Segment 0</th>
      <td>3920.0</td>
      <td>5954.0</td>
      <td>9243.0</td>
      <td>989.0</td>
      <td>2806.0</td>
      <td>861.0</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>Segment 1</th>
      <td>8846.0</td>
      <td>2214.0</td>
      <td>2777.0</td>
      <td>2042.0</td>
      <td>375.0</td>
      <td>744.0</td>
    </tr>
  </tbody>
</table>
</div>

[+] What set of establishments could each of the customer segments represent?<br>

<script src="https://gist.github.com/jmlb/a0e50c6eb48d9fc52c0cb3eeff1747c7.js"></script>

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center; border:1px solid;">
      <th></th>
      <th>Fresh</th>
      <th>Milk</th>
      <th>Grocery</th>
      <th>Frozen</th>
      <th>Detergents_Paper</th>
      <th>Delicatessen</th>
    </tr>
  </thead>
  <tbody>
    <tr style="text-align: center; border:1px solid;">
      <th>Segment 0</th>
      <td>-4584.0</td>
      <td>2327.0</td>
      <td>4487.0</td>
      <td>-537.0</td>
      <td>1990.0</td>
      <td>-105.0</td>
    </tr>
    <tr style="text-align: center; border:1px solid;">
      <th>Segment 1</th>
      <td>342.0</td>
      <td>-1413.0</td>
      <td>-1979.0</td>
      <td>516.0</td>
      <td>-441.0</td>
      <td>-222.0</td>
    </tr>
  </tbody>
</table>
</div>

The segment0 spends much less on 'Fresh', 'Frozen' and 'Delicatessen' products compared to the median and the mean customer. In contrast spending on 'Grocery', 'Milk' is higher. This customer segment could represent Grocery stores. The segment1 spends less on 'Milk' 'Grocery' and 'Detergents_paper' products than the median: this segment might represent Restaurant.<br>

Another way to look at the data is to compute the normalized spending for each segment i.e:
\begin{align}
\%cost = \frac{\text{feature spending}}{\text{total spending}}
\end{align}

<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid">
      <td></td>
      <td></td>
      <td>total cost</td>
      <td>%cost_Fresh</td>
      <td>%cost_Milk</td>
      <td>%cost_Grocery</td>
      <td>%cost_Frozen</td>
      <td>%cost_Detergents_Paper</td>
      <td>%cost_Delicatessen</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>segment 0</th>
      <td>23773</td>
      <td>16.5</td>
      <td>25.0</td>
      <td>38.9</td>
      <td>4.2</td>
      <td>11.8</td>
      <td>3.6</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>segment 1</th>
      <td>16998</td>
      <td>52.0</td>
      <td>13</td>
      <td>16.3</td>
      <td>12.0</td>
      <td>2.2</td>
      <td>4.4</td>
    </tr>
</table>
</div>

[+] **segment_0**: 'Grocery' represents the largest spending followed by 'Milk', 'Fresh' and 'Detergents_paper'. This could represent the purchasing pattern of a Retailer.<br>
[+] **segment_1**: 'Fresh' represents the largest spending by far, followed prractically evenly by 'Milk', 'Grocery' and 'Frozen'. This could represent the purchasing pattern of a Restaurant.<br>


[+] For each sample point, which customer segment best represents it?
<script src="https://gist.github.com/jmlb/ddedc8477d01ea8deb00c5bcfa24b4a6.js"></script>

<div>
<table border="1" class="dataframe" style="font-size:11px">
    <tr style="text-align: center; border:1px solid">
      <td></td>
      <td>Customer</td>
      <td>total cost</td>
      <td>%cost_Fresh</td>
      <td>%cost_Milk</td>
      <td>%cost_Grocery</td>
      <td>%cost_Frozen</td>
      <td>%cost_Detergents_Paper</td>
      <td>%cost_Delicatessen</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>0</th>
      <td>26465</td>
      <td>45.8</td>
      <td>12.1</td>
      <td>26.4</td>
      <td>1.8</td>
      <td>11.9</td>
      <td>2.1</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>1</th>
      <td>8434</td>
      <td>49.3</td>
      <td>4.4</td>
      <td>16.5</td>
      <td>27.3</td>
      <td>1.0</td>
      <td>1.5</td>
    </tr>
    <tr style="text-align: center; border:1px solid">
      <th>2</th>
      <td>39888</td>
      <td>43.5</td>
      <td>15.5</td>
      <td>24.3</td>
      <td>3.2</td>
      <td>9.1</td>
      <td>4.3</td>
    </tr>
</table>
</div>

[+] sample_0 and sample_2 are best represented by segment_0 as their purchasing pattern better match segment_0. Although those two samples show higher %cost of 'Fresh' products, they also show lowest %cost for 'Delicatessen' and 'Frozen' similarly to segment_0. <br>
[+] sample_2 purchasing pattern matches better segment_1 where the lowest %cost are for 'Delicatessen' and 'Detergents_paper'.

The model predictions for each sample confirm our conclusion above. Note that in a earlier section of the assignment, we argued that sample_0 and sample_2 are likely in the same core business because of their similar purchasing pattern, and it is also confirmed by the model.

#Conclusion
[+] Companies will often run [A/B tests](https://en.wikipedia.org/wiki/A/B_testing) when making small changes to their products or services to determine whether making that change will affect its customers positively or negatively. If the wholesale distributor is considering changing its delivery service for example from currently 5 days a week to 3 days a week, is that going to affect the business?<br>

A higher frequency of delivery is likely to positively affect the customers that purchase mainly 'Fresh' and 'Milk' products. It might have no effect on other customers, although a higher frequency of truck delivery could be a disturbance (more traffic, etc..) and impact the customer' bottom line. In order to determine which customers would respond positively to new delivery service, one could split the customers in 2 similar sets (setA and setB): i.e customers belonging to segment_0 would be equaly split between setA and setB, and same thing for customers of segment_1.
We would then expose only one set (for example setA) to the new delivery service, and keep the service unchanged for the 2nd set of customers setB.
We can then compare setA and setB to determine what group of customers that the new service affects the most. <br>

[+]Additional structure is derived from originally unlabeled data when using clustering techniques. Since each customer has a **customer segment** it best identifies with (depending on the clustering algorithm applied), we can consider 'customer segment' as an **engineered feature** for the data. If the wholesale distributor recently acquired ten new customers and each provided estimates for anticipated annual spending of each product category. Knowing these estimates, the wholesale distributor wants to classify each new customer to a **customer segment** to determine the most appropriate delivery service.  How can the wholesale distributor label the new customers using only their estimated product spending and the **customer segment** data?  <br>

The target variable would be the **customer_segment**. We could train a Decision Tree Classifier (or any other classification algorithm) on existing customers using the **customer_segment** as the label. The wholesale distributor can then 'run' the new customers through the classifier which would then outputs the segment that the new customers belong to.<br>

Let 's Visualize Underlying Distributions:

At the beginning of this project, it was discussed that the `'Channel'` and `'Region'` features would be excluded from the dataset so that the customer product categories were emphasized in the analysis. By reintroducing the `'Channel'` feature to the dataset, an interesting structure emerges when considering the same PCA dimensionality reduction applied earlier to the original dataset. We can see how each data point is labeled either `'HoReCa'` (Hotel/Restaurant/Cafe) or `'Retail'` the reduced space.

<script src="https://gist.github.com/jmlb/f5703793e46c8c08b40b411be9ed2000.js"></script>
<img src="/images/201608/market_segmentation_output_71_0.png" width="90%">


The clustering model with 2 clusters reproduces fairly well the distribution of Hotel/Restaurant/Cafe and Retailer customer. Though, there are a few data points (green) that are embedded in a large population of red data points. Our GMM model shows a clearer/sharper boundary between the 2 segments. The customers with Dimension2 in the range [-2, 2] and Dimension1 < -1 are likely to have a very high probability to belong to segment1, but there would still be a negligible probability that the customer belongs to segment0.