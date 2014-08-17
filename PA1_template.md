Reproducible Research: Programming Assignment 1
========================================================



This is a submit to the first programming assigment MOOC **'Reproducible Research'**
from August 2014, offered by Coursera in Cooperation with the John Hopkins
Bloomberg School of Public Health.

The objective of the assignemnt is to analyze a dataset collected by one anonymous person using a personal activity monitoring device like 'Nike Fuelband' or similar. The dataset is comprised of measurements of three variables. Each measurement represents a five minute time intervall during the months of October and November 2012. The **three variables** are: **'STEPS'** (number of steps the person has taken in the measured five minute intervall), **'DATE'** (date that that the measurement was done) and RVALL'* (identifier for the five minute intervall of a measurement)

The dataset was provided by the staff of the John Hopkins Bloomberg School for Public Health and was contained in a file named **'repdata_data_activity.zip'** which includes a single file named **'activity.csv'**. The zip-file is contained in the github-repository **'http://github.com/rdpeng/RepData_PeerAssessment1'** that the course instructors created for the purpose of this programming assignment.

The following reproducible analysis will be conducted according to the instructions for the programming assignment, as stated on the Coursera website in the data for the 'Reproducible Research' course. It will contain the following **7 steps**, all of which will be addresses in this Markdown-file:

*Note: Running the analysis file requires having the 'knitr'- and 'ggplot2'-packages installed.*

1. Forking the github repository 'http://github.com/rdpeng/RepData_PeerAssessment1' and cloning it to a local copy

2. Loading and pre-processing the dataset from the .csv-file

3. Calculating the mean total number of steps taken per day and plotting a histogram of the calculated results

4. Analysing and plotting the average daily activitiy pattern and identifying the five minute intervall that, on average of all days in the dataset, contains the maximum number of steps.

5. Imputing missing values (which are labeled 'NA') and redo step 5, including the imputed data. This step includes four sub-steps which will be explained later in this file.

6. Analyzing, if there are any differences in activity patterns between weekdays and weekends.

7. Commiting the finished Markdown-file first to your local repository, then to your forked online-repository at 'github.com' and submit a link to the forked online-repository as well as the SHA-1 hash for this repository to the course page on the coursera website.


## Step 1: Forking the github repository 'http://github.com/rdpeng/RepData_PeerAssessment1' and cloning it to a local copy:

This is neither an R nor a Markdown task, so the specifics on *HOW* to do this will not be presented here (the instructions to the assignment also do not require a description)

Please note, that I assume that you UNZIP the zipped date file in your local repository before you run this analysis Markdown file.

## Step 2: Loading and pre-processing the dataset from the .csv-file:

Load the data file in a dataframe called 'data':

```r
data<-read.csv("activity.csv")
```

Take a look at the data:

```r
str(data)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

As I am working on a computer whose system language is not english, I have to tell knitr to use english, by using the following code (I also add a 'weekday' columnn to the data, which will be needed later on):


```r
Sys.setlocale("LC_TIME", "English") 
```

```
## [1] "English_United States.1252"
```

```r
date = strptime(as.character(data[,2]), "%Y-%m-%d") 
wday = weekdays(date) 
steps = data[,1] 
interval = data[,3] 
data = data.frame(date, wday, steps, interval)
```

Let's have another look:

```r
str(data)
```

```
## 'data.frame':	17568 obs. of  4 variables:
##  $ date    : POSIXct, format: "2012-10-01" "2012-10-01" ...
##  $ wday    : Factor w/ 7 levels "Friday","Monday",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```


## Step 3: Calculating the mean total number of steps taken per day and plotting a histogram of the calculated results:

Calculate the sum of taken steps for each day in the observed time period of two months:


```r
histogram_data<-as.data.frame(tapply(data$steps,data$date,sum))
names(histogram_data)<-"steps_taken"
histogram_data
```

```
##            steps_taken
## 2012-10-01          NA
## 2012-10-02         126
## 2012-10-03       11352
## 2012-10-04       12116
## 2012-10-05       13294
## 2012-10-06       15420
## 2012-10-07       11015
## 2012-10-08          NA
## 2012-10-09       12811
## 2012-10-10        9900
## 2012-10-11       10304
## 2012-10-12       17382
## 2012-10-13       12426
## 2012-10-14       15098
## 2012-10-15       10139
## 2012-10-16       15084
## 2012-10-17       13452
## 2012-10-18       10056
## 2012-10-19       11829
## 2012-10-20       10395
## 2012-10-21        8821
## 2012-10-22       13460
## 2012-10-23        8918
## 2012-10-24        8355
## 2012-10-25        2492
## 2012-10-26        6778
## 2012-10-27       10119
## 2012-10-28       11458
## 2012-10-29        5018
## 2012-10-30        9819
## 2012-10-31       15414
## 2012-11-01          NA
## 2012-11-02       10600
## 2012-11-03       10571
## 2012-11-04          NA
## 2012-11-05       10439
## 2012-11-06        8334
## 2012-11-07       12883
## 2012-11-08        3219
## 2012-11-09          NA
## 2012-11-10          NA
## 2012-11-11       12608
## 2012-11-12       10765
## 2012-11-13        7336
## 2012-11-14          NA
## 2012-11-15          41
## 2012-11-16        5441
## 2012-11-17       14339
## 2012-11-18       15110
## 2012-11-19        8841
## 2012-11-20        4472
## 2012-11-21       12787
## 2012-11-22       20427
## 2012-11-23       21194
## 2012-11-24       14478
## 2012-11-25       11834
## 2012-11-26       11162
## 2012-11-27       13646
## 2012-11-28       10183
## 2012-11-29        7047
## 2012-11-30          NA
```

And plot a histogram of the total number of steps taken per day:


