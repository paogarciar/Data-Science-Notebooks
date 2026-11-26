Analysis and modeling notebooks by Paola Garcia. Each notebook states its question, the data it used, what was done, and what the data cannot tell you.

Portfolio with case studies and demos: https://paogarciar.github.io

## Start here

| Notebook | Question | What I found |


[Best Markets to Advertise in an E-learning Product](Best%20Markets%20to%20Advertise%20in%20an%20E-learning%20Product.ipynb) (guided project) |

Business question. Our company sells programming courses (mostly web and mobile) for $59/month. Which two countries are the best markets to advertise in?
Data. freeCodeCamp New Coder Survey: 18,175 respondents × 136 columns.
Approach
1.  Is the sample representative?
    ◦ Top job interests: full-stack, front-end and back-end web development, mobile, data science
    ◦ Only 31.7% of respondents have a single focus. Mixed interests suit a broad catalog.
    ◦ About 86% are interested in web or mobile development, so the sample fits.
2.  Market size: by country of residence, the US holds 45.7% of respondents, then India 7.7%, UK 4.6% and Canada 3.8%.
3.  Willingness to pay: monthly spend = total spent on learning ÷ months programming. Analysis focused on five English-speaking markets: US, India, UK, Canada, Australia.
4.  Outlier handling, using boxplots and investigating each case
    ◦ Removed reports above $20,000/month
    ◦ For values above $2,000–$6,000, checked whether the person attended a bootcamp and how long they had been coding. A large up-front bootcamp fee divided by 1–3 months gives an unrealistic monthly figure, so these were removed when not justified.

Results: average monthly spend after cleaning

<img width="641" height="230" alt="Screenshot 2026-11-25 at 16 28 52" src="https://github.com/user-attachments/assets/8210a0a6-5690-427e-93b8-8ab10987069b" />

Australia dropped from $419 to $63 once outliers were removed. This shows how much the cleaning step mattered. (Best%20Markets%20to%20Advertise%20in%20an%20E-learning%20Product.ipynb)
Recommendation
• Primary market: United States, the largest audience with the highest spend.
• Avoid the UK: average spend is below the subscription price.
• Second market: India or Canada. Canada spends more per person, and India has about twice as many potential customers. Suggested budget splits:
    ◦ US 60% / India 25% / Canada 15%
    ◦ US 70% / India 30%
    ◦ US 65% / Canada 35%
• Hand the India vs. Canada decision to the marketing team, possibly with a small follow-up survey.
Tools: Python, pandas, Matplotlib, Seaborn.

[Anti-diabetic drug sales](Anti-diabetic%20drug%20sales.ipynb)

Problem. Predict monthly anti-diabetic drug sales in Australia, and validate the forecasts with time-series cross-validation (backtesting) so the score does not depend on one lucky train/test split.
Why it matters. Diabetes is a chronic, growing condition. Accurate demand forecasts help pharmacies, distributors and health systems avoid shortages and overstock.

Approach
1.  EDA: plotted the series, autocorrelation and seasonality. The data has a clear upward trend and yearly seasonality.
2.  Stationarity (ADF test)
    ◦ Raw series: p = 1.00, non-stationary
    ◦ First difference: p = 0.117, still non-stationary
    ◦ Seasonal difference: p = 0.00006, stationary
3.  Backtesting over 4 periods, each one fiscal year (Jul–Jun): 2004/05, 2005/06, 2006/07, 2007/08.
4.  Models: Simple, Double and Triple Exponential Smoothing (additive and multiplicative), manual ARIMA, auto-ARIMA non-seasonal, auto-ARIMA seasonal, and an average ensemble.
5.  Evaluation: RMSE, MAE, MAPE.

Results: best model in every fold was Triple Exponential Smoothing (multiplicative)

<img width="805" height="211" alt="Screenshot 2026-11-25 at 16 22 12" src="https://github.com/user-attachments/assets/e1bded8a-3f7e-422a-a6fb-277b25082796" />


Key insights
• Seasonality is the main signal. Every model that captured it (Holt-Winters, seasonal ARIMA) beat every model that didn't by a wide margin.
• The multiplicative form fits best because the seasonal swings grow as sales grow.
• Seasonal auto-ARIMA was a consistent second or third place, which makes it a solid fallback.
• Averaging all models did not beat the single best model.
Tools: Python, pandas, NumPy, statsmodels (ExponentialSmoothing, ARIMA, adfuller), pmdarima (auto-ARIMA), Matplotlib, Seaborn.


