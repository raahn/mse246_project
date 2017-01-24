MSE 246 Data Join
================
Samuel Hansen
1/21/2017

-   [Overview](#overview)

Overview
========

This script joins together data from the Small Business Association (SBA), S&P 500, State-level GDP, and State-level unemployment rates.

``` r
# Initialize libraries and input files 
library(knitr)
library(lubridate)
library(stringr)
library(tidyverse)
loans_file_in <- "../data/SBA_Loan_data_full_edited.csv"
sp500_fioe_in <- "../data/SP500_ret.csv"
gdp_file_in <- "../data/STATE_GDP.csv"
unemploy_file_in <- "../data/unemployment_rates.csv"
```

``` r
df <- 
  # Read in loan data 
  read_csv(loans_file_in) %>%
  plyr::rename(replace = c("2DigitNAICS" = "NAICS")) %>%
  mutate(ApprovalDate = mdy(ApprovalDate)) %>%
  
  # Join S&P 500 data 
  left_join(read_csv(sp500_file_in), by = c()) %>%
  
  # Join State GDP data 
  left_join(read_csv(gdp_file_in), by = c()) %>%
  
  # Join unemplyment rate data 
  left_join(read_csv(unemploy_file_in), by = c())
```