### Seattle Airbnb

2020-11-6

Data exploration made for 3 data sets that are related to Airbnb Seattle hosts, guests, price, reviews etc. from 2009 to 2015 and 2016. Those are from [Seattle Airbnb Open Data](https://www.kaggle.com/airbnb/seattle/data)


Price fluctuation in 2016 is very stable and regular yearly, monthly and weekly. No unexpected or irregular spikes are observed. This trend is reasonably correlated to the availability. The prices of the weekend and holiday season are high and others are not. It seems to me that Seattle is a destination of tour or resort. See the following plots. 

![Fig.png](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/price_ave.png)
![Fig.2](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/availability.png)

Trend of price fluctuation from 2005 to 2015 are different from the above. For the earlier part of the period of 2005 to 2015, price fluctuations are very large and look irregular. When this trend is compared to the trend of increase in numbers of hosts, it looks that with the numbers of hosts being increased, the fluctuation of them is getting milder. In 2016, not only the fluctuation of the price is stable but also it looks to move regularly and expectedly as mentioned earlier. A hypothesis comes up in my mind that the rapid growth of Airbnb share house business in Seattle has now reached the saturation stage with the market  supply-demand has been well balanced. See following 2 plots.

![Fig.3](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/price_2009.png)
![Fig.4](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/number_host.png)


Those hosts categorized as 'super host’ got higher ratings and more reviews and non super hosts with less ratings also got more reviews that may be, I assume, less positive reviews.

![Fig.5](https://raw.githubusercontent.com/wythe0513/boilerplate/master/source/superhost.png)


I made Linear Regression analysis to find out what features are impacts on ratings. As a result, higher weights impacts on rating are types of property, Boat, Couch, Tent are top 3 and then followed by communication and cleanliness review score.

This, together with the above, makes me feel that in order to grow further beyond, it may be necessary to top up those properties and also find out new special experiences in Seattle together with them. 