Amazon Stock Price vs. the US Economy (1997–2022) 
[Amazon Stock Price vs. the US Economy (1997–2022)s book1](Amazon%20Stock%20Price%20Creating%20dataset%20book%201.ipynb)  [Amazon Stock Price vs. the US Economy (1997–2022) book2](Amazon%20Stock%20Price%20Analysis%201997%20to%202022%20book2.ipynb)

Can US economic indicators help explain a stock price? 

This project has two notebooks.
Notebook 1: Creating the dataset
• Stock data: daily Open/High/Low/Close/Volume for Amazon (since IPO on 15 May 1997) and eBay (since 24 Sep 1998), from the Yahoo Finance API (yfinance), through Nov 2022.
• Macro indicators: US GDP (World Bank), Monthly Retail Trade, CPI for All Urban Consumers (CPI-U), unemployment rate, inflation rate, 1997–2022.
• Each source was cleaned, standardized to a common date key and merged, with null handling (annual GDP was assumed constant within each year). Result: 6,525 rows × 17 columns.
• Amazon vs. eBay EDA
    ◦ Year-over-year price comparison
    ◦ 50- and 200-day moving averages: eBay led until about 2016, then Amazon took off
    ◦ Daily % change distributions: eBay's is the widest, so it is the more volatile stock
• Observation: the unemployment and inflation curves move in strikingly similar patterns.
Notebook 2: Analysis and forecasting
• Year-over-year and monthly price comparisons, autocorrelation (ACF), and seasonal decomposition into trend, seasonality and residual.
• Feature importance: correlation analysis, plus permutation importance on an SVC model, comparing original, normalized and standardized inputs (standardized worked best). CPI ranked highest and GDP was also high. The GDP result is flagged as possibly inflated by filling annual values across days.
• Forecasting
    ◦ Monthly median close; trained on May 1997 – Jan 2020 (273 months), tested on Feb 2020 – Nov 2022 (34 months)
    ◦ Models: Simple Exponential Smoothing (α = 0.2, 0.5, 1), Double (additive, multiplicative), Triple / Holt-Winters (additive, multiplicative), ensemble average
    ◦ Residual diagnostics: ACF, histogram, KDE, Q-Q plot

Results

<img width="451" height="250" alt="Screenshot 2026-11-25 at 16 25 03" src="https://github.com/user-attachments/assets/aa55cbbb-0ccf-43ef-9f99-f32aa7439f0c" />

Key insights
• What looks like outliers in Amazon's price history is actually exponential growth. That is why multiplicative models win.
• The test window covers the pandemic surge and the 2022 drawdown, so it is a hard, realistic test. Classical models capture the trend but not shocks.
• Next steps: add event or sentiment features (for example political turmoil, earnings) and try models such as SARIMAX, Prophet or LSTM.
Tools: Python, pandas, NumPy, yfinance, statsmodels, scikit-learn (SVC, permutation_importance, scalers), Matplotlib, Seaborn.

[Credit Card Customer Segmentation](Credit%20Card%20Customer%20Segmentation.ipynb) 
README
Goal. Use K-Means to assign every client to a segment, explain what makes each segment different, and suggest how to target it.
Data. 10,127 customers, 14 features, no missing values. Features include age, gender, dependents, education, marital status, tenure (months on book), number of products, months inactive, credit limit, total transaction amount and count, average utilization ratio and estimated income. 1,627 customers (16%) have churned.
Approach
1.  EDA
    ◦ Distributions, and transaction counts by churn status and gender. Churned customers cluster around about 50 transactions and about $2,500 in spend, which suggests a link to reward thresholds.
    ◦ Correlation heatmap:
        ▪ Age ↔ tenure: strong
        ▪ Credit limit ↔ income: positive
        ▪ Credit limit ↔ utilization: negative
        ▪ Transaction count ↔ transaction amount: strong
2.  Feature engineering
    ◦ Gender to binary
    ◦ Education to an ordinal scale
    ◦ Marital status one-hot encoded, because it has no natural order
3.  Scaling with StandardScaler.
4.  Choosing K: elbow curve for K = 1–10, giving K = 6.
5.  Profiling: per-cluster means of the numeric features, scatter plots of the correlated pairs, and categorical mix per cluster.

Results

<img width="584" height="397" alt="Screenshot 2026-11-25 at 16 26 08" src="https://github.com/user-attachments/assets/7dc9b5b7-dea6-4c40-a8b9-686d449d3b48" />

