# Sturbacks

## Introduction
As a part of the course of [Udacity](https://www.udacity.com/), Data Science Nanodegree, I work on data sets which are kindly given by [Starbucks](https://www.starbucks.com/) for the purpose of this course. This page is a summary of my findings and recommendations, if interested, code is [here](https://github.com/wythe0513/Starbucks).

I understand that data sets are created base on results of test marketing where 10 different types of offers were presented to total 17,000 customers. Those offers were made in 6 batches(start(0 hours), 168 hours(7days), 336 hours(14 days), 408 hours(17 days),  504 hours(21 days) and 576 hours(24 days) past since start). In each batch, 10 different offers were given to around 12,000 customers randomly through 4 different combination of several channels (e-mail, social networks, web and mobile ). It varies on customers how many and what kind of offers they received during this test period.

There are 10 kinds of offers categorized by 3 types. The discount offers are for customers who get some  rewards if  they complete the condition of those offers in time. BOGO(Buy One Get One) is an offer for customers to get one when they complete the condition. And the third one is just information without any benefit for customers.

Data sets makes up of following 3 files;

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

- Among 17000 customers, 2,885 customers group is categorized as ‘118 years old’ without  any demografic data available. This is considered a missing or unknown type of group and , without demografic data, it is meaningless to carry on exploration, therefore, I drop those customers from this exploration.
-  `loyalty` column is created in `profile` Data Frame that is a period since they become a member of Starbucks and can be considered as one of the indications of a level of their loyalty to Starbucks.
- Data Frame, `customers`(customers at least one transaction made during test period) and `non_transaction`(customers with no transaction made during period) are created. In those Data Frames, personal data(age, income, gender, loyalty) and transaction data(numbers of offers received and  completed,  and also numbers of transactions made) are contained. That makes me compare those groups demographically.
-Data Frame, `group_view`,  which is a group of customers who viewed offers, is created. And from `group_view`, Data Frame, `group_1`,  which is a group of customers who are meant to be influenced by the contes of offers, is created. Before they complete offers(meet the condition to get rewards), those costumes viewed the contents of offers. This Data Frame was used to make PCA analysis to see if any particular features are there to those who view and respond to Starbucks's marketing approach.
- Data Frame, `group_notview`,  which is a group of customers who viewed and not_viewed , is created. This data Frame is used for Supervised Lerning approach to see waht is main feature for customers to view or not view
- Data Frame,  from `customers`, Data Frame`customers_lreg`,  which is a group of customers who spent above 112 or less, and who transacted above 8 times or less, is created. for Logistic Regression supervised learning analysis

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

**Fig 5:**  A comparison of ratio on genders
![Fig.4](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/gender.png)
The ratio of male and femal in the transaction group is not so different from ratio of all customers, while in the non-transaction group, the ration of male is much larger than female.

#### 1.3 comparison among transaction customers

**Fig 6:**  Numbers of transactions and amount spent 
![Fig.6](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/transaction_amount.png)
Generally speaking, the number of transactions between male and female are similar and the younger generation transacted more than the older generation, while for an average amount spent, females are much higher.

**Fig 7:**  The average number of offer completion and offer completion/received ratio 
![Fig.7](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/completion_ratio.png)

Both the number of offer completion and ratio(completion out of receipt), female are higher than male. Female are more care to offers than men. This trens is consistant thout the whole generation. 

**Fig 8:**  Average amounts spent and average income 
![Fig.8](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/spent_income.png)

Aberage amount spent is positively correlated to age and income.Female is higher on both of them in most part of the generation.

#### 1.4 comparison among view not view

**Fig 9:**  Distribution of Offer type and comparison of  completion on offer types

![Fig.9](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/offer_complete.png)


The completed offers include only ones that are made after offer viewed.i.e.  completed but do not view offers are excluded. It seems to me that the discount offers are more attractive to customers than BOGO(Buy One Get One) because completion to discount offers are a bit higher than originally distributed offers particularly for females in the older generation. Having said that, the differences are very subtle, and it is hard to say anything decisive through the heuristic analysis. I try to explore this by machine learnings later part if the offer type may impact on customers activities.

**Fig 10:**  comparison among genders of offer view or not

It looks to me that no differences are found among genders if they view offers or not.
![Fig.10](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/offer_view.png)

**Fig 11:**  comparison customers and customers with less interest in offers

Customers with less interest in offers are the customers who completed less than 40% of offers received, that is lower 25% boundary of customers of at least one transaction. Those are significantly different. The number of male are much larger than females.

![Fig.11](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/offers_less_interest.png)




#### 1.5 Summary

- The customer population of male is larger than females throughout generation.
- More populated ages are 40s-60s and younger people are less. Also, male are more populated than females. It is intersting to me, in Japan where I live, the situation is the opposite, Starbucks is popular for younger(students) and female people. It is not so popular for 50s man to have a cafe latte in Starbucks in Japan
- The average income level for females is larger than male generally.
- The average amount spent for females is more than male in most ages, while the population of male is bigger than female. At the same time, numbers of offers  completed hence reward gained for females are larger than male.
- As a particular trend, a group of the older generation(90s) of male spend significantly larger amounts that is deviated from a general trend. They are maybe very loyal customers.
- Offers of any kind may not make a big impact on their action to buy although it is difficult to say about this from those data.
- The customers with less interest in offers(less than 40% of offers completed to the offer received((lower 25% boundary of total customers who made transaction))  has a particular feature that is the significant larger number of male compare to that of total customers. 
- It can be said that a more favorable target with higher possibility to respond to offers are females of 30s to 50s ages with higher incomes. 

### 2. Machine Learning approach

Three machine learning analysis were made in this section.

#### 2.1 Principal Component Analysis(PCA)

PCA, by scikit-learn, was made on group_1 customers for the purpose to know what are the main features of this group, who completed offers after they viewed offers

Results are shown in the following figure. It saids that the  important features of this group are `age`, `income` and `loyalty` that explain almost half of the features of all components. The `genders` only explain 10% and `type of offers`  only 4% of them.


**Fig.12** Results of PCA
![Fig.12](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/pca.png)

#### 2.2 Supervised Learning(classify customers with/without offer viewed) 
On the dataset, ‘group_notview’(customers with offers viewed or not viewed), supervised learning analysis by the scikit-learn was made to see how accurately can trained classifiers classify customers who viewed offers or not viewed, then, to find out what are the most predictive features of this classifier to decide. 

I started with three classifiers  Logistic Regression, SVC and AdaBoost as naive predictors to see the benchmarks of results, i.e. accuracy and f score. Those 3 models applied to 1%, 10% and 100% of training data, namely `samples_1`(532 samples), `samples_10`(5320 samples) and `samples_100’ (53200 samples)respectively and compare those of them.

Fig.13 plots the result of those analyses. It shows that the three classifiers' performances are not so different. The accuracy and f score are above 80% but,  time spent for processing by SVC is significantly longer than the two. 

Based on this, though the difference is subtle, I decided to choose AdaBoost as the best classifier. Its result is as follows;

Accuracy score on testing data: 0.8166
f-score on testing data: 0.8633

Then, by `GridSearchCV`, Fine tuned the parameters of this classifier that is as follows;

base estimator : Decision Tree Classifier(max_depth =6)
learning rate : 0.5

The result by the best classifier is as follows; 
Final accuracy score on the testing data: 0.8157
Final f-score on the testing data: 0.8716

**Fig.13** 
![Fig.13](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/view_notview.png)

By the best classifier chosen above, features importances are extracted as shown following Fig.14. The `loyalty`,` income` and `age` are almost the same, around 30% each. Those three occupy cumulatively more than 90% of features. That means that the communication channels(web, e-mail.etc.) are much less important features for customers' action for them to view or not.


**Fig.14** 
![Fig.14](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/view_notview_weight.png)


#### 2.3 Logistic Regression Analysis

On the Data Frame, ‘customers_lreg’, supervised learning analysis by logistic regression is made. The purpose of this analysis is to find how important the kinds of offers are for customers to transact and spend money. First ‘customers_lreg’ DataFrame is created where customers classify two groups one is for customers to spend more and transact more, and the other is the other way around. Boundary of two is 50% of distribution. 

The  process of data split, train and test, and analysis are the same as the above 2.2, but GridSearch and fine tuning of parameters are not made here because it seems that this process does not make a significant impact on performances on these data sets. 

The outcome showed in the following Fig. 15. Similar to 2.2, important features are  `loyalty`, `age` and `income`, and the kinds of offers are less important for classifiers' decision.

**Fig.15** Feature Importance
![Fig.15](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/logistic_regression.png)

#### 2.4 Summary 
- The results of PCA and supervised learning are reasonably agreed that level of `income`, `age`, `loyalty` are main features of their activities of transaction, and communication channels and contents of offers do not make a significant impact on their action.
- Those results also agree with the outcomes of the heuristic analysis.

## Results and Recommendations
1. Information about demographic properties is very Important to predict customers' actions(react to offers, how much they spend etc.),  and also expect potential customers' reactions. Those are  `loyalty`( a period of length membership), level of `income`, `age` and `gender` are important.
2. The females are more responsive to offers than male customers. Female eran amd spend than mele. And most populated generation zone is aroud 50s.
3. Therefore, females particularly the 30s – 50s generation with higher income can be mainly targeted to approach as potential loyal customers.
4. When making an approach, the offers presented, the discount offers seem better than BOGO, though the difference is subtle. 

