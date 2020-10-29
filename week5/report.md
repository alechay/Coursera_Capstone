# Capstone Project: Comparing Neighborhoods in Washington, DC
Alexander Hay <br>
October 2020

## 1. Introduction/ Business Problem
Washington, DC is the capital of the United States and is home to nearly 700,000 residents. In the early 2000s, the Office of Planning divided the city into 39 "reasonably descriptive units of the city," or "clusters." They eventually added 7 more clusters, including Rock Creek Park, the National Mall, and the Naval Observatory, which I have omitted from this analysis, as they lack "significant neighborhood character." Since their creation, these cluster boundaries have been maintained by city officials and have allowed for the comparison of different regions of the city.
Source: Open Data DC

It would be useful for city officials to group these clusters based on their characteristics. This would allow officials to identify inequities between the clusters, and allocate resources to different parts of the city more efficiently. Therefore, my first goal was to "cluster" these neighborhood clusters based on their number of restaurants, and on their crime, income, employment, population, and housing data.

Additionally, it would be useful for real estate developers to understand which neighborhood characteristics have the greatest effect on housing prices. Therefore, my second goal was to identify the features (number of restaurants, crime, income, employment, population) that are most correlated with housing prices. And, to go one step further, my third goal was to create a model, based on those features, that predicts housing prices.

To reiterate, this project has 3 main goals:
1. To "cluster" these neighborhood clusters based on their number of restaurants, and on their crime, income, employment, population, and housing data.
2. To identify the features (number of restaurants, crime, income, employment, population) that are most correlated with housing prices.
3. To create a model, based on those features, that predicts housing prices.

## 2. Data
To gather location data for the neighborhood clusters, I used Open Data DC. The geojson file included the shape and area of all 39 clusters. <br> 
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/geo.png) <br>
Link: [Open Data DC](https://opendata.dc.gov/datasets/f6c703ebe2534fc3800609a07bad8f5b_17)

To gather demographic data for the neighborhood clusters, I used data from Urban-Greater DC. The various csv files included information about crime, income, employment, population, and housing for all 39 clusters. For each file, I took the most recent data. I was able to get data from 2016 for crime, income, employment, and population, however I could only get data from 2010 for housing. I realize that this is a discrepancy and it could affect the results, but it is unlikely that the relative differences between housing prices across neighborhoods changed significantly from 2010 to 2016. <br>
Crime:<br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/crime.png) <br>
Income:<br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/income.png) <br>
Employment:<br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/employment.png) <br>
Population: <br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/population.png) <br>
Housing: <br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/population.png) <br>
Link: [Urban-Greater DC](https://greaterdc.urban.org/data-explorer?geography=cl17)

To gather nearby venue data in each of the clusters, I used the Foursquare API. Foursquare provides location data for venues and events. After I found the centroid of all the neighborhood clusters, I searched Foursquare to find restaurants within 250 meters of that point. Below is a map showing the restaurants in each cluster. <br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/restaurants.png)

## 3. Methodology
### "Clustering" neighborhood clusters
First, I read the geojson file and found the centroid of each neighborhood cluster. Each centroid point was split into its latitude and longitude components. I then used the Foursquare API to get a list of restaurants within 250 meters of each centroid. I counted the number of restaurants for each cluster and created a new dataframe. The resulting map is shown above.

Then, I read the demographic data (crime, income, employment, population, housing). For each file, I took the most recent data. I was able to get data from 2016 for crime, income, employment, and population, however I could only get data from 2010 for housing. I extracted the following features: property crimes per 1000 residents, violent crimes per 1000 residents, average family income, percentage unemployed, total population, and median sales price of a single family home. I merged this data with the restaurant data to get the full dataset.

I normalized the data using the sklearn.preprocessing.scale() function. This "centers the data by removing the mean value of each feature, then scales it by dividing non-constant features by their standard deviation." I did this because many machine learning algorithms assume that all features are centered around zero and have variance in the same order. I then clustered the neighborhood clusters using the KMeans algorithm, an unsupervised machine learning technique. The KMeans algorithm "clusters data by trying to separate samples in n groups of equal variance, minimizing a criterion known as the inertia or within-cluster sum-of-square." I decided on 3 clusters after experimenting with different numbers. I showed a map of the 3 neighborhood clusters and then tried to identify commonalities/differences between them. 

Source: sklearn documentation.

### Identifying features most correlated with housing prices
First, I split the data into the features and labels. The features consisted of property crimes per 1000 residents, violent crimes per 1000 residents, average family income, percentage unemployed, and total population. The labels were the median sales price of a single family home.

I then examined the distribution of each of the features and the labels. Most features were not normally distributed, and neither were the labels. This could have affected the results. <br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/restaurants_dist.png)
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/propcrimes_dist.png) <br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/viocrimes_dist.png)
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/income_dist.png) <br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/unemp_dist.png)
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/population_dist.png) <br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/housing_dist.png)

I then created a correlation matrix, showing the Pearson Correlation Coefficient for each two-variable relationship. I looked specifically at which features were most correlated with the median sales price of a single family home.

