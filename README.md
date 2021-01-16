Netflix, the world’s leading Internet television network and the most-valued largest streaming service in the world, had developed a movie recommendation system: CinematchSM. Its job is to predict whether someone will enjoy a movie based on how much they liked or disliked other movies.Cinematch uses "straightforward statistical linear models with a lot of data conditioning". And while Cinematch is doing pretty well, it can always be made better.Using only the training data, Cinematch scores an RMSE of 0.9514 on the quiz data and a similar performance on the test set, 0.9525</p>


<p><b>This case study is done with the motivation of learning how a recommendation system works by taking some of the knowledge already presented in the literature and attempting to implement some new ideas if it is possible to attain a better RMSE score than that obatined with Cinematch.However due to computation limitation, i am taking only a fraction of the total data provided in Netflix Prize data</b></p>

<br>
<br>

<p><b>The objectives of this case study and how we map those to a Machine Learning problem :</b></p> 

1. To predict the rating that a customer would give to a movie that he has not yet rated.For a given movie and user we need to predict the rating.This recommendation problem can also seen as a Regression problem <br>

2.	To minimize the difference between predicted and actual rating.The performance metrics used are Mean Absolute Percentage Error(MAPE) and Root Mean Square Error(RMSE)

3. While achieving the objectives we also need to mantain some form of interpretability.


<h2> Detailed Approach </h2>
--> We have used Surprise module of python to train models like baseline,SVD,SVD++.
--> We have also used XGBRegressor of xgboost module in python.

Details of python modules used:<br>
1. datetime( for handing date and time) <br>
2. pandas and numpy(mathematical modules) <br>
3. matplotlib and seaborn(for visualization) <br>
4. scipy( for handling sparsity)
5. sklearn ( for similarity measures and error metrics)<br>
6. xgboost and surprise (for modelling) <br>
7. os (handling os calls like to check if file is present in local directory) and random ( for  random number generation) <br>


Steps used: <br>

1. <b> Data Accumulation and Preprocessing:</b>  We have accumulated the data from txt files provided. However they are not in a proper format.So we transform the data into csv format and store them in csv files and also save them in temporary pickle files so that they can be reused easily(as the size of data is very large). The columns in the data are customrID, movieID , Ratings and the date.So for each customer(with custID) there is a movie(with movieID) that he has rated and the Ratings that he gave to the movie along with Date in which the rating was given.<br>

We then check for missing values in the data (found None). We also check and remove any duplicate entries.<br>

We then split the data into Train and Test Sets.<br>

2. Data Visualization: Basic data visualization is done the results of which are clearly described in the notebook.<br>

3. Now we create the USER_ITEM matrix which basically defines each user in rows and the columns describes the movies that he/she has rated. The Values in each cell represented by the rating that the USER(row Id) has given to the  movie(column Id). We use this matrix to calculate the Average Ratings per user, average rating per movie and also calculated the global average rating of all movies. All these will be used in the modelling later.<br>


5. We then create the USER-USER and USER-ITEM similarity matrix using the USER-ITEM matrix.These matrices are very important to the problem.The USER-USER matrix tells us for each user, which other users are most similar and the ITEM-ITEM matrix tells us for each movie , which movies are most similar.So we can recommend movies to a user by either looking at the top simiar users of that user and finding the movies that they have rated or by looking at the top similar movies to the movies that the user has rated and then recommend them the same.These are called NEIGHBOURHOOD methods.<br>

We calculate the average rating given by top similar users(of a user*) to a movie rated by a user* and use those as features in the original data matrix.Simalarly for movies, we find the average rating given to the top similar movies of the  movie rated by an  user and use those as features in the original data matrix.<br>


6.Then we go on to modelling.Here we first transorm the train data and test data that  can be used by Surprise for modelling.Then we apply XGBOOSt on the original data matrix to find the rating given by an USER to a MOVIE.We use the results as features on the subsequent models.We also calculte the RMSE and MAPE(error metrics) given by each models<br>


7. We apply surprise baseline method to create features, use those features with the XGBOOST features to create model,calculate the RMSE and MAPE given by that model and pass the results as features to the next model.<br>

8. We use neighbourhood methods for the next model and implement the Surprise KNN Baseline model to the given dataset.Then we use Latent factor models which uses SVD and then use the implicit feedback method by using SVDpp.
















 
    