```r
plot<-ggplot(histogram_data, aes(x=steps_taken))
plot<- plot + geom_histogram(binwidth=500) + labs(y="number of days") + labs(x="steps taken per day")
print(plot)
```

![plot of chunk plot histogram of steps per day](figure/plot histogram of steps per day.png) 

Now calculate the mean of total steps taken per day:

```r
mean(histogram_data[,1], na.rm=TRUE)
```

```
## [1] 10766
```

And the median of total steps taken per day:

```r
median(histogram_data[,1], na.rm=TRUE)
```

```
## [1] 10765
```

## Step 4: Analysing and plotting the average daily activitiy pattern and identifying the five minute intervall that, on average of all days in the dataset, contains the maximum number of steps:

First, calculate the average number of steps taken (averaged across all days) to create the daily activity pattern:


```r
ActivityPattern<-aggregate(steps ~ interval, data, mean)
ActivityPattern
```

```
##     interval     steps
## 1          0   1.71698
## 2          5   0.33962
## 3         10   0.13208
## 4         15   0.15094
## 5         20   0.07547
## 6         25   2.09434
## 7         30   0.52830
## 8         35   0.86792
## 9         40   0.00000
## 10        45   1.47170
## 11        50   0.30189
## 12        55   0.13208
## 13       100   0.32075
## 14       105   0.67925
## 15       110   0.15094
## 16       115   0.33962
## 17       120   0.00000
## 18       125   1.11321
## 19       130   1.83019
## 20       135   0.16981
## 21       140   0.16981
## 22       145   0.37736
## 23       150   0.26415
## 24       155   0.00000
## 25       200   0.00000
## 26       205   0.00000
## 27       210   1.13208
## 28       215   0.00000
## 29       220   0.00000
## 30       225   0.13208
## 31       230   0.00000
## 32       235   0.22642
## 33       240   0.00000
## 34       245   0.00000
## 35       250   1.54717
## 36       255   0.94340
## 37       300   0.00000
## 38       305   0.00000
## 39       310   0.00000
## 40       315   0.00000
## 41       320   0.20755
## 42       325   0.62264
## 43       330   1.62264
## 44       335   0.58491
## 45       340   0.49057
## 46       345   0.07547
## 47       350   0.00000
## 48       355   0.00000
## 49       400   1.18868
## 50       405   0.94340
## 51       410   2.56604
## 52       415   0.00000
## 53       420   0.33962
## 54       425   0.35849
## 55       430   4.11321
## 56       435   0.66038
## 57       440   3.49057
## 58       445   0.83019
## 59       450   3.11321
## 60       455   1.11321
## 61       500   0.00000
## 62       505   1.56604
## 63       510   3.00000
## 64       515   2.24528
## 65       520   3.32075
## 66       525   2.96226
## 67       530   2.09434
## 68       535   6.05660
## 69       540  16.01887
## 70       545  18.33962
## 71       550  39.45283
## 72       555  44.49057
## 73       600  31.49057
## 74       605  49.26415
## 75       610  53.77358
## 76       615  63.45283
## 77       620  49.96226
## 78       625  47.07547
## 79       630  52.15094
## 80       635  39.33962
## 81       640  44.01887
## 82       645  44.16981
## 83       650  37.35849
## 84       655  49.03774
## 85       700  43.81132
## 86       705  44.37736
## 87       710  50.50943
## 88       715  54.50943
## 89       720  49.92453
## 90       725  50.98113
## 91       730  55.67925
## 92       735  44.32075
## 93       740  52.26415
## 94       745  69.54717
## 95       750  57.84906
## 96       755  56.15094
## 97       800  73.37736
## 98       805  68.20755
## 99       810 129.43396
## 100      815 157.52830
## 101      820 171.15094
## 102      825 155.39623
## 103      830 177.30189
## 104      835 206.16981
## 105      840 195.92453
## 106      845 179.56604
## 107      850 183.39623
## 108      855 167.01887
## 109      900 143.45283
## 110      905 124.03774
## 111      910 109.11321
## 112      915 108.11321
## 113      920 103.71698
## 114      925  95.96226
## 115      930  66.20755
## 116      935  45.22642
## 117      940  24.79245
## 118      945  38.75472
## 119      950  34.98113
## 120      955  21.05660
## 121     1000  40.56604
## 122     1005  26.98113
## 123     1010  42.41509
## 124     1015  52.66038
## 125     1020  38.92453
## 126     1025  50.79245
## 127     1030  44.28302
## 128     1035  37.41509
## 129     1040  34.69811
## 130     1045  28.33962
## 131     1050  25.09434
## 132     1055  31.94340
## 133     1100  31.35849
## 134     1105  29.67925
## 135     1110  21.32075
## 136     1115  25.54717
## 137     1120  28.37736
## 138     1125  26.47170
## 139     1130  33.43396
## 140     1135  49.98113
## 141     1140  42.03774
## 142     1145  44.60377
## 143     1150  46.03774
## 144     1155  59.18868
## 145     1200  63.86792
## 146     1205  87.69811
## 147     1210  94.84906
## 148     1215  92.77358
## 149     1220  63.39623
## 150     1225  50.16981
## 151     1230  54.47170
## 152     1235  32.41509
## 153     1240  26.52830
## 154     1245  37.73585
## 155     1250  45.05660
## 156     1255  67.28302
## 157     1300  42.33962
## 158     1305  39.88679
## 159     1310  43.26415
## 160     1315  40.98113
## 161     1320  46.24528
## 162     1325  56.43396
## 163     1330  42.75472
## 164     1335  25.13208
## 165     1340  39.96226
## 166     1345  53.54717
## 167     1350  47.32075
## 168     1355  60.81132
## 169     1400  55.75472
## 170     1405  51.96226
## 171     1410  43.58491
## 172     1415  48.69811
## 173     1420  35.47170
## 174     1425  37.54717
## 175     1430  41.84906
## 176     1435  27.50943
## 177     1440  17.11321
## 178     1445  26.07547
## 179     1450  43.62264
## 180     1455  43.77358
## 181     1500  30.01887
## 182     1505  36.07547
## 183     1510  35.49057
## 184     1515  38.84906
## 185     1520  45.96226
## 186     1525  47.75472
## 187     1530  48.13208
## 188     1535  65.32075
## 189     1540  82.90566
## 190     1545  98.66038
## 191     1550 102.11321
## 192     1555  83.96226
## 193     1600  62.13208
## 194     1605  64.13208
## 195     1610  74.54717
## 196     1615  63.16981
## 197     1620  56.90566
## 198     1625  59.77358
## 199     1630  43.86792
## 200     1635  38.56604
## 201     1640  44.66038
## 202     1645  45.45283
## 203     1650  46.20755
## 204     1655  43.67925
## 205     1700  46.62264
## 206     1705  56.30189
## 207     1710  50.71698
## 208     1715  61.22642
## 209     1720  72.71698
## 210     1725  78.94340
## 211     1730  68.94340
## 212     1735  59.66038
## 213     1740  75.09434
## 214     1745  56.50943
## 215     1750  34.77358
## 216     1755  37.45283
## 217     1800  40.67925
## 218     1805  58.01887
## 219     1810  74.69811
## 220     1815  85.32075
## 221     1820  59.26415
## 222     1825  67.77358
## 223     1830  77.69811
## 224     1835  74.24528
## 225     1840  85.33962
## 226     1845  99.45283
## 227     1850  86.58491
## 228     1855  85.60377
## 229     1900  84.86792
## 230     1905  77.83019
## 231     1910  58.03774
## 232     1915  53.35849
## 233     1920  36.32075
## 234     1925  20.71698
## 235     1930  27.39623
## 236     1935  40.01887
## 237     1940  30.20755
## 238     1945  25.54717
## 239     1950  45.66038
## 240     1955  33.52830
## 241     2000  19.62264
## 242     2005  19.01887
## 243     2010  19.33962
## 244     2015  33.33962
## 245     2020  26.81132
## 246     2025  21.16981
## 247     2030  27.30189
## 248     2035  21.33962
## 249     2040  19.54717
## 250     2045  21.32075
## 251     2050  32.30189
## 252     2055  20.15094
## 253     2100  15.94340
## 254     2105  17.22642
## 255     2110  23.45283
## 256     2115  19.24528
## 257     2120  12.45283
## 258     2125   8.01887
## 259     2130  14.66038
## 260     2135  16.30189
## 261     2140   8.67925
## 262     2145   7.79245
## 263     2150   8.13208
## 264     2155   2.62264
## 265     2200   1.45283
## 266     2205   3.67925
## 267     2210   4.81132
## 268     2215   8.50943
## 269     2220   7.07547
## 270     2225   8.69811
## 271     2230   9.75472
## 272     2235   2.20755
## 273     2240   0.32075
## 274     2245   0.11321
## 275     2250   1.60377
## 276     2255   4.60377
## 277     2300   3.30189
## 278     2305   2.84906
## 279     2310   0.00000
## 280     2315   0.83019
## 281     2320   0.96226
## 282     2325   1.58491
## 283     2330   2.60377
## 284     2335   4.69811
## 285     2340   3.30189
## 286     2345   0.64151
## 287     2350   0.22642
## 288     2355   1.07547
```

