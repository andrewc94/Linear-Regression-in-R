# Linear-Regression-in-R

Note: The previous demo on R Studio basics can be found at: https://nicklyz.github.io/R-Studio-Tutorial/. This demo is self contained and does not need completion of the previous demo, but it may help provide background knowledge.


### Setup:

1. Download R at https://cran.rstudio.com/
2. Download R Studio at https://www.rstudio.com/products/rstudio/download/

### Linear Regression:
In this demo, we will perform linear regression on a simple dataset included in the data package in the base R installation.

First we will discover the data available within the data package. In the console, type data() to see a list of the available datasets available within the data package.

Type `data()` into the console.

We will be working with the cars dataset. We will first load the data and see what it contains:

`data(cars)`

`names(cars)`

We now want to visualize the data to see if we can get an understanding of how it is structured.
`plot(dist~speed, data=cars)`

Looking at the data, it seems that there seems to be a linear relationship between the speed and stopping distance of cars. For a quantitative measure of how well the data correlates, we can check the pearson and spearman correlation of the data:
`cor(cars, use="complete.obs", method="pearson")`
`cor(cars, use="complete.obs", method="spearman")`
We obtain a pearson correlation factors of 0.8068949 and a spearman correlation factor of 0.8303568. This is high enough to indicate that the variables are indeed related in some fashion. Now that we are convinced there is a relationship between the data, we can use the speed of a car to predict its stopping distance. To train a linear model on the data, we use the `lm()` command:

`model <- lm(dist~speed, data=cars)`
We now have a trained linear model that predicts the stopping distance of a car given its speed. To view details of the model, use the `summary()` command:

`summary(model)`

The output should look something like this:
