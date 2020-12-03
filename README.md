---
# File Structure

---

### Files:
* README.md - describes file
* [Presentation.pptx](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/Project_3_presentation.pptx)
    - presentation in powerpoint 
    - preferred viewing method
* [Presentation.pdf](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/Project_3_presentation.pdf) 
    - presentation in pdf form (alternate if powerpoint not an option)   

### Folders:
    
* code - folder includes files for running the analysis
   * [1_Download_Data_PRAW](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/Code/1_Download_Data_PRAW.ipynb) - import data using PRAW
   * [2_EDA_on_Praw](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/Code/2_EDA_on_Praw.ipynb) - import/clean test data
   * [3a_Download_Data_Pushshift](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/ode/3a_Download_Data_Pushshift.ipynb) - EDA on training data
   * [3b_Combine_subreddits_into_one_DF](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/Code/3b_Combine_subreddits_into_one_DF.ipynb) - based on 2a, prediction 1 
   * [4_EDA_on_Pushshift](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/Code/4_EDA_on_Pushshift.ipynb) - analyze categorical features
   * [5_Processing_and_Modelling](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/Code/5_Processing_and_Modeling.ipynb) - based on 3a, prediction 2 

* data - folder includes data used for the analysis
   * 14 files downloaded using the pushshift API for reddit (not included due to size of some files)
    - downloaded in multiple chunks due to computer timing out
    - see notebook 3a
   * [reps_and_dems.csv](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/Data/reps_and_dems.csv)         
    - combines the all of the reps and dems files downloaded from pushshift
    - cleaned up so only potential columns of interest are included
    - see notebook 3b
   * [politics.csv](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/Data/politics.csv)
    - downloaded data using the PRAW api for reddit
    - see notebook 1
  
* visuals:
   * screenshots used for README file
    
* for future research - Code and Data:
   * [Appendix_1_supplemental_subreddits](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/For_Future_Research--Code_and_Data/Appendix_1-Supplemental_subreddits.ipynb) - Downloaded data from additional 4 reddits using pushshift API
   * [Appendix_2_EDA_on_supplementalsubreddits](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/For_Future_Research--Code_and_Data/Appendix_2_EDA_on_supplemental_subreddits.ipynb) - Cleaned data from additional 4 reddits using pushshift API
   * 4 csv files of cleaned data are not included due to file size
    - Cleaned data from additional 4 reddits using pushshift API 
    - (note that the original files downloaded from online are too big to upload)
    
    
---
# Background

---
For this Project, I analyzed the r/Republican and r/democrats subreddits.  
    
The [r/Republican](https://www.reddit.com/r/Republican/) subreddit includes 154k members, and is the largest subreddit that notes it's a place for republicans.
    
![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/Republican_Header.png)
![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/Republican_About_Community.png)    
    
The [r/democrats](https://www.reddit.com/r/democrats/) subreddit includes 148k members, and is also the largest subreddit that notes it's a place for democrats.

![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/democrats_Header.png)
![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/Republican_About_Community.png)   

---
# Problem Statement

---

### This is a two-part problem.  
1) I want to understand what items are common/unique between the two reddits.

2) I want to predict whether a post is in the r/Republican subreddit or the r/democrats subreddit

---
# Data Dictionary

---

Key Data analyzed:
    
* Title of the reddit posts
* urls of the reddit posts

---
# Methodology

---

**Initial Download Data and basic EDA using the PRAW API**
**Notebooks 1 and 2**
    
I initially downloaded using the PRAW API (see notebook 1). I took the top 1000, newest 1000, most controversial 1000, and hottest 1000 posts from each subreddit.  I initially filtered the data I was looking at so that I only focused on items that appeared in most submissions - which were **title** and **url**.  When I began reviewing the data, I found that the urls were really interesting, however, even though I had almost 8000 posts, I had much fewer urls.

I found 402 urls referenced in the democrat subreddit and 558 urls referenced in the republican subreddit. In total, there were 851 unique urls when combining the two sets of data.  This means that only 109 urls appeared in both subreddits, so I decided I wanted more data.

