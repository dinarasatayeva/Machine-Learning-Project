---
title: "Machine Learning Project"
author: "Dinara Satayeva"
date: "3/31/2020"
output: 
  html_document: 
    keep_md: yes
---

## Overview
Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. 

In this project, the goal is to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. The goal of the project is to predict the manner in which they did the exercise. 
  
## Loading data

```r
training <- read.csv("pml-training.csv", na.strings = c("NA", "#DIV/0!", ""))
testing <- read.csv("pml-testing.csv", na.strings = c("NA", "#DIV/0!", ""))
```

## First look at the data

```r
dim(training)
```

```
## [1] 19622   160
```

```r
dim(testing)
```

```
## [1]  20 160
```

```r
head(training)
```

```
##   X user_name raw_timestamp_part_1 raw_timestamp_part_2   cvtd_timestamp
## 1 1  carlitos           1323084231               788290 05/12/2011 11:23
## 2 2  carlitos           1323084231               808298 05/12/2011 11:23
## 3 3  carlitos           1323084231               820366 05/12/2011 11:23
## 4 4  carlitos           1323084232               120339 05/12/2011 11:23
## 5 5  carlitos           1323084232               196328 05/12/2011 11:23
## 6 6  carlitos           1323084232               304277 05/12/2011 11:23
##   new_window num_window roll_belt pitch_belt yaw_belt total_accel_belt
## 1         no         11      1.41       8.07    -94.4                3
## 2         no         11      1.41       8.07    -94.4                3
## 3         no         11      1.42       8.07    -94.4                3
## 4         no         12      1.48       8.05    -94.4                3
## 5         no         12      1.48       8.07    -94.4                3
## 6         no         12      1.45       8.06    -94.4                3
##   kurtosis_roll_belt kurtosis_picth_belt kurtosis_yaw_belt skewness_roll_belt
## 1                 NA                  NA                NA                 NA
## 2                 NA                  NA                NA                 NA
## 3                 NA                  NA                NA                 NA
## 4                 NA                  NA                NA                 NA
## 5                 NA                  NA                NA                 NA
## 6                 NA                  NA                NA                 NA
##   skewness_roll_belt.1 skewness_yaw_belt max_roll_belt max_picth_belt
## 1                   NA                NA            NA             NA
## 2                   NA                NA            NA             NA
## 3                   NA                NA            NA             NA
## 4                   NA                NA            NA             NA
## 5                   NA                NA            NA             NA
## 6                   NA                NA            NA             NA
##   max_yaw_belt min_roll_belt min_pitch_belt min_yaw_belt amplitude_roll_belt
## 1           NA            NA             NA           NA                  NA
## 2           NA            NA             NA           NA                  NA
## 3           NA            NA             NA           NA                  NA
## 4           NA            NA             NA           NA                  NA
## 5           NA            NA             NA           NA                  NA
## 6           NA            NA             NA           NA                  NA
##   amplitude_pitch_belt amplitude_yaw_belt var_total_accel_belt avg_roll_belt
## 1                   NA                 NA                   NA            NA
## 2                   NA                 NA                   NA            NA
## 3                   NA                 NA                   NA            NA
## 4                   NA                 NA                   NA            NA
## 5                   NA                 NA                   NA            NA
## 6                   NA                 NA                   NA            NA
##   stddev_roll_belt var_roll_belt avg_pitch_belt stddev_pitch_belt
## 1               NA            NA             NA                NA
## 2               NA            NA             NA                NA
## 3               NA            NA             NA                NA
## 4               NA            NA             NA                NA
## 5               NA            NA             NA                NA
## 6               NA            NA             NA                NA
##   var_pitch_belt avg_yaw_belt stddev_yaw_belt var_yaw_belt gyros_belt_x
## 1             NA           NA              NA           NA         0.00
## 2             NA           NA              NA           NA         0.02
## 3             NA           NA              NA           NA         0.00
## 4             NA           NA              NA           NA         0.02
## 5             NA           NA              NA           NA         0.02
## 6             NA           NA              NA           NA         0.02
##   gyros_belt_y gyros_belt_z accel_belt_x accel_belt_y accel_belt_z
## 1         0.00        -0.02          -21            4           22
## 2         0.00        -0.02          -22            4           22
## 3         0.00        -0.02          -20            5           23
## 4         0.00        -0.03          -22            3           21
## 5         0.02        -0.02          -21            2           24
## 6         0.00        -0.02          -21            4           21
##   magnet_belt_x magnet_belt_y magnet_belt_z roll_arm pitch_arm yaw_arm
## 1            -3           599          -313     -128      22.5    -161
## 2            -7           608          -311     -128      22.5    -161
## 3            -2           600          -305     -128      22.5    -161
## 4            -6           604          -310     -128      22.1    -161
## 5            -6           600          -302     -128      22.1    -161
## 6             0           603          -312     -128      22.0    -161
##   total_accel_arm var_accel_arm avg_roll_arm stddev_roll_arm var_roll_arm
## 1              34            NA           NA              NA           NA
## 2              34            NA           NA              NA           NA
## 3              34            NA           NA              NA           NA
## 4              34            NA           NA              NA           NA
## 5              34            NA           NA              NA           NA
## 6              34            NA           NA              NA           NA
##   avg_pitch_arm stddev_pitch_arm var_pitch_arm avg_yaw_arm stddev_yaw_arm
## 1            NA               NA            NA          NA             NA
## 2            NA               NA            NA          NA             NA
## 3            NA               NA            NA          NA             NA
## 4            NA               NA            NA          NA             NA
## 5            NA               NA            NA          NA             NA
## 6            NA               NA            NA          NA             NA
##   var_yaw_arm gyros_arm_x gyros_arm_y gyros_arm_z accel_arm_x accel_arm_y
## 1          NA        0.00        0.00       -0.02        -288         109
## 2          NA        0.02       -0.02       -0.02        -290         110
## 3          NA        0.02       -0.02       -0.02        -289         110
## 4          NA        0.02       -0.03        0.02        -289         111
## 5          NA        0.00       -0.03        0.00        -289         111
## 6          NA        0.02       -0.03        0.00        -289         111
##   accel_arm_z magnet_arm_x magnet_arm_y magnet_arm_z kurtosis_roll_arm
## 1        -123         -368          337          516                NA
## 2        -125         -369          337          513                NA
## 3        -126         -368          344          513                NA
## 4        -123         -372          344          512                NA
## 5        -123         -374          337          506                NA
## 6        -122         -369          342          513                NA
##   kurtosis_picth_arm kurtosis_yaw_arm skewness_roll_arm skewness_pitch_arm
## 1                 NA               NA                NA                 NA
## 2                 NA               NA                NA                 NA
## 3                 NA               NA                NA                 NA
## 4                 NA               NA                NA                 NA
## 5                 NA               NA                NA                 NA
## 6                 NA               NA                NA                 NA
##   skewness_yaw_arm max_roll_arm max_picth_arm max_yaw_arm min_roll_arm
## 1               NA           NA            NA          NA           NA
## 2               NA           NA            NA          NA           NA
## 3               NA           NA            NA          NA           NA
## 4               NA           NA            NA          NA           NA
## 5               NA           NA            NA          NA           NA
## 6               NA           NA            NA          NA           NA
##   min_pitch_arm min_yaw_arm amplitude_roll_arm amplitude_pitch_arm
## 1            NA          NA                 NA                  NA
## 2            NA          NA                 NA                  NA
## 3            NA          NA                 NA                  NA
## 4            NA          NA                 NA                  NA
## 5            NA          NA                 NA                  NA
## 6            NA          NA                 NA                  NA
##   amplitude_yaw_arm roll_dumbbell pitch_dumbbell yaw_dumbbell
## 1                NA      13.05217      -70.49400    -84.87394
## 2                NA      13.13074      -70.63751    -84.71065
## 3                NA      12.85075      -70.27812    -85.14078
## 4                NA      13.43120      -70.39379    -84.87363
## 5                NA      13.37872      -70.42856    -84.85306
## 6                NA      13.38246      -70.81759    -84.46500
##   kurtosis_roll_dumbbell kurtosis_picth_dumbbell kurtosis_yaw_dumbbell
## 1                     NA                      NA                    NA
## 2                     NA                      NA                    NA
## 3                     NA                      NA                    NA
## 4                     NA                      NA                    NA
## 5                     NA                      NA                    NA
## 6                     NA                      NA                    NA
##   skewness_roll_dumbbell skewness_pitch_dumbbell skewness_yaw_dumbbell
## 1                     NA                      NA                    NA
## 2                     NA                      NA                    NA
## 3                     NA                      NA                    NA
## 4                     NA                      NA                    NA
## 5                     NA                      NA                    NA
## 6                     NA                      NA                    NA
##   max_roll_dumbbell max_picth_dumbbell max_yaw_dumbbell min_roll_dumbbell
## 1                NA                 NA               NA                NA
## 2                NA                 NA               NA                NA
## 3                NA                 NA               NA                NA
## 4                NA                 NA               NA                NA
## 5                NA                 NA               NA                NA
## 6                NA                 NA               NA                NA
##   min_pitch_dumbbell min_yaw_dumbbell amplitude_roll_dumbbell
## 1                 NA               NA                      NA
## 2                 NA               NA                      NA
## 3                 NA               NA                      NA
## 4                 NA               NA                      NA
## 5                 NA               NA                      NA
## 6                 NA               NA                      NA
##   amplitude_pitch_dumbbell amplitude_yaw_dumbbell total_accel_dumbbell
## 1                       NA                     NA                   37
## 2                       NA                     NA                   37
## 3                       NA                     NA                   37
## 4                       NA                     NA                   37
## 5                       NA                     NA                   37
## 6                       NA                     NA                   37
##   var_accel_dumbbell avg_roll_dumbbell stddev_roll_dumbbell var_roll_dumbbell
## 1                 NA                NA                   NA                NA
## 2                 NA                NA                   NA                NA
## 3                 NA                NA                   NA                NA
## 4                 NA                NA                   NA                NA
## 5                 NA                NA                   NA                NA
## 6                 NA                NA                   NA                NA
##   avg_pitch_dumbbell stddev_pitch_dumbbell var_pitch_dumbbell avg_yaw_dumbbell
## 1                 NA                    NA                 NA               NA
## 2                 NA                    NA                 NA               NA
## 3                 NA                    NA                 NA               NA
## 4                 NA                    NA                 NA               NA
## 5                 NA                    NA                 NA               NA
## 6                 NA                    NA                 NA               NA
##   stddev_yaw_dumbbell var_yaw_dumbbell gyros_dumbbell_x gyros_dumbbell_y
## 1                  NA               NA                0            -0.02
## 2                  NA               NA                0            -0.02
## 3                  NA               NA                0            -0.02
## 4                  NA               NA                0            -0.02
## 5                  NA               NA                0            -0.02
## 6                  NA               NA                0            -0.02
##   gyros_dumbbell_z accel_dumbbell_x accel_dumbbell_y accel_dumbbell_z
## 1             0.00             -234               47             -271
## 2             0.00             -233               47             -269
## 3             0.00             -232               46             -270
## 4            -0.02             -232               48             -269
## 5             0.00             -233               48             -270
## 6             0.00             -234               48             -269
##   magnet_dumbbell_x magnet_dumbbell_y magnet_dumbbell_z roll_forearm
## 1              -559               293               -65         28.4
## 2              -555               296               -64         28.3
## 3              -561               298               -63         28.3
## 4              -552               303               -60         28.1
## 5              -554               292               -68         28.0
## 6              -558               294               -66         27.9
##   pitch_forearm yaw_forearm kurtosis_roll_forearm kurtosis_picth_forearm
## 1         -63.9        -153                    NA                     NA
## 2         -63.9        -153                    NA                     NA
## 3         -63.9        -152                    NA                     NA
## 4         -63.9        -152                    NA                     NA
## 5         -63.9        -152                    NA                     NA
## 6         -63.9        -152                    NA                     NA
##   kurtosis_yaw_forearm skewness_roll_forearm skewness_pitch_forearm
## 1                   NA                    NA                     NA
## 2                   NA                    NA                     NA
## 3                   NA                    NA                     NA
## 4                   NA                    NA                     NA
## 5                   NA                    NA                     NA
## 6                   NA                    NA                     NA
##   skewness_yaw_forearm max_roll_forearm max_picth_forearm max_yaw_forearm
## 1                   NA               NA                NA              NA
## 2                   NA               NA                NA              NA
## 3                   NA               NA                NA              NA
## 4                   NA               NA                NA              NA
## 5                   NA               NA                NA              NA
## 6                   NA               NA                NA              NA
##   min_roll_forearm min_pitch_forearm min_yaw_forearm amplitude_roll_forearm
## 1               NA                NA              NA                     NA
## 2               NA                NA              NA                     NA
## 3               NA                NA              NA                     NA
## 4               NA                NA              NA                     NA
## 5               NA                NA              NA                     NA
## 6               NA                NA              NA                     NA
##   amplitude_pitch_forearm amplitude_yaw_forearm total_accel_forearm
## 1                      NA                    NA                  36
## 2                      NA                    NA                  36
## 3                      NA                    NA                  36
## 4                      NA                    NA                  36
## 5                      NA                    NA                  36
## 6                      NA                    NA                  36
##   var_accel_forearm avg_roll_forearm stddev_roll_forearm var_roll_forearm
## 1                NA               NA                  NA               NA
## 2                NA               NA                  NA               NA
## 3                NA               NA                  NA               NA
## 4                NA               NA                  NA               NA
## 5                NA               NA                  NA               NA
## 6                NA               NA                  NA               NA
##   avg_pitch_forearm stddev_pitch_forearm var_pitch_forearm avg_yaw_forearm
## 1                NA                   NA                NA              NA
## 2                NA                   NA                NA              NA
## 3                NA                   NA                NA              NA
## 4                NA                   NA                NA              NA
## 5                NA                   NA                NA              NA
## 6                NA                   NA                NA              NA
##   stddev_yaw_forearm var_yaw_forearm gyros_forearm_x gyros_forearm_y
## 1                 NA              NA            0.03            0.00
## 2                 NA              NA            0.02            0.00
## 3                 NA              NA            0.03           -0.02
## 4                 NA              NA            0.02           -0.02
## 5                 NA              NA            0.02            0.00
## 6                 NA              NA            0.02           -0.02
##   gyros_forearm_z accel_forearm_x accel_forearm_y accel_forearm_z
## 1           -0.02             192             203            -215
## 2           -0.02             192             203            -216
## 3            0.00             196             204            -213
## 4            0.00             189             206            -214
## 5           -0.02             189             206            -214
## 6           -0.03             193             203            -215
##   magnet_forearm_x magnet_forearm_y magnet_forearm_z classe
## 1              -17              654              476      A
## 2              -18              661              473      A
## 3              -18              658              469      A
## 4              -16              658              469      A
## 5              -17              655              473      A
## 6               -9              660              478      A
```

