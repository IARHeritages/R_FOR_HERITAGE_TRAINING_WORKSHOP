
# R FOR HERITAGE: TRAINING WORKSHOP
	
## 15 May 2019, University of Stirling

#### Sponsored by the Scottish Graduate School for Arts and Humanities
    
#### Authors: Marta Krzyzanska (marta.krzyzanska@stir.ac.uk) and Dr Chiara Bonacchi (chiara.bonacchi@stir.ac.uk)

#### Part 2: Data collection

In this section we will see how to extract tweets from Twitter's public API (Application Programming Interface). 

Please note that, through the procedures that we will show you, you will be accessing: 

1. Up to 3200 tweets from a public Twitter account (current *rtweet* documentation).
2. Tweets containing a specific # and published up to 7 days before the data collection starts (current Twitter developer documentation).
3. A small random sample of tweets containing a specific term amongst those that are published live during a specific time interval (current *rtweet* documentation).

These limits are constantly updated by social media companies, so make sure you check the relevant documentation each time.

## Creating an API access token

Before starting, define your working directory:


```R
setwd("/Users/yourusername/Documents/Rheritage")
```

To get data from Twitter, we will be using the *rtweet* library. In programming, libraries are collections of pre-defined routines that a program can use. In other words, they provide additional functions. In the case of Free and Open Source Software like R, these functions can be written and shared by anyone and published as packages. To use a library, you need to install it first: 


```R
install.packages("rtweet")
```

A new window should appear, prompting you to choose a cran mirror. Choose one of those located in the UK and double click on it. You may need to wait a moment before the package is installed. Then, load the library:


```R
library(rtweet)
```

To download data from Twitter we will need 'an access token', which is like a special key allowing us to access Twitter's public API. There is a function in the *rtweet* library that allows you to create an access token, called *create_token*. To check how it works type:


```R
?create_token
```

As you can see, the function requires 5 arguments: the name of the Twitter app (application) created by the user, the consumer key, the consumer secret, the access token and the access secret. To get all this information, you need to create a Twitter app in this way:

- Apply for a developer account going to this link: https://developer.twitter.com/en/apply/user.

- Log in to your Twitter account and make sure that you have provided your phone number as part of your profile information.

- Wait for an email confirming that your account has been approved.

- Go to https://apps.twitter.com/ and click on the *create New App* button.

