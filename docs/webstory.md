# Comparing the focus of news between China and US

## Introduction
China and the US, one is the biggest developing country and the other is the most powerful developed countries in the world. The relationship between these two countries has great influence on international relationship. Looking back, the relationship showed different trends at different time period. In the beginning of 21th century, Bush administration’s policy toward China expanded the cooperation of China and the US. In particular, the economic and trade relations between the two countries are closer. Expect from collaboration, there are also many competitions and confrontations between China and the US in politics, economy and culture. For instance, the trade war is a clear reflection of the confrontation between the two countries. The official news platforms are essential channels for government to show their political voice and communicate with people. What are the key words and most popular topics of two countries’ news headlines? Are these two countries share some focus points which means both countries are attaching importance to these issues? From differences behind headlines and content of news, are there any stories of China and the US can we find?

This paper compares the similarities and differences of two countries’ official news websites in the time period of March 2017 to November 2018. We select the State Council website of China and the White House website of America to do data collection. On the one hand, we are going to figure out the differences of news writing strategy between two countries and to explore the propaganda model of the official news websites of China and the US. On the other hand, during the time period we explore, Sino-US trade war started. We are going to find out whether the political tension between two countries can reflect from the news articles on the official websites. We compare top-10 and top-10 most frequent words in news titles between two countries. Then we let computer generates the news topics automatically based on LSA and LDA using both bag of words and  TF-IDF. 

According to some related studies, focusing on the discourse analysis of the news of the China and the US. Yang (2011) identified the differences of news articles in the New York Times and China Daily could be easily observed, which was the New York Times tended to apply anti-Chinese government frame while a pro-government frame is common in the China Daily. Besides, Ooi & D’Arcangelis (2017) investigated how China was being “othered”, using “discourse analysis” to connect “orientalism” and found out that orientalist tropes related to three areas of significance: China’s national currency valuation, cyber espionage, and maritime disputes in the East China Sea and South China Sea. Moreover, Shao, Zhang, & Meng (2015) used automatic text analysis to explore the propaganda model of Xinwen Lianbo. They thought that political textual analysis was an important way to understand political phenomenon, and used supervised learning methods to process the 4018 textual documents of Xinwen Lianbo provided by CCTV. 11524 key words were extracted as the objects of study for further political communication analysis. Lots of researchers have explored the political tensions, political enemy and political relationship through the discourse analysis. This study gained inspiration from the previous study, combing with the method of data analysis and raised two research questions.

In viewing of the official news websites of the China and the US, this research raise a big question: What are the differences of the focus of the news between China and US? In order to answer this big question, this research explored in a more precise and specific way by answering two research questions:

RQ1: What are the most popular keywords among the headlines?

RQ2: What are the hot issues of the news articles in these two official websites?

