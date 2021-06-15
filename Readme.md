# Time-Series Forecasting Ethereum Prices

## Goal

The goal of this project is to create a model that predicts prices that allow for successful day-trading. I want to make sure that the model predicts one day ahead, and that the culmination of all of these predictions follows the general trend of the actual prices, in order to allow day traders to make proper predictions to maximize profit or minimize loss.

## Data Source

This data was scraped from CoinMarketCap.com using the webscraper Octoparse. The webpages used ajax syntax for the "load page" button, and therfore ajax timeout time needed to be applied in order to properly extract the data. This data is only concerned with Ethereum, and no other coin or blockchain.

## Features

The data includes the following features:

1. Open
2. High
3. Low
4. Close 
5. Volume
6. Market Cap
<br>

This dataset provides a timeline of Ethereum prices and related data from August 7th, 2015 to June 8th, 2021.
<br>


## Visualizations

![image info](Visuals/Histograms.png)
- These histograms exemplify the volatility of the asset. The large majority of prices fall 0 and 1000, however there are low-frequency instances of prices that are 2, 3, and 4 times the max value of that range. This shows that the price spiked and fell, never maintaining a high value for very long at all. 

![image info](Visuals/acf_plots.png)
- Using the ACF and PACF plots shown above, we can conclude that an optimal value for p for an ARIMA model would be 1, and an optimal value for q for an ARIMA model would be 1 as well. You can identify this by the exaggreated correlation at the corresponding lag values

## EDA 

 - The rolling averages calculated from three different windows (30, 90, 365) provide some more insight to the data. As the window increases in size, the rolling averages' values have very different values during the highly volatile periods of the price of Ethereum. This volatility resulted in each of these periods having wildly different minimum and maximum values, which results in rolling averages that also different by quite a lot. Unsurprisingly, the 30-day and 90-day rolling averages were the most closely related, especially during the first period of steep upwards trend. The prices did not reach magnitude differences during these windows that warranted such a drastic rolling average difference. However, at the end of our time period, the rolling averages end up differing in value by almost $500, which goes to show the extreme volatility that Ethereum experienced during this time period (the most recent months when Ethereum had a meteoric rise). In short summary, the 365-day moving average had the lowest average value because it generalized the most volatility, however its final value was very below the true price. The 30-day moving average had the highest value because it strongly accounted for the high volatility, and its final value was a little higher than the true price (the extreme upper values pulled the average upwards). The 90-day moving average was the closest to the true price, showing that it both accounted for and generalized the volatility the best of the three windows!
<br>

<br>

- From the year 2015 to the first quarter of 2017, the price of Ethereum remained quite stationary, with a very strong rise starting between March and April, which led to a strong upwards trend that lasted throughout the rest of the year of 2017, bring the price to a maximum value of $826.82 by the end of the year. This constituted a 10,106% price increase from the minimum price of $8.17 in the year of 2017, which is by all standards a very strong upwards trend. The volume of trades also followed this trend quite closely, matching the sentiment idea that as an asset shoots up in price, more people attempt to join in on the ride, and hence more trades are made. After the year 2017, the price of Ethereum immediately started a strong downwards trend beginning in January of 2018, and by the end of 2018 the price had settled to a minimum value of $84.30, roughly a 94% drop from its all time high at the very beginning of 2018. Volume for the rest of 2018 remained on average higher than the two years afterwards and the year before because at first people were participating in frequent trades due to the meteoric rise in price, and then people continued to sell their coins over the year as the price tanked. From 2019 to mid-2020, the price once again mostly resumed the stationary trend that it had exemplified from 2015 to about a quarter of the way through 2017, indicating that perhaps people lost interest in the Ethereum block-chain, doubted its potential, or simply moved on to different investments. There was a sharp rise in prise to a little over $250 during 2019, but it just as quickly fell back to close to the minimum value of that year, failing to breakout of its strong downwards trend. The volume from 2019 to mid-2020 would never drop to the levels seen before the coin's meteoric rise, most likely because such a note-worthy event put Ethereum on the map permanently. During 2019, there was a sharp rise and fall in volume that mirrored the trend of the quick rise and fall of price during that year. 2019-2021 would be the period of time when Ethereum would consistently reflect a yearly upwards trend. Volume was higher than its ever been, and the price rose to an unprecedented level of roughly $4000. During this upwards trend, there were several downwards trends that occured during certain months of the years. They seemed to be relatively random, with no predictability in their occurences, highlighting the unstationarity of the price of Ethereum, and also the idea that the price follows a "cyclical trend". There are very clear bull and bear markets, however the trickly part is timing these.  

