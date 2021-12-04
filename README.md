<h1 align="center">Rossmann Sales Prediction</h1>
<center>

![image](/img/rossmann-vector-logo.png)

</center>


Rossmann is one of the largest drug store chains in Europe with around 56,200 employees and more than 4000 stores. In 2019 Rossmann had more than €10 billion turnover in Germany, Poland, Hungary, the Czech Republic, Turkey, Albania, Kosovo and Spain.

The product range includes up to 21,700 items and can vary depending on the size of the shop and the location. In addition to drugstore goods with a focus on skin, hair, body, baby and health, Rossmann also offers promotional items ("World of Ideas"), pet food, a photo service and a wide range of natural foods and wines. There is also a perfume range with around 200 commercial brands. Rossmann has 29 private brands with 4600 products (as of 2019).[wikipedia](https://en.wikipedia.org/wiki/Rossmann_(company))


## 1. Business Problem

Rossmann's CEO wants to renovate the company's stores to include new technologies and attract new customers.
To know the amount of resources available to renovate each store, the CEO needs to know how much each store will sell in the next 6 weeks.
Therefore, this project aims to solve the following business issue:

- what is the expected sales value for each store in the next 6 weeks?


## 2. Assumptions

| Variable | Assumption          | 
| :-----: | :-----------------------------------: | 
| **open**  | rows with closed stores (no sale) have been removed from the dataset |
| **competition_distance** | to replace the NAN values in this variable, we consider entering the value 200000, which is much larger than the variable's maximum value (75800), which would mean no close competitors |


## 3. Solution Planning

### 3.1 Final Product
The final product will be an API that provides the sales forecast for each store in the dataset within 6 weeks and will be accessible from the Telegram app

### 3.2 Tools
The tools used on this project were:


| Task | Tool |
| :-----: | :-----------------------------------: | 
| Programing language  | Python 3.8.10 |
| IDE for development  | Visual Studio Code |
| Data analysis   | Jupyter Notebook |
| Cloud platform to host the API  | Heroku |
| Bot for forecasts consults  | Telegram |


### 3.3 Process
The project steps were based on **CRISP-DM** metodology

- 1- **Data Collection**: The data used in this project was extracted from [Kaggle](https://www.kaggle.com/c/rossmann-store-sales/data) 

- 2- **Data Description**: Measures of central tendency and dispertion were used to better understand the data

- 3- **Feature Engineering**: creation of variables from existing variables and creation of list of business hypotheses

- 4- **Variable Filtering**: Filtering of variables and lines according to project assumptions

- 5- **Exploratory Data Analysis (EDA)**: main objectives:
  - Validate hypotheses created in the Feature Engineering step, generating business insights;
  - To know how the variables impact the phenomenon and what is the strength of this impact;
  - To know what variables could be important to the model.

- 6- **Data Preprocessing**: Apply techniques to prepare the data to the Machine Learning Models

- 7- **Feature Selection**: Selection of the most relevant features to the model

- 8- **Machine Learning Models**: Training and validate the machine learning models

- 9- **Hyperparameter Fine Tunning**: Choose the best values of the chosen model's parameters

- 10- **Error Interpretation**: Show the model result and interpretation of the model's error in a business language

- 11- **Deploy Model to Production**: Deploy the model in a production environment to make forecasts accessible to the business team, enabling quick practical data-driven business decision making.

## Business Hypothesis Validation
|         | Hypothesis          | Validation | Variable's relevance to Model |
| :-----: | :------------------ | :-----:     | :-----:             |
| **H1**  | Stores with a larger assortment should sell more | False | Low |
| **H2**  | Stores with closer competitors should sell less | False | Medium |
| **H3**  | Stores with longer-term competitors should sell more | False | Medium |
| **H4**  | Stores with promotions active for longer should sell more | False | Low |
| **H5**  | Stores with more consecutive promotions should sell more | False | Low |
| **H6**  | Stores open during the Christmas holiday should sell more | False | Medium |
| **H7**  | Stores should sell more over the years | False* | High |
| **H8**  | Stores should sell more in the second half of the year | False | High |
| **H9**  | Stores should sell more after the 10th of each month | True | High |
| **H10**  | Stores should sell less on weekends | True | High |
| **H11**  | Stores should sell less during school holidays | True | Low |

* although the dataset does not contain the full year 2015 data


## Models Performance Comparation

The table below shows the comparation between the models performance after the cross validation technique.

| Model Name | MAE CV | MAPE CV | RMSE CV |
| :-----: | :------------------ | :-----:     | :-----: |
|	Random Forest Regressor	| 837.7 +/- 219.24 |	0.12 +/- 0.02 |	1256.59 +/- 320.28
|	XGBoost Regressor	| 1048.45 +/- 172.04 |	0.14 +/- 0.02	| 1513.27 +/- 234.33
|	Linear Regression |	2081.73 +/- 295.63 |	0.3 +/- 0.02 |	2952.52 +/- 468.37
|	Lasso	| 2116.38 +/- 341.5 |	0.29 +/- 0.01	| 3057.75 +/- 504.26

**Note**: Although the best performance among the tested models was the Random Forest Regressor, the model chosen for implementation was the XGBoost Regressor since the small difference in performance between the two models does not justify the cost of implementing the Random Forest Regressor model.
Since the platform used for the deployment (Heroku) is free, it has space limitation.
Another reason is that we expect that the model's performance increase after Hyperparameter Fine Tuning step.


## Final Model Performance

As we expected, the model performance increased

| Model Name | MAE CV | MAPE CV | RMSE CV |
| :-----: | :------------------ | :-----:     | :-----: |
| XGBoost Regressor |	665.330873 |	0.097987 |	958.715745 |


## Business Performance and Interpretation Error

The table below shows the sales forecast for some stores. The MAE indicates the average error of the model, in which we can predict the best-case and worst-case sales scenario. Along with this information, we have the MAPE that indicates the mean absolute of the error in percentage.

As an example, we can cite store 5, which has a forecast value of sales for the next 6 weeks of $172722.39, in which this forecast value has a mean absolute percentage error of 8%,  making the forecast value varying from $348.81 to more (better scenario) or for less (worst scenario)

| store | predictions | worst scenario | best scenario | MAE | MAPE |
| :-----: | :------------------ | :-----:     | :-----: | :-----:  | :-----: |
|	1 |	164158.843750 |	163859.758954 |	164457.928546 |	299.084796 |	0.068630 |
|	2 |	172065.406250 |	171670.344443 |	172460.468057 |	395.061807 |	0.082030 |
|	3 |	261855.875000 |	261281.915112 |	262429.834888 |	573.959888 |	0.080642 |
|	4 |	349071.562500 |	348204.317436 |	349938.807564 |	867.245064 |	0.083969 |
|	5 |	172722.390625 |	172373.571388 |	173071.209862 |	348.819237 |	0.080151 |

### Graphical Analysis of Model Performance

To help understand the performance of the model, we generate the graph below. In it, it is possible to verify that the sales brands follow the actual sales very closely (plot 1). Regarding the error rate (plot 2), we can see where the model is overestimating the sales value (above the dotted line), or underestimating the sales value (below the dotted line).
Next, we have the error distribution, which approximates the normal (Gaussian) distribution, indicating that the error is symmetrically distributed around the mean (plot 3). Finally, we verified the low dispersion of the model's error (plot 4).

![image2](/img/final_plot.png)



## Financial Results

Finally, we can calculate the sum of all predictions made by the model, identifying the best and worst scenarios for the next 6 weeks of sale.

| scenario | values |
| :----: | :----: |
|	predictions |	$ 286,922,304.00 |
|	worst_scenario |	$ 286,176,354.10 |
|	best_scenario |	$ 287,668,215.25 |


## Final product

As planned, the final product is an API for forecasting sales for the next 6 weeks, which can be accessed through the Telegram app.
Below we have an image that illustrates the usage example of this application.


![image3](/img/telegram.jpeg)


## Conclusion
We can conclude that the demand was met with the delivery of a solution that can be accessed anywhere, through the Telegram application that addresses the business problem raised by the CEO of the Rossmann company.

## Next Steps
The next steps will be:
- identification of improvements in model performance, either by adding new data or by adding new features to the model;
- feedback from the business team on the usability of the application;
- identification of new business problems that can be resolved by upgrading the model;

