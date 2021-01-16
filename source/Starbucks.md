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

1. To try to understand the customers and answer the question, what is a features of customers(age, income, gender) who spend or not spend on Starbucks pruductsl. 
2. To try too find answers to the question, what kind of features of customers who view offers if yes, which channels work more effectively to communicate with
customers?
3. To try to find answers to the question, did offers are attractive enough to make them action if yes what offers are effective to what customers group?

In order to find answers of those questins, I made heuristice analysis through a comparing plots and also made machine learning approches of pca analysis by sklearn processing and supervised learnings by sklearn's SVM, Rogistic Regression Adaboost and Random Forest classifiers 

## Data pre-processing

Before exploration, following data cleaning and processing were made.

- Among 17000 customers, ages of 2885 customers are categolized as ‘118 years old’ and have no personal data
available. Without personal data, it is meaningless to carry on exploration, therefore, I drop those customers from this exploration.
- create 'loyalty' columns in `profile` dataframe that is a period since they become a member of Starbucks and may be considered one of the indication of a level of their loylaity to Stabusks.
- create dataframe `transaction`(customers at least made one transaction during test period) and `non_transaction`(no transaction during period). In this dataframe, personal data(age, income, gender, loyalty) and transaction data(numbers of offer received, completed and transaction) are around.
- crate dataframe 'group_1' which is a groups of customers who are ment to be influenced by the contes of offers made by Starbucks. Before they complete offers(clear the condition to get rewards), those costomes view the contents of offers. This datase was used to konw kind of feartures of customers to well respond to Starbucks's marketing approach.
- crate dataframe 'customers_notview' which is a group of customers who viewed or not viewd the offers. This datafmae was used to know which channels of communication(web, e-mail, sns and mobile) worked for them to view offers.

## Exploration and Findings

#### Heuristic approach

Following fig, is a distribution of the population of customers of this group.
![Fig.1](https://github.com/wythe0513/Starbucks/blob/master/amout_age_gender_px.html)
![Fig.2](https://github.com/wythe0513/boilerplate/blob/master/source/download.png)

The general trend is as follows.

- Population of Male is bigger than Female in most ages.
- Plots shows that the shape of customer population distribution looks like a normal distribution. More populated ages are 40-60 and youngers are less. And also, male are more populated than female. It is a bit strange for me. In Japan, where I live, a situation is contrary vice versa, Starbucks is popular for younger and female.
- Income level for female is larger than male generally
- Trend of population between female and male are close.
- Average mounts spent for female is more than male in most of ages, while population of male is bigger than female. At the same time, numbers of offer complete hence reward gained for female are larger than male.
- As a particular trend, a group of elder generation of male spend significantly larger amount that are away from a general trend. They are maybe a very loyal customers group.
- As a result, it is efficient to send offers to female of 30 to 50 ages with high income.

#### Machine Learning analysis
I made three machine learning analysis made on 2 groups,
Group that views offers
Group 1: offer completed after offer viewed
Group 2: offer completed before offer viewed
Group
 Principal Component Analysis by skid-learn on Group 1;
To identify what are the principal component for this group
 Logistic analysis by classifiers (logistic regression, ad boost and support vector
machine) on Group.
To train classifiers and test the result of the trains by a following process;
1. Prepare data
2. Data standardized

3. Train test split
4. Grid search to identify best classifier
5. Final test
 Logistic analysis by random forest
Same approach made besides 4 and 5
The above different approaches showed reasonably similar outcome as follows.
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

## Acknoledgements