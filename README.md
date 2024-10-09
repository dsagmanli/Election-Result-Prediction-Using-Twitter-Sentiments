
# Predicting 2020 US Election Results Using Tweet Sentiments

A comprehensive study where the 2020 US Presidential Election results were correctly predicted using the sentiments from 2 million tweets tweeted around the time of the election. The main assumption was that we would calculate positive tweets / all tweets per candidate. Between Donald Trump and Joe Biden, whoever had a higher ratio would have to have been the winner. This assumption turned out to be true as Joe Biden had a higher ratio.

Another goal was to create a novel approach and compare it to the state-of-the-art, while proving the novel approach to be scalable and fault-tolerant, which it was. 
## Table of Contents

- [Objective](#objective)
- [Methodology](#methodology)
- [How To Run](#how-to-run)
- [Credits](#credits)

## Objective

The objective of this study was to see the effect of social media sentiments on election results by analyzing whether the candidate with a higher ratio of positive tweets / all tweets actually won the election. Since we had to deal with more than 2 million tweets, this was a big data problem and had to be handled accordingly. We used Azure databricks and a custom server cluster to run the sentiment analysis. We used Apache Spark (PySpark) to handle data cleaning and wrangling. Textblob has been used to split sentiments into positive, neutral and negative. Then, the task was to split the neutral tweets further into positive and negative. This part had to be done with a logistic regressor ML model.


Another important goal was to show the scalability and fault tolerance of the solution. For this, we compared Spark MLlib's logistic regression model to a custom solution we made using Spark's MLlib's Random Forest model. 
## Methodology

The text data was cleaned and hashtags, emojis and non-English tweets were gotten rid of. Textblob was deployed to classify the tweets into positive, neutral and negative, to create the true labels for our data. Then, neutral labels had to be classified further to make a clear-cut assumption. To do so, two methods have been used. The first one was using Spark MLlib's logistic regression model. The second was a custom novel approach that used a distributed version of scikit-learn's logistic regression model. The novel aspect was that 3 logistic regression models were trained at the same time. The driver node would then broadcoast these models to the worker nodes to provide fault tolerance, and then the prediction results were collected back at the driver node to be aggregated. Majority voting has been used. This means that if 2 models claimed a tweet was positive but one model claimed it was negative, positive was chosen. This ensemble approach provided more accuracy and avoided bias.

The custom novel approach has been tested for scalability by comparing the runtime for training with 10% of the data and 20% of the data. If the difference in runtime was linearly correlated to the difference in data size, the approach would have been deemed scalable, which it was.

In addition, the mean absolute errors of the novel approach and the regular approach turned out to be comparable, while the custom approach proved to be scalable and more fault tolerant due to using a distributed approach.

Finally, both approaches found Joe Biden to be the more favorable candidate, which he was.
## How To Run

IMPORTANT: You need to import the .csv files and the notebooks into Azure Databricks and set up a cluster with at least one driver and one worker. The code is optimized to run under these conditions. If the code crashes or if you interrupt the code (This happens when you overload the cluster, it is not related with the code itself), be sure to disconnect from and reconnect to the cluster before re-running the entire notebook. The code has been split into several notebooks as the data is too big to be run at once and stored in memory altogether.

IMPORTANT 2: Since GitHub doesn't allow handling of files larger than 100mb, the data has not been included but can be found at: https://www.kaggle.com/datasets/manchunhui/us-election-2020-tweets

1) First run the TwitterSentimentAnalysis_V1_TRUMP.ipynb file to get the cleaned data for Donald Trump's tweets.

2) Run the TwitterSentimentAnalysis_V1_BIDEN.ipynb file to get the cleaned data for Joe Biden's tweets.

3) To get the solution for the regular ML solution for comparison for both candidates, run the regular ML Solution combined_mllib.ipynb file.

4) To get the solution for the custom ML solution for comparison for both candidates, run the Custom ML Solution combined.ipynb file.

## Credits

This project was done as a group effort with my teammates Arnaaz, Arun, Harish, Hassan and Saanidhya. The data came from the Kaggle competition at https://www.kaggle.com/datasets/manchunhui/us-election-2020-tweets

Refer to the Project_Report.pdf file to read the full report.
