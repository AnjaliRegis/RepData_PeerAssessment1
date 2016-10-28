---
title: "Reproducible Research Assignment week 2"
output:
  html_document: default
  pdf_document: default
---

Load Packages which will be need ahead.

```{r}

##install.packages("lattice")

library("lattice")

```

Code deafault to echo

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## 1-Reading data from the source

Reading data from the **activity** dataset  :

```{r reading data}
activitydata1 <-read.csv("activity.csv",header=TRUE)
```

## 2-Histogram of the total number of steps taken each day

1- Sum steps by date

2- Generate histogarm

```{r}
aggdata <-aggregate(activitydata1$steps,list(date = activitydata1$date) ,sum,na.rm=TRUE)

hist(aggdata$x, main = "Steps taken each day", xlab="Steps taken each day")

```

## 3- Mean and Median of steps by date/each day


```{r mean and median}
summary(aggdata)

```

## 4- Time series plot of the average number of steps taken
1- Aggregate average steps by interval

2- Generate line graph

```{r}
avgdata <- aggregate(activitydata1$steps,list(interval = activitydata1$interval) ,mean,na.rm=TRUE)

plot(avgdata$interval,avgdata$x,type="l", main="Time series plot of the average number of steps taken",
     ylab="Interval",xlab="Average number of steps"
     )
```

## 5- The 5-minute interval that, on average, contains the maximum number of steps


```{r}

subset.data.frame(avgdata,x== max(avgdata$x))
```

## 6- Code to describe and show a strategy for imputing missing data

Replacing NA values with mean of steps in the dataset

1- Making duplicate copy of the original activity dataset

2- Replacing NA with mean

```{r}

activitydata2<-activitydata1

meanofsteps<-mean(activitydata1$steps[!is.na(activitydata1$steps)])
 
activitydata2$steps[is.na(activitydata2$steps)]<-meanofsteps


```

## 7- Histogram of the total number of steps taken each day after missing values are imputed

1- Sum of steps by date

2- Generate histogram

```{r}

aggdata2<- aggregate(activitydata2$steps,list(date = activitydata2$date) ,sum)

hist(aggdata2$x, main = 
"Total # of steps taken each day after missing values are imputed", xlab= "Total number of steps taken each day")

```

## 8- Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends

1- Create new column for weekday and weekend
2- Average steps by interval and weekday vs weekend
3- Plot a line graph


```{r}

activitydata2$Day<-weekdays(as.Date(activitydata2$date))


activitydata2$Dayoftheweek[activitydata2$Day %in% c('Saturday','Sunday') ] <-'Weekend'

activitydata2$Dayoftheweek[is.na(activitydata2$Dayoftheweek)] <- 'Weekday'


aggdata3 <- aggregate(activitydata2$steps, list (interval=activitydata2$interval,WD=activitydata2$Dayoftheweek),mean)


xyplot(x~interval|WD, data = aggdata3, layout(1,2),type="l")
  
```




Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