## Data Acquisition
To answer the two aforementioned research questions, we chose to harvest the news section of the website of The White House ( https://www.whitehouse.gov/news/ ) and the news section of the website of The State Council of China ( http://english.gov.cn/news/topnews/ ) because they are the most authoritative news outlets in each country. We target at the date, the title, the url, and the full-text of each news article.
### White House
On a single webpage of the news section, all the data related to a certain piece of news were wrapped in a `<article>` html tag, with the title in a `<h2>` tag, the url in a `<a>` tag wrapped in the aforementioned `<h2>` tag, and the date in a `<p class = ‘meta_date’>` tag.

![WhiteHouseTitles](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/WhiteHouseTitles.png)
**Fig. 1. Snapshot of White House news list**

Based on this understanding of the webpage structure, we first found all the `<article>` tags in which we subsequently found all related tags by using the “requests” and “beautifulsoup4” packages, and appended them to their respective lists. Before starting harvesting, four empty lists named “dates”, “titles”, “urls” and “texts” had been created to store the harvested data. Since the data we needed were store on multiple webpages, we wrote a for-loop to scrape all 487 pages automatically.

Then we started to harvest full-texts by looping the items in the “urls” list. On the webpage of a specific news article, the full-text is wrapped in a `<div class = ‘page-content editor’>` tag. 

![WhiteHouseFullText](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/WhiteHouseFullText.png)
**Fig. 2. Snapshot of a White House news article**

However, besides the full-text, there were also some regular unwanted texts within this tag: “share-this-page-on-facebook”, “share-this-page-on-twitter”, “copy-url-to-your-clipboard”, and “All News”. To solve this problem, we used the replace() method to remove the unwanted texts. After the simple processing, the raw full-text is appended to the “texts” list. Next, we created a dictionary from the four existing lists, and created a dataframe from the dictionary we had just created. Then we saved the dataframe into a CSV file which was the raw dataset of White House news. The dataset has 4860 pieces of news, spanning a period from 2017-01-24 to 2018-11-19.

![WhiteHouseRaw](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/Screen%20Shot%202018-12-12%20at%2010.57.52.png)
**Fig. 3. Snapshot of the raw dataset of White House news**

### State Council
The news section of State Council had only one webpage although there are page numbers to click on. When clicking on a certain page number, the url in the address bar will not change. For this reason, we did not need to use for-loop to harvest.

![StateCouncilTitles](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/StateCouncilTitles.png)
**Fig. 4. Snapshot of State Council news list**

All the data related to a certain piece of news were wrapped in a `<div style = ‘display: block’>` or `<div style = ‘display: none’>`html tag, with the title and the url in a `<a>` tag, and the date in a `<span>` tag. Because of the webpage structure, we first harvested all the `<div style = ‘display: block’>`tags by using the “requests” and “beautifulsoup4” packages  and then harvested all the `<div style = ‘display: none’>` tags. As we had done in harvesting White House news, all the scraped data were appended to empty lists named “dates”, “titles”, and “urls” respectively which had been created at an earlier time.

After getting all the urls, we started to harvest the full-texts by looping all the items in the “urls” list. On the webpage of a specific news article, the full-text is wrapped in a `<content>` tag. 

![StateCouncilFullText](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/StateCouncilFullText.png)
**Fig. 5. Snapshot of a State Council news article**

When we were looping for the first time, we encountered a problem that some of the urls were invalid, which caused a breakdown to our program. To solve this, we added a try/except statement to skip those invalid urls. Each full-text was appended to the “texts” list. Next, a dictionary was created from the four existing lists, and  a dataframe was created from the dictionary which had been created. Then we saved the dataframe into a CSV file which was the raw dataset of State Council news. The dataset has 4792 titles, 4794 urls and 4790 full-texts, spanning a period from 2016-07-27 to 2018-11-20.


![StateCouncilRaw](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/Screen%20Shot%202018-12-12%20at%2012.06.56.png)
**Fig. 6. Snapshot of the raw dataset of State Council news**

## Data cleaning
Pandas, Numpy, TextBlob, and NLTK packages were used to clean the raw texts by removing the newline characters and tab characters, removing punctuations and numbers, changing uppercase to lowercase, lemmatizing words, and removing stopwords. Besides, the original dates were re-formatted by using the Pandas’ built-in methods “to_datetime” and “dt.date”. Two new dataframes were created to store the cleaned data and then were saved  into two new CSV files.

![WhiteHouseCleaned](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/Screen%20Shot%202018-12-12%20at%2013.12.11.png)
**Fig. 7. Snapshot of  the cleaned data of White House news**


![WhiteHouseCleaned](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/Screen%20Shot%202018-12-12%20at%2013.15.38.png)
**Fig. 8. Snapshot of  the cleaned data of State Council news**

## Data exploration

### Publishing frequency

![WhiteHouseCounts](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/WhiteHouseCounts.png)
**Fig. 9. White House news publishing frequency (daily & monthly)**

![StateCouncilCounts](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/StateCouncilCounts.png)
**Fig. 10. State Council news publishing frequency (daily & monthly)**

### Term frequency in titles

**Table 1. White House & State Council top-10 most frequent terms in news titles**
![table1](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/table1.png)

![top20whitehouse](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/top20whitehouse.png)
**Fig. 11. White House top-20 most frequent terms in news titles**

![top20whitehouse](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/top20statecouncil.png)
**Fig. 12. State Council top-20 most frequent terms in news titles**

![whitehousetop100](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/whitehousetop100.png)
**Fig. 13. Word cloud of White House top-100 most frequent terms in news titles**

![statecounciltop100](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/statecounciltop100.png)
**Fig. 14. Word cloud of State Council top-100 most frequent terms in news titles**

### Topic Model

**Table 2. The six topics and top-10 associated terms within each topic of White House news (topic modeling with LSA)**
![table2](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/table2.png)

**Table 3. The six topics and top-10 associated terms within each topic of State Council news (topic modeling with LSA)**
![table3](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/table3.png)

**Table 4. The six topics and top-10 associated terms within each topic of White House news
(topic modeling with LDA using bag of words)**
![table4](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/table4.png)

**Table 5. The six topics and top-10 associated terms within each topic of State Council news
(topic modeling with LDA using bag of words)**
![table5](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/table5.png)

**Table 6. The six topics and top-10 associated terms within each topic of White House news
(topic modeling with LDA using tf-idf)**
![table6](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/table6.png)

**Table 7. The six topics and top-10 associated terms within each topic of State Council news
(topic modeling with LDA using tf-idf)**
![table7](https://raw.githubusercontent.com/pu1ssant/CityUCOM5507_2018A/master/table7.png)

## References
Ooi, S., & D’Arcangelis, G. (2017). Framing China: Discourses of othering in US news and political rhetoric. Global Media And China, 2(3-4), 269-283. doi: 10.1177/2059436418756096

Yang, Y. (2011). A Comparative Analysis of the New York Times and China Daily's 2011 News Coverage of the Chinese Government (Postgraduate). Uppsala University.

Joshi, P., & Analytics Vidhya. (2018, December 04). A Simple Introduction to Topic Modeling in Python. Retrieved from https://www.analyticsvidhya.com/blog/2018/10/stepwise-guide-topic-modeling-latent-semantic-analysis/

Li, S. (2018, May 31). Topic Modeling and Latent Dirichlet Allocation (LDA) in Python. Retrieved from https://towardsdatascience.com/topic-modeling-and-latent-dirichlet-allocation-in-python-9bf156893c24

Ryan, C. (n.d.). Topic Modelling with LSA and LDA. Retrieved from https://www.kaggle.com/rcushen/topic-modelling-with-lsa-and-lda

邵梓捷, 张小劲, & 孟天广. (2015). 政治传播视角下《新闻联播》的宣传模式分析. 清华大学学报：哲学社会科学版, (03), 30-42.