In reviewing the PRAW documentation, I did not see a clear way to download more that the top 1000 of any given post, so I switched API's that I was using to download data.  
    
**Second Download Data and basic EDA using the pushshift API**
**Notebooks 3a and 3b**
    
I then downloaded up to 100 posts per day (the max allowed), and collected data for the last 8 years in each subreddit using the pushshift api. Because the computer sometimes timed out due to inactivity, I ended up having to collect the data in chunks. However, because I had a counter on the days (which I created after the first set of errors appeared), it was easy to restart where I left off with some overlap. 

I used the timestamps to help when combining the dataframes to account for the overlapping information in each field (I used the timestamp of the last entry for each file and found where that appeared in the subsequent file).  After combining the entire republican and entire democrat data and removing duplicates, I then selected key features that I found were similar in both, filtered both republican and democrat datasets so that I only had the key features in the two dataframes, and combined the dataframes into one file for easy analysis.    

**EDA on the large combined dataframe (downloaded using pushshift API)**
**Notebook 4**

I preprocessed the data (adding title length and word count to the dataframe), then divided my EDA into three main groups - 

* **Analyze the urls**
  * I looked at the urls that most commonly appeared in each subreddit
* **Analyze the title characteristics** (word count and title length)
  * I looked at the distribution of word count and length of the titles for the submissions
* **Analyze the words in the title**
  * I looked at the words in the title in three different ways
  * **Common Words**- using count vectorizer, I compared the top 25 words for each subreddit with and without stop words, and for 1-,2-,and 3-ngram word combinations.  I took the top 900 word for each subreddit, then graphed the top 25 for each one, and how it's value compared to the other subreddit.  I also normalized the appearance numbers since my data was imbalanced (democrats-123668, Republican-90351)
  * **Unique Words** - I looked at the top 1000 and top 5000 words in each subreddit. For each scenario I found the words that appeared the most in one subreddit while not appearing in the other.  I eliminated stop words for this analysis, I also looked at this for 1-,2-, and 3-ngram words.
  * **Sentiment Analysis** - I performed sentiment analysis on each subreddit and found a lot of sarcasm just in the first few posts of both subreddits (which sentiment analysis doesn't analyze well).  Both subreddits also had similar distributions, so I performed limited sentiment analysis.
  
The results of this analyses affected how I analyzed URLs, title characteristics, and title words in the modeling phase.  I did vectorization, but I did not do sentiment analysis.  See Notebook 5 for modeling.   
    
**Modeling on the large combined dataframe (downloaded using pushshift API)**
**Notebook 5**

I did modeling on the data downloaded from pushshift API. This was divided into the following parts:

* Preprocessing - I processed the data similar to notebook 4 - adding title length and title word count columns.

* Defined my baseline accuracy as 57.8%

* I generally tried two types of vectorizing in my models:
  * CountVectorizer (CV)
  * TfidfVectorizer (TV)
  
  
* I generally tried three types of classification models:
  * Multinomial Naive Bayes (MNB) - because it's good with words
  * Logistic (Log) - because it's very interpretable
  * Random Forest Classifier (RFC) - because it often gives good results


* I created models using the above combinations of vectorization and model types on four sets of data.  
  * **Model Group 1**: Modeled on Words in the Title 
  * **Model Group 2**: Modeled on URLS
  * **Model Group 3**: Modeled on Words in the Title, URLs, and Title Charactistics (word count and length of the title)
  * **Model Group 4**: Modeled on Title Charactistics (word count and length of the title)
   
    
**For Future Research**
**Notebooks Appendix 1 and 2 in the Future Research Folder**

I downloaded up to 100 posts per day for the last 4 years for 4 additional subreddits that were similar (libertarian, progressive, liberal, and conservative).  I also performed the same filtering on the as in notebook 3b, so that the analysis would be easy to perform in the future following what I did in notebooks 4 and 5.  The cleaned files are saved in the Future research folder, some of the original files were too large to save on git hub's normal account, so they are only local on my computer.
    
---
# Key Analysis

---
## EDA Findings

 * **Title Words**
 
  * **Common Words**
  * Democrats and Republicans have a lot of overlap in their top words
  
  * Common words for 3-word n-grams (looking at the top 900 words in each subreddit)
    ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/3word_ngram_rep.png)      ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/3word_ngram_dem.png)  
  
  * Common words for 1-word n-grams (looking at the top 900 words in each subreddit)
  
    ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/1word_ngram_rep.png)      ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/1word_ngram_dem.png)  
  
  * **Unique Words**
  * The words get more interesting when looking at the unique out of top 5000 words vs top 1000 words
  * when looking at the unique words out of the top 5000 words, the words tend to be very specific to platforms/values of a given party
  
  * Unique words out of the top 5000 words in each subreddit (looking at 1-word n-grams)
      ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/1word_unique_rep_5000.png)      ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/1word_unique_dem_5000.png)  
  
  * Unique words out of the top 1000 words in each subreddit(looking at 1-word n-grams)
 
     ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/1word_unique_rep_1000.png)      ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/1word_unique_dem_1000.png)  
 
 * **Sentiment Analysis**
 * I looked at the top five results for positive, negative, and compound sentiments for both the republican and democrat subreddits
 * There is a lot of sarcasm and harsh language in just the top five results, so please refer to Notebook 4 (a link to the sentiment analysis section is in the top of the notebook) if you're interested in looking at the results for this.
 
 
 * **URLs**
 
   *  The news sources for democrats and republicans overlap, but have distinct tendencies 
 ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/Democrat_URLs_short.png)
 ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/Republican_URLs_short.png)
 
 
 * **Title Characteristics**
 
   * Democrats have longer title lengths
  ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/Title_length.png)  
   
   * Democrats have more words in their title
  ![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/Title_word_count.png)  