Now create the plot:

```r
plotActivityPattern<-ggplot(ActivityPattern,aes(interval,steps)) + geom_line() + ylab("average steps") + xlab("interval")
print(plotActivityPattern)
```

![plot of chunk plot activity pattern](figure/plot activity pattern.png) 

And find out, which interval has the highest average number of steps taken:

```r
ActivityPattern$interval[which.max(ActivityPattern$steps)]
```

```
## [1] 835
```
And show the average number of steps for this intervall:

```r
max(ActivityPattern$steps)
```

```
## [1] 206.2
```


## Step 5: Imputing missing values (which are labeled 'NA') and redo step 3, including the imputed data. 

Let's see, how many values of the 'steps' variable are missing in the dataset:

```r
missings = is.na(steps) 
sum(missings) 
```

```
## [1] 2304
```

Now create a clone of the original dataset and impute the missing values with the mean value for the corresponding interval:

```r
dataClone <- data
fn <- function(steps,interval) ifelse(is.na(steps),ActivityPattern$steps[ActivityPattern$interval==interval],steps) 
dataClone$steps <- apply( data[,c('steps','interval')] ,1, function(y) fn(y['steps'],y['interval']) ) 
```

Now, with the cloned and imputed dataset, calculate again the sum of taken steps for each day in the observed time period of two months:


```r
histogram_data<-as.data.frame(tapply(dataClone$steps,dataClone$date,sum))
names(histogram_data)<-"steps_taken"
histogram_data
```

```
##            steps_taken
## 2012-10-01       10766
## 2012-10-02         126
## 2012-10-03       11352
## 2012-10-04       12116
## 2012-10-05       13294
## 2012-10-06       15420
## 2012-10-07       11015
## 2012-10-08       10766
## 2012-10-09       12811
## 2012-10-10        9900
## 2012-10-11       10304
## 2012-10-12       17382
## 2012-10-13       12426
## 2012-10-14       15098
## 2012-10-15       10139
## 2012-10-16       15084
## 2012-10-17       13452
## 2012-10-18       10056
## 2012-10-19       11829
## 2012-10-20       10395
## 2012-10-21        8821
## 2012-10-22       13460
## 2012-10-23        8918
## 2012-10-24        8355
## 2012-10-25        2492
## 2012-10-26        6778
## 2012-10-27       10119
## 2012-10-28       11458
## 2012-10-29        5018
## 2012-10-30        9819
## 2012-10-31       15414
## 2012-11-01       10766
## 2012-11-02       10600
## 2012-11-03       10571
## 2012-11-04       10766
## 2012-11-05       10439
## 2012-11-06        8334
## 2012-11-07       12883
## 2012-11-08        3219
## 2012-11-09       10766
## 2012-11-10       10766
## 2012-11-11       12608
## 2012-11-12       10765
## 2012-11-13        7336
## 2012-11-14       10766
## 2012-11-15          41
## 2012-11-16        5441
## 2012-11-17       14339
## 2012-11-18       15110
## 2012-11-19        8841
## 2012-11-20        4472
## 2012-11-21       12787
## 2012-11-22       20427
## 2012-11-23       21194
## 2012-11-24       14478
## 2012-11-25       11834
## 2012-11-26       11162
## 2012-11-27       13646
## 2012-11-28       10183
## 2012-11-29        7047
## 2012-11-30       10766
```

And plot a histogram of the total number of steps taken per day:


```r
plot<-ggplot(histogram_data, aes(x=steps_taken))
plot<- plot + geom_histogram(binwidth=500) + labs(y="number of days") + labs(x="steps taken per day")
print(plot)
```

![plot of chunk plot histogram of steps per day for clone](figure/plot histogram of steps per day for clone.png) 

Now calculate the mean of total steps taken per day:

```r
mean(histogram_data[,1], na.rm=TRUE)
```

```
## [1] 10766
```

And the median of total steps taken per day:

```r
median(histogram_data[,1], na.rm=TRUE)
```

```
## [1] 10766
```


## Step 6: Analyzing, if there are any differences in activity patterns between weekdays and weekends.

First, add a new factor variable to the cloned and imputed dataset, to differentiate between weekdays and weekends:


```r
FUN <- function(d) ifelse(is.na(match(weekdays(d),c("Saturday","Sunday"))),"weekday","weekend")
dataClone$weekendYN <- factor(apply(dataClone[c('date')],1,function(y) FUN(as.Date(y['date']))))
str(dataClone)
```

```
## 'data.frame':	17568 obs. of  5 variables:
##  $ date     : POSIXct, format: "2012-10-01" "2012-10-01" ...
##  $ wday     : Factor w/ 7 levels "Friday","Monday",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ steps    : num  1.717 0.3396 0.1321 0.1509 0.0755 ...
##  $ interval : int  0 5 10 15 20 25 30 35 40 45 ...
##  $ weekendYN: Factor w/ 2 levels "weekday","weekend": 1 1 1 1 1 1 1 1 1 1 ...
```

Then create a plot to see, if the average activity patterns for a timeline of intervals differ between weekdays and weekends:



```r
ActivityPatternClone<-aggregate(dataClone$steps, list(dataClone$interval,dataClone$weekendYN), mean)
ActivityPatternClone
```

```
##     Group.1 Group.2         x
## 1         0 weekday 2.251e+00
## 2         5 weekday 4.453e-01
## 3        10 weekday 1.732e-01
## 4        15 weekday 1.979e-01
## 5        20 weekday 9.895e-02
## 6        25 weekday 1.590e+00
## 7        30 weekday 6.927e-01
## 8        35 weekday 1.138e+00
## 9        40 weekday 0.000e+00
## 10       45 weekday 1.796e+00
## 11       50 weekday 3.958e-01
## 12       55 weekday 1.761e-02
## 13      100 weekday 4.205e-01
## 14      105 weekday 9.057e-02
## 15      110 weekday 1.979e-01
## 16      115 weekday 4.453e-01
## 17      120 weekday 0.000e+00
## 18      125 weekday 1.460e+00
## 19      130 weekday 2.222e+00
## 20      135 weekday 2.264e-02
## 21      140 weekday 2.226e-01
## 22      145 weekday 2.503e-01
## 23      150 weekday 3.463e-01
## 24      155 weekday 0.000e+00
## 25      200 weekday 0.000e+00
## 26      205 weekday 0.000e+00
## 27      210 weekday 1.395e+00
## 28      215 weekday 0.000e+00
## 29      220 weekday 0.000e+00
## 30      225 weekday 1.732e-01
## 31      230 weekday 0.000e+00
## 32      235 weekday 2.969e-01
## 33      240 weekday 0.000e+00
## 34      245 weekday 0.000e+00
## 35      250 weekday 2.029e+00
## 36      255 weekday 1.237e+00
## 37      300 weekday 0.000e+00
## 38      305 weekday 0.000e+00
## 39      310 weekday 0.000e+00
## 40      315 weekday 0.000e+00
## 41      320 weekday 2.767e-02
## 42      325 weekday 8.164e-01
## 43      330 weekday 1.239e+00
## 44      335 weekday 5.224e-01
## 45      340 weekday 4.210e-01
## 46      345 weekday 9.895e-02
## 47      350 weekday 0.000e+00
## 48      355 weekday 0.000e+00
## 49      400 weekday 2.696e-01
## 50      405 weekday 1.237e+00
## 51      410 weekday 2.231e+00
## 52      415 weekday 0.000e+00
## 53      420 weekday 4.453e-01
## 54      425 weekday 4.780e-02
## 55      430 weekday 3.371e+00
## 56      435 weekday 2.214e-01
## 57      440 weekday 3.777e+00
## 58      445 weekday 8.885e-01
## 59      450 weekday 2.348e+00
## 60      455 weekday 7.262e-01
## 61      500 weekday 0.000e+00
## 62      505 weekday 2.053e+00
## 63      510 weekday 3.933e+00
## 64      515 weekday 2.188e+00
## 65      520 weekday 4.221e+00
## 66      525 weekday 2.706e+00
## 67      530 weekday 2.746e+00
## 68      535 weekday 7.941e+00
## 69      540 weekday 2.040e+01
## 70      545 weekday 2.365e+01
## 71      550 weekday 5.035e+01
## 72      555 weekday 5.627e+01
## 73      600 weekday 4.129e+01
## 74      605 weekday 6.459e+01
## 75      610 weekday 7.008e+01
## 76      615 weekday 7.715e+01
## 77      620 weekday 6.393e+01
## 78      625 weekday 6.003e+01
## 79      630 weekday 6.644e+01
## 80      635 weekday 4.798e+01
## 81      640 weekday 5.567e+01
## 82      645 weekday 5.487e+01
## 83      650 weekday 4.705e+01
## 84      655 weekday 6.043e+01
## 85      700 weekday 5.060e+01
## 86      705 weekday 5.083e+01
## 87      710 weekday 6.202e+01
## 88      715 weekday 6.933e+01
## 89      720 weekday 6.310e+01
## 90      725 weekday 5.911e+01
## 91      730 weekday 6.622e+01
## 92      735 weekday 5.435e+01
## 93      740 weekday 6.272e+01
## 94      745 weekday 8.338e+01
## 95      750 weekday 6.774e+01
## 96      755 weekday 6.658e+01
## 97      800 weekday 8.272e+01
## 98      805 weekday 7.196e+01
## 99      810 weekday 1.440e+02
## 100     815 weekday 1.820e+02
## 101     820 weekday 2.006e+02
## 102     825 weekday 1.836e+02
## 103     830 weekday 1.989e+02
## 104     835 weekday 2.304e+02
## 105     840 weekday 2.189e+02
## 106     845 weekday 1.857e+02
## 107     850 weekday 1.912e+02
## 108     855 weekday 1.771e+02
## 109     900 weekday 1.677e+02
## 110     905 weekday 1.258e+02
## 111     910 weekday 9.395e+01
## 112     915 weekday 8.730e+01
## 113     920 weekday 1.035e+02
## 114     925 weekday 9.246e+01
## 115     930 weekday 5.852e+01
## 116     935 weekday 3.585e+01
## 117     940 weekday 2.746e+01
## 118     945 weekday 4.086e+01
## 119     950 weekday 3.913e+01
## 120     955 weekday 1.763e+01
## 121    1000 weekday 3.788e+01
## 122    1005 weekday 1.822e+01
## 123    1010 weekday 3.908e+01
## 124    1015 weekday 4.782e+01
## 125    1020 weekday 3.035e+01
## 126    1025 weekday 3.515e+01
## 127    1030 weekday 3.313e+01
## 128    1035 weekday 2.426e+01
## 129    1040 weekday 2.352e+01
## 130    1045 weekday 2.591e+01
## 131    1050 weekday 2.203e+01
## 132    1055 weekday 2.326e+01
## 133    1100 weekday 2.169e+01
## 134    1105 weekday 2.509e+01
## 135    1110 weekday 1.169e+01
## 136    1115 weekday 1.627e+01
## 137    1120 weekday 2.418e+01
## 138    1125 weekday 2.373e+01
## 139    1130 weekday 3.277e+01
## 140    1135 weekday 5.020e+01
## 141    1140 weekday 4.456e+01
## 142    1145 weekday 4.792e+01
## 143    1150 weekday 5.012e+01
## 144    1155 weekday 5.614e+01
## 145    1200 weekday 5.572e+01
## 146    1205 weekday 7.285e+01
## 147    1210 weekday 8.365e+01
## 148    1215 weekday 7.528e+01
## 149    1220 weekday 4.872e+01
## 150    1225 weekday 4.682e+01
## 151    1230 weekday 6.257e+01
## 152    1235 weekday 3.074e+01
## 153    1240 weekday 2.198e+01
## 154    1245 weekday 2.932e+01
## 155    1250 weekday 3.279e+01
## 156    1255 weekday 5.659e+01
## 157    1300 weekday 2.460e+01
## 158    1305 weekday 2.574e+01
## 159    1310 weekday 2.457e+01
## 160    1315 weekday 1.564e+01
## 161    1320 weekday 3.563e+01
## 162    1325 weekday 4.486e+01
## 163    1330 weekday 3.177e+01
## 164    1335 weekday 2.331e+01
## 165    1340 weekday 2.524e+01
## 166    1345 weekday 4.018e+01
## 167    1350 weekday 2.558e+01
## 168    1355 weekday 3.633e+01
## 169    1400 weekday 4.692e+01
## 170    1405 weekday 3.955e+01
## 171    1410 weekday 3.212e+01
## 172    1415 weekday 4.505e+01
## 173    1420 weekday 2.749e+01
## 174    1425 weekday 3.076e+01
## 175    1430 weekday 3.149e+01
## 176    1435 weekday 1.451e+01
## 177    1440 weekday 1.155e+01
## 178    1445 weekday 2.199e+01
## 179    1450 weekday 4.186e+01
## 180    1455 weekday 3.828e+01
## 181    1500 weekday 3.087e+01
## 182    1505 weekday 3.505e+01
## 183    1510 weekday 2.995e+01
## 184    1515 weekday 3.191e+01
## 185    1520 weekday 3.986e+01
## 186    1525 weekday 3.735e+01
## 187    1530 weekday 4.213e+01
## 188    1535 weekday 5.093e+01
## 189    1540 weekday 9.057e+01
## 190    1545 weekday 9.587e+01
## 191    1550 weekday 9.395e+01
## 192    1555 weekday 7.031e+01
## 193    1600 weekday 4.688e+01
## 194    1605 weekday 4.520e+01
## 195    1610 weekday 5.661e+01
## 196    1615 weekday 3.613e+01
## 197    1620 weekday 2.681e+01
## 198    1625 weekday 2.953e+01
## 199    1630 weekday 2.252e+01
## 200    1635 weekday 2.183e+01
## 201    1640 weekday 2.587e+01
## 202    1645 weekday 3.199e+01
## 203    1650 weekday 2.763e+01
## 204    1655 weekday 3.242e+01
## 205    1700 weekday 2.357e+01
## 206    1705 weekday 4.495e+01
## 207    1710 weekday 3.418e+01
## 208    1715 weekday 4.807e+01
## 209    1720 weekday 6.012e+01
## 210    1725 weekday 7.237e+01
## 211    1730 weekday 5.615e+01
## 212    1735 weekday 6.582e+01
## 213    1740 weekday 8.288e+01
## 214    1745 weekday 5.933e+01
## 215    1750 weekday 3.450e+01
## 216    1755 weekday 3.759e+01
## 217    1800 weekday 2.665e+01
## 218    1805 weekday 4.662e+01
## 219    1810 weekday 6.723e+01
## 220    1815 weekday 8.264e+01
## 221    1820 weekday 6.139e+01
## 222    1825 weekday 7.346e+01
## 223    1830 weekday 7.923e+01
## 224    1835 weekday 8.150e+01
## 225    1840 weekday 9.171e+01
## 226    1845 weekday 1.155e+02
## 227    1850 weekday 1.013e+02
## 228    1855 weekday 9.059e+01
## 229    1900 weekday 8.756e+01
## 230    1905 weekday 7.722e+01
## 231    1910 weekday 6.238e+01
## 232    1915 weekday 5.438e+01
## 233    1920 weekday 3.789e+01
## 234    1925 weekday 2.056e+01
## 235    1930 weekday 2.910e+01
## 236    1935 weekday 4.598e+01
## 237    1940 weekday 3.005e+01
## 238    1945 weekday 1.858e+01
## 239    1950 weekday 4.427e+01
## 240    1955 weekday 2.729e+01
## 241    2000 weekday 1.339e+01
## 242    2005 weekday 5.558e+00
## 243    2010 weekday 6.823e+00
## 244    2015 weekday 1.411e+01
## 245    2020 weekday 8.708e+00
## 246    2025 weekday 5.712e+00
## 247    2030 weekday 9.774e+00
## 248    2035 weekday 7.156e+00
## 249    2040 weekday 8.962e+00
## 250    2045 weekday 1.311e+01
## 251    2050 weekday 2.597e+01
## 252    2055 weekday 1.731e+01
## 253    2100 weekday 1.137e+01
## 254    2105 weekday 1.890e+01
## 255    2110 weekday 2.850e+01
## 256    2115 weekday 1.894e+01
## 257    2120 weekday 1.428e+01
## 258    2125 weekday 8.047e+00
## 259    2130 weekday 1.280e+01
## 260    2135 weekday 1.651e+01
## 261    2140 weekday 7.135e+00
## 262    2145 weekday 7.595e+00
## 263    2150 weekday 8.262e+00
## 264    2155 weekday 3.439e+00
## 265    2200 weekday 1.527e+00
## 266    2205 weekday 4.424e+00
## 267    2210 weekday 6.308e+00
## 268    2215 weekday 1.116e+01
## 269    2220 weekday 9.277e+00
## 270    2225 weekday 1.085e+01
## 271    2230 weekday 1.279e+01
## 272    2235 weekday 2.894e+00
## 273    2240 weekday 4.277e-02
## 274    2245 weekday 1.484e-01
## 275    2250 weekday 1.903e+00
## 276    2255 weekday 2.014e+00
## 277    2300 weekday 3.551e+00
## 278    2305 weekday 3.735e+00
## 279    2310 weekday 0.000e+00
## 280    2315 weekday 1.088e+00
## 281    2320 weekday 1.262e+00
## 282    2325 weekday 1.878e+00
## 283    2330 weekday 3.036e+00
## 284    2335 weekday 2.249e+00
## 285    2340 weekday 2.240e+00
## 286    2345 weekday 2.633e-01
## 287    2350 weekday 2.969e-01
## 288    2355 weekday 1.410e+00
## 289       0 weekend 2.146e-01
## 290       5 weekend 4.245e-02
## 291      10 weekend 1.651e-02
## 292      15 weekend 1.887e-02
## 293      20 weekend 9.434e-03
## 294      25 weekend 3.512e+00
## 295      30 weekend 6.604e-02
## 296      35 weekend 1.085e-01
## 297      40 weekend 0.000e+00
## 298      45 weekend 5.590e-01
## 299      50 weekend 3.774e-02
## 300      55 weekend 4.540e-01
## 301     100 weekend 4.009e-02
## 302     105 weekend 2.335e+00
## 303     110 weekend 1.887e-02
## 304     115 weekend 4.245e-02
## 305     120 weekend 0.000e+00
## 306     125 weekend 1.392e-01
## 307     130 weekend 7.288e-01
## 308     135 weekend 5.837e-01
## 309     140 weekend 2.123e-02
## 310     145 weekend 7.347e-01
## 311     150 weekend 3.302e-02
## 312     155 weekend 0.000e+00
## 313     200 weekend 0.000e+00
## 314     205 weekend 0.000e+00
## 315     210 weekend 3.915e-01
## 316     215 weekend 0.000e+00
## 317     220 weekend 0.000e+00
## 318     225 weekend 1.651e-02
## 319     230 weekend 0.000e+00
## 320     235 weekend 2.830e-02
## 321     240 weekend 0.000e+00
## 322     245 weekend 0.000e+00
## 323     250 weekend 1.934e-01
## 324     255 weekend 1.179e-01
## 325     300 weekend 0.000e+00
## 326     305 weekend 0.000e+00
## 327     310 weekend 0.000e+00
## 328     315 weekend 0.000e+00
## 329     320 weekend 7.134e-01
## 330     325 weekend 7.783e-02
## 331     330 weekend 2.703e+00
## 332     335 weekend 7.606e-01
## 333     340 weekend 6.863e-01
## 334     345 weekend 9.434e-03
## 335     350 weekend 0.000e+00
## 336     355 weekend 0.000e+00
## 337     400 weekend 3.774e+00
## 338     405 weekend 1.179e-01
## 339     410 weekend 3.508e+00
## 340     415 weekend 0.000e+00
## 341     420 weekend 4.245e-02
## 342     425 weekend 1.232e+00
## 343     430 weekend 6.202e+00
## 344     435 weekend 1.895e+00
## 345     440 weekend 2.686e+00
## 346     445 weekend 6.663e-01
## 347     450 weekend 5.264e+00
## 348     455 weekend 2.202e+00
## 349     500 weekend 0.000e+00
## 350     505 weekend 1.958e-01
## 351     510 weekend 3.750e-01
## 352     515 weekend 2.406e+00
## 353     520 weekend 7.901e-01
## 354     525 weekend 3.683e+00
## 355     530 weekend 2.618e-01
## 356     535 weekend 7.571e-01
## 357     540 weekend 3.690e+00
## 358     545 weekend 3.417e+00
## 359     550 weekend 8.807e+00
## 360     555 weekend 1.137e+01
## 361     600 weekend 3.936e+00
## 362     605 weekend 6.158e+00
## 363     610 weekend 7.909e+00
## 364     615 weekend 2.493e+01
## 365     620 weekend 1.068e+01
## 366     625 weekend 1.063e+01
## 367     630 weekend 1.196e+01
## 368     635 weekend 1.504e+01
## 369     640 weekend 1.125e+01
## 370     645 weekend 1.408e+01
## 371     650 weekend 1.011e+01
## 372     655 weekend 1.700e+01
## 373     700 weekend 2.473e+01
## 374     705 weekend 2.623e+01
## 375     710 weekend 1.813e+01
## 376     715 weekend 1.281e+01
## 377     720 weekend 1.287e+01
## 378     725 weekend 2.812e+01
## 379     730 weekend 2.602e+01
## 380     735 weekend 1.610e+01
## 381     740 weekend 2.285e+01
## 382     745 weekend 3.063e+01
## 383     750 weekend 3.004e+01
## 384     755 weekend 2.683e+01
## 385     800 weekend 4.711e+01
## 386     805 weekend 5.765e+01
## 387     810 weekend 8.843e+01
## 388     815 weekend 8.875e+01
## 389     820 weekend 8.839e+01
## 390     825 weekend 7.605e+01
## 391     830 weekend 1.166e+02
## 392     835 weekend 1.381e+02
## 393     840 weekend 1.313e+02
## 394     845 weekend 1.624e+02
## 395     850 weekend 1.614e+02
## 396     855 weekend 1.387e+02
## 397     900 weekend 7.537e+01
## 398     905 weekend 1.191e+02
## 399     910 weekend 1.518e+02
## 400     915 weekend 1.666e+02
## 401     920 weekend 1.042e+02
## 402     925 weekend 1.058e+02
## 403     930 weekend 8.784e+01
## 404     935 weekend 7.159e+01
## 405     940 weekend 1.729e+01
## 406     945 weekend 3.284e+01
## 407     950 weekend 2.331e+01
## 408     955 weekend 3.069e+01
## 409    1000 weekend 4.813e+01
## 410    1005 weekend 5.162e+01
## 411    1010 weekend 5.180e+01
## 412    1015 weekend 6.627e+01
## 413    1020 weekend 6.305e+01
## 414    1025 weekend 9.479e+01
## 415    1030 weekend 7.566e+01
## 416    1035 weekend 7.443e+01
## 417    1040 weekend 6.615e+01
## 418    1045 weekend 3.517e+01
## 419    1050 weekend 3.370e+01
## 420    1055 weekend 5.637e+01
## 421    1100 weekend 5.854e+01
## 422    1105 weekend 4.258e+01
## 423    1110 weekend 4.842e+01
## 424    1115 weekend 5.163e+01
## 425    1120 weekend 4.017e+01
## 426    1125 weekend 3.418e+01
## 427    1130 weekend 3.530e+01
## 428    1135 weekend 4.937e+01
## 429    1140 weekend 3.494e+01
## 430    1145 weekend 3.526e+01
## 431    1150 weekend 3.457e+01
## 432    1155 weekend 6.777e+01
## 433    1200 weekend 8.680e+01
## 434    1205 weekend 1.295e+02
## 435    1210 weekend 1.264e+02
## 436    1215 weekend 1.420e+02
## 437    1220 weekend 1.047e+02
## 438    1225 weekend 5.958e+01
## 439    1230 weekend 3.168e+01
## 440    1235 weekend 3.711e+01
## 441    1240 weekend 3.932e+01
## 442    1245 weekend 6.140e+01
## 443    1250 weekend 7.957e+01
## 444    1255 weekend 9.735e+01
## 445    1300 weekend 9.223e+01
## 446    1305 weekend 7.967e+01
## 447    1310 weekend 9.585e+01
## 448    1315 weekend 1.122e+02
## 449    1320 weekend 7.609e+01
## 450    1325 weekend 8.899e+01
## 451    1330 weekend 7.366e+01
## 452    1335 weekend 3.027e+01
## 453    1340 weekend 8.137e+01
## 454    1345 weekend 9.113e+01
## 455    1350 weekend 1.085e+02
## 456    1355 weekend 1.297e+02
## 457    1400 weekend 8.059e+01
## 458    1405 weekend 8.687e+01
## 459    1410 weekend 7.582e+01
## 460    1415 weekend 5.896e+01
## 461    1420 weekend 5.793e+01
## 462    1425 weekend 5.663e+01
## 463    1430 weekend 7.098e+01
## 464    1435 weekend 6.406e+01
## 465    1440 weekend 3.276e+01
## 466    1445 weekend 3.757e+01
## 467    1450 weekend 4.858e+01
## 468    1455 weekend 5.922e+01
## 469    1500 weekend 2.763e+01
## 470    1505 weekend 3.895e+01
## 471    1510 weekend 5.106e+01
## 472    1515 weekend 5.836e+01
## 473    1520 weekend 6.312e+01
## 474    1525 weekend 7.703e+01
## 475    1530 weekend 6.502e+01
## 476    1535 weekend 1.058e+02
## 477    1540 weekend 6.136e+01
## 478    1545 weekend 1.065e+02
## 479    1550 weekend 1.251e+02
## 480    1555 weekend 1.224e+02
## 481    1600 weekend 1.050e+02
## 482    1605 weekend 1.174e+02
## 483    1610 weekend 1.250e+02
## 484    1615 weekend 1.392e+02
## 485    1620 weekend 1.416e+02
## 486    1625 weekend 1.448e+02
## 487    1630 weekend 1.039e+02
## 488    1635 weekend 8.563e+01
## 489    1640 weekend 9.752e+01
## 490    1645 weekend 8.331e+01
## 491    1650 weekend 9.846e+01
## 492    1655 weekend 7.533e+01
## 493    1700 weekend 1.115e+02
## 494    1705 weekend 8.823e+01
## 495    1710 weekend 9.721e+01
## 496    1715 weekend 9.822e+01
## 497    1720 weekend 1.082e+02
## 498    1725 weekend 9.743e+01
## 499    1730 weekend 1.049e+02
## 500    1735 weekend 4.233e+01
## 501    1740 weekend 5.320e+01
## 502    1745 weekend 4.856e+01
## 503    1750 weekend 3.553e+01
## 504    1755 weekend 3.706e+01
## 505    1800 weekend 8.015e+01
## 506    1805 weekend 9.006e+01
## 507    1810 weekend 9.571e+01
## 508    1815 weekend 9.285e+01
## 509    1820 weekend 5.328e+01
## 510    1825 weekend 5.178e+01
## 511    1830 weekend 7.340e+01
## 512    1835 weekend 5.384e+01
## 513    1840 weekend 6.742e+01
## 514    1845 weekend 5.443e+01
## 515    1850 weekend 4.520e+01
## 516    1855 weekend 7.158e+01
## 517    1900 weekend 7.730e+01
## 518    1905 weekend 7.954e+01
## 519    1910 weekend 4.582e+01
## 520    1915 weekend 5.048e+01
## 521    1920 weekend 3.192e+01
## 522    1925 weekend 2.115e+01
## 523    1930 weekend 2.261e+01
## 524    1935 weekend 2.325e+01
## 525    1940 weekend 3.065e+01
## 526    1945 weekend 4.513e+01
## 527    1950 weekend 4.958e+01
## 528    1955 weekend 5.107e+01
## 529    2000 weekend 3.714e+01
## 530    2005 weekend 5.688e+01
## 531    2010 weekend 5.454e+01
## 532    2015 weekend 8.742e+01
## 533    2020 weekend 7.773e+01
## 534    2025 weekend 6.465e+01
## 535    2030 weekend 7.660e+01
## 536    2035 weekend 6.123e+01
## 537    2040 weekend 4.932e+01
## 538    2045 weekend 4.442e+01
## 539    2050 weekend 5.010e+01
## 540    2055 weekend 2.814e+01
## 541    2100 weekend 2.881e+01
## 542    2105 weekend 1.253e+01
## 543    2110 weekend 9.244e+00
## 544    2115 weekend 2.009e+01
## 545    2120 weekend 7.307e+00
## 546    2125 weekend 7.940e+00
## 547    2130 weekend 1.990e+01
## 548    2135 weekend 1.573e+01
## 549    2140 weekend 1.302e+01
## 550    2145 weekend 8.349e+00
## 551    2150 weekend 7.767e+00
## 552    2155 weekend 3.278e-01
## 553    2200 weekend 1.244e+00
## 554    2205 weekend 1.585e+00
## 555    2210 weekend 6.014e-01
## 556    2215 weekend 1.064e+00
## 557    2220 weekend 8.844e-01
## 558    2225 weekend 2.650e+00
## 559    2230 weekend 1.219e+00
## 560    2235 weekend 2.759e-01
## 561    2240 weekend 1.103e+00
## 562    2245 weekend 1.415e-02
## 563    2250 weekend 7.630e-01
## 564    2255 weekend 1.189e+01
## 565    2300 weekend 2.600e+00
## 566    2305 weekend 3.561e-01
## 567    2310 weekend 0.000e+00
## 568    2315 weekend 1.038e-01
## 569    2320 weekend 1.203e-01
## 570    2325 weekend 7.606e-01
## 571    2330 weekend 1.388e+00
## 572    2335 weekend 1.159e+01
## 573    2340 weekend 6.288e+00
## 574    2345 weekend 1.705e+00
## 575    2350 weekend 2.830e-02
## 576    2355 weekend 1.344e-01
```

Now create the plot:

```r
ggplot(ActivityPatternClone, aes(x=Group.1, y=x)) + geom_line() + facet_grid( Group.2 ~ .) + labs(y="average number of steps per interval") + labs(x="interval")
```

![plot of chunk compare weekdays and weekends](figure/compare weekdays and weekends.png) 



## Step 7: Commiting the finished Markdown-file first to your local repository, then to your forked online-repository at 'github.com' and submit a link to the forked online-repository as well as the SHA-1 hash for this repository to the course page on the coursera website.

As with Step 1, this is neither an R nor a Markdown task, so the specifics on *HOW* to do this will not be presented here (the instructions to the assignment also do not require this description).



.