## Cleaning data

```r
training <- training[, which(colSums(is.na(training)) == 0)] 
testing <- testing[, which(colSums(is.na(testing)) == 0)]
```

## Cross Validation
To perfrom cross validation I'll split training dataset into two subsets: training subset(70%) and testing subset(30%).

```r
library(caret)
```

```
## Loading required package: lattice
```

```
## Loading required package: ggplot2
```

```r
library(randomForest)
```

```
## randomForest 4.6-14
```

```
## Type rfNews() to see new features/changes/bug fixes.
```

```
## 
## Attaching package: 'randomForest'
```

```
## The following object is masked from 'package:ggplot2':
## 
##     margin
```

```r
set.seed(333)
inTrain = createDataPartition(y=training$classe,p=0.7, list=FALSE)

training_sub = training[inTrain,]
testing_sub = training[-inTrain,]
```

## Predicting using Recursive Partitioning and Regression Trees

```r
#building a model
fit1 = train(classe ~ ., data = training_sub, method = "rpart")
#predicting on our validation subset
validation<-predict(fit1, testing_sub)
confusionMatrix(testing_sub$classe, validation)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1674    0    0    0    0
##          B    0 1139    0    0    0
##          C    0    0    0    0 1026
##          D    0    0    0    0  964
##          E    0    0    0    0 1082
## 
## Overall Statistics
##                                           
##                Accuracy : 0.6619          
##                  95% CI : (0.6496, 0.6739)
##     No Information Rate : 0.522           
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.5696          
##                                           
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            1.0000   1.0000       NA       NA   0.3522
## Specificity            1.0000   1.0000   0.8257   0.8362   1.0000
## Pos Pred Value         1.0000   1.0000       NA       NA   1.0000
## Neg Pred Value         1.0000   1.0000       NA       NA   0.5857
## Prevalence             0.2845   0.1935   0.0000   0.0000   0.5220
## Detection Rate         0.2845   0.1935   0.0000   0.0000   0.1839
## Detection Prevalence   0.2845   0.1935   0.1743   0.1638   0.1839
## Balanced Accuracy      1.0000   1.0000       NA       NA   0.6761
```