## Modeling Results

 * Summary table of best results for each model group (see Notebook 5 for summary tables of all models run) 

| Model Group | Model | Dataset | Vectorizer| Model| Cross-Val Acc.| Train Acc.| Test Acc.| Spec.| Sens.| Notes |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |--- |
| **Model Group 1**| Model 1b |Title Words|CV|Log|0.690|0.714|0.695|0.753|0.617|Best of Group 1|
| **Model Group 2**| Model 2e |URL|TV|Log|0.776|0.797|0.780|0.903|0.611|Best of Group 2|
| **Model Group 3**| Model 3b |Title Words/URL/Title Chars|CV-words, TV-URL|Log|n/a|0.800|0.78|0.704|0.843|Best of Group 3|
| **Model Group 4**| Model 4b |Title Chars|n/a|Log|n/a|0.585|0.582|0.828|0.247|Best of Group 4|

* Confusion matrix and key related features for each of the models
**NOTE: In the confusion matrices below, 0 represents the democrat subreddit, and 1 represents the republican)**

 * Model 3b (analysis using Title Words, URL, and Title Characteristics)

![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/3b_conf_mat.png)

![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/3b_features.png)


 * Model 1b (analysis using Title Words)

![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/1b_Conf_Matr.png)

![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/1b_features.png)


 * Model 2e (analysis using URL)

![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/2e_conf_matr.png)

![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/2e_features.png)


 * Model 4b (analysis using Title Characteristics)

![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/4b_conf_mat.png)

![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/4b_features.png)

---
# Conclusions/Recommendations

---

### Conclusion
* ‘Trump’ is mentioned almost twice as often in Democrats vs Republicans
* Democrats like to write more words
* Urls tell more about the subreddit than the words in the title
* Combining the words, Urls, and title description is the best

* The model results are reasonable (a key feature for categorizing a post as republican was 'Kasich', which was one of the top unique words when looking at top 1000 words in each subreddit)

![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/1word_unique_rep_1000.png)

![](https://github.com/jenniferlwilliamson/Project_3-Subreddit_Analysis/blob/main/visuals/3b_features.png


### Next Steps
* Explore the relationships in the words more
* What ranking of words count for each least are the features in each model
* Explore similar subreddits – libertarian, conservative, progressive, liberal
