# Uncovering Coordinated Campaigns: Analyzing Artificial Inflation of Trend Popularity on Twitter 
# 10 Twitter trends - each trend of size in GB's is taken into consideration

# Abstract 
The use of social media has become a crucial part of modern communication, with Twitter being one of the most popular platforms for sharing information and opinions. However, the authenticity of trending topics on Twitter has come into question, with concerns over artificial inflation of trend popularity. In this project 10 trends were analyzed and processed 10 trends, for #advancehbdmaheshbabu trend, analyzing a dataset of over 302,595 tweets related to this topic. The main objectives were to identify any patterns or anomalies in the data and to develop a dashboard that could help users determine whether a trend is genuine or artificially inflated. Using various Py Spark and analytical techniques, some unusual metrics associated with the #advancehbdmaheshbabu trend, including a high percentage of tweets where likes were less than retweets (83%), a significant number of duplicate tweets (13%), and an average time gap of 3,230 seconds between tweets, average retweets per account were found to be 12 and total distinct users are 9,921, were discovered. These findings are significant, as they suggest that the trend may have been artificially inflated, rather than being a genuine reflection of user interest. To address these concerns, a dashboard that allows users to monitor trends and track audience engagement was developed. The dashboard features various metrics which can help users determine whether a trend is genuine or not. This tool has implications for both the intended audience and the broader community, providing a way to ensure that trending topics on Twitter accurately reflect user interests and opinions. 
 
# Introduction 
Social media platforms have become an integral part of our daily lives, and Twitter is no exception. With millions of tweets being posted every day, analyzing Twitter trends can provide valuable insights into the opinions and interests of people around the world. In this project, 10 different twitter trends were analyzed, focusing on the hashtag #advancehbdmaheshbabu, which has a dataset of 302,595 tweets. It also focuses on the trends in political campaigns with the Indian General Election in February 2019, two twitter trends emerged, one in support of Modi with the hashtag #Welcomemodi, and the other against him with #Gobackmodi. Using PySpark, data processing and analytics on the collected tweets were performed and key findings and results were derived. This project aims to help the intended audience and broader community to better understand the significance and implications of Twitter trends, and to develop tools such as a dashboard that can monitor trends and distinguish between genuine trends and artificially inflated ones. In the following sections an overview of the project's background, objectives, methodology, key findings, and implications is provided.  

 # Data Collection 
The data collection process for this project involved using the Twitter API to retrieve tweets for each of the 10 selected trends. The tweets were retrieved using specific search queries that included relevant hashtags, keywords, and timeframes. The data was collected in JSON format, which is the default format provided by the Twitter API. Each tweet was represented as a JSON object that contained various fields such as user information, date and time of the tweet, tweet content, and engagement metrics such as retweets and likes. 

![Archirechture!](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/0d6aa943-f729-4eea-aa43-704a8e0a2944)

3.1  Dataset Description 
A detailed explanation of each column that was pulled during the data collection process: 
- `tweet.card`: The Twitter card type of the tweet. 
- `tweet.cashtags`: The cashtags used in the tweet. 
- `tweet.content`: The text content of the tweet. 
- `tweet.conversationId`: The ID of the conversation thread that the tweet is a part of. 
- `tweet.coordinates`: The geolocation coordinates associated with the tweet, if any. 
- `tweet.date`: The date and time when the tweet was created. 
- `tweet.hashtags`: The hashtags used in the tweet. 
- `tweet.id`: The unique ID of the tweet. 
- `tweet.inReplyToTweetId`: The ID of the tweet that this tweet is in reply to, if any. 
- `tweet.inReplyToUser`: The username of the user that this tweet is in reply to, if any. 
- `tweet.json`: The raw JSON data of the tweet. 
- `tweet.lang`: The language of the tweet. 
- `tweet.likeCount`: The number of likes that the tweet has received. 
- `tweet.links`: The URLs included in the tweet. 
- `tweet.mentionedUsers`: The usernames of the users mentioned in the tweet. 
- `tweet.place`: The name of the place associated with the tweet, if any. 
- `tweet.quoteCount`: The number of times that the tweet has been quoted. 
- `tweet.quotedTweet`: The ID of the tweet that this tweet has quoted, if any. 
- `tweet.rawContent`: The raw content of the tweet. 
- `tweet.renderedContent`: The rendered content of the tweet. 
- `tweet.replyCount`: The number of times that the tweet has been replied to. 
- `tweet.retweetCount`: The number of times that the tweet has been retweeted. 
- `tweet.retweetedTweet`: The ID of the tweet that this tweet has retweeted, if any. 
- `tweet.source`: The source from which the tweet was posted (e.g., Twitter for iPhone). 
- `tweet.user`: The ID of the user who posted the tweet. 
- `tweet.username`: The username of the user who posted the tweet. 
- `tweet.vibe`: The sentiment score of the tweet. 
- `tweet.viewCount`: The number of times that the tweet has been viewed.  

