# Sturbacks

## Introduction
As a part of the course of [Udacity](https://www.udacity.com/), Data Science Nanodegree, I work on data sets which are kindly given by [Starbucks](https://www.starbucks.com/) for the purpose of this course. This page is a summary of my findings, if interested, code is [here](https://github.com/wythe0513/Starbucks).

Data sets are based on a test marketing where 10 different types of offers were presented to total 17,000 customers. They were made in 6 batches in around 6 weeks, almost once per week. In each batch, 10 different offers were given to around 15,000 customers randomly through 4 different channels (e-mail, social networks, web and mobile ). It varies on customers how many and what kind of offers they received during this test period.

There are 10 kinds of offers categorized by 3 types. The discount offers are for customers who get some  rewards if  they complete the condition of those offers. BOGO(Buy One Get One) is an offer for customers to get one when they complete the condition. And the third one is just information without any benefit for customers.

Data sets provided is a summary of the results of this test. It makes up of following 3 files;

1. `profile.json`(Rewards program users (17000 users x 5 fields))
- gender: (categorical) M, F, O, or null
- age: (numeric) missing value encoded as 118
- id: (string/hash)
- became_member_on: (date) format YYYYMMDD
- income: (numeric)

2. `portfolio.json`(Offers sent during 30-day test period (10 offers x 6 fields))
- reward: (numeric) money awarded for the amount spent
- channels: (list) web, email, mobile, social
- difficulty: (numeric) money required to be spent to receive reward
- duration: (numeric) time for offer to be open, in days
- offer_type: (string) bogo, discount, informational
- id: (string/hash)

3. `transcript.json`(Event log (306648 events x 4 fields))
- person: (string/hash)
- event: (string) offer received, offer viewed, transaction, offer completed
- value: (dictionary) different values depending on event type
- offer id: (string/hash) not associated with any "transaction"
- amount: (numeric) money spent in "transaction"
- reward: (numeric) money gained from "offer completed"
- time: (numeric) hours after start of test


## Purposes and approaches

The purpose of my exploration on those data is as follow;

1. To try to understand the customers and answer the question, what are the features of customers(age, income, genders etc.) who spend or not spend on Starbucks products during the test period. 
2. To try to find answers to the question, what kind of features of customers who view offers, if yes, which channels work more effectively to communicate with customers?
3. To try to find answers to the question, did offers are attractive enough to make them action, if yes, what offers are effective to what customers group?

In order to find answers of those questions, I made heuristic analysis through a comparing plots as well as machine learning analysis such as PCA(Principal Component Analysis) and supervised learnings by sklearn's SVM, Adaboost, Random Forest and also Logistic Regression classifiers.

## Data Pre-processing

Before the exploration, following data cleaning and processing were made.

- Among 17000 customers, 2,885 customers group is categorized as ‘118 years old’ without  any personal data available. This is considered a missing or unknown type of group and , without personal data, it is meaningless to carry on exploration, therefore, I drop those customers from this exploration.
-  'loyalty' column is created in `profile` Data Frame that is a period since they become a member of Starbucks and can be considered as one of the indications of a level of their loyalty to Starbucks.
- Data Frame, `transaction`(customers at least one transaction made during test period) and `non_transaction`(customers with no transaction made during period), are created. In those Data Frames, personal data(age, income, gender, loyalty) and transaction data(numbers of offers received and  completed,  and also numbers of transactions made) are contained. That makes me compare those groups demographically.
-Data Frame, 'group_view',  which is a group of customers who viewed offers, is created. And from ‘group_view’, Data Frame, 'group_1',  which is a group of customers who are meant to be influenced by the contes of offers, is created. Before they complete offers(meet the condition to get rewards), those costumes viewed the contents of offers. This Data Frame was used to know if any particular features are there to those who view and respond to Starbucks's marketing approach.
- Data Frame, 'group_notview',  which is a group of customers who viewed and not_viewed , is created. 
- Data Frame,  'customers_notview',  which is a group of customers whether they viewed offers or not, is created. This Data Frame was used to see if the channels of communication(web, e-mail, sns and mobile) makes difference  for them to view offers.
- Data Frame,  'customers_lreg',  which is a group of customers who spent above 120 or less, and who transacted above 8 times or less, is created. for Logistic Regression supervised learning analysis

## Exploration

### 1. Heuristic approach

#### 1.1 Customers population distribution

Fig.1 shows a distribution of customers on age and gender. Total population is **14,825** customers(17,000 customers except '118 years old'). The number of male is larger than females and the most populated generation zone is between 50s to 60s.

**Fig 1:**  Population of customers
![Fig](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/population.png)



#### 1.2 Comparison of transacted customers and non transacted customers

Comparisons were made between customers who made at least 1 transaction("transaction") and customers without any transaction("No transaction"). Among **14,825** customers, transaction customers are **11,986** and non transaction customers are **2,839**.

**Fig 2:**  A comparison of average incomes
![Fig.1](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/download_1.png)
The average income of the transaction group is significantly higher than the non-transaction group.

**Fig 3:**  A comparison average loyalty
![Fig.2](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/loyalty.png)
The loyalty of the transaction group is significantly higher than the non-transaction group.

**Fig 4:**  A comparison of average age
![Fig.3](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/age.png)
The average age of those two groups are not different.

**Fig 5:**  A comparison of numbers on genders
![Fig.4](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/gender.png)
The number of male and female in the transaction group is not so different, while in the non-transaction group, the number of male is much larger than female.

#### 1.3 comparison among transaction customers

**Fig 6:**  Numbers of transactions and amount spent 
![Fig.6](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/transaction_amount.png)
Generally speaking, the number of transactions between male and female are similar and the younger generation transacted more than the older generation, while for an average amount spent, females are much higher given the numbers of female customers(smaller than male).

**Fig 7:**  Numbers of transactions and amount spent 
![Fig.7](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/completion_ratio.png)

**Fig 8:**  Numbers of transactions and amount spent 
![Fig.8](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/spent_income.png)

#### 1.4 comparison among view not view

**Fig 9:**  Offer completion on offer type

This figure is a comparison between the offer types originally given by Strbucks(left hand side table below) and completed offers on offer type(right hand side below). The completed offers include only ones that are made after offer viewed.i.e.  completed but do not view offers are excluded. It seems to me that the discount offers are a bit more attractive to customers than BOGO(Buy One Get One) because completion to discount offers are a bit higher than originally distributed offers particularly for females in the older generation. Having said that, the differences are very subtle, and it is hard to say anything decisive through the heuristic analysis. I try to explore this by machine learnings later part if the offer type may impact on customers activities.
![Fig.9](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/offer_complete.png)

**Fig 10:**  comparison among genders of offer view or not

It looks to me that no differences are found among genders if they view offers or not.
![Fig.10](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/offer_view.png)


#### 1.5 Summary

- The customer population of male is larger than females throughout generation.
- Those series of figures show that the shape of customer population distribution looks like a normal distribution. More populated ages are 40-60 and younger people are less. Also, male are more populated than females. Very personal impression, It is a bit strange for me. In Japan, where I live, the situation is the opposite, Starbucks is popular for younger(students) and female people. It is not so popular for 50s man to have a cafe in Starbucks.
- The average income level for females is larger than male generally.
- The distribution trend of the customer population between female and male are close.
- The average amount spent for females is more than male in most ages, while the population of male is bigger than female. At the same time, numbers of offer complete hence reward gained for female are larger than male.
- As a particular trend, a group of the older generation(90s) of male spend significantly larger amounts that is deviated from a general trend. They are maybe very loyal customers.
- Offers of any kind may not make a big impact on their action to buy although it is difficult to say about this from those data.
- In summary, it is fair to say that a more favorable target with higher possibility to respond to offers are females of 30s to 50s ages with higher incomes. 

### 2. Machine Learning approach

Three machine learning analysis were made on 2 groups,
Group that views offers
Group 1: offer completed after offer viewed
Group 2: offer completed before offer viewed
Group
#### 2.1 Principal Component Analysis(PCA)

PCA, by sklearn, was made on group_1 customers to know what are the main features of this group.

Results are shown in the following figures. They say that the  important aspects of this group are age, income and loyalty that explain almost half of the features of all components. The genders only explain 10% and type of offers  only 4% of them.

**Fig.11** Results of PCA
![Fig.11](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/pca.png)

#### 2.2 Supervised Learning
On the dataset, ‘group_notview’(customers with offers viewed or not viewed), supervised learning analysis was made by three classifiers of sklearn, Logistic Regression, SVC and AdaBoost based on the following process.

1. Prepare data
2. Data standardized
3. Train test split
4. Grid search to identify best classifier
5. Final test

The result shows that the channels with customers are less important for customers to view offers.Rather, important features are loyalty that explains about 40% of all features followed by income and age.

**Fig.12** 
![Fig.12](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/view_notview.png)

**Fig.13** 
![Fig.13](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/view_notview_weight.png)


#### 2.3 Logistic Regression Analysis

On the Data Frame, ‘customers_lreg’, supervised learning analysis  by logistic regression is made. Approaches are the same as the above 2.2 besides(process 4 and 5).

The outcome showed similar to ones of with or without offered views.

**Fig.14** Feature Importance
![Fig.14](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/logistic_regression.png)

#### 2.4 Summary 
- The results of PCA and supervised learning are reasonably agreed that level of income, age, loyalty are main features of their activities of transaction and communication channels and contents of offers do not make a significant impact on their action.
- Those results also agree with the outcomes of the heuristic analysis.

## Results and Recommendations
1. Important features of their buying activity is loyalty( a period of length membership), level of income and age. Those three occupy  almost 90% of all features.
2. Any kinds of offers make less impact on their action to buy. 
3. The view of their action seems also not linked to the channels to convey offers.
4. The female customers are more responsive to male customers and particularly, ages of 30 – 50 generation are generally high-income clusters are most likely responded to offers especially discount rather than BOGO. 

