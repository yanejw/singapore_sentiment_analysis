# Sentiment Analysis with NLP

### Problem statement

As a small nation dependent on foreign investments, spending, and good relationships with other countries, Singapore has a vested interest in its public perception and international reputation. Public perception has an impact on economy and foreign relations. For example, travel vlogs depicting Singapore in a good light are amplified in the online space, attracting more visitors and tourist spending. Singapore's good reputation in terms of safety and political stability are also integral to attracting businesses to set up here. It is therefore important to monitor what the online sphere is saying about Singapore and take relevant sentiments/perceptions as feedback for improvement.

The goal for this project is to build a classification model that can identify negative sentiment against Singapore. We do this by training our model on Youtube comments from videos about Singapore. We will use different classification models, such as Naive Bayes and Random Forest. We will use the F1-score as as a metric as it takes into account both precision (reduce false positive) and specificity (reduce false negatives).

-----

### Video selection process

1. Filtered by most recent
   - Focused on videos about Singapore as a whole and/or its policies, rather than travel vlogs, businesses (ie. Singapore Airlines)
   - Disregarded non-English videos
   - Disregarded those with less than 100 comments or have comments disabled
2. Filtered by most views
   - Focused on videos about Singapore as a whole and/or its policies, rather than travel vlogs, businesses (ie. Singapore Airlines)
   - Disregarded non-English videos
   - Disregarded those with less than 100 comments or have comments disabled
3. Tried to get a mix of "positive" and "negative" sounding titles

#### Youtube videos used

|Video Title|Publish Date|Link|
|-|-|-|
|Singapore to hang first woman in nearly 20 years during executions set for this week: rights groups|26 Jul 2023|[Link](https://youtu.be/xJlgtV8L7Jc)|
|Inside Singapore’s deadly war on drugs: 101 East Documentary|19 Jan 2023|[Link](https://youtu.be/GL1JdIeoo4A)|
|The Dark Side of Singapore's Economic Miracle|9 Oct 2022|[Link](https://youtu.be/XDYy8z7krAI)|
|Why Singapore is One of the World's Richest Countries|9 Sep 2022|[Link](https://youtu.be/XSOgcpRbrCo)|
|Why Singapore Is Insanely Well Designed|14 Sep 2022|[Link](https://youtu.be/vyfJgJBB3Vk)|
|Singapore: The World's Only Successful Dictatorship?|3 Jul 2021|[Link](https://youtu.be/Hkxf4SC_SBk)|
|The Almost Perfect Country|26 Aug 2020|[Link](https://www.youtube.com/watch?v=8XNu282FkvM)|
|City of the Future: Singapore – Full Episode: National Geographic|24 Nov 2018|[Link](https://youtu.be/xi6r3hZe5Tg)|ates.

-----

### Data extraction and pre-processing

We used the Youtube API to extract comments from the videos specified in the table. The comments were extracted video-by-video, as opposed to in one go, in order to keep track of the quota.

The usual data cleaning (checking for duplicates, null values, check languages etc.) was done.

Since the corpus had no labels, it was put through the VADER sentiment analysis model to get a negative (0) or positive/neutral (1) classification. A manual sentiment analysis was also done on 100 randomly sampled comments to ensure that the accuracy of the VADER sentiment analysis model was at an acceptable threshold. 

-----

### Exploratory Data Analysis

EDA was done to find the stopwords, word frequency, n-grams and n-grams. These analyses helped to inform the final shape of the corpus.

-----

### Data Modelling

TF-IDF was used over CountVectorizer for all models as it gives higher weight to words with stronger semantic meaning and importance in the corpus. Whereas CountVectorizer would just assign all words similar weights.

We then ran the train set through three classification models (Logistic Regression, Multinomial Naive Bayes, and Random Forest) twice. Once without SMOTE, and once with SMOTE to counter the imbalanced data set.

#### Model comparison

The results of the model are detailed in the table below:

|Model|SMOTE|Train (F1)| Test (F1)|Remarks|
|-|-|-|-|-|
|Logistic Regression|No|0.8916|0.8356|Overfit|
|Logistic Regression|Yes|0.8899|0.8165|Overfit|
|Multinomial Naive Bayes|No|0.8863|0.8655|Overfit|
|Multinomial Naive Bayes|Yes|0.8748|0.8123|Overfit|
|**Random Forest**|**No**|**0.8598**|**0.8493**|**Good fit**|
|Random Forest|Yes|0.8780|0.8459|Overfit|

-----

### Conclusion

The Random Forest (without SMOTE) model gave the best F1 scores. It is not over nor underfitted, and the F1 scores is higher than the 80% threshold. Hence, the Random Forest (without SMOTE) model would be the best one (out of the six) to be put into production to analyse public sentiment about Singapore.

In order to improve the model, we could collect more data on the minority class in order to correct the imbalance of our data set. We might also want to use social media like Twitter/X, where opinions and public sentiment are more direct. The comments extracted from Youtube videos also included comments about the video creator, which could have skewed the analysis.

