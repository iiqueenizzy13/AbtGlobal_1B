# Abt Global 
Uncovering Correlations Between Weather Conditions and COVID-19 Rates

## Table of Contents
- [Project Description, Team's Objectives, and Goals](#project-description-teams-objectives-and-goals)
- [Methodology](#methodology)
- [Results and Key Findings](#results-and-key-findings)
- [Project Milestones and Timeline](#project-milestones-and-timeline)
- [Data Overview](#data-overview)
- [Python Libraries Used](#python-libraries-used)
- [License](#license)
- [Acknowledgements](#acknowledgements)

## Project Description, Team's Objectives, and Goals
Leveraging AI methodologies to understand correlations between weather conditions, in particular temperature fluctuations, and the rate of positive COVID-19 tests. This project involves working with public time series data, including COVID data from 2020–2021 and the corresponding weather data.

### Team's Objective 
- Predict the rate of positive COVID-19 tests using data on weather conditions.
- Use a sliding window method to handle the time series data.
- Evaluate predictive results for overfitting.

### Desired Outcomes
Present the results of our analysis of relationships between weather conditions and COVID-19 rates.  
This project hopes to provide critical insights that can inform public health strategies, improve resource allocation, community preparedness and resilience, and support policy/decision-making.

## Methodology
Initially there were three datasets:
- **Dataset 1:** Case Surveillance 
- **Dataset 2:** Weekly Transmission
- **Dataset 3:** PCR Testing
For the purposes of this project Dataset 2 was chosen because the timeline of Dataset 2 matched the timeline of the weather dataset. After selecting the dataset, the data was prepared for modeling by removing outliers, dealing with null categorical and numerical null values, removing duplicates, removing features that were not related to the problem. For categorical null values, the values were replaced with the mode while for numerical, they were replaced with the mean. Lastly, the COVID-19 dataset and the weather dataset were combined into one dataset, 'final_dataset.csv', which was used for the modeling. In the combined dataset, a new feature called "Temp Flux" for temperature fluctuations was created. The values of this feature were obtaine by subtracting the current week average temperature and the previous week average temperature. Afterwards, several models were created to better understand the relationship between the features Temperature Average (Avg Temp) and Temperature Fluctuation (Temp Flux) and COVID-19 rates. To determine which feature to use between Temperature Average and Temperature Fluctuation, graphs such as heat maps, pair plots, time series plots and scatter plots were used to visualize the relations. There were 4 main states that were considered: These states were chosen because they are in different time zones and portray different weather conditions
- Washington
- Wyoming
- Missouri
- Georgia 
These states were chosen because they are in different time zones and portray different weather conditions. The graphs depicted below are heat maps for the relation between the feature Temperature Average by week and state, the feature Temperature Fluctuation by week and state and COVID-19 Cases per 100K by week and state.
![COVID-19 Caes Per 100K](/visualizations/COVID-19Casesper100K.png)
![Temperature Average Heat Map](/visualizations/avgtempheatmap.png)
![Temperature Fluctuation Heat Map](/visualizations/tempfluxheatmap.png)

## Results and Key Findings
For the purposes of this project, Temperature is a pandemic indictator. The models based on Temperature Average and Temperature Fluctuation yield similar predictive accuracies, which can be seen in the images below. The randome forest and XGboost models portray modest improvements over the linear regression models; however, all models fail to capture the all variabilty in trends, which suggests that there are other factors that play significant roles in the relation between weather conditions and COVID-19 rates. 

### Multiple Linear Regression Models:
![Temperature Average Multiple Regression Model Without Outliers](/visualizations/avgtempmultlinreg.png)
![Temperature Fluctuation Multiple Linear Regression Model Without Outliers](/visualizations/avgtempmultlinreg.png)
As illustrated by the images above, the multiple linear regression model is too simple and do not properly capture the relationship between positive rates and temperature. Since both models portray the same exact metrics of accuracy, RMSE and log loss, it shows that using either Temperature Average and Temperature Fluctuation will yield the same results. The accuracy of approximately 92.6% suggests that a significant percentage of binary classifications (0 or 1) were correct. However, a high accuracy in imbalanced datasets can be misleading. The RMSE, which represents the average error in the predicition of the model, was approximately 43.83%, which is not too bad since the scale of the target variable (cases_per_100K_7_day_count) is large. Lastly, the log loss quantifies the performance of a classification model where the output is a probability between 0 and 1. The closer the log loss is to 0, the better the model's predictions. A value of 2.67 indicates that there is room for improvement, especially if the predictions are far from the true labels. 

### Pivoting from a regression to a classification problem
The problem of this project is very data-exploring dependent; thus, there is no right or wrong answer. As a result, a new feature called the Pandemc Indicator is created in the final_dataset.csv file. This feature is a binary classification on the COVID-19 positve rates used to indicate whether a positive case is a pandemic or not. The value of 1 will be given to any value in the cases_per_100K_7_day_count feature if it is greater than 56.505; otherwise, a value of 0 will be given to any value less than 56.505.

### Logistic Regression Model:
![Logistic Regression Model with Temperature Avergae vs Pandemic Indicator](/visualizations/avgtemologreg.png)
If the violin plot for Temperature Average is wider at higher temperatures for a Pandemic Indicator of 1, it suggests that higher average temperatures are more frequently observed during pandemic periods. An accuracy of 0.5815 means the model correctly predicted about 58.15% of the cases. A log loss of 0.6740 indicates moderate prediction uncertainty. An RMSE of 0.6469 suggests the average deviation between the predicted and actual values.

### Random Forest Model:
![Random Forest Model Temperature Avergae vs Pandemic Indicator Accuracy](/visualizations/avgtemprandforacc.png)
![Random Forest Model Temperature Avergae vs Pandemic Indicator Confusion Matrix](/visualizations/avgtempconfmat.png)
![Random Forest Model Temperature Avergae vs Pandemic Indicator ROC Curve](/visualizations/avgtemproc.png)
The random forest model evaluated the best value for number of estimators 216. The accuracy score of 0.5664 means that the model correctly predicted approximately 56.64% of the cases. The AUC, short for Area Under the Curve, of 0.588 indicates that the model is able to classify about 58.8% of the cases. The log loss of 0.7602 shows slightly above average performance. This portrays that having more features may improve performance.


### XGBoost Model With Ensemble Models:
![XGBoost Model Temperature Avergae vs Pandemic Indicator Confusion Matrix](/visualizations/avgtempsxgbconfmat)
![XGBoost Model Temperature Avergae vs Pandemic Indicator ROC Curve](/visualizations/avgtempxgbroc.png)
The accuracy of 58.73% depicts that the model is able to accurately predicted more than half of the cases. The RMSE of 0.642 is slightly higher than the logistic regressiom model. The log loss of 0.677 is within the range of the logstic regression model and the random forest model. Additionally, the log loss reduced significantly from 14.7 to 0.64 compared to the XGBoost model. The AUC-ROC Score of 0.6076623851213757 portrays that the model is classify between positive and negative classes. The AUC-ROC, short for Area Under the Receiving Operating Characteristic Curve, evaluates how well a model can distinguish between postive and negative classes by considering all possible classification thresholds. The higher the AUC-ROC score, the better the performance. 
Overall, COVID-19 positive rates are affected by more than just average temperature, hence the relatively lower accuracy.

## Potential Next Steps
For future work related to this project, it would be best to expand the scope to be global not just the United States in order to gather more data and figure out what other features to look into. Using more complex models such as neural networks and exploring the model with more regression values can help to better understand the relationship between weather conditions and COVID-19 rates. Lastly, combining the Temperature Average and Temperature Fluctuation into one vector and creating an indicator variable to improve model accuracy. 

## Project Milestones and Timeline 
- **Milestone 1:** Exploratory Data Analysis
- **Milestone 2:** Data Preprocessing
- **Milestone 3:** Explore Data and Identify Model
- **Milestone 4:** Modeling
- **Milestone 5:** Evaluation
- **Milestone 6:** Report

## Data Overview
- **Weather Data:** `weekly_covid_county_level_US.csv`
- **COVID-19 Data:** `weekly_tavg_county_cont_US.csv`
- **Dataset used in models:** `final_dataset.csv`

## Python Libraries Used
- Pandas
- Scikit-learn
- Numpy
- Datetime
- Matplotlib
- OS
- Seaborn

## License
Copyright 2024 Uma Uppuloori, Thienkim Nguyen, Madison Harman, Sukaina Zaidi and Izabel Miminoshvili

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## Acknowledgements
Thanks to the ABT GLOBAL team, Laurence Baskett, Nora Connor, and Madeline Zhang; Break Through Tech (BTT) and the student team TA from BTT, Daniel Shao, for their help and guidance with this project.  
Further thanks to the student team Uma Uppuloori, Thienkim Nguyen, Madison Harman, Sukaina Zaidi, and Izabel Miminoshvili.