## Predicting using Random Forest

```r
#building a model
fit2 = train(classe ~ ., data = training_sub, method = "rf", ntree = 250)
#predicting on our validation subset
validation<-predict(fit2, testing_sub)
confusionMatrix(testing_sub$classe, validation)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1674    0    0    0    0
##          B    0 1139    0    0    0
##          C    0    1 1025    0    0
##          D    0    0    0  964    0
##          E    0    0    0    0 1082
## 
## Overall Statistics
##                                      
##                Accuracy : 0.9998     
##                  95% CI : (0.9991, 1)
##     No Information Rate : 0.2845     
##     P-Value [Acc > NIR] : < 2.2e-16  
##                                      
##                   Kappa : 0.9998     
##                                      
##  Mcnemar's Test P-Value : NA         
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            1.0000   0.9991   1.0000   1.0000   1.0000
## Specificity            1.0000   1.0000   0.9998   1.0000   1.0000
## Pos Pred Value         1.0000   1.0000   0.9990   1.0000   1.0000
## Neg Pred Value         1.0000   0.9998   1.0000   1.0000   1.0000
## Prevalence             0.2845   0.1937   0.1742   0.1638   0.1839
## Detection Rate         0.2845   0.1935   0.1742   0.1638   0.1839
## Detection Prevalence   0.2845   0.1935   0.1743   0.1638   0.1839
## Balanced Accuracy      1.0000   0.9996   0.9999   1.0000   1.0000
```

## Prediction
Random random model showed higher accuracy than the rpart model, therefore I will use it to predict observations from testing dataset.

```r
pred= predict(fit2, testing)
pred
```

```
##  [1] A A A A A A A A A A A A A A A A A A A A
## Levels: A B C D E
```
