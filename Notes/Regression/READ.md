# Regression Model

- [Introduction](#Introduction)
- [Load a dataset](#Loading-a-dataset)
- [-](#-)
- [-](#-)

# Introduction
A regression model is a simple and straight forward statstical tool that analyze the relationship between dependent variable (y) and other independent variables. In the case of linear regression mode, we investigate whether the relationship of variables is a straight line. By creating a linear regression model, we can use it to forecast by knowing how one independent variable changes can cause the change on the dependent variable. This post is served to demonstrate some basic examples of how to use R to make linear regression model. The content will be updated on a continuous basis. 


```r
# Load libraries --------------------------------------
if (!require("pacman")) install.packages("pacman")
pacman::p_load(tidyverse, 
               janitor, # Cleaning column names  
               scales, # Transform axis scales
               ggplot2,
               mlbench) # MLBench is a framework for distributed machine learning. A collection of artificial and real-world machine learning benchmark problems.    
```


# Load a dataset
The dataset I am using is a dataset called Bonston Housing from the package of MLBENCH. [`Here`](https://rdrr.io/cran/mlbench/man/BostonHousing.html) is the link to the metadata of the dataset.
```r
# Load the dataset from MLBench -----------------------
data("BostonHousing2")
head(BostonHousing2)
```



# External Resource
* [Boston Housing Data from MLbench]([https://ggplot2.tidyverse.org/reference/scale_brewer.html](https://rdrr.io/cran/mlbench/man/BostonHousing.html)
* [Simple Linear Regression](https://finnstats.com/index.php/2021/10/25/simple-linear-regression-in-r/?utm_source=ReviveOldPost&utm_medium=social&utm_campaign=ReviveOldPost) by Finnstats

