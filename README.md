# Predicting Video Games Sales using Machine Learning
Goal: This analysis will be aimed at answering the following business questions:

1. What are the factors and patterns in successful video games that result in high sales?
2. How does the user and critic reviews impact the global sales?
3. How does the sales vary between the Americas, Europe & Asia?
4. What are some actionable insights which can be used by both sides of the aisle, creators and publishers, to enhance the user experience and increase the revenue?

<img src="img/1.png?raw=true"/>

## INTRODUCTION
The video game industry is a rapidly growing industry; in 2018, worldwide video games sales generated revenue of $134.9 billion. Since the recent Coronavirus pandemic has forced people to socially isolate and find activities to do alone, online gaming is seeing record numbers since video games can be a great way to counter boredom and connect with friends digitally. In fact, the World Health Organization even recommended video games as a healthy pastime during social isolation. Given this trend, companies are directing efforts in the development of new games. Game developers who want to capitalize on this growing market would benefit from knowing how best to spend their efforts in order to maximize sales.

In order to find answers to these important business questions, we use a supervised model (decision tree) & an unsupervised model (clustering) after conducting the preliminary data exploration & setting up the initial hypothesis.

### Data Preparation
In this project, we analyzed a cross-referenced dataset from VGChartz and MetaCritic, found on Kaggle.com. The data from VGChartz contains the globalwide video game sales data classified by region and the MetaCritic data contains the rating and feedback critiques from users. These had been combined, cross-referenced by game name. The following figures show the variables of the two datasets. 

<img src="img/2.png?raw=true"/>

In the VGChartz dataset, we get basic information as well as the sales information of the video games. With the video game names cross referenced with the MetaCritic dataset, we get the information of the ratings, user scores and counts, and critic scores and counts of each video game. 

### Data Cleaning
Before jumping into the building of the analytical models, we conducted data cleaning and briefly explored the dataset with some descriptive statistics and exploratory analysis to get a big picture of the trends and patterns going on in the industry and the data. 
To avoid redundancy and bias in analysis, our data cleaning process consists of three main parts: 1) delimitation to transform commas to rows/columns 2)removing missing and null values 3)labeling nominal strings. The data cleaning process helped us remove about 1000 inadequate data points (originally 7504 data points to 6497 data points).

### Data Exploration
Our exploratory data analysis helped us fly through our data set with an overall view and grasps of benchmark. The following histogram and bar charts show the amount of video sales by platform, release year, genre, and rating. Some key exploratory points from the statistics are:

●	PS2, X360, and PS3 are the top three platforms of video game sales <br>
●	Video game sales were booming during 2002-2009 <br>
●	Action, Sports, and Shooter games are the top three selling game genre <br>
●	Games of E (everyone) and T (teenager) content rating proportioned for more sales <br>

<img src="img/3.png?raw=true"/>
<img src="img/4.png?raw=true"/>
<img src="img/5.png?raw=true"/>
<img src="img/6.png?raw=true"/>

As for regional sales, we can see from the following bar chart that North America accounted for about half of the global video game sales. 
<img src="img/7.png?raw=true"/>

In addition, the following chart of user and critic counts and scores gives us a broad picture and bench mark of how users rate and critique. 
<img src="img/8.png?raw=true"/>

## Model: Supervised Learning
#### Hypothesis

For Global Sales, we hypothesize that
1.	Higher critic and user scores are correlated with higher sales. 
2.	Brand matters. So, the publisher and platform will have a significant impact on sales.
3.	On average, games that have been available for a long time are likely to have sold more copies than relatively newer games.

#### Data Transformation & Selection
Target: Global Sales <br>
Inputs: Genre, Platform, Publisher, Rating, Year of Release, User Score, Critic Score <br>
Assessment Method: Average Squared Error <br>
After data exploration, we found that the distributions of User Score and Critic Score are quite skewed. So we use the log function to transform those 2 variables. Then their skewness decreased. Besides, we also filtered the rows with missing values in critic scores and user scores.  <br>

#### Model Selection
We tried both decision tree and logistic regression. Since there are too many publishers and platforms, logistic regression doesn’t work well, and also it gives us higher error value. Thus, we decide to use the decision tree.

### Decision Tree
For the decision tree model, we also compared 2-branch and 3-branch models. The latter gives us a  lower error value of 4.470248. Thus, we decide to choose the 3-branch model for analysis.

<img src="img/9.png?raw=true"/>
<img src="img/10.png?raw=true"/>

