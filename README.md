# COVID-Demark
Extracted COVID-19 data from Denmark and applied GAM to determine which produces the best model for new cases of covid-19 from January to May 7th in Denmark.
# Introduction

Coronavirus disease (COVID-19) is an infectious disease caused by a newly discovered coronavirus. Most people infected with the COVID-19 virus will experience mild to moderate respiratory illness and recover without requiring special treatment. The COVID-19 pandemic in Denmark is part of the ongoing worldwide pandemic of COVID-19 caused by severe acute respiratory syndrome coronavirus 2.
There is a collection of the COVID-19 data maintained by Our World Data. The dataset is updated rapidly on confirmed cases, deaths, and testingm throughout the duration of the COVID-19 pandemic. In this assignment, we extracted data for demark only on May 7th. There are more variables in the original data, we have extracted four variables(location,date,total_case,new_cases) in this exercise.
Generalised Additive Models (GAM) will be used. A GAM is a non-parametric model where the linear coefficients of previous model are replaced by smoothers in the case where the response variable can not necessarily be explained by the normal distribution. In this assignment, we will be looking at different smoothing parameters to determine which produces the best model for new cases of covid-19 from January to May 7th in denmark.

# 1
The dataset reads 128 observations by the 7th of May,2020. Inspection of the of the data was performed to ensure there were no missing values. Unusual data was removed from the dataset before undergoing the modelling process.
```{r}
#read data into R
covid<- read.csv("Covid cases.csv")
covid$date <- as.Date(covid$date)
library(ggplot2)
total_date <-ggplot(covid, aes(x=date, y=total_cases))
total_date <- total_date + geom_point() + labs(title = "Total covid cases vs Date", 
                                           y = "cumulative total cases")
total_date 

new_date <-ggplot(covid, aes(x=date, y=new_cases))
new_date <- new_date + geom_point() + labs(title="New covid cases vs Date",
                                             y ="new caeses each day")
new_date
```

There is a trend between the cumulative tatal cases and the date since around the middle of the March until recent May.

The graph of new cases of covid aganist date trended up since March but there is a noticeable decrease in the middle of April.

# 2
```{r}
library(gam)
model01 <- gam(new_cases ~ s(date, spar=0.1),data=covid)
summary(model01)
covid$fits01 <- fitted(model01)
plot01 <- ggplot(covid, aes(x=date, y=new_cases)) + geom_point() + labs(title="New cases against date using spar = 0.1")
plot01 <- plot01 + geom_line(aes(x=date, y=fits01))
plot01
```

```{r}
model02 <- gam(new_cases ~ s(date, spar=0.3),data=covid)
summary(model02)
covid$fits02 <- fitted(model02)
plot02 <- ggplot(covid, aes(x=date, y=new_cases)) + geom_point() + labs(title="New cases against date using spar = 0.3")
plot02 <- plot02 + geom_line(aes(x=date, y=fits02))
plot02
```

```{r}
model03 <- gam(new_cases ~ s(date, spar=0.5),data=covid)
summary(model03)
covid$fits03 <- fitted(model03)
plot03 <- ggplot(covid, aes(x=date, y=new_cases)) + geom_point() + labs(title="New cases against date using spar = 0.5")
plot03 <- plot03 + geom_line(aes(x=date, y=fits03))
plot03
```

```{r}
model04 <- gam(new_cases ~ s(date, spar=0.7),data=covid)
summary(model04)
covid$fits04 <- fitted(model04)
plot04 <- ggplot(covid, aes(x=date, y=new_cases)) + geom_point() + labs(title="New cases against date using spar = 0.7")
plot04 <- plot04 + geom_line(aes(x=date, y=fits04))
plot04
```


```{r}
model05 <- gam(new_cases ~ s(date, spar=0.9),data=covid)
summary(model05)
covid$fits05 <- fitted(model05)
plot05 <- ggplot(covid, aes(x=date, y=new_cases)) + geom_point() + labs(title="New cases against date using spar = 0.9")
plot05 <- plot05 + geom_line(aes(x=date, y=fits05))
plot05
```

Comment for each model:

The first graph of model 1 for covid new cases against date was fitted with a Generalised Additive Model (GAM) using the spar value 0.1. The fitted line was spikey and seemed like it was overfitting the data. The AIC of model 1 is 1127.765. The residual deviance is 17302.33. We can also see from the R squared term, that 98.63513%-calculated using 1-(13124/68695). This suggested that the model explains around 98.63513% of the variability in the response has been explained by the model. It supports the previous comment that this model is overfitting the data.

The graph of model 2 using spar value 0.3, appeared to be slightly smoother than the graph using spar value 0.1. However, the fitted line was still relatively spikey and seemed like it was still overfitting the data. The AIC of model 2 is 1181.373. The residual deviance is 38445.58. We can also see from the R squared term, that 96.96728%-calculated using 1-(38445.58/1267693). This suggested that the model explains around 96.96728% of the variability in the response has been explained by the model. 

The graph of model 3 using spar value 0.5, appeared to be much smoother than the previous graphs using spar value 0.1 and 0.3. The fitted line also illustrated the fluctuations, particularly the jump around the middel of March and the drop around the middel of April. The AIC of model 3 is 1238.911. The residual deviance is 85603.29. We can also see from the R squared term, that 93.24732%-calculated using 1-(85603.29/1267693). This suggested that the model explains around 93.24732% of the variability in the response has been explained by the model. 

The graph using spar value 0.7 and 0.9 appeared to be significantly smoother than the previous 3 graphs, the AIC is 1305.318 and 1359.443 respectively. The residual deviance is 170628.2 and 280803.8 respectively. They explains around 86.54026% and 77.84923% respectively of the variability in the response has been explained by the model.This is consistent with the information we by looking at the plot that these two lines are underfitting the data. And the key characteristics of the data, jump in new cases around the middle of March and drop in new cases of the middle of April were not displayed well.

# 3
Adjusted the spar value to 0.45.
```{r}
model06 <- gam(new_cases ~ s(date, spar=0.45),data=covid)
summary(model06)
covid$fits06 <- fitted(model06)
plot06 <- ggplot(covid, aes(x=date, y=new_cases)) + geom_point() + labs(title="New cases against date using spar = 0.45")
plot06 <- plot06 + geom_line(aes(x=date, y=fits06))
plot06
```

Here we adjusted the spar value to 0.45 to get the best model. By comparing the result of fitting Generalised Additive models to the data with different spar value, the spar value used to find the best models was 0.45 for new cases. The graph of this model using spar value 0.45 illustrated the fluctuations, particularly the jump around the middel of March and the drop around the middel of April. The AIC of the model with 0.45 spar is 1226.781, which is smaller than the models with 0.5, 0.7 and 0.9 spar. The residual deviance is 72739. We can also see from the R squared term, that 94.2621%-calculated using 1-(72739/1267693). This suggested that the model explains around 94.2621% of the variability in the response has been explained by the model. Also, by looking at the graphs, the model of 0.45 spar capture the trend of the new cases against the date resonably well. It is neither overfitted nor underfitted as other models.

