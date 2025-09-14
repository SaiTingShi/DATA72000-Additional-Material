1 Data Preparation
1.1 House Transaction Dataset
       House transaction dataset contains 1 csv file: shanghai_house_price_8.csv, and it comes from
https://sh.ke.com/. This is the core dataset. Each entry records a housing transaction, including fields 
describing the physical attributes of the house (such as unit price, number of bedrooms, and age), 
fields describing the house's environment (such as WGS84 coordinates and neighborhood greening rate), 
and fields describing the transaction (such as the transaction date). 
1.2 POI Dataset
       POI dataset contains 6 xlsx files: bank_poi.xlsx, middle_school_poi.xlsx, primary_school_poi.xlsx,
scenic_spot_poi.xlsx, shopping_center_poi.xlsx and top_tier_hospital_poi.xlsx, and they come from
https://lbsyun.baidu.com/. Each entry describes a facility, with fields including name, type, 
WGS84 coordinates, etc. This dataset is used to create six new fields for each house transaction 
record, namely the closest distance from each property to a certain type of facility. 'price_add_poi.ipynb'
is the corresponding script.
1.3 Macroeconomic Factors Dataset
       Macroeconomic factors dataset contains 1 xlsx file: A_macroeconomic_factors_eng_raw.xlsx,
and it comes from https://www.stats.gov.cn/. Each entry describes the economic situation in a 
particular month, with fields such as date, GDP YoY, GDP Mom, PPI YoY, etc. This dataset is used 
to create six new fields for each house transaction record, namely the average of a certain 
economic indicator in the three months before each transaction. 'price_poi_add_macro.ipynb' is the 
corresponding script, and its result is a clean dataset for machine learning.

2 Main Body
       The corresponding script of 2 Main Body is 'main_body.ipynb'. The script's basic work involves 
defining three feature groups and constructing three scenarios for comparison: physical features only, 
physical features + neighborhood features, and physical features + neighborhood features + macroeconomic indicators. 
       The code then performs exploratory data analysis, outputting descriptive statistics, plotting housing 
price distribution histograms, and feature correlation heatmaps. 
       The modeling phase uses an XGBoost regressor with fixed hyperparameters. The three scenarios 
are trained and tested separately, recording the number of training iterations and the final RMSE, and 
plotting learning curves. 
       Error analysis is performed on the test set predictions, calculating metrics such as RMSE, MAE, and R². 
       The interpretive analysis phase outputs a gain-based importance bar chart and generates summary 
and interaction effect plots using SHAP values ​​to help explain the model's decision-making mechanism 
for housing prices. 
       Finally, the code performs an ablation comparison, summarizing the evaluation metrics for the 
three scenarios in a comparison table.

3 Spatial Analysis
       The corresponding script of 3 Spatial Analysis is 'spatial_analysis1.ipynb'. 
       This script first  converts dataframe to a GeoDataFrame, sets the coordinate system to WGS84, and then 
projects it to the Web Mercator coordinate system for spatial weight calculation. Next, the code constructs an 8-neighbor k-nearest 
neighbor spatial weight matrix based on the housing location and calculates global Moran's I for 
the unit housing price. It then outputs statistics for global spatial autocorrelation (including Moran's I value, 
expected value, z-score, p-value, and number of permutations) to determine whether housing prices 
exhibit overall spatial clustering. The code then calculates local Moran's I and generates a local spatial 
autocorrelation index, significance level, and quadrant category for each observation point, categorizing 
it as high-high, low-low, high-low, low-high, or insignificant. Finally, a map of the local spatial clustering 
of housing prices is plotted.

4. Residual Analysis
       The corresponding script of 4 Residual Analysis is 'residual_analysis.ipynb'. 
       The first part of this script is similar to the main body, but after model training and prediction are 
complete, it specifically performs diagnostics and visualization of the prediction errors. It first calculates 
the mean, standard deviation, skewness, and kurtosis of the residuals to test the basic characteristics of 
the error distribution. It then uses the Shapiro–Wilk test to assess whether the residuals conform to a 
normal distribution and the Breusch–Pagan test to determine whether the residual variances are heteroskedastic. 
It then draws a variety of charts to visually demonstrate the residual behavior, including a scatter plot of 
the residuals against the fitted values, a histogram of the residuals with an overlay of the normal distribution 
curve, a Q–Q plot of the residuals, and a plot of the standardized residuals against the sequence number.