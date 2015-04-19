---
title: "Reproducible Research"
output: 
  html_document:
    keep_md: true
---
  
    


## Loading and preprocessing the data


```r
data_dir <- "C:/Users/RW/Documents";
fileURL <- "https://github.com/Arcapalo/RepData_PeerAssessment1/" 
fileSource <-"activity.zip"
source_path <- paste(data_dir, "/", fileSource , sep="")
txt_file <- paste(data_dir, "/","activity.csv", sep="")

if (!file.exists(txt_file)) {
        if (!file.exists(source_path)) {
                message(paste("Please Wait! Load", fileURL, "..."));
                download.file(fileURL, destfile=source_path);
        } 
        else {
            message(paste("Please Wait! Unzip", source_path, " file..."));
            unzip(source_path, exdir = data_dir);
        }
}
message(paste("Please Wait! Load", txt_file, " to dataset..."));
```

```
## Please Wait! Load C:/Users/RW/Documents/activity.csv  to dataset...
```

```r
activity <- read.csv(txt_file, header=TRUE,   sep=",",
                    colClasses=c("numeric", "character", "numeric"))
activity$interval <- factor(activity$interval)
activity$date <- as.Date(activity$date, format="%Y-%m-%d")
```
## What is mean total number of steps taken per day?

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 

The MEAN of total number of steps taken per day is 10766.19  
The MEDIAN of total number of steps taken per day is 10765

## What is the average daily activity pattern?

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png) 


Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?  
835  

## Inputing missing values



The total number of missing values in the dataset is 2304 rows.


```r
## Filling in missing values  
fill_missing <- function(data, defaults) {
        na_indices <- which(is.na(data$steps))
        na_replacements <- unlist(lapply(na_indices, FUN=function(idx){
                interval = data[idx,]$interval
                defaults[defaults$interval == interval,]$steps
        }))
        fill_steps <- data$steps
        fill_steps[na_indices] <- na_replacements
        fill_steps
}

##  New dataset with the missing data filled in
data_fill <- data.frame(  
        steps = fill_missing(activity, steps_time_interval),  
        date = activity$date,  
        interval = activity$interval)
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png) 

Mean of total number of steps (with missing) taken per day is 10766.19.  
Median of total number of steps (with missing) taken per day is 10766.19.  

These values DO differ from the estimates from the not filled ones 

The MEAN of total number of steps taken per day is 10766.19  
The MEDIAN of total number of steps taken per day is 10765

Despite missing values had increased the peak, the impact on the predictions is not significant  


  
##Are there differences in activity patterns between weekdays and weekends?


![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9-1.png) 
