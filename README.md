# Preface
The analysis in that repository is conducted on mock dataset from VieON. Key objective is to practice insights discovery through data while upgrading data analytics skill on time serries dataset. Techniques that are employed on that project are Descriptive analysis (trends analysis in particular) and Deep Learning to forecast on time series data
# Executive summary of the project
The VIP VieON package has seen significant and ongoing fluctuations in subscription numbers over the past two years (from April 2021 to January 2023), making it challenging to predict viewer trends. This has created a need to forecast subscription trends for VIP VieON. Developing a forecasting model that considers external factors influencing accuracy can help VieON better plan its business strategies and marketing efforts for the next three months.
# Trend analysis
## Overall trends of subscription
![image](https://github.com/user-attachments/assets/4721b8eb-798d-480d-a6d4-d7f1505adff7)
<br> _Plot showing the overall trends of subscription_
<br> The number of subs is fluctuating, with peaks in June 2021, August 2021, and January 2023. There is a trigger point in June 2022. The trend from August 2021 to February 2022 is a notable decline. It's necessary to consider what happened during this period.
<br><br> Using **Exponential Moving Average (EMA)** to clearly identify the trend
<br> ![image](https://github.com/user-attachments/assets/5ce582f3-00ac-4fc1-b371-e9f198afb963)
<br> _Plot of Exponential Moving Average (EMA) about subscription_
<br><br> Based on observation, the number of subscribers shows a strong increasing trend from April 2021 to November 2021, followed by a decreasing trend until June 2022. From June 2022 onwards, the trend shifts to an increasing trend.
## Time series decomposition
### Seasonality
Use the **Autocorrelation Function (ACF)** to identify the Seasonality of datasets about subs over time
<br> ![image](https://github.com/user-attachments/assets/6def637f-b5db-497e-a334-e6067814d3f6)
<br><br> The x-axis represents the lag and the y-axis represents the correlation coefficient
<br><br> Since the time series being observed belongs to an entertainment media website, I hypothesizes that there may be a regular pattern by month or by quarter.
<br><br> However, based on the observation of the results, there is **no cycle appearing within a year (i.e., no cycle by month or by quarter)**
### Stationary
> The dataset is Non-stationary

Using the **Augmented Dickey-Fuller (ADF) test** to verify the stationarity of the dataset.
<br><br> Results of ADF test:
- ADF Statistic: -2.73418
- p-value: 0.06830
- Critical Values:
  + 1%: -3.7697
  + 5%: -3.0054
  + 10%: -2.6425

![image](https://github.com/user-attachments/assets/f6ab4472-edca-40dd-9763-2f2112f81000)
<br> _Decomposing the time series into seasonal and trend components_
## Correlation of Show and Subs variables
![image](https://github.com/user-attachments/assets/9699f447-3c85-4163-8b1e-7f9580c89df4)
<br><br> Based on observation, there are many shows and TV series that appear in the months with significantly higher subscription numbers compared to other shows. This suggests the idea of further examining the characteristics and creating features to analyze the relationship between shows and subscriptions
<br> ![image](https://github.com/user-attachments/assets/bd7fcf7c-9b89-4871-a23b-16833cb29a19)
<br> ![image](https://github.com/user-attachments/assets/a7d7b0b7-2442-4858-bb42-d15ed3631ce9)
<br> _Correlation between types of shows and subscription_
<br><br> From the above analysis, it can be seen that broadcasting many shows at the same time can create a trigger point and an increasing trend in the number of subscriptions, meaning that showing many shows will stimulate viewers to subscribe to the VIP VieON package.
<br><br> At the same time, the quality of the shows will create peaks in the number of subscriptions, meaning that the presence of popular shows will help the number of subscriptions reach its highest point during that time period.
# Deep learning for forecasting
> Due to the non-stationarity of the dataset, Deep Learning is relevant techniques to forecast subsciption for the next 3 months
## Modeling pipeline
There are 3 phases in the modelling pipeline:
- **Phase 1:** Create a training dataset to build forecasting models and a hold-out dataset to evaluate model performance
- **Phase 2:** Build **Deep Learning models** for **univariate** and **multivariate** time series forecasting based on the training dataset and evaluate them using the **hold-out dataset**
- **Phase 3:** Select the model with better forecasting performance to predict the number of subscribers for the next 3 months of the dataset.
### Data preparation
![image](https://github.com/user-attachments/assets/e2ac7444-185a-4918-8272-7ae5c9ee8e2d)
<br> _Subs by date_
<br><br>

- Training: first 600 days (04/2021 - 11/2022)
- Hold-out: last 85 days (11/2022 - 02/2023)
- Forecasting period: 02/2023 - 06/2023

## MLP Univariate 
![image](https://github.com/user-attachments/assets/124841d1-11f9-4d84-afb4-079fec35ba1a)
- Period of time: 25 days
- RMSE: 1942.680
- MAE: 1797.3298

## MLP Multivariables
### Feature selection
Applying CART Regression algorithm to evaluate FIS (feature importance score), which is criteria for features selection
![image](https://github.com/user-attachments/assets/70f659e6-a042-4ef1-8ad4-32413bf6d6eb)
<br><br> Based on the results, four variables are selected with FIS greater than 0.1 to add to the multivariate forecasting model. These features, 0, 1, 4, and 5, correspond to comedy, drama, talkshow, and gameshow, respectively
![image](https://github.com/user-attachments/assets/592b02b9-a0d7-4c34-92ff-b6a3a89d7746)
- Period of time: 15 days
- RMSE: 1074.416
- MAE: 904.549
### Forecasting
![image](https://github.com/user-attachments/assets/670a3780-9e5f-4889-9514-dfa5f82ab4cd)
<br> _Forecasting results_
## Factors affecting model result
- **TV shows:** Notable content released within the forecast period
- **Seasonality:** Unclear seasonality can be a factor that affect forecasting result
- **Other factors:** Objective factors influencing the decision to VIP VieON subscription of users
## Insights
- Certain program genres can influence the outcome of the forecasting model. Through validation and testing on the model, the shows that contribute to increased forecasting accuracy are: comedy TV shows, dramas, talk shows, and game shows.
- Time series analysis and broadcast schedules for Q2 predict a steady upward trend in VIP subscriber numbers during this period.
- Forecasts indicate that error rates will increase over time, and combined with external factors, results will become less accurate the further out they are. Performing forecasts regularly at fixed intervals will help provide more accurate information.
