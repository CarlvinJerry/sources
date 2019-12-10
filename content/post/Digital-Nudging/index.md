Introduction:
=============

Here’s a fun fact; An average human being (probably an adult) makes
close to 30,000 conscious decisions every day. This isn’t entirely true
though, in fact, I just made that number up. I could be right because if
you think about it, how many decisions would you say you make on a day
to day basis? Depending on who you are the above obviously varies widely
and you know best. We all make ****n**** decisions every day- what to
do, eat, buy or hit. The real question however is, do our daily choices
solely depend on our consciousness? Are there any other factors at hand
that influence our decision making process? Are all these factors, if
any, always straight forward choices or do we sometimes get **“nudged”**
into these choices we make?

**Nudge theory** basically states that; by understanding how people
think and what drives their decisions, we can use those factors to steer
them into making decisions differently, through positive reinforcement.
Research has shown that, by presenting choices differently rather than
in a legislative manner, people can be influenced into making specific
desired choices. This theory is widely used in [**behavioral
economics**](https://en.wikipedia.org/wiki/Behavioral_economics) by
presenting subtle **nudge units** intended to influence people’s
thoughts about financial products. The theory was however initially more
of a moral aspect meant to help people make better decisions in life and
not as a tool for commercial gain. Over years of practice, different
applications of the theory emerged.

Now that we have a basic understanding of what nudge theory is about, we
can explore an applicable example. This post mainly focuses on a short
research project I happened to be part of, actually my first hackathon
experience hosted by Safaricom PLC. Let’s dive in!

### The Challenge

This photo a team mate took at the hackathon contains a problem
statement for the challenge:

</center>
<figure>
![the
challenge](https://github.com/CarlvinJerry/sources/blob/master/static/MyImages/theChallenge.jpg?raw=true "fig:")
<figcaption>
Figure 1: The challenge
</figcaption>
</figure>
</center>

Tools used:
===========

Our twitter data was fetched using **R**, I have done a post on setting
up a twitter API to fetch twitter data
[here](https://beyondrawdata.rbind.io/2019/01/25/data-mining-in-r/).
**R** has several packages (such as **“tweeteR”** and **“rtweet”**) that
one can use to stream data from twitter. Our data cleaning and
pre-processing was mainly done in **Python**.

> **Note:** To keep this post concise, code for the workings has been
> minimized. [The source code for this post can be found
> here,](https://github.com/CarlvinJerry/sources/blob/master/content/post/2019-02-20-digital-nudging-rmd.Rmd)
> for anyone interested in trying out the same process. The code is well
> commented for easier understanding as well.

#### 1.Fetching Data

The team agreed on a few terms to query data on from twitter. For an
unbiased range of topics, we settled on fetching tweets under trending
topics and a few more from random words. We had tweets from or
containing the following:

-   **\#MenConference2019**  
-   **“Here”**  
-   **\#r\_Stats**  
-   **“PWC”**  
-   **\#Friday Feeling**

A total of **7000 tweets** were captured. The data frame had a total of
88 columns which we treated as variables for the research. However, not
all variables were used in the research we therefore had to do some data
cleaning. Here is a preview of the variables in our raw data.

     [1] "user_id"                 "status_id"               "created_at"             
     [4] "screen_name"             "text"                    "source"                 
     [7] "display_text_width"      "reply_to_status_id"      "reply_to_user_id"       
    [10] "reply_to_screen_name"    "is_quote"                "is_retweet"             
    [13] "favorite_count"          "retweet_count"           "hashtags"               
    [16] "symbols"                 "urls_url"                "urls_t.co"              
    [19] "urls_expanded_url"       "media_url"               "media_t.co"             
    [22] "media_expanded_url"      "media_type"              "ext_media_url"          
    [25] "ext_media_t.co"          "ext_media_expanded_url"  "ext_media_type"         
    [28] "mentions_user_id"        "mentions_screen_name"    "lang"                   
    [31] "quoted_status_id"        "quoted_text"             "quoted_created_at"      
    [34] "quoted_source"           "quoted_favorite_count"   "quoted_retweet_count"   
    [37] "quoted_user_id"          "quoted_screen_name"      "quoted_name"            
    [40] "quoted_followers_count"  "quoted_friends_count"    "quoted_statuses_count"  
    [43] "quoted_location"         "quoted_description"      "quoted_verified"        
    [46] "retweet_status_id"       "retweet_text"            "retweet_created_at"     
    [49] "retweet_source"          "retweet_favorite_count"  "retweet_retweet_count"  
    [52] "retweet_user_id"         "retweet_screen_name"     "retweet_name"           
    [55] "retweet_followers_count" "retweet_friends_count"   "retweet_statuses_count" 
    [58] "retweet_location"        "retweet_description"     "retweet_verified"       
    [61] "place_url"               "place_name"              "place_full_name"        
    [64] "place_type"              "country"                 "country_code"           
    [67] "geo_coords"              "coords_coords"           "bbox_coords"            
    [70] "status_url"              "name"                    "location"               
    [73] "description"             "url"                     "protected"              
    [76] "followers_count"         "friends_count"           "listed_count"           
    [79] "statuses_count"          "favourites_count"        "account_created_at"     
    [82] "verified"                "profile_url"             "profile_expanded_url"   
    [85] "account_lang"            "profile_banner_url"      "profile_background_url" 
    [88] "profile_image_url"      

<br>

#### 2.Data pre-processing.

This stage involved cleaning up our data by removing the unwanted
columns/variables. We decided to do with a select few variables we
thought would be most appropriate for our case study. We chose the
following **seven variables:**

-   **Text** - This column contained the actual tweets text.
-   **Verified** - whether or not the user is verified on twitter.
-   **Protected** - Whether a user is or isn’t protected (Locked
    accounts).
-   **Location** - Based on our challenge stated in the **figure
    above**, this variable was our most important variable. Rows with
    **NULL** values for location simply meant that the specific user
    **DID NOT GEOTAG** their tweet.
-   **Followers Count** - Number of followers the user had.
-   **Retweet Verifie** - Whether the tweet had been retweeted by a
    verified user or not.
-   **Source** - Source of the tweet i.e “Android”, “web client” e.t.c

Code for the data cleanup and variables setting that was done in Python
can be found
[here](https://github.com/Ogutu-Brian/DataCleanup/blob/develop/analysis.py).

<br>

#### 2.1 Re-importing Data in R and setting up for the Models

After cleaning up the data, we imported it into R, the code chunk shows
a preview of the top 4 rows of the input data.

                                                                                                                                                                        Text
    1074                                                                                                                                                            Want to know how to optimize hyper-parameters in Caret with cost-specific functions? #rstats #datascience https://t.co/cupvirSXU9
    1316                                                                                                                              via @RichardEudes - Quick Guide to R and Statistical Programming https://t.co/GfyhLMgiuB #analytics, #datascience, #rstats, #statistics https://t.co/Cx3TGJTOoI
    2636                                              small #rstats trick: if you need to know if a *sorted* variable is equally spaced (e.g., if it's a contiguous sequence of ints, which was my use case) you can look at the characteristics of diff(x), e.g.\n\nsummary(diff(x))\ntable(diff(x))
    2939 my #ggplot2 flipbook project is online! <U+0001F60E><U+0001F913><U+0001F913> Incrementally walks through plotting code (#MakeoverMonday, soon #TidyTuesday plots). Using #xaringan with reveal function; thanks, @statsgen @grrrck. #rstats. https://t.co/bBBzv0iZLw https://t.co/tFtD78IOHZ
         Verified Protected          Location Followers VerifiedRetweet
    1074    FALSE     FALSE         Singapore      1570           FALSE
    1316    FALSE     FALSE     Paris, France      2151              NA
    2636    FALSE     FALSE Pleasant Hill, CA      1207              NA
    2939    FALSE     FALSE         Sri Lanka      2623           FALSE
                  Characters
    1074          DS-retweet
    1316               IFTTT
    2636  Twitter Web Client
    2939 Twitter for Android

We still had to do some data pre-processing for the models which
includes checking for and removing NULL values if present. Below is a
sample table of the final data set used in the analysis.

<table>
<thead>
<tr class="header">
<th>Verified</th>
<th>Protected</th>
<th>Location</th>
<th>Followers</th>
<th>VerifiedRetweet</th>
<th>Characters</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>NO</td>
<td>NO</td>
<td>TAGGED</td>
<td>&gt;500</td>
<td>YES</td>
<td>55</td>
</tr>
<tr class="even">
<td>YES</td>
<td>YES</td>
<td>NON-TAGGED</td>
<td>&gt;500</td>
<td>YES</td>
<td>80</td>
</tr>
<tr class="odd">
<td>NO</td>
<td>NO</td>
<td>TAGGED</td>
<td>&gt;500</td>
<td>YES</td>
<td>193</td>
</tr>
<tr class="even">
<td>NO</td>
<td>NO</td>
<td>TAGGED</td>
<td>&gt;500</td>
<td>NO</td>
<td>188</td>
</tr>
<tr class="odd">
<td>YES</td>
<td>NO</td>
<td>TAGGED</td>
<td>500&lt;</td>
<td>YES</td>
<td>188</td>
</tr>
<tr class="even">
<td>NO</td>
<td>NO</td>
<td>NON-TAGGED</td>
<td>500&lt;</td>
<td>NO</td>
<td>190</td>
</tr>
</tbody>
</table>

From the table above, we can observe a new column “Characters”. This was
an additional variable derived by counting the number of characters in
the tweet text.

<br>

#### 3.Model Specifications

Due to the nature of our problem,(we had several uncorrelated variables)
we decided to do a classification analysis. This means we had to come up
with a classifier model to regress **n** variables based on our
dependent variable, the **Location** variable. The main challenge of
classifier models is knowing what really goes on inside the models that
leads to the final output. Even with higher levels of accuracy in some
models, it is quite difficult o understand the paths of a given model.
However, using **Random forests** and **Decision Tree** classifiers can
give us a graphical representation of the criteria followed by the
models to arrive at a given output. Another upper hand of decision tree
models is that they require minimal data cleaning, less time consuming.
Here is a detailed read on [how decision trees
work](https://medium.com/x8-the-ai-community/decision-trees-an-intuitive-introduction-86c2b39c1a6c).

Creating Train & Test Sets.
===========================

For the training and test data sets, we randomly split our data set into
two sates. Usually, the best practice is to train the model with with a
larger proportion of the data set. We therefore took **80%** for
training and **20%** for test purposes.

<br>

Model Training.
===============

We trained our decision tree model to predict a **class** “location”.
Whether a location is **geotagged** or **not geotagged** based on
whether the user is verified, protected, has over 500 followers, is
retweeted by another verified user and the number of characters in their
tweet. Bellow is the visual output of the trained model.

</center>
<figure>
![Tags](https://github.com/CarlvinJerry/sources/blob/master/static/MyImages/tagged.png?raw=true "fig:")
<figcaption>
Figure 2: The Tree
</figcaption>
</figure>
</center>

When interpreting decision trees, you start at the root node. The root
node is the one on top of the decision tree. Since what we want is those
nodes with geotagged locations, it is safe to ignore the non-tagged
nodes. Note that our highest entropy level was observed on one variable
only, the number of characters in the tweet text. This might not always
be the case with decision trees though, it is possible to have more than
one factor. In such situations, it is best to run several decision trees
to build a **random forest** and make a decision based on the most
prevalent variables.  
For our case, we only focus on what we found:

1.  At the top node, we can see the overall probability of a user
    geotagging their tweets. **75 percent** of the users in the training
    set geotagged their tweets. not

2.  Our second node asks whether the number of characters are **more
    than 134** and goes to depth 2 where we can observe the **highest
    number of users tweeted more than 134 characters** at **80 percent**
    with an **80 percent probability** of geotagging their tweets.

3.  Node 3 checks if the number of characters in a tweet is **less than
    134**. If yes, head to depth 3, where we can see that **20 percent**
    of users had less than 134 characters with a **50 percent
    probability** of geotagging their tweets.

4.  Finally, looking at depth 4 which originates from the node that
    checks is number of characters is **equal to or more than 122**, we
    can see that **12 percent** of users had tweets with character equal
    to or more than 124, with **88 percent probability** of geotagging
    their tweets.

<br>

#### 3.1 Model Testing and performance accuracy.

With our model trained and outputs observed, we were able to run a test
with our test subset. Here is our confusion matrix.

Confusion matrix
================

              predict_geotags
                 NON-TAGGED TAGGED
      NON-TAGGED         90    248
      TAGGED              2   1043

<br>

Model Accuracy
==============

    > #performance
    > accuracy_Test <- sum(diag(table_mat)) / sum(table_mat)
    > print(paste('Accuracy for test is', accuracy_Test))
    [1] "Accuracy for test is 0.819233550253073"

From the confusion matrix above, we can observe that the model had a
**true negative of 90 predictions**. That is, 90 predictions were
correctly predicted as not geotagged. A **false positive of 248
predictions** was observed where the model wrongly predicted 248 tweets
were geotagged whereas in real sense they were not.

For the tagged tweets, we had a **false negative of 2 predictions**
against a **true positive of 1043 predictions**. This means that our
model was able to correctly predict 1043 geotagged tweets from the test
data. The accuracy of the model turned out pretty good, at an **82
percent accuracy level**. The theoretical formula for the accuracy is
the proportion of true positives and the true negatives divided by the
sum of the confusion matrix.

<center>

$$ Accuracy = \\frac{TP + TN}{TP + TN + FP + FN}$$

</center>

For a better accuracy level, the model’s hyper-parameters can be tweaked
to improve performance. Another option is implementing a random forest
test.

<br>

#### 4. Conclusion and Recommendation

With our decision tree model, we were able to attain a high level of
accuracy for a model that test whether users with **tweets containing
characters equal to or above 122** are likely to geotag their tweets.
Our **nudge** in this case is the number of characters in a tweet and
precisely, **124 or more**. Our recommendation therefore would be to
encourage users to tweet longer or engage them in trending topics that
require one to write more, for example a TT like \#
MyLifeHistoryInANutshell…-in the hope that a user will eventually geotag
their tweet.

> Come to think of it, did twitter really increase the number of
> characters just for tweeps to tweet more and as they said, to get more
> people to join twitter? I have a theory, it was a NUDGE!

<br>

References
==========

1.  [Business balls official
    website](https://www.businessballs.com/improving-workplace-performance/nudge-theory/)

2.  Thaler, R.H., Sunstein, C.R., and Balz, J.P. Choice Architecture.
    SSRN Electronic Journal (2010), 1–18;
    <a href="https://ssrn.com/abstract=1583509" class="uri">https://ssrn.com/abstract=1583509</a>

3.  **Thaler, R.H. and Sunstein, C.R. Nudge: Improving Decisions About
    Health, Wealth, and Happiness**. Yale University Press, New Haven,
    CT, and London, U.K., 2008.

<br>

<center>

![](https://fontmeme.com/permalink/190129/8b378e9ce35b7a28dd150c4f1d656807.png)

<center>

<br>
