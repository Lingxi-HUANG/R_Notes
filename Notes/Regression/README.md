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
  geom_smooth(mapping = aes(x = rm, y = medv), method = "lm", se = FALSE) +     # add a regression line
  labs(title = "Relationship between median home value & number of rooms per dwelling",
       subtitle = "Median home value in 1000 USD",
       x = "Number of rooms per dwelling",
       y = "Median home value")

medv_rm                                                                         # visualize the graph
```
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/Regression/Figure/Medv_rm.jpeg)

# Fit the model
```r
# Create the regression ------------------------------------------------------
lm <- lm(medv ~ rm, data = BostonHousing2) # fit the model
summary(lm)                                # Review the result
```
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/Regression/Figure/medv~rm.PNG)

# Assumptions

The Linear Regression model should be validated for all model assumptions including the definition of the functional form. If the assumptions are violated, we need to revisit the model.

The assumptions are as follow:
* The dataset must have some `linear relationship`. There exists a linear relationship between the coefficient of the parameters (independent variables) and the dependent variable Y
* `Multivariate normality` - the dataset variables must be statistically Normally Distributed (i.e. resembling a Bell Curve)
It must have no or little multicollinearity - this means the independent variables must not be too highly correlated with each other. This can be tested with a Correlation matrix and other tests
* `No auto-correlation` - Autocorrelation occurs when the residuals are not independent from each other. For instance, this typically occurs in stock prices, where the price is not independent from the previous price.
* `Homoscedasticity` - meaning that the residuals are equally distributed across the regression line i.e. above and below the regression line and the variance of the residuals should be the same for all predicted scores along the regression line.
* `Multi-collinarity` - There is no (low) correlation between independent variables. This happens in the case of multivariable regression. When one independent variable is highly correlates with another indepedent variable in the model, it is hard to isolate the impact of an indepdent variable when it changes as the other highly correlated indepedent variable cannot be easily controlled. 

# Residual versus fitted values
The linear regression algorithm assumes that there is a linear relationship between the parameters of independent variables and the dependent variable Y. If the true relationship is not linear, we cannot use the model as the accuracy will be significantly reduced.

Thus, it becomes important to validate this assumption. This is done by plotting Residual Plots. 
The residual plot should not show any pattern i.e. the residuals should be randomly dispersed around the horizontal axis. In case there is a pattern, we can infer that there is a problem with some aspect of the linear model i.e. an incorrect functional form has been used.

In the case below, it appears that the relationship between the independent variable and depedent variable is not linear. 
```r
plot(lm, which = 1)
```
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/Regression/Figure/Residuals.jpeg)

# Homoscedasticity
```r
plot(lm, which = 3)
```
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/Regression/Figure/Homoscedasticity.jpeg)

# Normality
```r
plot(lm, which = 2)
```
![](https://github.com/tomtomhuang/R_Notes/blob/main/Notes/Regression/Figure/Normality.jpeg)

# 

# External Resource
* [Boston Housing Data from MLbench](https://rdrr.io/cran/mlbench/man/BostonHousing.html)
* [Simple Linear Regression](https://finnstats.com/index.php/2021/10/25/simple-linear-regression-in-r/?utm_source=ReviveOldPost&utm_medium=social&utm_campaign=ReviveOldPost) by Finnstats
* https://www.datacamp.com/tutorial/linear-regression-R
* https://www.kellogg.northwestern.edu/faculty/weber/emp/_session_3/nonlinearities.htm
* https://rpubs.com/iabrady/residual-analysis

