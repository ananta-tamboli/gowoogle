---
layout: post
title:  "Twitter Sentiment Analysis"
author: Ananta
date: "2021-04-07"
categories: [ python, mini project, tutorial]
image: "assets/images/mini projects.png"
comments: false
---
## YouTube link

Sentiment analysis refers to analyzing an opinion or feelings about something using data like text or images, regarding almost anything. Sentiment analysis helps companies in their decision-making process. For instance, if public sentiment towards a product is not so good, a company may try to modify the product or stop the production altogether in order to avoid any losses.

There are many sources of public sentiment e.g. public interviews, opinion polls, surveys, etc. However, with more and more people joining social media platforms, websites like Facebook and Twitter can be parsed for public sentiment.

In this article, we will see how we can perform sentiment analysis of text data ie. tweets obtained from twitter. This will is live project ie. we are going to fetch tweets in real time from Twitter and perform sentiment analysis on them using textblob. Output will be classified in three types `love tweets percentage:`, `hate tweets percentage:`, `neutral tweets percentage:` these tweets are not necessarily love or hate tweets it just means that tweets are positive, negative & neutral respectively.

### imports

```python
import re
import tweepy
from tweepy import OAuthHandler
from textblob import TextBlob
```

1. re : regular expressions for preprocessing of tweets
2. tweepy : Tweepy is an open source Python package that gives you a very convenient way to access the Twitter API with Python. Tweepy includes a set of classes and methods that represent Twitter's models and API endpoints, and it transparently handles various implementation details, such as: Data encoding and decoding. It is great for simple automation and creating twitter bots.
3. textblob : Textblob provides a simple API for diving into common natural language processing (NLP) tasks such as part-of-speech tagging, noun phrase extraction, sentiment analysis, classification, translation, and more.

### authentication

Here, we pass tokens and keys that we got from twitter developer console. This step is Non-GUI equivalent of Log in and instead of ID and Password we pass access tokens and keys.

[Click her for OAuth token generation process](https://developer.twitter.com/en/docs/authentication/oauth-1-0a)

```python
class TwitterClient(object):
    #Generic Twitter Class for sentiment analysis.
    def __init__(self):
        # keys and tokens from the Twitter Dev Console
        consumer_key = ''
        consumer_secret = ''
        access_token = ''
        access_token_secret = ''
 
        # attempt authentication
        try:
            # create OAuthHandler object
            self.auth = OAuthHandler(consumer_key, consumer_secret)
            # set access token and secret
            self.auth.set_access_token(access_token, access_token_secret)
            # create tweepy API object to fetch tweets
            self.api = tweepy.API(self.auth)
        except:
            print("Error: Authentication Failed")
```

### fetch tweets

If above step was executed successfully that means in a way we are logged in to our account. To fetch tweets from twitter we need to pass a query. Query can be changed in main function I am passing Elon Musk for our example.  

```python
    def get_tweets(self, query, count = 10):
        '''
        Main function to fetch tweets and parse them.
        '''
        # empty list to store parsed tweets
        tweets = []
 
        try:
            # call twitter api to fetch tweets
            fetched_tweets = self.api.search(q = query, count = count)
 
            # parsing tweets one by one
            for tweet in fetched_tweets:
                # empty dictionary to store required params of a tweet
                parsed_tweet = {}
 
                # saving text of tweet
                parsed_tweet['text'] = tweet.text
                # saving sentiment of tweet
                parsed_tweet['sentiment'] = self.get_tweet_sentiment(tweet.text)
 
                # appending parsed tweet to tweets list
                if tweet.retweet_count > 0:
                    # if tweet has retweets, ensure that it is appended only once
                    if parsed_tweet not in tweets:
                        tweets.append(parsed_tweet)
                else:
                    tweets.append(parsed_tweet)
 
            # return parsed tweets
            return tweets
 
        except tweepy.TweepError as e:
            # print error (if any)
            print("Error : " + str(e))
 ```

### pre-processing

After we get our raw data ie. tweets we need to clean them by that I mean removing any special characters and links from the tweets.

 ```python
    def clean_tweet(self, tweet):
        '''
        Utility function to clean tweet text by removing links, special characters
        using simple regex statements.
        '''
        return ' '.join(re.sub("(@[A-Za-z0-9]+)|([^0-9A-Za-z \t])|(\w+:\/\/\S+)", " ", tweet).split())
```

### sentiment analysis

Using textblob we will perform sentiment analysis task.
It will give output in three types of sentiment: Negative, Positive & Neutral.

```python
    def get_tweet_sentiment(self, tweet):
        '''
        Utility function to classify sentiment of passed tweet
        using textblob's sentiment method
        '''
        # create TextBlob object of passed tweet text
        analysis = TextBlob(self.clean_tweet(tweet))
        # set sentiment
        if analysis.sentiment.polarity > 0:
            return 'positive'
        elif analysis.sentiment.polarity == 0:
            return 'neutral'
        else:
            return 'negative'
```

### `main` function

In main function we are going to pass query ie. the string that we want to search on twitter and count is the number of tweets to download to perform pre-processing and analysis.

```python
def main():
    # creating object of TwitterClient Class
    api = TwitterClient()
    # calling function to get tweets
    tweets = api.get_tweets(query = 'Elon Musk', count = 200)
 
    # picking positive tweets from tweets
    ptweets = [tweet for tweet in tweets if tweet['sentiment'] == 'positive']
    # percentage of positive tweets
    print("Love tweets percentage: {} %".format(100*len(ptweets)/len(tweets)))
    # picking negative tweets from tweets
    ntweets = [tweet for tweet in tweets if tweet['sentiment'] == 'negative']
    # percentage of negative tweets
    print("Hate tweets percentage: {} %".format(100*len(ntweets)/len(tweets)))
    # percentage of neutral tweets
    print("Neutral tweets percentage: {} %".format(100*(len(tweets) - len(ntweets) - len(ptweets))/len(tweets)))
 
    # printing first 5 positive tweets
    print("\n\nPositive tweets:")
    for tweet in ptweets[:10]:
        print(tweet['text'])
 
    # printing first 5 negative tweets
    print("\n\nNegative tweets:")
    for tweet in ntweets[:10]:
        print(tweet['text'])
```

### Calling `main` function

To call main function.

```python
if __name__ == "__main__":
    # calling main function
    main()
```

<script src="https://gist.github.com/ananta-tamboli/3473968aeb38b64067319a13681ea7d2.js"></script>

### OUTPUT

![Alt](/assets/images/twitterop.png "Output")

[Github Repo](https://github.com/ananta-tamboli/Go-Woogle/tree/main/twitter%20sentiment%20analysis)
