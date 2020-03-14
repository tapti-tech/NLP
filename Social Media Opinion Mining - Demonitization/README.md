Demonetization Twitter Analysis using K-means
***
                                    
Problem Statement 
---

To build an application to model social cognition patterns, i.e. how people form opinions about socially relevant issues (such as demonetisation) on social media platforms (such as Twitter).

Objective
---
The objective of this project is to build a model to understand how 'opinions' about a certain topic get formed. An opinion has two elements:

**Abstraction:** What the opinion is about, for e.g. an opinion on demonetisation can be 'about a topic' such as 'Digital India', corruption, PM Modi, etc.

**Expression:** The 'sentiment' of the opinion, i.e. positive, negative or neutral

Other Related Terms of Opinion Mining :
---

**World View** - is formed by opinions which are internalized by ground truth

**Narratives** - when multiple opinions are shared among people, similar opinions start teaming up, reinforce other similar opinions, and thus become stronger. In other words, people start supporting other people having similar opinions, and as a result, opinions turn into narratives. Lot of opinions and mutually reinforce each other forms Narrative. Narrative is compatible opinions that come together for a particular topic.
example, if a large number of people express an opinion such as 'demonetisation will wipe out corruption in India' (an opinion 'about' corruption and having a 'positive expression'), these opinions attract other similar opinions (via retweets, other similar tweets etc.), and it eventually turns into a 'narrative' - a collective worldview shared by a large number of people.

**Social Cognition** - Narratives become the basis for social cognition. That is, if we are able to identify such narratives on social media, we will be able to understand the common opinions shared by groups of people on a certain topic. Further, we will also know how many people share a particular narrative, the abstraction and expression components of these narratives, etc.

Topic of Social Media Opinion Mining - Semantic Processing – Demonitization
---


To summarise, the objective is to identify the various narratives given a certain topic and to label the narratives using some characteristic words/phrases. 

The process we will follow is summarised below:
* Getting tweets talking about a certain topic
* Cleaning the tweets (removing stop words etc.) 
*	Computing embeddings of tweets using pre-trained word embeddings (such as wor2vec)
*	Computing tweet embeddings with sentiment by appending a 'sentiment score' to the 'tweet embeddings'
*	Clustering the tweet-sentiment embeddings (each cluster representing a 'narrative')
*	Identifying key characteristic phrases of each narrative using TF-IDF frequencies of commonly occurring phrases


Project Pipeline
---
![Project Pipeline](https://github.com/tapti-tech/NLP/blob/master/Social%20Media%20Opinion%20Mining%20-%20Demonitization/1.jpg)

The main step in the project pipeline are the - steps for getting the tweets, cleaning them, computing the sentiment scores, using the word embeddings etc.

**1.	Retrieve Data**

Get tweets on demonetisation from Kaggle (23 Nov 2016 to 11 April 2017) or using Twitter API (http://dev.twitter.com/apps)

**2.	Preprocess Data**

Clean the tweets: Remove special symbols such as RT (header for denoting retweets), URLs, numbers etc.; strip whitespace characters, convert to lowercase, remove stop words

**3.	Compute Word Embeddings**

*	Using word2vec library (Skip gram model), get word vector for each word in  corpus .
*	Compute tweet embeddings by averaging the word embeddings and appending the respective sentiment scores to them
 

**4.	Compute sentiment score of each tweet**

Add Sentiment to each word embedding i.e., Calculate the sentiment score of each tweet using a standard Python library.
*	'opinions' comprises of Abstraction and Expression
*	Word Embeddings captures Abstraction part of tweet
*	Sentiment is added as Expression at last dimension to end of each tweet vector.
*	Each tweet vector is now combination of Abstraction and Expression - tweet embeddings
 
	
**5.	Cluster Tweet Embeddings using K-Means Clustering algorithm**

*	All Word embeddings are clustered using K-Means Clustering algorithm
*	Cluster the tweets into k clusters, where k is identified using the Silhouette score (the value of k having the maximum average Silhouette score of clusters) [Silhouette score - is the measure how similar an object is to its own cluster compared to other cluster using Euclidean Distance]
*	Optimal value of k is calculated by calculating Silhouette score from k varying from 4 to 11.
*	After clustering, clusters with Silhouette score<0.2 are discarded.

**6.	Identify characteristic opinions of Narratives**

*	Identify characteristic phrases ie extract particular set of phrases - by 'chunking' POS tags, such as noun phrase, adjective, followed by a noun, etc.
*	Noun Phrase like India, PM Modi
*	Adjective Phrase like Economic impact, … so on

**7.	Find the Top Phrases**

*	Identify the top characteristic phrases (the 'abstraction' element) of each cluster using the TF-IDF scores of phrases
*	Tf-Idf score of each phrase is then calculated. (Each Narrative is one document).
*	Term frequency is number tweets the phrase occurs in the narrative.
*	Document frequency is number of narratives the phrase occurs in.
*	Phrases are then arranged in descending order of their Tf-Idf score, and threshold is at the point where consecutive difference between any two phrases is maximum.
*	All the phrases greater than this threshold are considered as characteristic abstraction for the narrative.
*	For these selected phrases, sentiment is the maximum occurring sentiment in all tweets this phrase is a part of.
*	These selected phrases and their respective sentiment are considered as the characteristic opinion of the narrative.

**8.	Calculate Sentiment Score**

Finally, computed the sentiment score (the 'expression' element) of each characteristic phrase. This is done by filtering all the tweets containing the phrase and computing the 'most frequently occurring sentiment score' in those tweets.