# Data Cleaning and Preprocessing 
After loading all the datasets into one, some data cleaning and preprocessing on the twitter data was performed. First, missing values were checked in each column and dropped the 'Unnamed: 0' column. The 'date' column was then cast to a timestamp data type and dropped unwanted columns such as card, cashtags, coordinates, and more. Next, the missing values in numeric columns were filled with 0 and replaced empty strings with ‘None’ values. To prepare the text data for analysis, all text were converted to lowercase, leading/trailing spaces and duplicate whitespaces were removed and any remaining multiple whitespaces with single whitespaces were replaced. Overall, these data cleaning and preprocessing steps were important to ensure that the data was in a usable format for analysis. By removing unnecessary columns and filling missing values, the noise in the data was reduced and ensured that the analysis was based on a complete dataset. 



# Project Architecture 
The project started with the goal of analyzing Twitter data related to a particular hashtag to gain insights. To achieve this, it was decided that cloud-based architecture would be used to handle the large volume of data and the processing required. Snscrape and Sntwitter were first used. Then, TwitterSearchScraper was used to retrieve twitter data related to the hashtag and dump it into an S3 bucket in JSON format. S3 was chosen because it is a scalable and cost-effective way to store large volumes of unstructured data. Next, PySpark was used to perform data cleaning, analysis, and structuring of the data on AWS EMR. Then the cleaned data was stored in a structured form in Parquet format back in S3. Parquet was chosen because it is a columnar storage format that is optimized for performance and can handle large datasets efficiently. 
Finally, the structured data from S3 was loaded into AWS Redshift, a cloud-based data warehousing solution. Redshift was chosen because it allows for high-speed querying and can handle large volumes of structured data efficiently. Then, AWS Quicksight was connected to Redshift to create an interactive dashboard that provides real-time insights into user sentiment and behavior related to the hashtag. 

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/b9da366e-b5ca-4297-8bad-cb2db5ac8439)


In summary, this architecture allows efficient processing and analysis of large volumes of twitter data using cloud-based solutions. This then enables the gain of valuable insights into user behavior and sentiment related to the hashtag in real-time, which can be used for various business and marketing purposes. 

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/a538bcb8-7116-49ee-aa3a-cc647a0796b6)

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/cd7bb89d-069e-4998-a1fd-d2b1b0e0d880)

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/5e8073e5-ed01-4bb2-9583-db7197bf2856)

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/431a0bfd-3ada-4de0-aa07-ce72dcb7973c)

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/1c911f42-d2ec-4f56-aedf-2f18c6da5c78)




# Data Analysis and Dashboard 

