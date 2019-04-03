# Wang Pei (Peggy)

## Personal Info
1. GIRL
2. Master in EE at [EPFL](https://www.epfl.ch/)
3. Love coding (a little bit)
4. Interest in Machine Learning, Deep Learning and food.

## Projects List
Projects are always courses' projects at EPFL. SAD >_<
### 1. Movie Recommendation
This is the final project of NTDS course. Our team uses date sets from [kaggle](kaggle.com). As we first wrote our own project for recommendation engine, this project seems a little bit simple. 
#### - Data
[MovieLens](https://grouplens.org/datasets/movielens/20m/)
Stable benchmark dataset. 20 million ratings and 465,000 tag applications applied to 27,000 movies by 138,000 users. Includes tag genome data with 12 million relevance scores across 1,100 tags. Released 4/2015; updated 10/2016 to update links.csv and add tag genome data.
We used all three .csv file for recommendation with different pruposes: movie recommendations for movie and user.
#### - Codes
As course required, we need to write jupyter notebook for final presentation. The code can be found [here](https://github.com/peggy95/movie_recommendation/tree/master/code).
#### - Presentation
Benefitting from the course presentation, there is a [PPT](https://github.com/peggy95/movie_recommendation/blob/master/ppt.key) for this project.
#### - Improvement
After learning other advanced methods for recommendation, shallow learning like Collaborative Filtering based on items or users and deep learning like DCN, DeepFMs can be employed for recommendation in terms of these data sets. 

### 2. Robust Journey Planning
This is the final project of Lab of Data science course. We used the data published by the Open Data Platform Swiss Public Transport (<https://opentransportdata.swiss>). This project is not only plan a journey like google maps but also need to consider the probabilities of failure when journey containing transmits.
#### - Data
[SBB dataset](<https://opentransportdata.swiss>)
Unfortunately, the full description from opentransportdata.swiss is only provided in German. The folder contains the actual data [istdaten](<https://opentransportdata.swiss/en/dataset/istdaten>) and the station list data [BFKOORD_GEO](https://opentransportdata.swiss/de/cookbook/hafas-rohdaten-format-hrdf/#Abgrenzung). For more info about [data](https://github.com/peggy95/route_planning/edit/master).
#### - Codes
The code in .ipynb can be found [here](https://github.com/peggy95/route_planning/blob/master/final_deliverable.ipynb).
#### - Presentation
There is a [PPT](https://github.com/peggy95/route_planning/blob/master/presentation_Dslab.pdf) for this project. We briefly present our main idea and show planned jounery.
#### - Improvement
Firstly I would like to use google provided api to plan the origin journey instead of implementing by myself as the route plan part was too raw. Then, more attention needs be paid to failure probabilities of transits. In this project, we just caculatue the statistics for every station. It would be fine if the number of distinct stations is handleable but when the number is huge it would be better to cluster stations using their historical data. Also, rush hours and non-rush hours need to be considered.
