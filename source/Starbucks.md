# Sturbacks

## Introduction
As a part of the couse of [Udacity](https://www.udacity.com/), Data Science Nanodegree, I work on data sets which is kindly given by [Starbucks](https://www.starbucks.com/) for the purpose of this course. This page is a summary of my findingns, if interested, code is [here](https://github.com/wythe0513/Starbucks).

Data sets are based on a test marketing where 10 deffrent types of offers were presented to total 17,000 customers. They were made in 6 batches in around 6 weeks, almost once per week. In each bach, 10 different offers were given to around 15000 customers randomly through 4 different channels (e-mail, social networks, web and mobile ). It varies on customers how many and what kind of offers they received durign this test period.

Data sets is a summary of the outcome of this test. It makes up of following 3 files;

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

1. To try to understand the customers and answer the question, what is a features of customers(age, income, gender) who spend or not spend on Starbucks pruducts. 
2. To try to find answers to the question, what kind of features of customers who view offers if yes, which channels work more effectively to communicate with
customers?
3. To try to find answers to the question, did offers are attractive enough to make them action if yes what offers are effective to what customers group?

In order to find answers of those questins, I made heuristice analysis through a comparing plots and also made machine learning approches of pca analysis by sklearn processing and supervised learnings by sklearn's SVM, Rogistic Regression Adaboost and Random Forest classifiers 

## Data pre-processing

Before exploration, following data cleaning and processing were made.

- Among 17000 customers, ages 2885 customers are categolized as ‘118 years old’ and have no personal data
available. Without personal data, it is meaningless to carry on exploration, therefore, I drop those customers from this exploration.
- create 'loyalty' columns in `profile` dataframe that is a period since they become a member of Starbucks and may be considered one of the indication of a level of their loylaity to Stabusks.
- create dataframe `transaction`(customers at least made one transaction during test period) and `non_transaction`(no transaction during period). In this dataframe, personal data(age, income, gender, loyalty) and transaction data(numbers of offer received, completed and transaction) are around.
- crate dataframe 'group_1' which is a groups of customers who are ment to be influenced by the contes of offers made by Starbucks. Before they complete offers(clear the condition to get rewards), those costomes view the contents of offers. This datase was used to konw kind of feartures of customers to well respond to Starbucks's marketing approach.
- crate dataframe 'customers_notview' which is a group of customers who viewed or not viewd the offers. This datafmae was used to know which channels of communication(web, e-mail, sns and mobile) worked for them to view offers.

## Exploration

### 1. Heuristic approach

#### 1.1 Customers population distribution

Fig.1 shows a distribution of customers on age and gender. Total population is **14,825** customers(17,000 cusmtomers except '118 years old'). The numbers of male is larger than female and most populated generations are between 50s to 60s.

**Fig 1:**  Population of customers
![Fig](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/population.png)



#### 1.2 Comparison of transacted customers and non transacted customers

Comparisons were made between customers who made at leaste 1 transaction("transaction") and customers without any transaction("No transaction"). Among **14,825** customers, transaction customers are **11,986** and non transaction customers are **2,839**.

**Fig 2:**  A comparison of average incomes
![Fig.1](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/download.png)
The average income of transaction group is significantly higher than non transaction group.

**Fig 3:**  A comparison average loyalty
![Fig.2](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/loyalty.png)
The loyalty of transaction is significantly higher than non transaction group.

**Fig 4:**  A comparison of average age
![Fig.3](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/age.png)
The average age of those tow group are not different.

**Fig 5:**  A comparison of numbers on genders
![Fig.4](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/gender.png)
Group of transaction defference of gender is not so big, while non transaction group numbers of male is much larger than femail.

#### 1.3 comparison among transaction customers

**Fig 6:**  Numbers of transactions and amount spent 
![Fig.6](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/transaction_amount.png)
Generally speaking, numbers of transaction between male and female are similer and younger generatio transacted more than elder generation, while for an average amount spent, female are much higher given the numbers of female customers(smaller than male).

**Fig 7:**  Numbers of transactions and amount spent 
![Fig.7](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/completion_ratio.png)

**Fig 8:**  Numbers of transactions and amount spent 
![Fig.8](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/spent_income.png)

#### 1.4 comparison among view not view

**Fig 9:**  Offer completion on offer type

This figure is a comparison between the offer types originally given by Strbucks(left hand side table below) and completed offers on offer type(right hand side below). The completed offers include only ones that are made after offer viewed. That means that completed but do not view offers are excluded. It seems to me that the discount offers are a bit attractive to customers than BOGO(Buy One Get One) because completion to discout offers are a bit higher than originally distributed offers particulary for females in elder generation. having said that, the differeces are very subtle, and it is hard to say anything decisive through the heuristic analysis. I try to explore this by machine lernings later part if the offer type may impact on customers activities.
![Fig.9](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/offer_complete.png)

**Fig 10:**  comparison among genders of offer view or not

It looks to me that no diffrences found among genders if they view offers or not.
![Fig.10](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/offer_view.png)


#### 1.5 Summary of huristic approach

- Customers population of Male is bigger than Female in most ages.
- Plots shows that the shape of customer population distribution looks like a normal distribution. More populated ages are 40-60 and youngers are less. And also, male are more populated than female. It is a bit strange for me. In Japan, where I live, a situation is contrary vice versa, Starbucks is popular for younger and female.
- Income level for female is larger than male generally
- Trend of population between female and male are close.
- Average mounts spent for female is more than male in most of ages, while population of male is bigger than female. At the same time, numbers of offer complete hence reward gained for female are larger than male.
- As a particular trend, a group of elder generation of male spend significantly larger amount that are away from a general trend. They are maybe a very loyal customers group.
- Offer types may not make big impact on their activity althoug it is difficult to say about this from this analysis
- As a result, it is fair to say that a more favorable target with higher possibility to respond to offers are female of 30 to 50 ages with higher income.

### 2. Machine Learning approach

Three machine learning analysis were made on 2 groups,
Group that views offers
Group 1: offer completed after offer viewed
Group 2: offer completed before offer viewed
Group
#### 2.1 Principal Component Analysis)PCA

PCA, by skelern, was made on Group 1 customers to know what are the main features of this group of customers who completed offers after offfer viewed.

Results showed important aspects of this group are age, income and loyalty that explain almost half of this groups. Genders only explain 10% and female is positively related to this group. Offers are only 4%. 

**Fig.11 Results of PCA
![Fig.11](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/pca.png)

#### 2.2 Supervised Learning
On the dataset, customers with or without offer viewed, Supervised learning was made by three classifiers of sklearn, Logistic Regression, SVC and AdaBoost based on the following process.

1. Prepare data
2. Data standardized
3. Train test split
4. Grid search to identify best classifier
5. Final test

The result shows that the channels with customers are less important for customers to view offers.Rather, important features are loyallty that explains about 40% of all features followed by income and age.

**Fig.11 Results of PCA
![Fig.12](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/view_notview.png)

**Fig.12 Results of PCA
![Fig.13](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/view_notview_weight.png)


#### 2.3 Logistic Regression Analysys

On the data that contain two groups with the amount spent above 120 or less, supervised lerning by logistic regression is made. Approaces are same as the above 2.2 besides(process 4 and 5).

The outcome showed similar to ones of with or without offered viewed.

**Fig.11 Feature Importance
![Fig.14](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/logistic_regression.png)

#### 2.4 Summary 
- The results of PCA and spervised learning are reasonablly agree that level of income, age, loyalty are main features of their activities of transaction and comunication chaneels and contents of offers do not make significant impact on their action.
- Those result also agree with heuristic analysis.

## Results and Recommendations
1. Important features of their reactions are periods of length membership, level of income
and age. All three features are almost 90% of features.
2. Kinds of offers are less important for their action. They seem not to decide whether to
buy Starbucks product or not based on such advertisement. Their action to go
Starbucks seems like a part of their life work.
3. View of not view of their action seems also not linked to the channels to convey offers.
4. It is a trial how to improve current Starbucks loyal customers by a special offer and the
outcome of the trial said that discount/BOGO offer has a little impact on their activities.
5. Nevertheless, of the above 5, female customers are more responsive to male
customers and particularly, ages of 30 – 50 generation are generally high-income
clusters are most likely responded to offers especially discount rather than BOGO.
Those may be a target of commercial activities.