Business value. Each customer now has a segment label, so marketing can tailor offers (rewards, limit increases, engagement campaigns) instead of using one message for everyone.
Tools: Python, pandas, NumPy, scikit-learn (KMeans, StandardScaler), Matplotlib, Seaborn.


[Euro–Dollar Exchange Rates: Data Storytelling (1999–2021)](Data%20Visualization%20on%20Exchange%20Rates.ipynb)

Data. European Central Bank euro daily reference rates, Jan 1999 – Jan 2021: 5,699 rows, 40 currencies plus a date column.
Cleaning
• Renamed columns and converted dates to datetime.
• Removed 62 non-trading days marked - and converted the US dollar column to float.
• Applied a 30-day rolling mean to smooth daily noise.
Stories visualized
1.  EUR/USD over time, the full 22-year trend.
2.  2007–08 financial crisis, a focused window highlighting the peak and the collapse.
3.  COVID-19: 2020 against a 2016–2019 baseline. The euro rose noticeably during the pandemic.
4.  Three US presidencies: a combined grid layout (three panels over one wide panel).
    ◦ Bush: the euro rose
    ◦ Obama: it fell
    ◦ Trump: it stayed roughly flat
Next step. Test whether the rate's peaks and troughs line up with specific economic or political events.
Tools: Python, pandas, NumPy, Matplotlib (styles, gridspec-style layouts, annotations).

[I-94 Traffic Dataset](I-94%20Traffic%20Dataset.ipynb)

Objective. Find indicators of heavy traffic on I-94, an east–west interstate running from Montana to Michigan.
Data. UCI Metro Interstate Traffic Volume: 48,204 hourly rows, 9 columns, 2 Oct 2012 – 30 Sep 2018. Includes holidays, temperature, rain, snow, cloud cover, weather type and traffic volume. It comes from one westbound station, so results describe that area and not the whole highway.
Approach and findings
1.  Distribution: hourly volume ranges from 0 to 7,280 cars (average 3,260). The 25th percentile is 1,193 cars and the 75th is 4,933, which pointed to a day vs. night split.
2.  Day (7 AM–7 PM) vs. night: daytime traffic is left-skewed (75% of hours have ≥ 4,252 cars) and nighttime is right-skewed (75% of hours have < 2,819). The rest of the analysis uses daytime only.
3.  Time indicators
    ◦ Month: heavier from March to October, lighter from November to February, at about 5,000 cars/hour on warm-month days. July 2016 is an anomaly, consistent with road construction.
    ◦ Day of week: business days are much heavier. Weekends stay below 4,000.
    ◦ Hour: on business days the rush hours are 7 AM and 4 PM, at over 6,000 cars/hour.
4.  Weather indicators
    ◦ Temperature, rain, snow and clouds correlate only weakly with volume.
    ◦ No broad weather category averages above 5,000 cars/hour.
    ◦ Three detailed weather types do: shower snow, light rain and snow, and proximity thunderstorm with drizzle.
Conclusion. Time is the strongest indicator: warm months, business days and rush hours. A few kinds of mildly bad weather also raise traffic, plausibly because pedestrians and cyclists switch to cars.
Tools: Python, pandas, Matplotlib (grid charts, histograms, line plots).


## All notebooks

- [Best Markets to Advertise in an E-learning Product](Best%20Markets%20to%20Advertise%20in%20an%20E-learning%20Product.ipynb)

- [Credit Card Customer Segmentation](Credit%20Card%20Customer%20Segmentation.ipynb)

- [Data Visualization on Exchange Rates](Data%20Visualization%20on%20Exchange%20Rates.ipynb)

- [I-94 Traffic Dataset](I-94%20Traffic%20Dataset.ipynb)

- [Investigating Fandango Movie Ratings](Investigating%20Fandango%20Movie%20Ratings.ipynb)

- [Predicting Bike Rentals](Predicting%20Bike%20Rentals.ipynb)

- [Predicting car prices](Predicting%20car%20prices.ipynb)

- [Predicting the Stock Market with Linear Regression](Predicting%20the%20Stock%20Market%20with%20Linear%20Regression.ipynb)

- [Predicting house prices](Predicting_house_prices.ipynb)

- [Profitable App Profiles for the App Store and Google Play Markets](Profitable%20App%20Profiles%20for%20the%20App%20Store%20and%20Google%20Play%20Markets.ipynb)

- [Traffic Behavior Analysis, Sao Paulo](Traffic%20sao%20paulo-%20Traffic%20Behavior%20Analysis.ipynb)

## Tools

Python, pandas, NumPy, scikit-learn, matplotlib, Jupyter.
