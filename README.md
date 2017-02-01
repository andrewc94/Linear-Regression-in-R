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

```
data(cars)
names(cars)
```
We now want to visualize the data to see if we can get an understanding of how it is structured.

`plot(dist~speed, data=cars)`

Looking at the data, it seems that there seems to be a linear relationship between the speed and stopping distance of cars. For a quantitative measure of how well the data correlates, we can check the pearson and spearman correlation of the data:

```
cor(cars, use="complete.obs", method="pearson")
cor(cars, use="complete.obs", method="spearman")
```

We obtain a pearson correlation factors of 0.8068949 and a spearman correlation factor of 0.8303568. This is high enough to indicate that the variables are indeed related in some fashion. Now that we are convinced there is a relationship between the data, we can use the speed of a car to predict its stopping distance. For our first model, we will train a model of the form Y = β<sub>1</sub> + β<sub>2</sub>X + ε where Y is the car breaking distance and X is the car's speed. To train a linear model on the data, we use the `lm()` command:

`model <- lm(dist~speed, data=cars)`
We now have a trained linear model that predicts the stopping distance of a car given its speed. From the output of the model, we can also see our regression line: Distance = -17.58 + 3.93 * Speed. To visualize our regression line, we can overlay it with the original training data. 

`abline(model, col="red")`

PLOT PIC HERE

To view additional details of the model, use the `summary()` command:

`summary(model)`

The output should look something like this:

![Alt text](/images/lm_call.png?raw=true)

The summary provides lots of data on the model such as the R squared and adjusted R squared values, the F statistic, and the p-value and is a valuable tool for evaluating the model. Since p < 0.05, we can reject the null hypothesis. We can also view other information such as the error sum of squares and mean sum of squares through the `anova()` command

`anova(model)`

ANOVA PIC HERE

To demonstrate the capabilities of R, we can also easily calculate relevant values on our own. For instance the following code will produce the SSE, SST, estimated variance, R^2, and adjusted R^2 values. Note that we can use our model to make predictions using the `predict()` command.

```
prediction <- predict(model, cars)
#can get error sum of squares SSE:
SSE <- sum((prediction - cars$dist)^2)

#estimated variance
variance <- SSE/(n-2)

#can get total sum of squares SST
meanDist <- mean(cars$dist)
SST <- sum((cars$dist - meanDist) ^2)

#get coefficient of determination R^2
r2 <- 1 - (SSE/SST)

#get adjusted R^2
r2adj <- ((n-1)*r2 - k) / (n-1-k)
```

Now let's move on to something slightly more complicated. If we look back at our plot, it appears that there may be an exponential rather than linear relationship with speed. To address this, we can synthesize an additional independent variable (speed^2) and use that for predictions.
First, we will extend our dataset:

```
cars2 <- cars
cars2["speed^2"] <- cars$speed^2
```

Now we can train our model using different training and test sets. The following lines of code randomly sample the data we have and splits it into a training set with 80% of the data and a test set with 20% of the data.

```
set.seed(100)
trainingIndex <- sample(1:nrow(cars), 0.8*nrow(cars))
cars2_train <- cars2[trainingIndex, ]
cars2_test <- cars2[-trainingIndex, ]
```

Training the model is as easy as before!

```
model2 <- lm(dist ~ speed + `speed^2`, data=cars2_train)
```

Our model now predicts the breaking distance using the speed and the squared speed. If we want to visualize this data, we will need to create a 3d plot. First, we will need to install and load the required 3d scatterplot package:

```
install.packages("scatterplot3d")
library("scatterplot3d")
```
To visualize the data in 3d:

```
cars2plot <- scatterplot3d(cars2_train$speed, cars2_train$`speed^2`, cars2_train$dist, angle=45)
```

3D PLOT HERE

We can also draw our regression plane:

```
cars2plot$plane3d(model2)
```

3D PLOT HERE

We can view further information about our new model as before with the `summary()` and `anove()` commands. Predicitions are also made similarly with the `predict()` command.
