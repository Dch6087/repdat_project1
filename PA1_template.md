
# Reproducible research Assignment 1
  
  
##What is mean total number of steps taken per day?
  
1. Calculate the total number of steps taken per day

```r
a<-read.csv("activity.csv",stringsAsFactors=FALSE)
a[,2]<-as.Date(a[,2])
Nbstep<- as.data.frame(xtabs(steps ~ date, data = a))
names(Nbstep)[2]<-"sum of steps"
```
  
2. Make a histogram of the total number of steps taken each day
![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 
  
3. Calculate and report the mean and median of the total number of steps taken per day

The mean of the total number of steps is : **1.0766189 &times; 10<sup>4</sup>**  
The median of the total number of steps is : **1.0765 &times; 10<sup>4</sup>**
  
## What is the average daily activity pattern?
  
1. Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)
![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png) 
  

2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

The maximum average number of steps is **206.1698113** and is observed at interval **835**
  
    
## Imputing missing values
  
*Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.*
  
1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```r
l<-length(a[is.na(a$steps)==TRUE,1])
```
the total number of rows with NAs is **2304**
  
2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
  
3 methods are possible but we have no visibility of the NA repartition
- the mean of the 2 next value => problem of entire missing days
- the average value of the day => lack of pick of activity
- the average value per day => best solution
  
3.  Create a new dataset that is equal to the original dataset but with the missing data filled in.


```r
b<-a
for (i in 1:nrow(a)) 
        {if (is.na(a[i,1])==TRUE)
        {
        e<-as.numeric(as.character(a[i,3]))
        g<-floor(e/100)*12 + (e-floor(e/100)*100)/5
        b[i,1]<-d[g+1,1]
        }}
```

4.  Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```r
b[,2]<-as.Date(b[,2])
Nbstep2<- as.data.frame(xtabs(steps ~ date, data = b))
names(Nbstep2)[2]<-"sum of steps"
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9-1.png) 
  
The mean of the total number of steps is : **1.0766189 &times; 10<sup>4</sup>**  
The median of the total number of steps is : **1.0765 &times; 10<sup>4</sup>**

These values are identique from the original ones  
The impact of imputing missing data is that the most frequent observation is more important than before  
That's logical because I filed the NA with the most frequent data !

  
## Are there differences in activity patterns between weekdays and weekends?

*For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.*

1. Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day. Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis).


```r
library(lubridate)
for (i in 1:nrow(b))
        {if (wday(b[i,2])==1 | wday(b[i,2])==7) {b[i,4]<-"Weekend"}
        else {b[i,4]<-"Weekday" }
}  

b[,4]<-as.factor(b[,4])

h<-aggregate(steps ~ interval + V4, data = b, FUN= "mean" )
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11-1.png) 
  
We can see that this people is more active during the weekend than during the weekday  
We can see also during weekdays that he is walking more in the morning than in the evening => His job way is different in the morning and in the evening. Maybe he is doing some running or he is bringing his child to school.
