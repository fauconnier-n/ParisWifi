# Paris Wifi usage: forecasting a time serie after an abrupt change due to COVID-19 lockdown


The goal of **[this notebook](https://nbviewer.jupyter.org/github/fauconnier-n/ParisWifi/blob/master/TSanalysis.ipynb)** is to forecast the usage of Paris Open Wifi, more specifically the hourly data usage. 

The data has been gathered using **[ODSQL](https://help.opendatasoft.com/apis/ods-search-v2/#language-elements)** on **[Paris Open Data Website](https://parisdata.opendatasoft.com/explore/dataset/paris-wi-fi-utilisation-des-hotspots-paris-wi-fi/information/?disjunctive.incomingzonelabel&disjunctive.incomingnetworklabel&disjunctive.device_portal_format&disjunctive.device_constructor_name&disjunctive.device_operating_system_name_version&disjunctive.device_browser_name_version&disjunctive.userlanguage&basemap=jawg.matrix&location=13,48.86099,2.37588)**'s API.

**Packages/models**: Exponential Smoothing (fitted with statsmodel), SARIMA, Facebook Prophet

The challenge of this forecast is the presence an **abrupt change in the time serie** due to the COVID-19 lockdown. It results in a time serie composed of **two very distinct periods**, the second one showing lower data consumption and less difference from day to day (less weekly seasonality).

***

## Best model

We find that **Holt-Winters exponential smoothing** performs the best regarding the RMSEs : 
![](https://github.com/fauconnier-n/ParisWifi/blob/master/Images/RMSE.jpg)  
*See the notebook for additional models*

### The time serie:
![](https://github.com/fauconnier-n/ParisWifi/blob/master/Images/TS.png)

### The daily saisonnal decomposition of the serie:
![](https://github.com/fauconnier-n/ParisWifi/blob/master/Images/daily_decomp.png)

### The model: Holt-Winters exponential smoothing
#### Fitted values decomposition:
![](https://github.com/fauconnier-n/ParisWifi/blob/master/Images/HW_values_of_fitted_values.png)

#### Predictions:
![](https://github.com/fauconnier-n/ParisWifi/blob/master/Images/HW_predictions.png)

***

## Findings
**Exponential smoothing performs slightly better** than the other models (better RMSE overall). But the differences in performance might be due to randomness in the variation of the time serie (noise).

We need to keep in mind that the serie seems quite noisy with no clear daily pattern during open hours. **The approach we took is very naive** in that regard (applying a simple daily pattern and a trend). The Wifi usage is **dependant of other external variables** not taken into account (current weather, bank holiday, amount of tourists in Paris that day, etc). Using external data or having a few years of consistent data would obviously help.

Therefore, our approach is very **limited and incomplete**, even if it produces *decent* short term forecasts. Its **naivety**, the fact that the models all have been trained on a **VERY short time window**, the **noisyness of the serie**, are all servere weaknesses to our models. They are **unsuitable for long term predictions** by design (only using daily seasonality).