- Under application details, enter app name (e.g. *token_1* or anything else that is not already taken), app description (e.g. *token for R*) and website (e.g. *http://example.com*). As Callback URL enter: *http://127.0.0.1:1410*; accept the Developer Agreement and click on the *Create* button.

- Click the *modify app permissions* button and under *Access* choose: *Read, Write and Access direct messages*; then click *Update Settings*

- Now go to the *Keys and Access Tokens* tab to find your consumer key and consumer secret and provide those and your app name as arguments in the function *create_token* (enter them as strings, in quotation marks!):


```R
token <- create_token(
  app = "app_name",
  consumer_key = "your_consumer_key",
  consumer_secret = "your_consumer_secret",
  access_token = "your_access_token",
  access_secret = "your_access_secret")
```

Now save your newly-created token as an R object file, so that you can re-use it later:


```R
save(token, file = "token.R")
```


```R
load("token.R")
```

## User timeline extraction

We are ready to access Twitter's API and extract tweets. There are various ways of extracting tweets. 

For example, we can extract all the tweets posted by a specific, public account. To do this, we will use the *get_timeline* function from the *rtweet* library. To find out what the function does and what information we need to feed into it, type:


```R
?get_timeline
```

This should open a browser window with documentation about the function. From this documentation, we learn that the function can return tweets posted either by a given user or by the accounts followed by that user. If we are interested in what a user has been tweeting, we will keep the default setting of the home argument to false. (Under *usage*, you can see what the default settings of a parameter is. If default is what you want, you do not have to include that parameter).

&nbsp;

The *Arguments* section tells you what arguments you need to define:

- Screen name of the user, for example "@MicroPasts" (it needs to be input as a string i.e. in quotation marks).
- The number of tweets to return (input as numeric value, so without quotation marks). 
- As token, we will use our newly created token stored in the variable with the same name.
- We can keep the other arguments as 'default', so we do not need to include them.

The function should return a data frame, which we will store in the variable tweets:


```R
tweets<-get_timeline(user="@MicroPasts", n=100,token=token)
```


```R
# Show the top of the data frame, displaying only the first 5 columns:
head(tweets[,c(1:5)])
```

## Search by hashtag

If, instead, we want to search for tweets containing specific terms or hashtags, we can use the *search_tweets()* function.   
For example, to search for tweets with **"#EdSciFest"**:


```R
tweets2 <- search_tweets("#EdSciFest", n = 100, token=token)
```


```R
# Show the top of the data frame, displaying only the first 5 columns:
head(tweets2[,c(1:5)])
```


```R
# Now have a look at all the names of the columns in your data frame
# This will give you an idea of the information you are extracting together with the text of tweets
colnames(tweets2)
```

## Data anonymisation

Immediately after collecting Twitter data, we proceed with anonymising it, for ethical reasons and to protect the human subjects involved. 

In particular, we will anonymise the identification numbers of tweets, and either drop or anonymise user names and user identification numbers. To demonstrate how this can be done, we will use the second dataset we extracted (tweets2). 

To anonymise *tweet ids*, substitute the original ids, with random numbers. First, find out how many random numbers are needed, by checking the length of the unique entries in the column containing the *status id*:


```R
n <- length(unique(tweets2$status_id))
n
```

R has a number of functions that can generate random numbers. To anonymise our data, we will use the one called *sample()*. As the first argument, this function takes the list of numbers from which to sample; as the second argument, it takes the number of samples needed (in this case equal to n); and as the third argument, it asks you to specify TRUE or FALSE depending on whether you want to allow the same number to be drawn more than once or not (in our case, we don't want to and we give FALSE as argument):


```R
# This generates a sample of n random numbers drawing from integers ranging from 1 to 10000
# It does so without replacement, so that the same number does not appear in the sample more than once
anonymised_id<-sample(1:10000, n, replace=FALSE)
# Now have a look at your anonymised ids
anonymised_id
```

Now get the list of unique ids from the original dataset:


```R
status_id <- unique(tweets2$status_id)
```

Make a data frame with the unique tweet ids in one column and the random numbers in the other and inspect it:


```R
randuser <- cbind(status_id,anonymised_id)
head(randuser)
```

Now we can merge this data frame with the original dataset, by matching up the anonymised ids with the corresponding ids of the tweets:


```R
rand.df <- merge(randuser, tweets2, by="status_id") 
# This function merges two data frames by a specific column, in this case status_id
```

Inspect the first few rows of the data frame, to see that there is a new column with anonymised tweet ids:


```R
head(rand.df[,c(1:5)])
```

Since we have the column with the anonymised ids, we can drop the one with the original ids. To drop the column, simply set the values of the column as NULL, and inspect the top of the data frame to see that the column has in fact been dropped:


```R
rand.df$status_id <- NULL # This gets rid of the status_id column
head(rand.df[,c(1:5)])
```

We also need to anonymise the columns containing user ids and screen names. To do this, you can either substitute these ids and names with random numbers, or simply drop the columns altogether:


```R
rand.df$user_id <- NULL # This gets rid of the status_id column
rand.df$screen_name <- NULL 
head(rand.df[,c(1:5)])
```

Finally, to fully anonymise our dataset, we need to get rid of the usernames contained in the text of the tweets. To do that, we can either substitute the handles with random numbers, or simply delete them altogether. 

To delete the handles from the text, you can use the *gsub()* function. As a first argument, this function takes the pattern we want to delete, which may be a specific string. For example, to delete @MicroPasts handles, we would use '@MicroPasts' as the first argument. To delete all the handles present in the text of the tweets, we will use a *regular expression*, which tells R to delete all the characters starting from and including '@', to the first whitespace, denoted as 'w+' (regular expressions will be further discussed in the 'Data Analysis' section of this tutorial). The second argument in the *gsub()* function indicates what we want to replace the pattern with. Since we just want to delete the handles, we put an empty string here. The third argument is the list of the texts from which we want to delete the handles:


```R
rand.df$text <- gsub("@\\w+","",iconv(rand.df$text)) 
# rand.df$text is wrapped in the iconv() function, which transforms the utf-8 enconding into a format readeble by R.
# in most cases it is not needed, but if you encounter errors related to utf-8 encoding, simply use this function.
head(rand.df$text)
```

Check how the data frame looks like after the anonymisation:


```R
head(rand.df[,c(1:5)])
```

Now, check which other columns in the data frame contain personal identifiers:


```R
colnames(rand.df)
```

It looks like there are a few of those columns (reply_to_status_id, reply_to_user_id, etc). Anonymise and/or remove all of those as well, as shown above. 

## Streaming tweets

A third option to extract tweets is 'streaming', which means that you collect tweets as they are published by people during a certain time interval. 

To do this, use the function *stream_tweets()*. For example, to stream tweets containing the term 'bigdata', provide 'bigdata' as first argument. As second argument, give the time interval during which you want the streaming to take place. So, to stream tweets containing the term 'bigdata' for 30 seconds, type:


```R
tweets3 <- stream_tweets("bigdata", timeout = 30,token=token)
```


```R
# Now have a look at the kind of data you have collected
colnames(tweets3)
```
