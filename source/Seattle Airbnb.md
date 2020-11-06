### Seattle Airbnb

2020-11-07

What is Airbnb business development in Seattle ? 

Airbnb, a home sharing business model that is new and has rapidly been growing in the US and all over the world. However they are not so popular in Japan where I live. This may be due to communication capability and their shy character. Therefore, it is my interest to understand how it has been growing in the world, what is current status and are there any issues etc.

For this, I am going to use data from Seattle Airbnb Open Data to analyze real data to approach those points. 

1. What are the trends of the growth of business development and situation ?
2. What is the value of a guest ? 
3. what is the trend of the price ?
4. Why are they not popular in Japan ?

Data exploration made for 3 data sets that are related to Airbnb Seattle hosts, guests, price, reviews etc. from 2009 to 2015 and 2016. Those are from [Seattle Airbnb Open Data](https://www.kaggle.com/airbnb/seattle/data)

As a result of exploration, I foud following;

- Price fluctuation in 2016 is very stable and regular yearly, monthly and weekly. No unexpected or irregular spikes are observed. This trend is reasonably correlated to the availability. The prices of the weekend and holiday season are high and others are not. It seems to me that Seattle is a destination of tour or resort.
 
![Fig.png](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/price_ave.png)
![Fig.2](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/availability.png)

- Trend of price fluctuation from 2005 to 2015 are different from the above. For the earlier part of the period of 2005 to 2015, price fluctuations are very large and look irregular. When this trend is compared to the trend of increase in numbers of hosts, it looks that with the numbers of hosts being increased, the fluctuation of them is getting milder. In 2016, not only the fluctuation of the price is stable but also it looks to move regularly and expectedly as mentioned earlier. A hypothesis comes up in my mind that the rapid growth of Airbnb share house business in Seattle has now reached the saturation stage with the market  supply-demand has been well balanced. 

![Fig.3](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/price_2009.png)
![Fig.4](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/number_host.png)


- Those hosts categorized as 'super host’ got higher ratings and more reviews and non super hosts with less ratings also got more reviews that may be, I assume, less positive reviews.

![Fig.5](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/superhost.png)

- No license is required for Airbnb business in Seattle. It was a surprise for me. This is the one of the important issues that Airbnb has not been prevailing in Japan. I thought before analysis communication was the inhibitor though this is still true from Linear Regression that says communication is the 4th ranked aspect to get a high score. 
- Linear Regression analysis finds out what features are impacts on ratings. As a result, higher weights impacts on rating are types of property, Boat, Couch, Tent are top 3 and then followed by communication and cleanliness review score.
- This, together with the above, makes me feel that in order to grow further beyond, it may be necessary to top up those properties and also find out new special experiences in Seattle together with them. 