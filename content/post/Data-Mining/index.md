### Part I

> This first section assumes you have no knowledge in building a twitter
> app to be used for fetching data. You can skip directly to [the second
> section here](#PartII) if you are able to build your own twitter app
> and get the required authentication keys.

#### INTRODUCTION

Social media usage has grown rapidly over the past few decades. Most
social networks we can think of now are so well established, making them
a platform where people can not only interact but also a haven for
anyone in need of **“unstructured”** data. With an almost constant rate
of increasing users each day, social networks such as Facebook and
Twitter have become great sources of data which can be used in the broad
field of Data Science:Talk of (those pretty annoying) targeted ads for
example…

<center>
![](https://media.giphy.com/media/3o6Mbmg6AchRmB4ylO/giphy.gif)
<center>

<br>

With the help of APIs, we are easily able to get data from such
platforms to be used for further analysis.In this article we will go
through the preliminaries of text mining in R using Twitter data.The
main advantage of these APIs is that the data we will fetch comes in a
well-structured format which makes our work easier when crunching.

In this case we will use the readily available Twitter API and create
our own Twitter app that will then help in fetching the data.

<br>

#### CREATING A TWITTER APP

To create a twitter app we can use for fetching metadata, we first need
to have a Twitter account. We then need to go to the [**twitter dev
site**](https://developer.twitter.com/) and log in with our user
account. On the top right corner should be a drop down menu next to your
username, go to APPS. At this point if you are doing this for the first
time your **Apps** section should be blank. Click on **“Create an app”**
to… create an app.

<center>
![create
app](https://github.com/CarlvinJerry/sources/blob/master/static/MyImages/apps.PNG?raw=true "fig:")
</center>

We then have to fill in the form below appropriately. Here is a
breakdown of what’s required:

-   **Name:** Give your app a unique name of your choice, e.g
    **UniqueName**
-   **Description:** This can always be changed later, use this to
    provide a brief note on what your app is all about to be able to
    distinguish it from other apps you might create in future.
-   **Website:** This should be your application’s home page web site.
    It is however not applicable for most personal apps. Anything goes
    here e.g
    <a href="https://carlvinjerry.github.io" class="uri">https://carlvinjerry.github.io</a>
-   **Callback URL:** I would ignore the Callback URL field. If you are
    allowing users to log into your app to authenticate themselves,
    you’d enter the URL where they would be returned after they’ve given
    permission to Twitter to use your app.

<center>

![App
details](https://github.com/CarlvinJerry/sources/blob/master/static/MyImages/apps2.PNG?raw=true)

</center>

<br>

The remaining fields should be quite straight forward but must be
filled. Click **“Create”** once done and there you have your first
twitter app. On your app is a menu with **Keys and tokens**. These are
the most important components since we will need them to access data
from the API.Generate both consumer and access tokens (if not readily
available) and take note of them. **NB:** **Keys are private
property…most of the times**

<center>
![Keys](https://github.com/CarlvinJerry/sources/blob/master/static/MyImages/keys.PNG?raw=true "fig:")
</center>

<br>

The final bit of setting up our twitter app is granting access
permissions. We will mostly do fine with **read-only** if all we need is
to fetch data but it can always be changed later.

<center>
![Permisions](https://github.com/CarlvinJerry/sources/blob/master/static/MyImages/permisions.PNG?raw=true "fig:")
</center>

<br><br>

Now we can move on to the next step where we set up **R** to query data
from Twitter.

<br>

------------------------------------------------------------------------

------------------------------------------------------------------------

<br>

### Part II

#### SETTING UP R TO FETCH TWITTER DATA

With our twitter app set up [in part I above](#PartI) and we are able to
get the authentication keys for the API, we can now easily fetch data
from twitter in R. The following steps will help us do this:

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>

#### Prerequisites:

-   **Twitter API Keys:** At this point we already have our twitter app
    with the required API keys.
-   **R and an IDE of choice:** We also need to have R installed,
    advisably the latest version. Microsoft’s [**enhanced R
    distribution**](https://mran.microsoft.com/open) is recommended over
    the [**base R**](https://cran.r-project.org/bin/windows/base/) but
    for this specific task either can do just fine. I would recommend
    [**R STUDIO**](https://www.rstudio.com/products/rstudio/download/)
    for an **IDE**. One obvious advantage of all these is that they’re
    open-source tools.

<br>

#### 1.Install and Load the required packages in R

R has a standard set of packages, each with different tasks. You can
find some packages for download  
[**here**](https://cran.cnr.berkeley.edu/). The code chunk below
installs and loads the specific packages we need for this task. Take
note of comments at each line of code, initiated by an octothorp.

    # Install packages
    install.packages("twitteR") #------Extracts data from twitter
    install.packages("httr") #--------Tools for Working with URLs and HTTP

    # We can now load the two packages
    library("twitteR")
    require("httr") #-------------Both require() and library() can be used to call an installed package

<br>

**NB:** Windows users might need to download a certification file and
store it in the working directory. This certificate file initiates a
handshake between R and the Twitter API.

    # Download "cacert.pem" file
    download.file(url = "https://curl.haxx.se/ca/cacert.pem", destfile = "cacert.pem")

<br><br>

#### 2.Create and store objects containing the twitter authenticated credentials

This is where we invoke the twitter API using the credentials from our
app and query the data we need.

    # Authentication keys
    consumer_key <- "hjksdha08097afnjhaa90uaf"
    consumer_secret <- "hjksdha08097afnjhaa90uaf"
    access_token <- "hjksdha08097afnjhaa90uaf"
    access_secret <- "hjksdha08097afnjhaa90uaf"

    # The above tokens are what we made the twitter app for.

<br><br>

#### 3.Query data from twitter

We can now go ahead and fetch our data. Due to limitations on the
twitter standard apps, it is advisable to store your data in R locally.
This will reduce the number of times you have to make requests to fetch
data. You will therefore do much more with your app that way regardless
of the limitations- Or you can as well buy the premium rights. In my
example below, I am fetching data for a user on twitter called
@UKenyatta.

    # Connect to Twitter
    setup_twitter_oauth(consumer_key, consumer_secret, access_token, access_secret)
    tweets <- userTimeline("UKenyatta", n = 3200) # Standard twitter apps are limited to 3200 tweets per                                                  #download session. This could come out less depending on
    # the app

    # create a data frame of the tweets
    UKenyatta_Tweets <- tbl_df(map_df(tweets, as.data.frame))

    # Save tweets for later (and note when saved):
    save(UKenyatta_Tweets, file = "UKenyatta_Tweets.RData")

    # You can then access them later at will...
    # load("UKenyatta_Tweets.RData")

You can now manipulate your data and see what you find out…

<br>

<center>
![](https://fontmeme.com/permalink/190129/8b378e9ce35b7a28dd150c4f1d656807.png)
</center>

<br>
