Lab10_LewisCheng
================
Lewis Cheng
2023-10-22

\#Q1

``` r
library("caret")
```

    ## Loading required package: ggplot2

    ## Loading required package: lattice

``` r
library("kernlab")
```

    ## 
    ## Attaching package: 'kernlab'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     alpha

``` r
data("GermanCredit")
subCredit <- GermanCredit[,1:10]
str(subCredit)
```

    ## 'data.frame':    1000 obs. of  10 variables:
    ##  $ Duration                 : int  6 48 12 42 24 36 24 36 12 30 ...
    ##  $ Amount                   : int  1169 5951 2096 7882 4870 9055 2835 6948 3059 5234 ...
    ##  $ InstallmentRatePercentage: int  4 2 2 2 3 2 3 2 2 4 ...
    ##  $ ResidenceDuration        : int  4 2 3 4 4 4 4 2 4 2 ...
    ##  $ Age                      : int  67 22 49 45 53 35 53 35 61 28 ...
    ##  $ NumberExistingCredits    : int  2 1 1 1 2 1 1 1 1 2 ...
    ##  $ NumberPeopleMaintenance  : int  1 1 2 2 2 2 1 1 1 1 ...
    ##  $ Telephone                : num  0 1 1 1 1 0 1 0 1 1 ...
    ##  $ ForeignWorker            : num  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ Class                    : Factor w/ 2 levels "Bad","Good": 2 1 2 2 1 2 2 2 2 1 ...

``` r
help("GermanCredit")
```

    ## starting httpd help server ...

    ##  done

\#Q2

``` r
trainList <- createDataPartition(y=subCredit$Class,p=.40,list=FALSE)  #making the training dataset
```

\#Q3

``` r
head(trainList)
```

    ##      Resample1
    ## [1,]         1
    ## [2,]         3
    ## [3,]         4
    ## [4,]         5
    ## [5,]         8
    ## [6,]        10

``` r
tail(trainList)
```

    ##        Resample1
    ## [395,]       982
    ## [396,]       988
    ## [397,]       990
    ## [398,]       993
    ## [399,]       995
    ## [400,]       999

``` r
#all good, trainList is list of indices balanced around class factor in Q2
```

\#Q4

``` r
#trainList is list of indices balanced around class factor in Q2
```

\#Q5

``` r
trainSet <- subCredit[trainList,] #make the train data set
dim(trainSet)
```

    ## [1] 400  10

\#Q6

``` r
testSet <- subCredit[-trainList, ] # make the test data set

dim(testSet)
```

    ## [1] 600  10

\#Q7

``` r
#ignored
```

\#Q8

``` r
creditOutput <- ksvm(Class ~., data=trainSet, kernel='rbfdot', 
                     kpar='automatic', C=5, cross=3,  prob.model=TRUE) #train the model
creditOutput
```

    ## Support Vector Machine object of class "ksvm" 
    ## 
    ## SV type: C-svc  (classification) 
    ##  parameter : cost C = 5 
    ## 
    ## Gaussian Radial Basis kernel function. 
    ##  Hyperparameter : sigma =  0.0861143791846613 
    ## 
    ## Number of Support Vectors : 259 
    ## 
    ## Objective Function Value : -937.6695 
    ## Training error : 0.1925 
    ## Cross validation error : 0.297572 
    ## Probability model included.

\#Q9

``` r
#ok model
```

\#Q10

``` r
predictions <- predict(creditOutput, trainSet) #use prediction function to generate output
str(predictions)
```

    ##  Factor w/ 2 levels "Bad","Good": 2 2 2 1 2 2 2 1 2 1 ...

\#Q11

``` r
confusionMatrix <- table(trainSet[, 10], predictions)
confusionMatrix # 60 bad, 4 erroneously marked as good 
```

    ##       predictions
    ##        Bad Good
    ##   Bad   51   69
    ##   Good   8  272

``` r
#276 good, 64 erroneously marked as bad??
```

\#Q12

``` r
diag(confusionMatrix) # 56/60 were correctly identified as bad, while 276/300 were correctly identified as good.
```

    ##  Bad Good 
    ##   51  272

``` r
confusionMatrix(trainSet[, 10], predictions) #alll good
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction Bad Good
    ##       Bad   51   69
    ##       Good   8  272
    ##                                          
    ##                Accuracy : 0.8075         
    ##                  95% CI : (0.7654, 0.845)
    ##     No Information Rate : 0.8525         
    ##     P-Value [Acc > NIR] : 0.9942         
    ##                                          
    ##                   Kappa : 0.4638         
    ##                                          
    ##  Mcnemar's Test P-Value : 8.051e-12      
    ##                                          
    ##             Sensitivity : 0.8644         
    ##             Specificity : 0.7977         
    ##          Pos Pred Value : 0.4250         
    ##          Neg Pred Value : 0.9714         
    ##              Prevalence : 0.1475         
    ##          Detection Rate : 0.1275         
    ##    Detection Prevalence : 0.3000         
    ##       Balanced Accuracy : 0.8310         
    ##                                          
    ##        'Positive' Class : Bad            
    ## 
