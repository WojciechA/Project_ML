# Practical Machine Learning Prediction Project 

Author Name: Wojciech Ambrożewicz

#Introduction
Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. 
        In this project, our goal is  to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. More information is available from the website here:http://groupware.les.inf.puc-rio.br/har (see the section on the Weight Lifting Exercise Dataset).

# Data Processing 
The training data for this project are available here:

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

The test data are available here:

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv

## Download data and load into the data frame
We load given data to the data frame

```r
given_training<-read.csv("C:/COURSERA/Machine Learning/Project/pml-training.csv",stringsAsFactors=FALSE)
given_testing<-read.csv("C:/COURSERA/Machine Learning/Project/pml-testing.csv",stringsAsFactors=FALSE)
```

We count number of loaded into the data frame rows:


```r
nrow(given_training)
```

```
## [1] 19622
```

```r
nrow(given_testing)
```

```
## [1] 20
```

## Clean and process data
We analyze  contend of prepared data frame:


```r
str(given_testing)
```

```
## 'data.frame':	20 obs. of  160 variables:
##  $ X                       : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ user_name               : chr  "pedro" "jeremy" "jeremy" "adelmo" ...
##  $ raw_timestamp_part_1    : int  1323095002 1322673067 1322673075 1322832789 1322489635 1322673149 1322673128 1322673076 1323084240 1322837822 ...
##  $ raw_timestamp_part_2    : int  868349 778725 342967 560311 814776 510661 766645 54671 916313 384285 ...
##  $ cvtd_timestamp          : chr  "05/12/2011 14:23" "30/11/2011 17:11" "30/11/2011 17:11" "02/12/2011 13:33" ...
##  $ new_window              : chr  "no" "no" "no" "no" ...
##  $ num_window              : int  74 431 439 194 235 504 485 440 323 664 ...
##  $ roll_belt               : num  123 1.02 0.87 125 1.35 -5.92 1.2 0.43 0.93 114 ...
##  $ pitch_belt              : num  27 4.87 1.82 -41.6 3.33 1.59 4.44 4.15 6.72 22.4 ...
##  $ yaw_belt                : num  -4.75 -88.9 -88.5 162 -88.6 -87.7 -87.3 -88.5 -93.7 -13.1 ...
##  $ total_accel_belt        : int  20 4 5 17 3 4 4 4 4 18 ...
##  $ kurtosis_roll_belt      : logi  NA NA NA NA NA NA ...
##  $ kurtosis_picth_belt     : logi  NA NA NA NA NA NA ...
##  $ kurtosis_yaw_belt       : logi  NA NA NA NA NA NA ...
##  $ skewness_roll_belt      : logi  NA NA NA NA NA NA ...
##  $ skewness_roll_belt.1    : logi  NA NA NA NA NA NA ...
##  $ skewness_yaw_belt       : logi  NA NA NA NA NA NA ...
##  $ max_roll_belt           : logi  NA NA NA NA NA NA ...
##  $ max_picth_belt          : logi  NA NA NA NA NA NA ...
##  $ max_yaw_belt            : logi  NA NA NA NA NA NA ...
##  $ min_roll_belt           : logi  NA NA NA NA NA NA ...
##  $ min_pitch_belt          : logi  NA NA NA NA NA NA ...
##  $ min_yaw_belt            : logi  NA NA NA NA NA NA ...
##  $ amplitude_roll_belt     : logi  NA NA NA NA NA NA ...
##  $ amplitude_pitch_belt    : logi  NA NA NA NA NA NA ...
##  $ amplitude_yaw_belt      : logi  NA NA NA NA NA NA ...
##  $ var_total_accel_belt    : logi  NA NA NA NA NA NA ...
##  $ avg_roll_belt           : logi  NA NA NA NA NA NA ...
##  $ stddev_roll_belt        : logi  NA NA NA NA NA NA ...
##  $ var_roll_belt           : logi  NA NA NA NA NA NA ...
##  $ avg_pitch_belt          : logi  NA NA NA NA NA NA ...
##  $ stddev_pitch_belt       : logi  NA NA NA NA NA NA ...
##  $ var_pitch_belt          : logi  NA NA NA NA NA NA ...
##  $ avg_yaw_belt            : logi  NA NA NA NA NA NA ...
##  $ stddev_yaw_belt         : logi  NA NA NA NA NA NA ...
##  $ var_yaw_belt            : logi  NA NA NA NA NA NA ...
##  $ gyros_belt_x            : num  -0.5 -0.06 0.05 0.11 0.03 0.1 -0.06 -0.18 0.1 0.14 ...
##  $ gyros_belt_y            : num  -0.02 -0.02 0.02 0.11 0.02 0.05 0 -0.02 0 0.11 ...
##  $ gyros_belt_z            : num  -0.46 -0.07 0.03 -0.16 0 -0.13 0 -0.03 -0.02 -0.16 ...
##  $ accel_belt_x            : int  -38 -13 1 46 -8 -11 -14 -10 -15 -25 ...
##  $ accel_belt_y            : int  69 11 -1 45 4 -16 2 -2 1 63 ...
##  $ accel_belt_z            : int  -179 39 49 -156 27 38 35 42 32 -158 ...
##  $ magnet_belt_x           : int  -13 43 29 169 33 31 50 39 -6 10 ...
##  $ magnet_belt_y           : int  581 636 631 608 566 638 622 635 600 601 ...
##  $ magnet_belt_z           : int  -382 -309 -312 -304 -418 -291 -315 -305 -302 -330 ...
##  $ roll_arm                : num  40.7 0 0 -109 76.1 0 0 0 -137 -82.4 ...
##  $ pitch_arm               : num  -27.8 0 0 55 2.76 0 0 0 11.2 -63.8 ...
##  $ yaw_arm                 : num  178 0 0 -142 102 0 0 0 -167 -75.3 ...
##  $ total_accel_arm         : int  10 38 44 25 29 14 15 22 34 32 ...
##  $ var_accel_arm           : logi  NA NA NA NA NA NA ...
##  $ avg_roll_arm            : logi  NA NA NA NA NA NA ...
##  $ stddev_roll_arm         : logi  NA NA NA NA NA NA ...
##  $ var_roll_arm            : logi  NA NA NA NA NA NA ...
##  $ avg_pitch_arm           : logi  NA NA NA NA NA NA ...
##  $ stddev_pitch_arm        : logi  NA NA NA NA NA NA ...
##  $ var_pitch_arm           : logi  NA NA NA NA NA NA ...
##  $ avg_yaw_arm             : logi  NA NA NA NA NA NA ...
##  $ stddev_yaw_arm          : logi  NA NA NA NA NA NA ...
##  $ var_yaw_arm             : logi  NA NA NA NA NA NA ...
##  $ gyros_arm_x             : num  -1.65 -1.17 2.1 0.22 -1.96 0.02 2.36 -3.71 0.03 0.26 ...
##  $ gyros_arm_y             : num  0.48 0.85 -1.36 -0.51 0.79 0.05 -1.01 1.85 -0.02 -0.5 ...
##  $ gyros_arm_z             : num  -0.18 -0.43 1.13 0.92 -0.54 -0.07 0.89 -0.69 -0.02 0.79 ...
##  $ accel_arm_x             : int  16 -290 -341 -238 -197 -26 99 -98 -287 -301 ...
##  $ accel_arm_y             : int  38 215 245 -57 200 130 79 175 111 -42 ...
##  $ accel_arm_z             : int  93 -90 -87 6 -30 -19 -67 -78 -122 -80 ...
##  $ magnet_arm_x            : int  -326 -325 -264 -173 -170 396 702 535 -367 -420 ...
##  $ magnet_arm_y            : int  385 447 474 257 275 176 15 215 335 294 ...
##  $ magnet_arm_z            : int  481 434 413 633 617 516 217 385 520 493 ...
##  $ kurtosis_roll_arm       : logi  NA NA NA NA NA NA ...
##  $ kurtosis_picth_arm      : logi  NA NA NA NA NA NA ...
##  $ kurtosis_yaw_arm        : logi  NA NA NA NA NA NA ...
##  $ skewness_roll_arm       : logi  NA NA NA NA NA NA ...
##  $ skewness_pitch_arm      : logi  NA NA NA NA NA NA ...
##  $ skewness_yaw_arm        : logi  NA NA NA NA NA NA ...
##  $ max_roll_arm            : logi  NA NA NA NA NA NA ...
##  $ max_picth_arm           : logi  NA NA NA NA NA NA ...
##  $ max_yaw_arm             : logi  NA NA NA NA NA NA ...
##  $ min_roll_arm            : logi  NA NA NA NA NA NA ...
##  $ min_pitch_arm           : logi  NA NA NA NA NA NA ...
##  $ min_yaw_arm             : logi  NA NA NA NA NA NA ...
##  $ amplitude_roll_arm      : logi  NA NA NA NA NA NA ...
##  $ amplitude_pitch_arm     : logi  NA NA NA NA NA NA ...
##  $ amplitude_yaw_arm       : logi  NA NA NA NA NA NA ...
##  $ roll_dumbbell           : num  -17.7 54.5 57.1 43.1 -101.4 ...
##  $ pitch_dumbbell          : num  25 -53.7 -51.4 -30 -53.4 ...
##  $ yaw_dumbbell            : num  126.2 -75.5 -75.2 -103.3 -14.2 ...
##  $ kurtosis_roll_dumbbell  : logi  NA NA NA NA NA NA ...
##  $ kurtosis_picth_dumbbell : logi  NA NA NA NA NA NA ...
##  $ kurtosis_yaw_dumbbell   : logi  NA NA NA NA NA NA ...
##  $ skewness_roll_dumbbell  : logi  NA NA NA NA NA NA ...
##  $ skewness_pitch_dumbbell : logi  NA NA NA NA NA NA ...
##  $ skewness_yaw_dumbbell   : logi  NA NA NA NA NA NA ...
##  $ max_roll_dumbbell       : logi  NA NA NA NA NA NA ...
##  $ max_picth_dumbbell      : logi  NA NA NA NA NA NA ...
##  $ max_yaw_dumbbell        : logi  NA NA NA NA NA NA ...
##  $ min_roll_dumbbell       : logi  NA NA NA NA NA NA ...
##  $ min_pitch_dumbbell      : logi  NA NA NA NA NA NA ...
##  $ min_yaw_dumbbell        : logi  NA NA NA NA NA NA ...
##  $ amplitude_roll_dumbbell : logi  NA NA NA NA NA NA ...
##   [list output truncated]
```

