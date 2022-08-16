# Regression Model

- [Introduction](#Introduction)
- [Load a dataset](#Loading-a-dataset)
- [-](#-)
- [-](#-)

# Introduction
A regression model is a simple and straight forward statstical tool that analyze the relationship between dependent variable (y) and other independent variables. In the case of linear regression mode, we investigate whether the relationship of variables is a straight line. By creating a linear regression model, we can use it to forecast by knowing how one independent variable changes can cause the change on the dependent variable. This post is served to demonstrate some basic examples of how to use R to make linear regression model. `The content will be updated on a continuous basis`. 


```r
# Load libraries --------------------------------------
if (!require("pacman")) install.packages("pacman")
pacman::p_load(tidyverse, 
               janitor, # Cleaning column names  
               scales,  # Transform axis scales
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
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/Regression/Figure/BostonHousing2.png)

# Visualize the relationship between two medv and rm
```r
medv_rm <- ggplot(data = BostonHousing2) +
  geom_point(mapping = aes(x = rm, y = medv)) +
  geom_smooth(mapping = aes(x = rm, y = medv), method = "lm", se = FALSE) +
  labs(title = "Relationship between median home value & number of rooms per dwelling",
       subtitle = "Median home value in 1000 USD",
       x = "Number of rooms per dwelling",
       y = "Median home value")

medv_rm # visualize the graph
```
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/Regression/Figure/Medv_rm.jpeg)

# Fit the model
```r
# Create the regression ------------------------------------------------------
lm <- lm(medv ~ rm, data = BostonHousing2) # fit the model
summary(lm)                                # Review the result
```
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/Regression/Figure/medv~rm.PNG)

# Rsidual versus fitted values
```r
plot(lm, which = 1)
```

# Homoscedasticity
```r
plot(lm, which = 3)
```

# Normality
```r
plot(lm, which = 2)
```

# 

# External Resource
* [Boston Housing Data from MLbench](https://rdrr.io/cran/mlbench/man/BostonHousing.html)
* [Simple Linear Regression](https://finnstats.com/index.php/2021/10/25/simple-linear-regression-in-r/?utm_source=ReviveOldPost&utm_medium=social&utm_campaign=ReviveOldPost) by Finnstats