Twitter Trends Analysis: #advancehbdmaheshbabu 
The data consisted of a total of 302,595 tweets, with 83% of tweets having more likes than retweets. A total of 38,719 duplicate tweets were found, making up 13% of the total tweets. The data also showed that there were 9,921 distinct users and an average of 12 retweets per account. The average time gap between tweets was calculated to be 3,230 seconds. The data further showed that there were 12,923 quote tweets and 23,121 replies in total. 
To better understand the data and its patterns, common users in the trends were also analyzed, revealing that only 4.08% of the users were common across different trends. The data visualization process allowed for a clear representation of these insights and facilitated better decision-making for this project. 

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/8502edf6-82ab-4d49-b58b-7d6894e24228)

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/a554812b-bb9b-4fa6-af0a-63f22adabd27)

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/53145c08-2c86-4b3d-a0c6-2df7cdb9fcf5)

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/e66c66e8-49aa-4576-9c25-52d36ee84d31)


# Twitter Trends Analysis: #Gobackmodi vs #Welcomemodi 
During the Indian General Election in February 2019, two Twitter trends emerged, one in support of Modi with the hashtag #Welcomemodi, and the other against him with #Gobackmodi. Using data engineering and analytics techniques, data related to these two trends were extracted and processed to gain insights into user engagement and sentiment. 
For the anti-Modi campaign (#Gobackmodi), it was observed that a total of 57,202 tweets, 188,825 retweets, and 289,639 likes. Additionally, it was found that 18% of tweets had more retweets than likes, and 19% of tweets were duplicates. There were a total of 14,162 distinct users, with an average of four retweets per account and an average time gap between tweets of 2,248. Furthermore, there were 17,658 quote tweets and 24,039 replies, with only 2.86% common users in trends. 
On the other hand, for the pro-Modi campaign (#Welcomemodi), 32,493 tweets were analyzed, 75,053 retweets, and 114,410 likes. Similarly, 18% of tweets had more retweets than likes, and 40% of tweets were duplicates. The number of distinct users was 5,992, with an average of three retweets per account and an average time gap between tweets of 455. 7,490 quote tweets and 10,014 replies, with 2.86% common users in trends were also found. 

# The first two images show how the content is copy pasted by individuals

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/1386075f-8b30-46b1-a358-c0e88c16e0cb)

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/503b451c-1aef-4fb0-ae85-da60df776cf6)

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/93a26c4a-a7fb-4cfa-9f94-0b616a98bf6e)

![image](https://github.com/mksesetti/Uncovering-Coordinated-Twitter-Campaigns-Using-Spark-and-AWS/assets/172356704/aa1ddca7-e2f8-4307-ba83-cad53322c381)

Overall, this data visualization and analysis allowed the identification of key patterns and insights into user engagement and sentiment in these two twitter trends. 

# Conclusion and Future Work 
# Conclusion 
In conclusion, this project on uncovering coordinated campaigns by analyzing artificial inflation of trend popularity on Twitter has successfully addressed the research questions and achieved its objectives. Through the analysis of a dataset of over 300,000 tweets, the presence of coordinated campaigns that artificially inflated trend popularity on Twitter were identified. Several interesting trends and patterns in the data, such as the high percentage of likes compared to retweets and the significant number of duplicate tweets were also observed. 
Despite the limitations and technical difficulties faced during the project, an end-to-end data engineering and data visualization pipelines were developed using AWS technologies such as S3, EMR, Redshift, and QuickSight. The findings of this project can help researchers, social media platforms, and policymakers to understand and combat the problem of coordinated campaigns on social media platforms.  
# Future Work 
As mentioned, due to pricing restrictions, all the data for some hashtags could not be gathered. In the future, cost-effective solutions can be explored to collect a larger volume of data for more accurate analysis. Currently, batch data processing for our analysis was used. In the future, the feasibility of real-time streaming data processing to enable more timely analysis and insights can be explored.Advanced machine learning and deep learning techniques to enhance our analysis, including sentiment analysis, network analysis, and anomaly detection can also be explored. This can help in identifying more nuanced patterns and potentially detect coordinated campaigns with higher accuracy. Additionally, exploring the use of graph databases to analyze the connections between users and detect potential coordinated campaigns or networks of bots can also be done. Overall, this project has provided valuable insights into the problem of coordinated campaigns on Twitter and has demonstrated the potential of data engineering and data visualization techniques for social media analysis