From the whole training and testing data sets we want to extract only the part which we will probably need in our analyze. In order to do it we have to analyze its content more deeply.
We count a NA`s in each column:


```r
tna_re<-as.data.frame(sapply(given_testing, function(x) sum(is.na(x))))
na_re<-as.data.frame(sapply(given_training, function(x) sum(is.na(x))))
```

We count unique values in each column:


```r
tnr_uniq<-as.data.frame(sapply(given_testing, function(x) length(unique(x))))
nr_uniq<-as.data.frame(sapply(given_training, function(x) length(unique(x))))
```

Now we collect all this data together


```r
tstat<-cbind(tna_re,tnr_uniq)
names(tstat)<- c("tNA_count","tnr_unique")
stat<-cbind(na_re,nr_uniq)
names(stat)<- c("NA_count","nr_unique")
stat_all<-cbind(stat,tstat)
```
Let's see what we have:

```r
stat_all
```

```
##                          NA_count nr_unique tNA_count tnr_unique
## X                               0     19622         0         20
## user_name                       0         6         0          6
## raw_timestamp_part_1            0       837         0         20
## raw_timestamp_part_2            0     16783         0         20
## cvtd_timestamp                  0        20         0         11
## new_window                      0         2         0          1
## num_window                      0       858         0         20
## roll_belt                       0      1330         0         18
## pitch_belt                      0      1840         0         20
## yaw_belt                        0      1957         0         18
## total_accel_belt                0        29         0          9
## kurtosis_roll_belt              0       397        20          1
## kurtosis_picth_belt             0       317        20          1
## kurtosis_yaw_belt               0         2        20          1
## skewness_roll_belt              0       395        20          1
## skewness_roll_belt.1            0       338        20          1
## skewness_yaw_belt               0         2        20          1
## max_roll_belt               19216       196        20          1
## max_picth_belt              19216        23        20          1
## max_yaw_belt                    0        68        20          1
## min_roll_belt               19216       185        20          1
## min_pitch_belt              19216        17        20          1
## min_yaw_belt                    0        68        20          1
## amplitude_roll_belt         19216       149        20          1
## amplitude_pitch_belt        19216        14        20          1
## amplitude_yaw_belt              0         4        20          1
## var_total_accel_belt        19216        66        20          1
## avg_roll_belt               19216       192        20          1
## stddev_roll_belt            19216        70        20          1
## var_roll_belt               19216        97        20          1
## avg_pitch_belt              19216       215        20          1
## stddev_pitch_belt           19216        44        20          1
## var_pitch_belt              19216        64        20          1
## avg_yaw_belt                19216       241        20          1
## stddev_yaw_belt             19216        59        20          1
## var_yaw_belt                19216       146        20          1
## gyros_belt_x                    0       140         0         14
## gyros_belt_y                    0        69         0          6
## gyros_belt_z                    0       169         0         13
## accel_belt_x                    0       164         0         17
## accel_belt_y                    0       143         0         15
## accel_belt_z                    0       299         0         17
## magnet_belt_x                   0       327         0         20
## magnet_belt_y                   0       298         0         16
## magnet_belt_z                   0       457         0         16
## roll_arm                        0      2654         0         13
## pitch_arm                       0      3087         0         13
## yaw_arm                         0      2876         0         13
## total_accel_arm                 0        66         0         17
## var_accel_arm               19216       396        20          1
## avg_roll_arm                19216       331        20          1
## stddev_roll_arm             19216       331        20          1
## var_roll_arm                19216       331        20          1
## avg_pitch_arm               19216       331        20          1
## stddev_pitch_arm            19216       331        20          1
## var_pitch_arm               19216       331        20          1
## avg_yaw_arm                 19216       331        20          1
## stddev_yaw_arm              19216       328        20          1
## var_yaw_arm                 19216       328        20          1
## gyros_arm_x                     0       643         0         19
## gyros_arm_y                     0       376         0         19
## gyros_arm_z                     0       248         0         18
## accel_arm_x                     0       777         0         19
## accel_arm_y                     0       537         0         20
## accel_arm_z                     0       792         0         20
## magnet_arm_x                    0      1339         0         20
## magnet_arm_y                    0       872         0         19
## magnet_arm_z                    0      1265         0         20
## kurtosis_roll_arm               0       330        20          1
## kurtosis_picth_arm              0       328        20          1
## kurtosis_yaw_arm                0       395        20          1
## skewness_roll_arm               0       331        20          1
## skewness_pitch_arm              0       328        20          1
## skewness_yaw_arm                0       395        20          1
## max_roll_arm                19216       291        20          1
## max_picth_arm               19216       264        20          1
## max_yaw_arm                 19216        52        20          1
## min_roll_arm                19216       279        20          1
## min_pitch_arm               19216       291        20          1
## min_yaw_arm                 19216        39        20          1
## amplitude_roll_arm          19216       307        20          1
## amplitude_pitch_arm         19216       295        20          1
## amplitude_yaw_arm           19216        52        20          1
## roll_dumbbell                   0     16523         0         20
## pitch_dumbbell                  0     16040         0         20
## yaw_dumbbell                    0     16381         0         20
## kurtosis_roll_dumbbell          0       398        20          1
## kurtosis_picth_dumbbell         0       401        20          1
## kurtosis_yaw_dumbbell           0         2        20          1
## skewness_roll_dumbbell          0       401        20          1
## skewness_pitch_dumbbell         0       402        20          1
## skewness_yaw_dumbbell           0         2        20          1
## max_roll_dumbbell           19216       339        20          1
## max_picth_dumbbell          19216       340        20          1
## max_yaw_dumbbell                0        73        20          1
## min_roll_dumbbell           19216       333        20          1
## min_pitch_dumbbell          19216       357        20          1
## min_yaw_dumbbell                0        73        20          1
## amplitude_roll_dumbbell     19216       388        20          1
## amplitude_pitch_dumbbell    19216       384        20          1
## amplitude_yaw_dumbbell          0         3        20          1
## total_accel_dumbbell            0        43         0         14
## var_accel_dumbbell          19216       385        20          1
## avg_roll_dumbbell           19216       398        20          1
## stddev_roll_dumbbell        19216       392        20          1
## var_roll_dumbbell           19216       392        20          1
## avg_pitch_dumbbell          19216       398        20          1
## stddev_pitch_dumbbell       19216       392        20          1
## var_pitch_dumbbell          19216       392        20          1
## avg_yaw_dumbbell            19216       398        20          1
## stddev_yaw_dumbbell         19216       392        20          1
## var_yaw_dumbbell            19216       392        20          1
## gyros_dumbbell_x                0       241         0         18
## gyros_dumbbell_y                0       278         0         16
## gyros_dumbbell_z                0       206         0         17
## accel_dumbbell_x                0       425         0         20
## accel_dumbbell_y                0       466         0         18
## accel_dumbbell_z                0       410         0         19
## magnet_dumbbell_x               0      1128         0         20
## magnet_dumbbell_y               0       844         0         20
## magnet_dumbbell_z               0       676         0         19
## roll_forearm                    0      2176         0         19
## pitch_forearm                   0      2915         0         20
## yaw_forearm                     0      1991         0         20
## kurtosis_roll_forearm           0       322        20          1
## kurtosis_picth_forearm          0       323        20          1
## kurtosis_yaw_forearm            0         2        20          1
## skewness_roll_forearm           0       323        20          1
## skewness_pitch_forearm          0       319        20          1
## skewness_yaw_forearm            0         2        20          1
## max_roll_forearm            19216       272        20          1
## max_picth_forearm           19216       156        20          1
## max_yaw_forearm                 0        45        20          1
## min_roll_forearm            19216       270        20          1
## min_pitch_forearm           19216       172        20          1
## min_yaw_forearm                 0        45        20          1
## amplitude_roll_forearm      19216       294        20          1
## amplitude_pitch_forearm     19216       184        20          1
## amplitude_yaw_forearm           0         3        20          1
## total_accel_forearm             0        70         0         13
## var_accel_forearm           19216       400        20          1
## avg_roll_forearm            19216       323        20          1
## stddev_roll_forearm         19216       321        20          1
## var_roll_forearm            19216       321        20          1
## avg_pitch_forearm           19216       325        20          1
## stddev_pitch_forearm        19216       324        20          1
## var_pitch_forearm           19216       325        20          1
## avg_yaw_forearm             19216       325        20          1
## stddev_yaw_forearm          19216       323        20          1
## var_yaw_forearm             19216       323        20          1
## gyros_forearm_x                 0       298         0         18
## gyros_forearm_y                 0       741         0         20
## gyros_forearm_z                 0       307         0         20
## accel_forearm_x                 0       794         0         20
## accel_forearm_y                 0      1003         0         20
## accel_forearm_z                 0       580         0         20
## magnet_forearm_x                0      1524         0         20
## magnet_forearm_y                0      1872         0         20
## magnet_forearm_z                0      1683         0         20
## classe                          0         5         0         20
```

The is no need to use for prediction factors with Na's so we remove them


```r
xx<-subset(stat_all,NA_count==0)
xx<-subset(xx,tNA_count==0)
final<-xx[1:60,]
selected_columns<-rownames(final)
train_used<-given_training[,selected_columns]
```

#Results

In this part of analysis we are going to build tree prediction models with various set of features. In order  to make the comparable we will use only one, a decision tree, method.
In the beginning we make two partitions: 


```r
library(caret)
```

```
## Warning: package 'caret' was built under R version 3.2.4
```

```
## Loading required package: lattice
## Loading required package: ggplot2
```

```
## Warning: package 'ggplot2' was built under R version 3.2.3
```

```r
library(rpart)
train_part = createDataPartition(train_used$classe, p = 0.6, list = FALSE)
training_all = train_used[train_part, ]
testing_all = train_used[-train_part, ]
set.seed(1777)
```


## Model nr 1

The first model we will build using all chosen before features


```r
training<-training_all
testing<-testing_all
modFit1 <- rpart(classe ~ ., data=training, method="class")
predictions1 <- predict(modFit1, testing, type = "class")
confusionMatrix(predictions1, testing$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2231    0    0    0    0
##          B    1 1518    2    0    0
##          C    0    0 1366    0    0
##          D    0    0    0 1285    0
##          E    0    0    0    1 1442
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9995          
##                  95% CI : (0.9987, 0.9999)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9994          
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9996   1.0000   0.9985   0.9992   1.0000
## Specificity            1.0000   0.9995   1.0000   1.0000   0.9998
## Pos Pred Value         1.0000   0.9980   1.0000   1.0000   0.9993
## Neg Pred Value         0.9998   1.0000   0.9997   0.9998   1.0000
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2843   0.1935   0.1741   0.1638   0.1838
## Detection Prevalence   0.2843   0.1939   0.1741   0.1638   0.1839
## Balanced Accuracy      0.9998   0.9998   0.9993   0.9996   0.9999
```

## Model nr 2

The second model we will build using only the one feature:raw_timestamp_part_1.
There is an interesting relationship with it and the class: In the training data there are 837 unique values in the column row_timestamp_part_1. Happily 816 of 837 refers only to one unique value in the class column. So a accuracy of this model should be no less then (816/837) * 100 (97,4%). Lets check:  


```r
training<-training_all[,c('raw_timestamp_part_1','classe')]
testing<-testing_all[,c('raw_timestamp_part_1','classe')]

modFit2 <- rpart(classe ~ ., data=training, method="class")
predictions2 <- predict(modFit2, testing, type = "class")
confusionMatrix(predictions2, testing$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2215    2    0    0    0
##          B   17 1511   11    0    0
##          C    0    5 1348    6    0
##          D    0    0    9 1271    0
##          E    0    0    0    9 1442
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9925          
##                  95% CI : (0.9903, 0.9943)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9905          
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9924   0.9954   0.9854   0.9883   1.0000
## Specificity            0.9996   0.9956   0.9983   0.9986   0.9986
## Pos Pred Value         0.9991   0.9818   0.9919   0.9930   0.9938
## Neg Pred Value         0.9970   0.9989   0.9969   0.9977   1.0000
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2823   0.1926   0.1718   0.1620   0.1838
## Detection Prevalence   0.2826   0.1962   0.1732   0.1631   0.1849
## Balanced Accuracy      0.9960   0.9955   0.9918   0.9935   0.9993
```

As we can see the accuracy is higher then 97,4%

## Model nr 3

The third model we will build using only the one feature referring only to actual measurements 


```r
training<-training_all[,8:60]
testing<-testing_all[,8:60]
modFit3 <- rpart(classe ~ ., data=training, method="class")
predictions3 <- predict(modFit3, testing, type = "class")
confusionMatrix(predictions3, testing$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2019  244   87  142   44
##          B   86  962  125  122  143
##          C   50  131 1031  213  166
##          D   47  114   84  691   58
##          E   30   67   41  118 1031
## 
## Overall Statistics
##                                           
##                Accuracy : 0.7308          
##                  95% CI : (0.7209, 0.7406)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.6577          
##  Mcnemar's Test P-Value : < 2.2e-16       
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9046   0.6337   0.7537  0.53733   0.7150
## Specificity            0.9079   0.9248   0.9136  0.95381   0.9600
## Pos Pred Value         0.7961   0.6690   0.6480  0.69517   0.8011
## Neg Pred Value         0.9599   0.9132   0.9461  0.91316   0.9373
## Prevalence             0.2845   0.1935   0.1744  0.16391   0.1838
## Detection Rate         0.2573   0.1226   0.1314  0.08807   0.1314
## Detection Prevalence   0.3232   0.1833   0.2028  0.12669   0.1640
## Balanced Accuracy      0.9062   0.7793   0.8336  0.74557   0.8375
```

Unfortunately in this model the accuracy is not as high in the previous

## Final prediction

Finally we use model nr 2 to make prediction for a given testing data


```r
testing_final<-given_testing[,c('raw_timestamp_part_1','user_name')]
names(testing_final)<-c('raw_timestamp_part_1','class')
predictions_final <- predict(modFit2, newdata=testing_final)
round(predictions_final) 
```

```
##    A B C D E
## 1  0 1 0 0 0
## 2  1 0 0 0 0
## 3  0 1 0 0 0
## 4  1 0 0 0 0
## 5  1 0 0 0 0
## 6  0 0 0 0 1
## 7  0 0 0 1 0
## 8  0 1 0 0 0
## 9  1 0 0 0 0
## 10 1 0 0 0 0
## 11 0 1 0 0 0
## 12 0 0 1 0 0
## 13 0 1 0 0 0
## 14 1 0 0 0 0
## 15 0 0 0 0 1
## 16 0 0 0 0 1
## 17 1 0 0 0 0
## 18 0 1 0 0 0
## 19 0 1 0 0 0
## 20 0 1 0 0 0
```
