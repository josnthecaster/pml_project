# Practical Machine Learning Project
Jose Armando Navarrete  
Friday, November 20, 2015  


```
## [1] "English_United States.1252"
```

For this project I had no analize the data first.  

the original data has too many loose values and some colummns are recognized as factor, so on loading, I changed them first to character and then to numeric, then, I made a 70% of the data into the training, and the rest is for testing.




I cleaned the data, and take out any ID data we don't need like **"X"** or any **timestamp**.  

I checked for any zero variance predictors to take them out too, and make the training easier.  

There were still many NA's on some predictors, and they were too many!, so the right way treat so many missing values is not to take them on account, so those are out too.  



The next step is to make the model!  

For the model I choose **Random Forest** because it is clasification job, at first I tried with a **General Lineal Model** but the scores where not right, something like 1.65... or 4.6719.  

If the model is right, I don't want to do it again!, so if the object exists it "should not" make it again (or cache in the rmd)


```r
modelar<- function(){
  nuevo <- modelFrame(training)
  
  mymodel <- train(
    classe ~ .,
    data = nuevo,
    trControl = trainControl(method = "cv", number = 3),
    method = "rf"
    )
  
  mymodel
  }

if(!("modelfit" %in% objects())){
     modelfit <- modelar()
     }
```
  
After making the model, I had to test it, so I tried with a confussion Matrix:  


```r
conf1 <- confusionMatrix(training$classe, predict(modelfit, training))
conf1
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 3906    0    0    0    0
##          B    0 2658    0    0    0
##          C    0    0 2396    0    0
##          D    0    0    0 2252    0
##          E    0    0    0    0 2525
## 
## Overall Statistics
##                                      
##                Accuracy : 1          
##                  95% CI : (0.9997, 1)
##     No Information Rate : 0.2843     
##     P-Value [Acc > NIR] : < 2.2e-16  
##                                      
##                   Kappa : 1          
##  Mcnemar's Test P-Value : NA         
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            1.0000   1.0000   1.0000   1.0000   1.0000
## Specificity            1.0000   1.0000   1.0000   1.0000   1.0000
## Pos Pred Value         1.0000   1.0000   1.0000   1.0000   1.0000
## Neg Pred Value         1.0000   1.0000   1.0000   1.0000   1.0000
## Prevalence             0.2843   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2843   0.1935   0.1744   0.1639   0.1838
## Detection Prevalence   0.2843   0.1935   0.1744   0.1639   0.1838
## Balanced Accuracy      1.0000   1.0000   1.0000   1.0000   1.0000
```
     
And the accuracy if of 100%, seems to be *Overfitted*, but have to test it on the testing set.  


```r
conf2 <- confusionMatrix(testing$classe, predict(modelfit, testing))
conf2
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1674    0    0    0    0
##          B   10 1126    2    1    0
##          C    0    5 1014    7    0
##          D    0    0   10  952    2
##          E    0    1    2    2 1077
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9929          
##                  95% CI : (0.9904, 0.9949)
##     No Information Rate : 0.2862          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.991           
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9941   0.9947   0.9864   0.9896   0.9981
## Specificity            1.0000   0.9973   0.9975   0.9976   0.9990
## Pos Pred Value         1.0000   0.9886   0.9883   0.9876   0.9954
## Neg Pred Value         0.9976   0.9987   0.9971   0.9980   0.9996
## Prevalence             0.2862   0.1924   0.1747   0.1635   0.1833
## Detection Rate         0.2845   0.1913   0.1723   0.1618   0.1830
## Detection Prevalence   0.2845   0.1935   0.1743   0.1638   0.1839
## Balanced Accuracy      0.9970   0.9960   0.9920   0.9936   0.9986
```
   
On the testing set the model still hass a high accuracy, as of 99.29%, so my expected error rate is 0.71% which makes me very happy.

And so I try to predict the values and this is what I get:


```r
pr <- loadPrData()
predict(modelfit, pr)
```

```
##  [1] B A A A A E D B A A B C B A E E A B B B
## Levels: A B C D E
```

A beautiful chain of characters depicting all right answers.

To make this model better, I would have to analize the correlation matrix and chose the most important features or use a Principal Components PreProcessing.

I didn't include any graphics or exploratory analisys because any of what I tried was too difficult to analyze or check.