Using the 3-branch model, we get 29 leaves after pruning to avoid overfitting. The sequence of variable importance is: Transformed Critic Score > Publisher > Platform > Genre > Transformed User Score > Rating

<img src="img/11.png?raw=true"/>
By looking into the variable importance and the treemap, we are able to know what kind of combinations of criteria are correlated to higher sales on average. Thus, we check our hypotheses.

### Results
#### Global Sales
Variable importance shows that the Transformed Critic Score is closely associated with global sales. Then are Publisher and Platform.
The treemap shows us the leaves with the highest average global sales. We also apply the corresponding criteria into the analysis tool to get the number of games fitting into that class.

Transformed critic score:[4.4128, 4.5379); Platform: PS2, X360, PS3, WII, PSP...; Publisher: Activision; Genre: Shooter
Number of games: 25 
Examples: Call of Duty series, Overwatch
Average Sales: 9.3814 million
<br>
Transformed critic score:[4.4128, 4.5379); Platform: PS2, X360, PS3, WII, PSP...; Publisher: Nintendo 
Number of games: 85
Examples: Advance Wars series, Animal Crossing
Average Sales: 7.2010 million
<br>

#### Hypothesis Test:

1.	Generally speaking, high critic scores are likely to be associated with high sales, while user scores do not exert a significant impact on sales.
2.	Both platform and publisher are influential to sales. As expected, Activision
and Nintendo are the most competitive publishers. Newer platforms had much
higher sales.
3.	The year of release is not significant to sales. We assume that there should be a lifecycle for 
most games. Perhaps within 1 to 3 years after release is the period when most transactions are done.
Thus, among our 3 hypotheses, only the second is fully confirmed. The first one is half true, while the last one is false. This conclusion may be helpful for those developers. A game developer can look into the games that won high critic scores to analyze some similarities among those games. Also, choosing the right platform and publishers plays a significant role in affecting sales. If possible, famous Publishers like Nintendo and Activision are always safe choices.

After analyzing global sales, we also want to know whether regional sales followed a similar pattern or not. Using the same inputs and methods, we look into the sales in North America, Europe, and Japan.

#### Regional Sales

##### 1.	Sales in North America <br>

After running our decision tree with NA_Sales as the target variable, there were 24 leaves after pruning, and the  ASE value was 1.123327. <br>
The output from the decision tree node also yielded a table of Variable Importances.

<img src="img/12.png?raw=true"/>

Variable Importance: Transformed Critic Score >  Platform > Publisher > Rating > Transformed User Score > Year of Release <br>

Generally speaking, North America Sales follows a similar pattern with Global Sales. The transformed Critic score still ranks first. Nintendo is still favored by the public. However, compared to Global Sales, Platform exceeds Publisher, and Genre becomes an insignificant factor in North America. <br>

We check our 3 hypotheses with the North America results: <br>
●	Critic Score plays an important role in deciding the sales in North America, however, user scores do not have a big impact. NA Sales shares the same pattern with Global Sales <br>
●	Supporting our initial hypothesis big platform & publisher brands do matter when it comes to sales in North America <br>
●	Contrary to our hypothesis, the effect of the year of release on sales is also negligible <br>


##### 2.	Sales in Europe 

After running our decision tree with EU_Sales as the target variable, we obtained a tree with 17 leaves after pruning to avoid overfitting, and the ASE value of the model was 0.571663. The output from the decision tree node also yielded a table of Variable Importances. 

<img src="img/13.png?raw=true"/>

The order of Variable Importance is: Transformed Critic Score > Platform > Publisher  > Transformed User Score >  Rating > Genre. <br>
Like the Global and North American sales, the critic score, platform, and publisher continue to be the three most important factors affecting sales in Europe. <br>

Verification of our Initial hypothesis: <br>
●	Supporting our first hypothesis, higher transformed critic score and user score correlate with higher sales. Compared to the user score, the critic score had a greater impact on sales <br>
●	In line with our second hypothesis, publisher & platform brands did matter in the European sales  <br>
●	Contrary to our third hypothesis, the year of release shad no impact on sales in Europe <br>


##### 3.	Sales in Asia (Japan)  
After running our decision tree with Jp_Sales as the target variable, we obtained a tree with 11 leaves after pruning to avoid overfitting, and the ASE value of the model was 0.073981. The output from the decision tree node also yielded a table of Variable Importances.

<img src="img/14.jpg?raw=true"/>

