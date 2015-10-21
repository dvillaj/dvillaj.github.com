---
layout:     post
title:      Importing data from Twitter with python and tweepy
date:       2015-10-21 09:02:20
summary:    How to import data from twitter using python and Tweepy
categories: Twitter Python
permalink:  /Import_Data_From_Twitter_Python/
minutes:    10
---

![png](/images/import-data-from-twitter/tweepy.png)


[Twitter](https://twitter.com) provides an API that lets you download data from this social network. To do this we will use python and the library [tweepy](https://github.com/tweepy/tweepy).

The aim is to retrieve tweets related with the word 'NoSQL' and store them in a file for later analysis.

The first thing to do is [register](https://www.google.es/webhp?q=create+twitter+app) a new Twitter application via the [Twitter Application Management](https://apps.twitter.com/) page.

After registering our application, Twitter will give us the keys that we need to access it using its API


{% highlight python %}
consumer_key = 'hRNtRgjzGq4wq3mt3fbuUkQ2c'
consumer_secret = 'yBbXnvNRpm93wvblpG9xhUMFF7w9sgxLfQT8k15Fs3k1RN4pnQ'
access_token_key = '12391902-qHOsgUBIvKuv7DjajXBmdm3SyZH8vgmR3jcpLVnnM'
access_token_secret = '9ViwfNW5FhOLhagaf4qfmDLXfY6qDtGzJ1MmAQM0gN3LK'
{% endhighlight %}

The next step is to import the library and login in twitter.


{% highlight python %}
import tweepy

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token_key, access_token_secret)

api = tweepy.API(auth)
{% endhighlight %}

## Testing the library

To test the library we will retrieve information about a user of twitter, his name, his followers, and so on ..


{% highlight python %}
user = api.get_user('NoSQLDigest')
{% endhighlight %}



{% highlight python %}
print "Name:", user.screen_name
print "Description:", user.description
print "Followers count:", user.followers_count
print "Friends' count:", user.friends_count
print "Statues Count [Number of Tweets]: ", user.statuses_count

{% endhighlight %}

    Name: NoSQLDigest
    Description: NoSQL Digest of tweets.
    Followers count: 9785
    Friends' count: 12
    Statues Count [Number of Tweets]:  668120


## Retrieving Tweets via a search term


{% highlight python %}
lookup ='NoSQL'
{% endhighlight %}

Using the following method we can download tweets quickly, but it depends on the limit set by Twitter


{% highlight python %}
max_tweets = 200
search_results = api.search(q=lookup, lang = 'en', count=max_tweets)

print len(search_results)

{% endhighlight %}

    100


{% highlight python %}
from prettytable import PrettyTable

table = PrettyTable(["User", "Fecha", "Texto"])
table.align["User"] = "l"
table.align["Texto"] = "l"

for tweet in search_results[0:10]:
    table.add_row([tweet.user.screen_name, tweet.created_at, tweet.text[:80]])
 
print table
{% endhighlight %}

|User|Fecha|Texto|
--------------------
|retweetjava | 2015-10-21 04:00:19 | RT @MusicHackFest: #GoGettersNetwork #CodeTalk  Apache Hadoop and NoSQL as Analy |
|NeuvooPhpCA | 2015-10-21 04:00:11 | CyberCoders is hiring a #Senior #Backend Developer - Ruby, Python, PHP, Agile, S |
|GujaratiGuy789  | 2015-10-21 03:57:05 | RT @SoftwareJokes: 3 database admins walked into a NoSQL bar. A little while lat |
|astosyk     | 2015-10-21 03:53:11 | RT @couchbase: Get rapid ramp-up on #NoSQL application development with lessons  |
|tjmickol    | 2015-10-21 03:40:36 | Listening to Sweet Home Wundermude by Spill &amp; Freunde on #kexp #galvanize #z |
|MooglieTwitimon | 2015-10-21 03:38:26 | Couchbase CEO on rise of NoSQL - https://t.co/9Zeif7Afw4 https://t.co/yglu7h3WA0 |
|MooglieTwitimon | 2015-10-21 03:38:24 | Couchbase CEO on rise of NoSQL - https://t.co/9Zeif7Afw4 https://t.co/yglu7h3WA0 |
|bph         | 2015-10-21 03:19:39 | RT @couchbase: The 3 mega-trends that define businesses leading the digital econ |
|VVagias     | 2015-10-21 03:13:36 | RT @geneolot: It's jut great Tuesday evening,  Bambini Dahlonega,  Georgia #BigD |
|retweetjava | 2015-10-21 03:13:35 | RT @geneolot: It's jut great Tuesday evening,  Bambini Dahlonega,  Georgia #BigD |


## Retrieving a user's timeline



{% highlight python %}
timeline_results = api.user_timeline(screen_name = 'NoSQLDigest', count = 1000, include_rts = True)
len(timeline_results)
{% endhighlight %}

    198



## Retrieving Tweets via a search term using a cursor

This method uses a cursor that skips the restriction of 200 tweets ;-)


