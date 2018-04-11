# Fast Food and Twitter
## Mapping Consumer Sentiment to Better Understand and Target Customers

NOTE: This README is a work in progress 

The intersection of fast food consumers and Twitter users is wider than you may think. Contrary to conventional wisdom, the vast majority of fast food consumers are middle class- in fact, even the most affluent are only [marginally less likely](https://www.cnn.com/2017/07/12/health/poor-americans-fast-food-partner/index.html) (80% vs. 73%) than the poorest to eat fast food on a regular basis. Fast food consumption also trends young, peaking in the 18-29 age group. Likewise, *Twitter* trends young and affluent. Of its ~69 million regular users in the United States, [36% are in the 18-29 age group](https://blog.hootsuite.com/twitter-statistics/)(its largest demographic) and these users are more likely than not to have some college education. Equally, if not more important, 74% of Twitter users get at least some of their news on Twitter (more than the 68% of *Facebook* users) and a similar number follow brands for product updates and promotions. 

With numbers like these, building and maintaining a strong brand presence on Twitter is vital to stay competitve. In that light, I collected 170,000 customer (and ~10,000 company) tweets over three weeks (March 9-30) to evaluate brand favorability and consumer behavior for 11 fast food chains (targeting tweets that used the companies handle, ex.'@McDonalds'). With scripts collecting data four times per day on an Amazon Web Service EC2, I sought to use customer profiles and behavior to predict favorability toward a brand, identified the best times and days for marketing, and examined the interplay of favorability and stock price (targeting the following independent, publicly traded companies for that reason: *McDonald's, Chipotle, Starbucks, Denny's, Wendy's, Dunkin Donuts, Domino's, Sonic, Shake Shack, Wingstop, Cracker Barrel*). 

## #Data & Limitations 

This project presented a number of challenges- some surmountable, others not. First, Twitter's API limits the number of tweets you can capture per day (without paying), making a comprehensive analysis impossible and introducing a time and seasonal bias in the data collection (only 3 weeks of data and possible artificial concentrations at certain times of day). Second, there is no way to ensure that tweets are from genuine customers/consumers and not automated accounts ([some estimates suggest 15% of Twitter 'users' are actually bots](https://www.cnbc.com/2017/03/10/nearly-48-million-twitter-accounts-could-be-bots-says-study.html)). Third, using natural langauge processing to assess consumer preference on a large scale is an error prone process (I tested prebuilt NLTK & textblob sentiment analyzers)- manually assessing a randomized subset of tweets, I identified a misclassication rate of ~20%. 

Facing these challenges, I tested various solutions (detailed throughout this README) and do my best to be fully transparent where results might be biased. For example, of the 170,000+ tweets collected, I had to eliminate nearly 80,000 due to missing or incomplete data. Likewise, I  dropped two companies from the dataset (*Potbelly & Red Robin*) because there were insuffucient observations. 

## #Brand Positivity

I measured brand positivity using the mean [TextBlob](https://textblob.readthedocs.io/en/dev/quickstart.html#sentiment-analysis) sentiment (also often referred to as *polarity*) score (a floating number between -1 (totally negative) & 1 (totally positive) of each tweet.  TextBlob's polarity score is robust to negation (ex. 'not great', is scored ~-1, 'great' ~1 ), can interpret emoticon sentiment :), and is built off of the academic work of the [respected *pattern* library](https://www.clips.uantwerpen.be/pages/pattern-web). Overall, 44% of tweets were positive, 15% were negative, and 43% were neutral.

The graphic below shows brand positivity, company by company, as a measure of the polarity of customer mentions:

<p align="left">
  <img src="https://github.com/slevin886/twitter_fast_food_analysis/blob/master/images/Mean_Positivity.png" height="460" width="500">
</p>

In addition to showing substantial variation in brand performance (all, on average, more positive than negative), the results are reassuring of the validity of the metric based on prior knowledge of brand popularity. The top performer, Shake Shack, is renown for its carefully crafted branding and the (generally positive) hype surrounding [its burgers and market expansion](https://www.investors.com/research/the-new-america/how-shake-shacks-growth-may-catch-up-to-its-hype/). In contrast, the worst performer, Domino's, has had its brand repeatedly battered by scandal and, despite what the [*Harvard Business Review* suggests](https://hbr.org/2016/11/how-dominos-pizza-reinvented-itself), has not completely rebounded. 

However, including a measure (the orange bars below) of only unique tweets- eliminating retweets and multiple tweets by the same user- shows a clear decline in overall sentiment and a reordering of the companies' comparative scores. 

<p align="left">
  <img src="https://github.com/slevin886/twitter_fast_food_analysis/blob/master/images/polarity.png" height="420" width="560">
</p>

Starbucks and Shake Shack both see 6% declines in overall favorability, while Wendy's favorability increases by 2%. The mechanism for the decrease is not only through fewer positive tweets, but also a proportional increase in negative tweets. That the change in score varies widely across companies can lend itself to various interpretations. It could be that positive tweets are simply more likely to be retweeted and/or that the cross-company variation is purely random (NOTE: these are NOT retweets of the company's tweets- I purposely did not include those in this analysis).  On the other hand, it could indicate that some companies are purchasing more retweets of positive customer tweets- [a common strategy](http://www.businessinsider.com/fake-twitter-followers-test-2012-8). 

## #Positivity & Popularity

Another measure of brand favorability is the 'favorite count' (the equivalent of 'liking' a post on *facebook*). In the sample, favorite count is heavilly skewed (84% of tweets have 0 favorites, while the top tweet has 857). Below you can see the mean sentiment score contrasted with the mean favorite count. 

<p align="left">
  <img src="https://github.com/slevin886/twitter_fast_food_analysis/blob/master/images/company_pops.png" height="420" width="560">
</p>

As you can see above, there is no obvious visual (or quantitative for that matter) correlation between mean favorite count and mean sentiment. This runs against the idea of [hypercriticism](http://bigthink.com/ideafeed/is-negative-content-more-likely-to-go-viral) (the idea that negative statements tend to be more intelligent, and by extension, more popular) and instead suggests that at least in this sample neither positivity nor negativity drive the popularity of a tweet. 

This is in marked contrast to the favorite count of companys' own tweets, which have a strong positive correlation with their overall brand favorability  (0.31 pearson correlation). This is not a contradiction. It makes intuitive sense that having more customers willing to favorite company tweets (inevitably positive or neutral in sentiment) would indicate more customers willing to tweet their own positive reaction to the brand. 

## #Marketing by Day & Time

Over the three weeks of data collected, Tuesday narrowly edged out Thursday for the highest overall volume of tweets (with nearly 3x Sunday's tweet volume- the slowest day). Tuesday also had the highest overall positive sentiment, while Thursday slipped to 4th behind Monday and Wednesday. Undoubtedly, running ads when Twitter is heavilly trafficked with positive consumers is an advantageous strategy- particularly regarding promotions or new products. 

Below, you can see the sentiment of customer tweets by company across each day of the week (the width of the line being a measure of the mean favorite count by day):

<p align="left">
  <img src="https://github.com/slevin886/twitter_fast_food_analysis/blob/master/images/weekdaysentiment.png" height="420" width="560">
</p>

The peak in Tweet positivity on Tuesday is unmistakable, although there is substantial cross-company variation (Cracker Barrel peaks Thursday, Domino's Wednesday). Likewise, Saturday and Sunday clearly trail the weekdays in positive sentiment. This is not to say these days shouldn't be a focus on online engagement. Targeting your own dissatisfied customers through direct messages or other company's dissatisfied customers through a campaign can both be winning strategies.    

The graphic below shows the average daily tweet volume by company. In contrast to the variation in daily sentiment across companies, there is higher covariance in tweet volume. However, it can't be ruled out that this isn't at least somewhat biased by the artificial limits Twitter put on the amount of Tweets I could scrape. 

<p align="left">
  <img src="https://github.com/slevin886/twitter_fast_food_analysis/blob/master/images/mentions_by_day.png" height="365" width="630">
</p>

Identifying the optimal time of day to target customers was trickier. At any given collection time, Twitter returns a 'randomized' subset of a given query- not necessarily uniformly distributed across time. Likewise, I automated the script to collect customer tweets at 5pm and 12am EST daily, not every hour. To solve for this problem, I looked at the negativity *rate* (i.e. the number of negative tweets as a percentage of all tweets at a given hour). 

<p align="left">
  <img src="https://github.com/slevin886/twitter_fast_food_analysis/blob/master/images/negrate2.png" height="460" width="1000">
</p>

From the graphic above, 4pm and 9am jump out as times with a high volume of tweets and a low negativity rate- ideal for targeting large numbers of satisfied customers. On the other hand, and unsurprisingly, negativity tends to peak overnight with fewer customers tweeting.

#Company Tweet Characteristics & Customer Reuse

<p align="left">
  <img src="https://github.com/slevin886/twitter_fast_food_analysis/blob/master/images/comp_tweet_behav.png" height="500" width="640">
</p>

#Company Behavior on Twitter

<p align="left">
  <img src="https://github.com/slevin886/twitter_fast_food_analysis/blob/master/images/comp_behav.png" height="320" width="560">
</p>

#Consumer Behavioral Profiles

<p align="left">
  <img src="https://github.com/slevin886/twitter_fast_food_analysis/blob/master/images/consumer_behavior.png" height="400" width="950">
</p>

#Sentiment & Stock Price

<p align="left">
  <img src="https://github.com/slevin886/twitter_fast_food_analysis/blob/master/images/stock_sentiment_2.png" height="420" width="740">
</p>

#Vocabulary & Sentiment

<p align="left">
  <img src="https://github.com/slevin886/twitter_fast_food_analysis/blob/master/images/sentiment_vocab.png" height="460" width="700">
</p>