The order of Variable Importance is: Publisher > Transformed Critic Score > Year of Release > Transformed User Score > Platform > Rating > Genre. Publisher, Critic score, and Year of Release are the 3 most significant factors affecting sales in Japan. This is quite different compared to the global sales, as the year of release plays an important role in Japanese sales unlike in any other region. <br>

Verification of our Initial hypothesis: <br>

●	Supporting our first hypothesis, higher critic scores and user scores correlated with higher sales. While critic scores played a larger role out of the two, user scores also played their part <br>
●	Contrary to our second hypothesis, platforms with a great brand value had little impact on sales in Japan <br> 
●	Unlike global patterns and in confirmation with our third hypothesis, the year of release did not matter in predicting sales in Japan <br>

Overall, the inference from the regional sales models is that North American and European video game sales were quite similar to the global sales pattern whereas Japanese sales were a bit different.

### Model: Unsupervised Learning
#### Hypothesis
The results of the decision tree analysis helped us find the effects of specific variables, like Platform, Publisher, Year of Sale and Region, on video game sales. Next, the team decided to use an unsupervised model to obtain some more holistic patterns and similarities about successful video games.  <br>
To do this, a Clustering model was built, to group games that were similar across all features. Prior to clustering, the team did not have many hypotheses about similarities in video games; rather we were curious to discover interesting patterns in the data. The two initial hypotheses were:

1.	The most consistently well-rated games will also generate the highest sales. 
2.	The majority of the highest selling games will be Action and Shooter games. 

#### Model
The initial data was imported using a File Import node. The imported data was passed to Filter node, which filtered out data points that had missing values (which only occurred in the critic score and critic count variables). This brought the number of datapoints from 7504 data points in the original dataset, to 6497 data points in the filtered dataset. The output of this was passed into the Cluster node in SAS, and only the Interval variables (all sales, user and critic scores and counts) were used in the Clustering. The Cluster properties were set to automatically determine the number of clusters. The results of the Cluster node were used to make inferences about similarities in video games, using the mean datapoint of each cluster. The export data feature was also used to tabulate the designated cluster for every datapoint, and visually identify patterns in the members of each cluster. Finally, the output of this Cluster node was passed into a Segment Profile node, which gave detailed statistics about the video games in each cluster. Histograms of data distribution per cluster were used to quantitatively compare the typical characteristics of each cluster. 

#### Results

1.	The Cluster node yielded the Mean Statistics data shown in Figure X below, which shows the mean value for each of the interval variables used for clustering.

<img src="img/missing.png?raw=true"/>

The model generated four clusters using Ward’s method. Cluster 1 had 3902 data points, cluster 2 had 278, cluster 3 had 1931 and cluster 4 had 403 data points. Cluster 1 had the lowest sales, scores and number of reviews in all the parameters. Cluster 3 was the next-lowest in terms of sales, yielding 3-4 times more sales than Cluster 1, but still less sales than Clusters 2 and 4. Cluster 4 was the “best” cluster, performing consistently better in terms of all sales variables, except Japan Sales. For Japan sales alone, the average game in Cluster 2 had 7.7 times more sales in Japan than the average game in Cluster 4 [54,516 sales in cluster 4, compared to 419,245 sales in cluster 2]. 

2.	Looking closer at the properties of Cluster 4 in Figure X, it is observed that these games with the highest global sales also had the greatest User_Count (i.e., number of user ratings on Metacritic). However, they did not have as high of a User_Score (mean value of user reviews out of 100) as Clusters 2 and 3. Therefore, it appears that for these “best” games in Cluster 4, the number of users leaving reviews had a stronger correlation with the sales than the actual mean score left by those users.

3.	To understand this strange behavior in Japan Sales for Cluster 2 games, the specific members of each Cluster was viewed using the ‘Exported Data’ section of the Cluster node. The output showed each datapoint, along with a new column indicating the finalized cluster. A portion of the data for Cluster 2 is shown below. 

<img src="img/15.jpg?raw=true"/>

The games in Cluster 2 did not show any unique patterns in the platform, Genre or Year of Release. However, it was observed qualitatively from the ‘Developer’ and ‘Publisher’ field that the vast majority of games were made by Japanese developers, and were released by Japanese publishers like Nintendo, Sony, Namco, Square Enix and Sega. 

4.	The output from the Cluster node also yielded a table of Variable Importances.

<img src="img/16.png?raw=true"/>

Global_Sales had the highest variable importance in segmenting the clusters (1.00000), while User_Count (number of users who left reviews) had the second highest importance (0.83614). This further supports the earlier observation that the User_Count is strongly correlated with the sales of a video game. Notably, User_Score (mean value of user reviews out of 100) had the least importance among all the variables. 