{% highlight python %}
c = tweepy.Cursor(api.search, q= lookup).items()
    
search_results = []
while True:
    try:
        tweet = c.next()
        # Insert into db
        search_results.append(tweet)
    except tweepy.TweepError as e:
        print e
        break

print len(search_results)
{% endhighlight %}

    {"errors":[{"message":"Rate limit exceeded","code":88}]}
    2648


This fails too!. In this case is because of a timeout limit :-(.

## Retrieving a user's timeline using a cursor


{% highlight python %}
import sys

c = tweepy.Cursor(api.user_timeline,id='NoSQLDigest').items()    
timeline_results = []
while True:
    try:
        tweet = c.next()
        # Insert into db
        timeline_results.append(tweet)
    except:
        print "Error: ", sys.exc_info()[0]
        break

print len(timeline_results)
{% endhighlight %}

    Error:  <type 'exceptions.StopIteration'>
    3136


## Visualizing the content of a Tweet

Twitter returns data in [JSON](http://www.json.org/json-es.html) format. Let's see what a tweet looks like:


{% highlight python %}
import pprintpp

tweet = search_results[0]
pprintpp.pprint(tweet._json)
{% endhighlight %}

    {
        u'contributors': None,
        u'coordinates': None,
        u'created_at': u'Wed Oct 21 04:00:19 +0000 2015',
        u'entities': {
            u'hashtags': [
                {u'indices': [19, 36], u'text': u'GoGettersNetwork'},
                {u'indices': [37, 46], u'text': u'CodeTalk'},
                {u'indices': [131, 135], u'text': u'IoT'},
                {u'indices': [136, 140], u'text': u'Java'},
                {u'indices': [139, 140], u'text': u'API'},
                {u'indices': [139, 140], u'text': u'Linux'},
            ],
            u'symbols': [],
            u'urls': [
                {
                    u'display_url': u'bit.ly/1FAQ7B5',
                    u'expanded_url': u'http://bit.ly/1FAQ7B5',
                    u'indices': [107, 130],
                    u'url': u'https://t.co/z2lcnaHeov',
                },
            ],
            u'user_mentions': [
                {
                    u'id': 3243621325,
                    u'id_str': u'3243621325',
                    u'indices': [3, 17],
                    u'name': u'Hackathon News',
                    u'screen_name': u'MusicHackFest',
                },
            ],
        },
        u'favorite_count': 0,
        u'favorited': False,
        u'geo': None,
        u'id': 656681395348217856,
        u'id_str': u'656681395348217856',
        u'in_reply_to_screen_name': None,
        u'in_reply_to_status_id': None,
        u'in_reply_to_status_id_str': None,
        u'in_reply_to_user_id': None,
        u'in_reply_to_user_id_str': None,
        u'is_quote_status': False,
        u'lang': u'en',
        u'metadata': {u'iso_language_code': u'en', u'result_type': u'recent'},
        u'place': None,
        u'possibly_sensitive': False,
        u'retweet_count': 1,
        u'retweeted': False,
        u'retweeted_status': {
            u'contributors': None,
            u'coordinates': None,
            u'created_at': u'Tue Oct 20 22:37:58 +0000 2015',
            u'entities': {
                u'hashtags': [
                    {u'indices': [0, 17], u'text': u'GoGettersNetwork'},
                    {u'indices': [18, 27], u'text': u'CodeTalk'},
                    {u'indices': [112, 116], u'text': u'IoT'},
                    {u'indices': [117, 122], u'text': u'Java'},
                    {u'indices': [123, 127], u'text': u'API'},
                    {u'indices': [128, 134], u'text': u'Linux'},
                ],
                u'symbols': [],
                u'urls': [
                    {
                        u'display_url': u'bit.ly/1FAQ7B5',
                        u'expanded_url': u'http://bit.ly/1FAQ7B5',
                        u'indices': [88, 111],
                        u'url': u'https://t.co/z2lcnaHeov',
                    },
                ],
                u'user_mentions': [],
            },
            u'favorite_count': 0,
            u'favorited': False,
            u'geo': None,
            u'id': 656600272781877248,
            u'id_str': u'656600272781877248',
            u'in_reply_to_screen_name': None,
            u'in_reply_to_status_id': None,
            u'in_reply_to_status_id_str': None,
            u'in_reply_to_user_id': None,
            u'in_reply_to_user_id_str': None,
            u'is_quote_status': False,
            u'lang': u'en',
            u'metadata': {u'iso_language_code': u'en', u'result_type': u'recent'},
            u'place': None,
            u'possibly_sensitive': False,
            u'retweet_count': 1,
            u'retweeted': False,
            u'source': u'<a href="http://ifttt.com" rel="nofollow">IFTTT</a>',
            u'text': u'#GoGettersNetwork #CodeTalk  Apache Hadoop and NoSQL as Analysis Engines for IoT Data ▸ https://t.co/z2lcnaHeov #IoT #Java #API #Linux …',
            u'truncated': False,
            u'user': {
                u'contributors_enabled': False,
                u'created_at': u'Fri Jun 12 20:02:51 +0000 2015',
                u'default_profile': False,
                u'default_profile_image': False,
                u'description': u'Music + Technology + Hackathons = @MusicHackFest',
                u'entities': {u'description': {u'urls': []}},
                u'favourites_count': 105,
                u'follow_request_sent': False,
                u'followers_count': 2143,
                u'following': False,
                u'friends_count': 1388,
                u'geo_enabled': False,
                u'has_extended_profile': False,
                u'id': 3243621325,
                u'id_str': u'3243621325',
                u'is_translation_enabled': False,
                u'is_translator': False,
                u'lang': u'en',
                u'listed_count': 2879,
                u'location': u'Los Angeles, CA',
                u'name': u'Hackathon News',
                u'notifications': False,
                u'profile_background_color': u'000000',
                u'profile_background_image_url': u'http://abs.twimg.com/images/themes/theme1/bg.png',
                u'profile_background_image_url_https': u'https://abs.twimg.com/images/themes/theme1/bg.png',
                u'profile_background_tile': False,
                u'profile_banner_url': u'https://pbs.twimg.com/profile_banners/3243621325/1434140299',
                u'profile_image_url': u'http://pbs.twimg.com/profile_images/609454174841876481/GMtjuot9_normal.jpg',
                u'profile_image_url_https': u'https://pbs.twimg.com/profile_images/609454174841876481/GMtjuot9_normal.jpg',
                u'profile_link_color': u'ABB8C2',
                u'profile_sidebar_border_color': u'000000',
                u'profile_sidebar_fill_color': u'000000',
                u'profile_text_color': u'000000',
                u'profile_use_background_image': False,
                u'protected': False,
                u'screen_name': u'MusicHackFest',
                u'statuses_count': 301851,
                u'time_zone': u'Pacific Time (US & Canada)',
                u'url': None,
                u'utc_offset': -25200,
                u'verified': False,
            },
        },
        u'source': u'<a href="http://not.yet/" rel="nofollow">final one kk</a>',
        u'text': u'RT @MusicHackFest: #GoGettersNetwork #CodeTalk  Apache Hadoop and NoSQL as Analysis Engines for IoT Data ▸ https://t.co/z2lcnaHeov #IoT #Ja…',
        u'truncated': False,
        u'user': {
            u'contributors_enabled': False,
            u'created_at': u'Thu Dec 25 02:07:45 +0000 2014',
            u'default_profile': False,
            u'default_profile_image': False,
            u'description': u"Hey, I retweet #Java related tweets. Follow us and maybe you'll learn something new! Questions/concerns? Contact @jdf221 or @Jordanb844",
            u'entities': {u'description': {u'urls': []}},
            u'favourites_count': 62458,
            u'follow_request_sent': False,
            u'followers_count': 2819,
            u'following': False,
            u'friends_count': 19,
            u'geo_enabled': False,
            u'has_extended_profile': False,
            u'id': 2942356560,
            u'id_str': u'2942356560',
            u'is_translation_enabled': False,
            u'is_translator': False,
            u'lang': u'en',
            u'listed_count': 2696,
            u'location': u'',
            u'name': u'Java',
            u'notifications': False,
            u'profile_background_color': u'022330',
            u'profile_background_image_url': u'http://abs.twimg.com/images/themes/theme15/bg.png',
            u'profile_background_image_url_https': u'https://abs.twimg.com/images/themes/theme15/bg.png',
            u'profile_background_tile': False,
            u'profile_image_url': u'http://pbs.twimg.com/profile_images/547942714675712000/Kr9dDPXJ_normal.png',
            u'profile_image_url_https': u'https://pbs.twimg.com/profile_images/547942714675712000/Kr9dDPXJ_normal.png',
            u'profile_link_color': u'0084B4',
            u'profile_sidebar_border_color': u'A8C7F7',
            u'profile_sidebar_fill_color': u'C0DFEC',
            u'profile_text_color': u'333333',
            u'profile_use_background_image': True,
            u'protected': False,
            u'screen_name': u'retweetjava',
            u'statuses_count': 164272,
            u'time_zone': None,
            u'url': None,
            u'utc_offset': None,
            u'verified': False,
        },
    }


## Parsing the result

The aim of the following functions to simplify the information we keep on file because Twitter provide too much information


{% highlight python %}
def parse_user(usr):
    user = {}  
    user["created_at"] = usr['created_at']
    user["description"] = usr['description']
    user["favourites_count"] = usr['favourites_count']
    user["followers_count"] = usr['followers_count']
    user["friends_count"] = usr['friends_count']
    user["geo_enabled"] = usr['geo_enabled']
    user["_id"] = usr['id']
    user["id_str"] =usr['id_str']
    user["name"] = usr['name']
    user["screen_name"] = usr['screen_name']
    user["statuses_count"] = usr['statuses_count']
    user["profile_image_url"] = usr['profile_image_url']
    if usr['time_zone'] <> None:
        user["time_zone"] = usr['time_zone']
    
    return user
{% endhighlight %}


{% highlight python %}
def parse_tweet(t):
    tweet = {}
    tweet['created_at'] = t['created_at']
    #for ht in tweet.entities.hashtags:
    #    print ht.text

    tweet['entities'] = []
    for k in t['entities']['hashtags']:
        tweet['entities'].append(k['text'])
  
    tweet['user_mentions'] = []
    for k in t['entities']['user_mentions']:
        k.pop("indices", None)
        tweet['user_mentions'].append(k)

    tweet['favorite_count'] =  t['favorite_count']

    if t['geo'] <> None:
        tweet['geo'] = t['geo']

    tweet['_id'] = t['id']
    tweet['id_str'] = t['id_str']  

    tweet['lang'] = t['lang']
    tweet['retweet_count'] = t['retweet_count']
    tweet['source'] = t['source']
    tweet['text'] = t['text']
    tweet['user'] = parse_user(t['user'])

    if 'retweeted_status' in t.keys():
        tweet['retweeted_status'] = parse_tweet(t['retweeted_status'])
        
    return tweet
{% endhighlight %}

We parse the content we've previously downloaded ...


{% highlight python %}
tweets = []

for tweet in search_results:
    tweets.append(parse_tweet(tweet._json))
    
{% endhighlight %}

Now the content also in JSON format but much simpler


{% highlight python %}
pprintpp.pprint(tweets[0])
{% endhighlight %}

    {
        '_id': 656681395348217856,
        'created_at': u'Wed Oct 21 04:00:19 +0000 2015',
        'entities': [
            u'GoGettersNetwork',
            u'CodeTalk',
            u'IoT',
            u'Java',
            u'API',
            u'Linux',
        ],
        'favorite_count': 0,
        'id_str': u'656681395348217856',
        'lang': u'en',
        'retweet_count': 1,
        'retweeted_status': {
            '_id': 656600272781877248,
            'created_at': u'Tue Oct 20 22:37:58 +0000 2015',
            'entities': [
                u'GoGettersNetwork',
                u'CodeTalk',
                u'IoT',
                u'Java',
                u'API',
                u'Linux',
            ],
            'favorite_count': 0,
            'id_str': u'656600272781877248',
            'lang': u'en',
            'retweet_count': 1,
            'source': u'<a href="http://ifttt.com" rel="nofollow">IFTTT</a>',
            'text': u'#GoGettersNetwork #CodeTalk  Apache Hadoop and NoSQL as Analysis Engines for IoT Data ▸ https://t.co/z2lcnaHeov #IoT #Java #API #Linux …',
            'user': {
                '_id': 3243621325,
                'created_at': u'Fri Jun 12 20:02:51 +0000 2015',
                'description': u'Music + Technology + Hackathons = @MusicHackFest',
                'favourites_count': 105,
                'followers_count': 2143,
                'friends_count': 1388,
                'geo_enabled': False,
                'id_str': u'3243621325',
                'name': u'Hackathon News',
                'profile_image_url': u'http://pbs.twimg.com/profile_images/609454174841876481/GMtjuot9_normal.jpg',
                'screen_name': u'MusicHackFest',
                'statuses_count': 301851,
                'time_zone': u'Pacific Time (US & Canada)',
            },
            'user_mentions': [],
        },
        'source': u'<a href="http://not.yet/" rel="nofollow">final one kk</a>',
        'text': u'RT @MusicHackFest: #GoGettersNetwork #CodeTalk  Apache Hadoop and NoSQL as Analysis Engines for IoT Data ▸ https://t.co/z2lcnaHeov #IoT #Ja…',
        'user': {
            '_id': 2942356560,
            'created_at': u'Thu Dec 25 02:07:45 +0000 2014',
            'description': u"Hey, I retweet #Java related tweets. Follow us and maybe you'll learn something new! Questions/concerns? Contact @jdf221 or @Jordanb844",
            'favourites_count': 62458,
            'followers_count': 2819,
            'friends_count': 19,
            'geo_enabled': False,
            'id_str': u'2942356560',
            'name': u'Java',
            'profile_image_url': u'http://pbs.twimg.com/profile_images/547942714675712000/Kr9dDPXJ_normal.png',
            'screen_name': u'retweetjava',
            'statuses_count': 164272,
        },
        'user_mentions': [
            {
                u'id': 3243621325,
                u'id_str': u'3243621325',
                u'name': u'Hackathon News',
                u'screen_name': u'MusicHackFest',
            },
        ],
    }


## Saving the data in a file

Finally we will record the data downloaded to a file, so that we can analyze later


{% highlight python %}
import json
    
with open('./data/tweets.json',"w") as file:
    for t in tweets:
        r = json.dumps(t)
        file.write(r)
        file.write("\n")
{% endhighlight %}


{% highlight python %}
print "Number of tweets ... ", len(tweets)
{% endhighlight %}

    Number of tweets ...  3136



{% highlight python %}
tweets = []
for tweet in timeline_results:
    tweets.append(parse_tweet(tweet._json))
    
with open('./data/timeline.json',"w") as file:
    for t in tweets:
        r = json.dumps(t)
        file.write(r)
        file.write("\n")
        
print "Number of tweets ... ", len(tweets)
{% endhighlight %}

    Number of tweets ...  3136

Hope this helps!
