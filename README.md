# Twitter Topic Analysis
### System Information
<p align="center">
  <img width="400" height="140" src="/system_information_HP.png">
  <p align="center">
  Python  : 3.7.3
  </p>
  <p align="center">
  Jupyter : 5.7.8
  </p>
  <p align="center">
  Spark   : 2.4.3
  </p>
</p>

### 1. Mining Tweets Data
In this case, I will use "bawaslu" as a keyword to doing the analysis. Bawaslu stands for Badan Pengawas Pemilu (General Election Supervisory Agency in Indonesia). 

The analysis using data for 14 days (May 14, 2019 - May 27, 2019).

Tweets data collected using Jupyter Notebook with Python 3 ([tweepy](https://github.com/tweepy/tweepy)) named [Mining Tweets Data - Tweepy.ipynb](https://github.com/fchrulk/social-twitter/blob/master/Mining%20Tweets%20Data%20-%20Tweepy.ipynb)

#### How to use tweepy-twitter-search-keywords.ipynb:
1) Prepare your authentication. Follow the instructions from this [link](https://www.digitalocean.com/community/tutorials/how-to-authenticate-a-python-application-with-twitter-using-tweepy-on-ubuntu-14-04) to find out more.
2) Save your credential into the yaml file named [auth_twitter.yaml](https://github.com/fchrulk/social-twitter/blob/master/auth_twitter.yaml)
3) Please see this [video](https://youtu.be/ZGSpgLttT4Q) to find out more about how I use tweepy-twitter-search-keywords.ipynb to mining tweets data.
4) The result will be saved in json format like [this](https://drive.google.com/open?id=10XqnRtvFNc4CqRjnHvSMGZHHdKaF3N5Z). You can download the data that I use [here](https://drive.google.com/open?id=1WfU4luEXz-IVoCPcIYZmEZUvsBgMvVuW).
5) Let's go to the next step *Pre-Processing Tweets Data* when the result is ready.

### 2. Pre-Processing Tweets Data
In this step I will parsing the search result using [Pyspark](https://spark.apache.org/docs/latest/api/python/index.html) or [Pandas](https://pandas.pydata.org/) (you just need choose one approach).
I named the scripts for parsing as follows:
1. [PreProcessing Tweets Data - Pyspark.ipynb](https://github.com/fchrulk/twitter-topic-analysis/blob/master/PreProcessing%20Tweets%20Data%20-%20Pyspark.ipynb)
2. [PreProcessing Tweets Data - Pandas.ipynb](https://github.com/fchrulk/twitter-topic-analysis/blob/master/PreProcessing%20Tweets%20Data%20-%20Pandas.ipynb)

As we know that result output from *Mining Tweets Data* saved in json format, I only take the required fields for my analysis then save it into csv format. I think we will easily make some visualizations or analysis on tabular data.

I will divide the json output into two parts, namely: *Collection of Tweets* and *Collection of Users*
#### A. Collection of Tweets
| Column Name | Description | JSON Fields |
| --- | --- | --- |
| `created_at` | GMT+7 time when the Tweet was created | `created_at_local` |
| `tweet_id` | ID of the tweet | `id_str` |
| `tweet_type` | Type of the tweet. There are four types: tweet, reply, quote, retweet | Use conditions in my opinion |
| `user_id` | ID of the user who created tweet | `user` `id_str` |
| `user_screenname` | Screen name or username of the user who created tweet | `user` `screen_name` |
| `user_followings_count` | Number of followings of the user who created tweet | `user` `friends_count` |
| `user_followers_count` | Number of folowers of the user who created tweet | `user` `followers_count` |
| `user_tweets_count` | Number of folowers of the user who created tweet | `user` `statuses_count` |
| `user_location` | Location of the user who created tweet (Based on his profile) | `user` `location` |
| `is_verified` | Whether the user who created tweet is verified user or not | `user` `verified` |
| `tweet_text` | Text of the tweet | `full_text` |
| `favorite_count` | Number of favorites obtained by related tweet | `favorite_count` |
| `retweet_count` | Number of retweets obtained by related tweet | `retweet_count` |
| `place_fullname` | Full place name that has been tagged in related tweet | `place` `full_name` |
| `place_name` | Name of the place that has been tagged in related tweet (Usually the name of the city) | `place` `name` |
| `place_type` | Type of the place name that has been tagged in related tweet | `place` `place_type` |
| `place_country` | Country of the place that has been tagged in related tweet | `place` `country` |
| `quoted_from_tweet_id` | If `tweet_type` is *quote*, this column contains ID of the quoted tweet | `quoted_status_id_str` |
| `quoted_from_user_id` | If `tweet_type` is *quote*, this column contains ID of the user who created the tweet that has been quoted by related user | `quoted_status` `user` `id_str` |
| `quoted_from_user_screenname` | If `tweet_type` is *quote*, this column contains screen name or username of the user who created the tweet that has been quoted by related user | `quoted_status` `user` `screen_name` |
| `reply_to_tweet_id` | If `tweet_type` is *reply*, this column contains ID of the replied tweet | `in_reply_to_status_id_str` |
| `reply_to_user_id` | If `tweet_type` is *reply*, this column contains ID of the user who created the tweet that has been replied by related user | `in_reply_to_user_id_str` |
| `reply_to_user_screenname` | If `tweet_type` is *reply*, this column contains screen name or username of the user who created the tweet that has been replied by related user | `in_reply_to_screen_name` |
| `retweeted_from_tweet_id` | If `tweet_type` is *retweet*, this column contains ID of the retweeeted tweet | `retweeted_status` `id_str` |
| `retweeted_from_user_id` | If `tweet_type` is *retweet*, this column contains ID of the user who created the tweet that has been retweeted by related user | `retweeted_status` `user` `id_str` |
| `retweeted_from_user_screenname` | If `tweet_type` is *retweet*, this column contains screen name or username of the user who created the tweet that has been retweeted by related user | `retweeted_status` `user` `screen_name` |
       
#### B. Collection of Users
| Column Name | Description | JSON Fields |
| --- | --- | --- |
| `user_id` | ID of the user who created tweet | `user` `id_str` |
| `user_screenname` | Screen name or username of the user who created tweet | `user` `screen_name` |
| `user_fullname` | Full name of the user who created tweet | `user` `name` |
| `user_created_at` | ID of the user who created tweet | `user` `id_str` |
| `user_followings_count` | Number of followings of the user who created tweet | `user` `friends_count` |
| `user_followers_count` | Number of folowers of the user who created tweet | `user` `followers_count` |
| `user_tweets_count` | Number of folowers of the user who created tweet | `user` `statuses_count` |
| `user_location` | Location of the user who created tweet (Based on his profile) | `user` `location` |
| `is_verified` | Whether the user who created tweet is verified user or not | `user` `verified` |

### 3. Exploratory Data Analysis
### 4. Simple Dashboard
### 5. Perform Stastical Analysis

