# MS&E 246 Final Report
Samuel Hansen, Theo Vadpey, Alex Elkrief, Ben Ertringer  
2/23/2017  





#Exectutive Summary

In *MS&E 246: Financial Risk Analytics*, our team analyzed a data set of 
roughly 150,000 loans backed by the US Small Business Administration 
(SBA) between 1990 and 2014. In doing so, we aimed to implement and test models
of the risk and loss of loan default. This report summarizes our findings from exploratory data analysis, details our approaches to modeling loan 
default probability and loss, and presents our methods of estimating
the loss distributions of tranches backed by a 500-loan portfolio. 

#Exploratory Data Analysis

Prior to model building, we explored the data to detect patterns that may 
provide signal for models of loan default. Because we first aimed to build 
binary response models of default probability, we excluded "Exempt" loans from 
our exploratory analysis. Subsequently, we examined the relationship between 
default rates and the predictor variables, including `Business Type`, 
`Loan Amount`, `NAICS Code`, and `Loan Term`, among others. 

Further, we collected additional predictor variables such as monthly 
`GDP`, `Crime Rate`, and `Unemployment Rate` by State, as well as macroeconomic
predictors such as monthly measures of the `S&P 500`, `Consumer Price Index`, 
and 14 other votalitity market indicies (see Data Cleaning section for 
data collection details). We include insights from exploratory analysis of 
these measures as well. 

##Default Rate vs. Business Type 

First, we examined the relationship between default rate and `Business Type`
by loan approval year. As shown on the plot below, we observe an interaction
effect between these three features, such that default rates spiked for 
loans that were approved around the Great Recession (approximately 2006- 2009). 
Further, the different trajectories of the 3 curves implies the "individual" 
`Business Type` suffered greater default rates than coporations and 
partnerships. Although corporations constitute a greater share of the data set,
as evidenced by the greater mass in the red circles, they exhibit medium 
default risk, as compared to the other business types. Taken together, 
this plot reveals business types were affected differently by the recession,
offering useful signal for subsequent modeling. 

![](final_report_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

##Default Rate by Loan Amount

Second, we examined whether we would observe a similar time-dependent 
interaction effect between default rate and `Loan Amount`. The plot below 
reveals that loans of all sizes approved around the Great Recession faced the
greatest default rates. However, loans of sizes \$500k-\$1m and \$1m-\$2m
appear to have experienced larger default rates over time compared to smaller
loans of size \$100k-\$300k and \$300k-\$500k. The spiking behavior of \$1m-\$2m
loan in 1999 and of loans greater than \$2m seem to be due to small sample 
sizes, as depicted by circle diameter. Overall, since loans of different sizes
have different default rate patterns over time, we would also expect 
the `Loan Amount` feature to offer predictive power. 

![](final_report_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

##Default Rate by NAICS Code

Third, we hypothesized different economic sectors would exhibit 
different default rates over time. In turn, we extracted the North American 
Industry Classification System (NAICS) code for each loan and truncated it
to the first two digits, which represents broad industry classes such as 
"Agriculture" and "Manufacturing." The following plot shows the default 
rate for loans of each truncated NAICS code approved in each year between 
1990-2014. We observe considerable variance in default rates between sectors;
for instance, codes 72, corresponding to "Accomodation & Food Services", 
has one of the highest default rates even before the recession. However,
code 54, corresponding to "Professional, Scientific, and Technical Services,"
consistently has one the lowest default rates. These patterns are consistent
with intuition, and underscore the value of including the truncated NAICS code
as a predictive feature of defaulting. 

![](final_report_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

##State GDP vs. Default Rate

- Make plot here

#Modeling Default Probability 

Building upon our exploratory data analysis, we constructed two types of 
predictive models of loan default probability: binary response models and 
cox proportional hazards models. Here, we present our approach to fitting
both model types, including data cleaning, feature engineering, 
feature selection, hyper-parameter optimization, and evaluation. 

##Binary Response Models

First, we built binary response models of small-businesses defaulting on loans,
which estimate the probability that a given loan *ever* defaults. To do so,
we implemented a machine learning pipeline that: 

1. Performs feature engineering;
2. Splits the data into train and test sets;
3. Normalizes continuous features;
4. Selects features using recursive feature elimination;
5. Trains binary response predictive models, including LASSO and random 
forests. 

Lastly, we evaluate the performance of these models on resampled partitions 
of the training data, and on a held-out test set in terms of AUC, sensitivity, 
and calibration. 

###Feature Engineering

Building on insights derived from exploratory data analysis,
we engineered the following features from the raw data: 

- `NAICS_code`: truncated to the first two digits of the NAICS code;
- `subprogram`: condensed infrequent factor levels into "other" category;
- `approval_year`: extracted year from loan approval datetime object.

In effect, these features represent dimensionality reduction of factors 
with many levels. For instance, there are 1,239 unique NAICS six-digit NAICS
codes in the raw data, yet only 25 unique 2-digit codes. Although we lose 
fine-grained detail by truncating the NAICS code, we aimed to optimize our
models by reducing variance introduced by high dimensionality. 

In addition to engineering features from the raw data, we also incorporated 
data from external sources, including monthly State-based measures of 
crime rate, GDP, and unemployment rate. We also joined in time-varying risk 
factors, including monthly snapshots of the `S&P 500`, `Consumer Price Index`, 
and 14 other votalitity market indicies. 

- BEN: Fill in where the data came from and any other important info 

###Preprocessing

After engineering features and joining in external data sources, 
we applied several preprocessing steps to our main data frame.
First, we centered and scaled the continuous predictors to apply regularization techniques during the modeling phase. Doing so adjusted for variables being
on different scales; for example, `Gross Approval` 
varies in dollar amounts from \$30,000  to \$4,000,000, whereas 
`Term in Months` ranges from 1 to 389. Second, we applied a filter to remove 
features with near zero variance to eliminate predictors that do not offer 
meaningful signal. 







##Cox Proportional Hazards Models 

- THEO

#Modeling Loss at Default 

##Value-at-Risk

- ALEX

##Average Value-at-Risk

- ALEX

#Loss Distributions by Tranche

- BEN