## Modeling

### Random-Walk
<br>
- The random-walk model performed interestingly. It followed a relatively similar trend as the actual forecast, however the values it provided were EXTREMELY exaggerated. When I run this code multiple times, the forecast can change dramatically. It is best to look into more comprehensive models to try and forecast this data.

#### Metric and Evaluation
<br>
- The RMSE value for the Random-Walk model varied with each run of the code, however the best value that was achieved was:

### ARIMA, SARIMAX, and One-Step-Ahead
<br>

- The stationarity of the data was tested using an Augmented Dickey-Fuller Test. Before differencing, the ADF-Test gave a result that indicated that the data was strongly non-stationary. After first-order differencing, the ADF-Test gave a result that indicated the data was now stationary. 
<br>

- The One-Step-Ahead Forecast was very accurate! It makes sense, considering the prices of Ethereum strongly follow daily trends.
<br>

- Using the calculated ACF and PACF graphs, along with the ADF-Test, the p, d, and q values of 1, 1, 1 were maually selected for the ARIMA model beore using AutoArima for automatic optimazation. The ARIMA model performed poorly for the data provided. This can almost certainly be attributed to the exaggerated volatility of Ethereum prices. The period of time that ARIMA was trained on showed an interesting trend. The price remained low, then spiked to a value that was much higher than before, and just as quickly fell down to a very low value again and remained there for quite some time. In other words, it was relatively stationary, then had a steep upwards trend, a steep downwards trend, and then remained relatively stationary again. The two main determinants of ARIMA predicitons, past values and moving average, are very hard to predict upon because thei values vary by so much. Even when using the AutoArima package to optimize the values of p,d, and q, the model did not improve. The same results occured with the SARIMAX model as well, an expected outcome due to the fact that the SARIMAX tries to include seasonality in its calculations, and this data shows no seasonality.
<br>

### Metric and Evaluation
<br>

The RMSE values for the ARIMA and SARIMAX models were very poor, howing high error margins and therefore indicating that they were poor models. The One-Step-Ahead model's RMSE score was great, indicating that it was quite an excellent model.
<br>

### LSTM
<br>

- For the LSTM models, I used a total of 25 epochs, allowing the MSE score to converge towards a value towards the final epochs. At first, a single LSTM layer was used, however the results were very poor, and so I opted to add in two extra LSTM layers to try and improve the results. The loss metric use was Mean-Squared-Error, and the optimizer was the 'adam' optimizer. The addition of the two LSTM layers aided in the model's predictions, and created a model that more closely followed the trend of the actual prices.
<br>

- The model predicted against the test data is shown below:
![image info](Visuals/lstmforecast.png)
<br>

- Red Line: Predictions
- Blue Line: Actual Prices
<br>

#### Metric and Evaluation
- The RMSE score for the LSTM model was the lowest of all of the models that came before, indicating that it would the most accurate. 

- Out of all of the models, the LSTM model performed the best by far, following the general trend of the actual prices, but it still undervalues the price of the asset by a significant amount. This can most certainly be considered a flawed model due to this, however overall, if one were to use this model to attempt to make day trades, they still could have succesfully maximized profit or minimized loss due to the model following the actual trend. The model predicted rises and falls of the data rather well, and simply just undershot the true value. Since profit is based on percentage rise and percetage fall, following the model would have resulted in good investment decisions, and in fact the reality of the situation would have resulted in a much better results than the model would have predicted, due to the extreme values that Etheruem rose to during the time that the model made its predictions.
<br>

## Conclusion
- Ethereum is an extremely volatile asset with a lack of seasonality. Due to these features, the most effective model for properly predicting the trend of the actual prices most closely in order to ensure profit is the LSTM model. Using this model, a decision on whether to sell, hold, or buy would have been properly made at almost any period of time, due to the trend of the actual data being very closely predicted by the LSTM model, even though the model itself undershot the true values of the Ethereum asset. 
<br>
- Further research of exogenous variables and the addition of those variables into the training of the model could greatly improve the performance, since it is common that Ethereum prices follow the price changes of other assets such as Bitcoin, and follow the overall market trend as well. Furthermore, an exogenous variable found in the same DataFrame such as volume could also be used to attempt to increase model accuracy. 