### Creating a model that predicts housing prices
First, I performed a 70/30 split of my dataset into a train and test set. I then fit 3 different models to the training data and made predictions on the test data. I evaluated these predictions using mean squared error.

The first model I tried was linear regression, which "fits a linear model with coefficients w = (w1, …, wp) to minimize the residual sum of squares between the observed targets in the dataset." Not suprisingly, this is best suited for linear data.

The second model I tried was random forest regression, which is an ensemble technique. Specifically, random forest "is a meta estimator that fits a number of classifying decision trees on various sub-samples of the dataset and uses averaging to improve the predictive accuracy and control over-fitting." This is better suited for nonlinear data than linear regression.

The third model I tried was gradient boosting regression, which is also an ensemble technique. "GB builds an additive model in a forward stage-wise fashion; it allows for the optimization of arbitrary differentiable loss functions. In each stage a regression tree is fit on the negative gradient of the given loss function." Again, this is better suited for nonlinear data than linear regression.

Source: sklearn documentation

## 4. Results
### "Clustering" neighborhood clusters
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/cluster_map.png)

Using the KMeans algorithm, I clustered the neighborhood clusters into 3 groups. As you can see, the groups separated nicely, with little geographic overlap. I made the following observations upon inspection of the groups:
- The first group (red) had more varied features than the others, but generally these neighborhood clusters had lots of venues, a low unemployment rate, and a high population.
- The neighborhood clusters in the second group (purple) typically had low average income, a high unemployment rate, and low housing prices.
- The neighborhood clusters in the third group (green) typically had a low crime rate, a high average income, a low unemployment rate, and high housing prices.

### Identifying features most correlated with housing prices
Using a correlation matrix, I examined each two-variable relationship. The matrix is below: <br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/corr_matrix.png) <br>

I also looked specifically at which features were most correlated with the median sales price of a single family home. It appeared that the unemployment rate, average family income, and violent crimes per 1000 residents were most correlated with the housing prices. <br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/label_corr.png)

### Creating a model that predicts housing prices
The predictions of the linear regression, random forest, and gradient boosting models had mean squared errors of 0.33, 0.31, and 0.55. None of the models performed especially well, but the linear regression and random forest models both performed much better than the gradient boosting model. Overall, the random forest model performed the best. <br>
![alt text](https://github.com/alechay/Coursera_Capstone/blob/main/week5/images/models.png)

## 5. Discussion
The KMeans algorithm clustered the neighborhood clusters nicely; there was a clear geographic separation between the groups. The groupings made sense, as western DC consists of wealthy areas such as Georgetown, Adam's Morgan, and Friendship Heights. Central DC is considered "downtown", and consists of areas such as Dupont Circle, Columbia Heights, Chinatown, and Capitol Hill. Eastern DC consists of poor areas such as Fort Totten, Ivy City, and Anacostia. These clusters reinforce what DC officials already know about the city, but nevertheless it is useful to visualize these boundaries on a map. In the future, I would like to form clusters based on more features, and do a more thorough manipulation of the number of KMeans clusters.

Unsuprisingly, I found that the unemployment rate, average family income, and violent crimes per 1000 residents were most correlated with the housing prices. It makes sense that housing prices are high where there is a low unemployment rate, a low crime rate, and a high average income. This is a well-established phenomenon in many cities, but nonetheless it is useful for real estate developers to know that DC is no exception. Additionally, the relationship between number of restaurants and housing prices was modest, as it had a Pearson's Correlation coefficient of 0.4. Perhaps the relationship is only modest because I did not specify the type of restaurant. I suspect that if I limited the criteria to only expensive restaurants, there would be a stronger correlation with housing prices. In the future, I would like to see how other features, such as average school rating, are correlated with housing prices.

None of the models performed especially well, but the linear regression and random forest models both predicted housing prices better than the gradient boosting model. Perhaps if I tuned the hyperparameters of the gradient boosting model it would have performed better. Overall, I think the models performed poorly because there were not enough observations. A 70/30 split gives only 19 observations in the training set and 8 observations in the test set. In the future, I would like to add more observations, and potentially more features, to boost the performance of these models. Additionally, I would like to apply more complicated models, such as neural networks.

## 6. Conclusion
This project produced 3 main findings.

1. Using the number of restaurants, property crimes per 1000 residents, violent crimes per 1000 residents, average family income, percentage unemployed, total population, and median sales price of a single family home, the KMeans clustering algorithm produced 3 groups of DC neighborhood clusters. The city was effectively divided into the west side, downtown, and east side.
2. The variables most correlated with housing prices were the unemployment rate, average family income, and violent crimes per 1000 residents. Housing prices were only modestly correlated with the number of restaurants.
3. Using the number of restaurants, property crimes per 1000 residents, violent crimes per 1000 residents, average family income, percentage unemployed, and total population, the random forest regressor best predicted the median sales price of a single family home. The linear regressor performed similarly, and the gradient boosting regressor performed the worst. However, none of the models performed especially well, and more observations are needed to create a better model.

## Sources
- Open Data DC: Geojson data
- Demographic data: Greater DC public data
- Foursquare API data: Nearby venues
- Sklearn documentation
- Dependencies: See requirements.txt
