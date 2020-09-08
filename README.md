# Table of Contents
* [Motivation](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#motivation)

* [Data Collection](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#data-collection)

* [Data Description + Intial thoughts](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#data-description--intial-thoughts)

* [EDA](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#eda)

  * [Mean of Popularity of songs Per year](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#mean-of-popularity-of-songs-per-year)
  * [Univariate Plots ](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#univariate-plots)
  * [Bivariate Plots](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#bivariate-plots)
  * [Correlation Heatmap](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#bivariate-plots)
  * [ANOVA](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#bivariate-plots)
 
* [Classification](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#classification)

  * [Logistic Regression](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#logsitic-regression)
  * [Random Forest](https://github.com/ArnabPushilal/SpotifyProject/blob/master/README.md#random-forrest-classifier)
* [Regression](https://github.com/ArnabPushilal/SpotifyProject#regression)
  * [Random Forest Regressor](https://github.com/ArnabPushilal/SpotifyProject#regression)
    * [Metrics](https://github.com/ArnabPushilal/SpotifyProject#metrics)
    * [Output Distribution](https://github.com/ArnabPushilal/SpotifyProject#output-distribution-vs-actual-distribution)
    * [Feature Importance] (https://github.com/ArnabPushilal/SpotifyProject#feature-importance-according-to-this-model)
  * [Ridge Regression ] (https://github.com/ArnabPushilal/SpotifyProject#ridge-regression-with-polynomial-features)
* [My Song](https://github.com/ArnabPushilal/SpotifyProject#my-own-song)
* [Further Work] (https://github.com/ArnabPushilal/SpotifyProject#further-work)
  
 
## Motivation
Just in the past year I had released 2 songs on spotify. As far as marketing went I felt it just bounced around my friends and back. As I forayed into Data Science, I realized that Spotify analyzes each track & assigns it a score.  So what my main motive was to see if my song 'lacked' in some elements according to spotify. To do this I wanted to create a model which would predict the popularity score assigned by spotify for an unknown track.

## Data Collection
I collected the data using the spotipy library built around the spotify API

I intially queried the song's by year  & maximum items returned per call was 50, but you could offset the index of the first result which helps you get more than 50( This allowed a max of 2000 per year) . To make the data base large enough I collected samples from 1980 - 2020 ( ~80000 data points).The other function I defined was to collect audio features by spotify for each track based on the track id.

* https://developer.spotify.com/documentation/web-api/reference/tracks/get-audio-features/ ( end point for audio features )
* https://spotipy.readthedocs.io/en/2.14.0/ ( spotipy library )

## Data Description + Intial thoughts

### Description of the Data Collected ( according to spotify docs) :
|Attribute |Description| Type |
|--- | --- | --- |
|1 - Acousticness (float) | A confidence measure from 0.0 to 1.0 of whether the track is acoustic.| Numerical|
|2 - Danceability (float)| Danceability describes how suitable a track is for dancing based on a combination of musical elements including tempo, rhythm stability, beat strength, and overall regularity.| Numerical |
|3 - duration_ms (int)| Duration of the track in ms|Numerical|
|4 - energy (float)| Energy is a measure from 0.0 to 1.0 and represents a perceptual measure of intensity and activity. Typically, energetic tracks feel fast, loud, and noisy.|Numerical|
|5 - instrumentalness (float) | Predicts whether a track contains no vocals. “Ooh” and “aah” sounds are treated as instrumental in this context. |Numerical|
|6 - key (int)| The estimated overall key of the track. 0 represents C. 1 C# and so on.| Categorical |
|7 - liveness (float)|Detects the presence of an audience in the recording. Higher liveness values represent an increased probability that the track was performed live.|Numerical|
|8 - loudness| The overall loudness of a track in decibels (dB) |Numerical|
|9 - mode (int)| Mode indicates the modality (major or minor) of a track, the type of scale from which its melodic content is derived.|Categorical |
|10 - speechiness |(float)  Speechiness detects the presence of spoken words in a track. The more exclusively speech-like the recording (e.g. talk show, audio book, poetry), the closer to 1.0 the attribute value.|Numerical|
|11 - tempo (int)| The overall estimated tempo of a track in beats per minute (BPM). In musical terminology, tempo is the speed or pace of a given piece and derives directly from the average beat duration. |Numerical|
|12 - time signature (int)|  An estimated overall time signature of a track.| Categorical |
|13 - valence (float) |  A measure from 0.0 to 1.0 describing the musical positiveness conveyed by a track. Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), while tracks with low valence sound more negative (e.g. sad, depressed, angry).| Numerical| 

Apart from this I added another data point 'Numer of Markets' the song was released in. Country wise list was available from the 'search' end point
As a musician some of the data points already made sense to me. For instance the 'key' , 'mode' ,'time signature' , ' tempo' are familiar terms. Now some of the abstract and insteresting terms are 'valence' , 'instrumentalness' , 'energy' ,'Danceablity' are kind of vague. It will be interesting to see if these features are contributing to overall popularity. Since we have no way of physically changing the 'valence' as we do for the 'key' of the song.

# EDA

First things first, I can see a direct correlation with Year of Release and Popularity. 

## Mean of Popularity of songs Per year

![Year Vs Average PopularityScore](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/Year_vs_AvgPopScore.png)

* We can see there is a direct correlation between a song's popularity & the year of release. 
* Initially I was about to dump all the data into one model, then I thought it would be prudent of me to *make the model decade-wise*. Since all the new test data can't really be from the past. 
* For 2020 there is a drop in average & 605 Null values were there. This goes to suggest that there is probably a mimimum threshold of time that a song needs to be on spotify for the PopularityScore to be calculated. For this purpose I will ignore the songs of 2020.
* It would also be interesting to see the how different are the features for the songs in each decade & how they correlate with each other


## Univariate plots

### 2000s
![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/Univariate_00s.png)

### 1980s
<img src="https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/Univariate_80s.png" width="1000" height="400">

* Distributions of 'acousticness', 'speechiness' , 'energy ', 'instrumentalness' seems to be skewed. I will probably have to try the box-cox trasnformation for some of the variables. '
* In categorical variables 'key' of 4/4 is the most common 

## Bivariate plots

* I transformed PopularityScore into 'Popular' & 'Not Popular' *by percentile for each decade*. Using qcut , I divided the data into 10 quantines and then took the 10th quantile as Popular & rest as not Popular. I tried out lesser divisions of quantiles (top 25th percentile, top 20th percentile) but that didn't work out well since I felt that the score was not high enough for the top 25th percentile to classify it as popular (it was 44 for the 80s). I didn't go beyond 10 quantiles to make sure *my classes are not too imbalanced.
* 
(Plots are Prior to log transformation)

### 1980s pairplot 
![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/80s.png)

* For single variables distributions of 'PopQuant 0 & 1' look very similar. This is not good.
* Loudness looks like it has a slightly different mean for 0 & 1
* By virtue of linear separability we can the data can be seaparated by loudness & dancebility / loudness & valence.
* We can see some variables which are linearly correlated, which I guess would be more clear with correlation heatmap

## Correlation Heat Map Year-wise

### 1980s

![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/HeatMap_80s%20(1).png)

### 1990s

![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/HeatMap_90s%20(1).png)

### 2000s

![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/HeatMap_00s%20(1).png)

### 2010s

![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/HeatMap_10s%20(1).png)

* No single variable has good linear correlation with 'PopQuant' which goes to *show a linear model won't do really well.

* 'Energy & Loudness' are correlated heavily, will probably get rid of one of the variables

* Interestingly 'danceability' & 'PopQuant' has better correlation in the 10s than any other decade

* Similary 'loudness' & 'danceability' also have increasing correlation along the decades. I guess louder is more danceable in the present

## One-way ANOVA for Numerical variables[between groups PopQuant==0 & PopQuant ==1  post log transformation ]
### 1990s
|Attribute|danceability|	energy|	loudness|	valence	|liveness|	tempo|	acousticness|	duration_ms|	instrumentalness|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|f	|5.301233e+01	|8.507439e+01	|2.053819e+02	|13.541535|	14.840970|	2.452146|	8.469009e+01|	1.019611|	1.105531e+02|
|pValue|	3.437428e-13|	3.151434e-20|	2.372030e-46|	0.000234	|0.000117|	0.117380|	3.824398e-20|	0.312624|	8.656026e-26|

* So usually a low p value would help us reject our null hypothesis. But I'm suprised that it's so low for danceability,energy,loudness, acousticness,instrumentalness even when the distributions in the pairplot looked very identical. 

* Perhaphs the a non parametric test like Kruskals would have been a better choice.

# Classification [ 2010s DataSet ]

## Logsitic Regression

* I made a param grid consisiing of the different solvers & C values ( inverse of regularization ) and then applied a simple grid search approach to find the best model. 
* To select the metric I thought the AUC of the ROC would be better since, If I chose a normal accuracy metric you would get a high score even when you predict all 0s since I have explicitly chosen a 1/10 ratio for Popular & Not popular.

* I used class weight = 'balanced' to help with the imblance problem I have.

### ROC  
* logistic : test ROC AUC=0.704
* logistic :train ROC AUC=0.713
![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/ROC_logistic.png)


### Confusion Matrix for Logistic Regression

| Class     |  precision   | recall | f1-score |  support|
| --- | --- | --- | --- | --- |
|0     |  0.95     | 0.66  |    0.77  |    3613|
|1      | 0.17  |    0.65   |   0.27    |   387|


* So  it looks like my model is getting a lot of false positives for the 'Popular' class. This is expected as using class weight='balanced' you tend to get more false alerts.

### Feature Importance with this model

![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/feature_imp_2.png)

* Most important features contributing to 'Popularity - 1' seems to be loudness , danceability & NoOfmarkets. 
 .
## Random Forest Classifier


* For Random Forest I did a random search on the hyperparameters.

### ROC

* Random : test ROC AUC=0.729
* Random :train ROC AUC=1.000
![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/ROC_Random%20(2).png)
 
* The model turned out to be completely overfitting on the training data & not as good on the test data.

# Regression [2010s DataSet]

## Random Forest Regressor

* Similar to classification, a random-search + cv was performed on the RandomForest Regression Model

### Metrics

|Data |Mean Absolute Error (MAE) | Mean Squared Error (MSE)| Root Mean Squared Error (RMSE) |Mean Absolute Percentage Error| Accuracy|
| --- | --- | --- | --- | --- | --- |
|Train |2.3380337756197442| 8.5386173504373 |2.922091263194444| 3.9 | 96.1|
|Test | 6.422792491484789 |63.37485329803256| 7.960832450066548| 10.64 |89.36|

* MAPE is about 10.6% & Absolute Error is 6.4422 for test data. So for a scale of 0-100 , Not bad?

### Output Distribution vs Actual Distribution
### Train Data 

![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/Actual%20vs%20Fitted%20Values%20for%20Popularity_train_rf%20(2).png)
* Train data seems to be following the distribution pretty well.

### Test Data
![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/Actual%20vs%20Fitted%20Values%20for%20Popularity%20_test_rf.png)
* Although means seems about the same, std-dev has gone up. Extremely high / low predictions may be very error prone.
* Training data is clearly being overfit, this is similar to the classifier!

### Feature Importance according to this model

![](https://github.com/ArnabPushilal/SpotifyProject/blob/master/images/feature_imp.png)

* It looks like the model can't distinguish between the features too well, but nevertheless 'danceability' & 'No of Market' has high importance
* Intrestingly, the controllable musical features like 'key' ,'mode','time signature' is contributing the least! Therefore , I will probably have to dig deeper into how spotify calculates features like 'danceability', 'acousticness'

## Ridge Regression with polynomial features 

* Although polynomial trends weren't clear in the EDA, I wanted to try it out.
* It makes sense to use Ridge Regression to penalize higher coefficients.
* So I made a grid up of alpha from 0.1 - 1000 & degrees 1,2,3 to see it could trump the random forest model.

|Data |Mean Absolute Error (MAE) | Mean Squared Error (MSE)| Root Mean Squared Error (RMSE) |Mean Absolute Percentage Error| Accuracy|
| --- | --- | --- | --- | --- | --- |
|Train |6.66623249851526| 62.309629632070724| 7.893644888900863| 10.53| 89.47|
|Test | 6.463273484157742 |64.66623249851526| 8.041531725891234| 10.75 |89.25|


# My own song

| Track_Name | Valence | Danceability | loudness | duration_ms |instrumentalness | key | liveness |time_signature |energy | track_id | NoOfMarkets|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Far Away |  0.289 | 0.653 | -15.519 |181680 | 0.404|7 |  0.108|4 |0.332 |0y2Ga0Au2NZaRuIGEAiwTQ | 89 |

* If you go back to the univariate distributions the only thing which I can see improve is 'danceability'. Other factors don't seem to matter as much. That makes sense too , who cares what key or time signature you are in. 


# Further Work

* A lot more things probability go into popularity. We can try and add features from artist's social media- activity, no of followers, likes -etc
* Feature Engineering could be done to help the model
* Compare songs with high danceability scores in order to understand what the feature exactly mean
* Compare each decade and see what features are important in each decade




 
