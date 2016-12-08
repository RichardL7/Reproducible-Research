---
title: "PA1_ Template"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Create the repo Reproduce-Research/PA1_Template.Rmd


## A. What is mean total number of steps taken per day?
1. Histogram of total number of steps taken each day

Read the .csv file

```{r}
activity <- read.csv(".//activity.csv")

StepsPerDay <- aggregate(steps~date,activity,sum)
hist(StepsPerDay$steps, xlab="Total Steps Each Day", ylab="Total Each Day",main="Histogram: Daily Steps Count")
```

2. Calculate and report the mean and median total number of steps taken per day

```{r}
Mean.1 <- mean(StepsPerDay$steps, na.rm=TRUE)
Median.1 <- median(StepsPerDay$steps, na.rm=TRUE)
```
#### Mean =
[1] 10766.19

#### Median =
[1] 10765

## B. Average daily activity pattern
1. Time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```(r)
avg.Interval <- aggregate(steps ~ interval, data=activity, FUN=mean)
plot(avg.Interval, type="l",xlab="5min Interval", ylab="Avg Daily Activity Pattern",  main="Avg Number of Steps")
```
2. 5-minute interval, average across the days with the maximum number of steps

```(r)
avg.Interval$interval[which.max(avg.Interval$steps)]
```
### Avg days with maximum number of steps in 5-minute interval
[1] 835

## C. Imputing missing values
1.Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```(r)
sum(is.na(activity))
```
#### Missing values =
[1] 2304


3.Create a new dataset that is equal to the original dataset but with the missing data filled in.

```(r)
activity.Merge = merge(activity, avg.Interval, by="interval")
activity.Merge$steps.x[is.na(activity.Merge$steps.x)] = activity.Merge$steps.y[is.na(activity.Merge$steps.x)]
```
```()
    interval    steps.x       date    type   steps.y
1          0  1.7169811 2012-10-01 weekday 1.7169811
2          0  0.0000000 2012-11-23 weekday 1.7169811
3          0  0.0000000 2012-10-28 weekend 1.7169811
4          0  0.0000000 2012-11-06 weekday 1.7169811
5          0  0.0000000 2012-11-24 weekend 1.7169811
6          0  0.0000000 2012-11-15 weekday 1.7169811
7          0  0.0000000 2012-10-20 weekend 1.7169811
```

4. A Histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```(r)
activity.Merge <- aggregate(steps.x~interval,activity.Merge,sum)
hist(activity.Merge$steps.x, xlab="Total Steps Each Day", ylab="Total Each Day",main="Histogram: Daily Steps Count")
```

4.a Recalculate the mean and median total number os steps taken per day.

```(r)
Mean.2 <- mean(activity.Merge$steps, na.rm=TRUE)
Median.2 <- median(activity.Merge$steps, na.rm=TRUE)
```

#### Mean = 
[1] 2280.339

### Median =
[1] 2080.906

## D. Are there differences in activity patterns between weekdays and weekends?

1. Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.

```(r)
type <- function(date) {
    if (weekdays(as.Date(date)) %in% c("Saturday", "Sunday")) {
        "weekend"
    } else {
        "weekday"
    }
}
activity$type <- as.factor(sapply(activityR$date, type))
```
```()
interval    steps.x       date    type   steps.y
1          0  1.7169811 2012-10-01 weekday 1.7169811
2          0  0.0000000 2012-11-23 weekday 1.7169811
3          0  0.0000000 2012-10-28 weekend 1.7169811
4          0  0.0000000 2012-11-06 weekday 1.7169811
5          0  0.0000000 2012-11-24 weekend 1.7169811
6          0  0.0000000 2012-11-15 weekday 1.7169811
7          0  0.0000000 2012-10-20 weekend 1.7169811
```

2. Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.

```(r)
average <- aggregate(steps ~ interval + type, data = activity, mean)
library(ggplot2)
ggplot(average, aes(interval, steps)) + geom_line() + facet_grid(type ~ .)
    xlab("5-min Interval") + ylab("Avg Number of Steps")
```