### Analysis

The initial hypothesis that the most well rated games generated maximum sales was confirmed. The model also confirmed that Action and Shooter games were the most popular genres and found the Sports genre to do well too. <br>

##### Qualitative Interpretation of Clusters: <br>
The results of Mean Statistics helped in the formation of some high-level interpretations of how the video games were segmented by the clustering algorithm: <br>
●	Cluster 1: Low performing games with low global sales (mean 210,000 units), low mean user and critic scores (64 and 67), and very low user and critic counts. These games lacked both quality and profitability, and from the low number of reviews, it can be said that they did not generate much traction/ interest from gamers. <br>

●	Cluster 2: Japanese-made games that sold very well in Japan (mean 419,000 units), that are also rated well by users and critics. This small cluster of 278 games seems to represent Japanese games targeted towards and loved by Japanese audiences, that thoroughly outperformed the global best-selling games. This phenomenon will be further discussed below. <br>

●	Cluster 3: Fringe well-reviewed games that sold much less than the global best-sellers, but still sold 3-4 times more, on average, than games in Cluster 1. Qualitative analysis of the cluster showed that this cluster consisted of uncommon games that did not have great brand value. <br>
However, these games seem to be of high quality since they had higher user scores and comparable critic scores to those of Cluster 4. This quality can be further noted by seeing that the mean sales for this category was about 25% of the mean sales in the “best” group, Cluster 4. However, the mean number of user reviews for this category seems to be a disproportionately large 56% of the number of reviews in Cluster 4. This shows that despite the lower sales, a greater fraction of users felt motivated to leave reviews of the game, which, as seen from the mean user score and critic score, were mostly positive.  <br>

●	Cluster 4: The “best” cluster, with the highest sales, highest critic scores and number of user and critic reviews. They also had moderately high user scores. These appeared to be the popular, best-selling games from large international brands like EA, Activision, and Nintendo. The eliteness of this class can be seen by noticing that only 403 games out of the 6497 games in the dataset fall into this class. An extension of this project might choose to determine what differentiates these games, by performing a qualitative analysis of the game content, as well as an in-depth market analysis for each game. <br>

##### Correlation between User_Count and Sales: <br>
Figure X shows an interesting correlation: across all 4 clusters, the number of users leaving reviews had a stronger correlation with the global sales than the actual mean score left by those users. This is consistent with the variable importance table, which shows that the two most important variables were Global_Sales and User_Count. One interpretation of this is that the best-selling games are not necessarily ones that are of high quality (higher score), but ones that catch the attention of users online. It is possible that games that are unique, controversial and generate more discussion are more likely to stand out and market themselves through word-of-mouth, compared to games that work on safe, established formulae. This is related to Jonah Berger’s Contagious Theory, which highlights the importance of sharing content online as one of the key drivers for how products and ideas catch on. <br>

##### Japan Sales for games in Cluster 2: <br>
The anomalous behavior of Japan sales in Cluster 2 points to demographic differences in players in Japan vs. other regions. Video games that did well in Japan did well elsewhere as well, however games that were popular in NA and EU were, on average, much less popular in Japan. Japanese developer Keiji Inafune attributed this difference to the fundamental difference in style preferences between Eastern and Western players. He notes that while Japanese players grow up watching anime and manga and prefer more fantasy games, western players are used to realistic movies and TV shows, and prefer the same in their video games. [2] These findings can help publishers and creators understand their users’ preferences. The importance of this finding is that Western publishers should spend greater effort trying to popularize their games in Japan, since it is the third largest video game market in the world. By understanding what works in Japan and replicating those traits in their games, Western developers can help their games become much stronger competition to native Japanese games.  <br>

## Conclusion
The discussed analysis gave a few answers to the questions we began with. It found actionable insights which can be used by both sides of the aisle, creators and publishers to enhance user experience and increase revenue. To highlight a few of them: <br>

●	Newer platforms like XBOX, PS2, PS3, etc. had high sales which can be attributed to the fact that many players already have such platforms and hence it makes sense to make games which are compatible with these platforms.  <br>
●	The importance of customer centricity in product life cycle. Western video game creators and publishers would do well to understand the fundamental difference between eastern and western players as delineated before, to get a foothold in the lucrative Japanese market.  <br>
●	The importance of traction and a game’s ability to create a ‘buzz’ as a way to generate publicity and higher global sales.   <br>

