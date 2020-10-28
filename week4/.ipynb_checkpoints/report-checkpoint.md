# Capstone Project: Comparing Neighborhoods in Washington, DC
Alexander Hay
October 2020

## 1. Introduction/ Business Problem
Washington, DC is the capital of the United States and is home to nearly 700,000 residents. In the early 2000s, the Office of Planning divided the city into 39 "reasonably descriptive units of the city," or "clusters." They eventually added 7 more clusters, including Rock Creek Park, the National Mall, and the Naval Observatory, which I have omitted from this analysis, as they lack "significant neighborhood character." Since their creation, these cluster boundaries have been maintained by city officials and have allowed for the comparison of different regions of the city.
Source: Open Data DC

It would be useful for city officials to group these clusters based on their characteristics. This would allow officials to identify inequities between the clusters, and allocate resources to different parts of the city more efficiently. Therefore, my first goal was to "cluster" these neighborhood clusters based on their number of restaurants, and on their crime, income, employment, population, and housing data.

Additonally, it would be useful for real estate developers to understand which neighborhood characteristics have the greatest effect on housing prices. Therefore, my second goal was to identify the features (number of restaurants, crime, income, employment, population) that are most correlated with housing prices. And, to go one step further, my third goal was to create a model, based on those features, that predicts housing prices.

To reiterate, this project has 3 main goals:
1. To "cluster" these neighborhood clusters based on their number of restaurants, and on their crime, income, employment, population, and housing data.
2. To identify the features (number of restaurants, crime, income, employment, population) that are most correlated with housing prices.
3. To create a model, based on those features, that predicts housing prices.

## 2. Data
To gather location data for the neighborhood clusters, I used Open Data DC. The geojson file included the shape and area of all 39 clusters. 
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")
Link: [Open Data DC](https://opendata.dc.gov/datasets/f6c703ebe2534fc3800609a07bad8f5b_17)

To gather demographic data for the neighborhood clusters, I used data from Urban-Greater DC. The various csv files included information about crime, income, employment, population, and housing for all 39 clusters. For each file, I took the most recent data. I was able to get data from 2016 for crime, income, employment, and population, however I could only get data from 2010 for housing. I realize that this is a discrepancy and it could affect the results, but it is unlikely that the relative differences between housing prices across neighborhoods changed significantly from 2010 to 2016.
Crime:<br>
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")
Income:<br>
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")
Employment:<br>
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")
Population: <br>
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")
Housing: <br>
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")
Link: [Urban-Greater DC](https://greaterdc.urban.org/data-explorer?geography=cl17)

To gather nearby venue data in each of the clusters, I used the Foursquare API. Foursquare provides location data for venues and events. After I found the centroid of all the neighborhood clusters, I searched Foursquare to find restaurants within 250 meters of that point. Below is a map showing the restaurants in each cluster.
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

## Sources
- Open Data DC: Geojson data
- Demographic data: Greater DC public data
- Foursquare API data: Nearby venues
- Dependencies: See requirements.txt