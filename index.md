---
title       : Developing Data Products Course Project
subtitle    : Predicting MPG
author      : Tony Dreher
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [quiz]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Data & Purpose

* This Shiny app uses the `mtcars` dataset from the dataset library

* Its purpose is to predict the gas mileage of a vehicle based on the user's inputs.

--- .class #id 

## Linear Regression Model

The app predicts by using a parsimonious linear regression model.  Having already discovered that the wt, qsec, and am variables are the most significant in predicting mpg through the use of a all variable model followed by a step algorithm.  


```r
allvariablemodel <- lm(mpg ~ ., data = mtcars)
bestmodel <- step(allvariablemodel, direction = "both")
parsedmodel <- lm(mpg ~ wt + qsec + am, data = mtcars)
```

---

## UI Side

The user is able to use sliders to choose a weight, quarter mile time, and transmission type.  

The minimums and maximums of the variables from the mtcars dataset were used to keep the user's choices within somewhat realistic ranges.

For example:

```r
wtmin <- round(min(mtcars$wt),1)
wtmax <- round(max(mtcars$wt),1)
wtmean <- round(mean(mtcars$wt),1)

sliderInput("weight",label = "Weight (1000 lbs)",min = wtmin,max = wtmax,
                value = wtmean),
```

--- 

## Server Side

The user's inputs are used as new data in the predict function's ability to derive confidence intervals. The second and third element - the lower and upper bound of the confidence interval - are then pasted into the text output.


```r
mpgPredict <- predict(parsedmodel,
        newdata = data.frame(wt=input$weight,qsec=input$qsec,
        am=as.integer(input$transmission)),interval = "confidence")


paste("Your predicted MPG is between", round(mpgPredict[2],digits = 2),
          "and",round(mpgPredict[3],digits = 2))
```
