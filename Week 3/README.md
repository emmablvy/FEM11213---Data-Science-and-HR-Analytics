## 04 Regularization


### Theory: K-mean Clustering 
Video: https://www.youtube.com/watch?v=4b5d3muPQmA
Notes:
- Select number of clusters you want to identify. i.e K =3
- Randomly select three data points, these are the initial clusters
- Measure the distance between the first point and the three initial clusters. 
<img width="611" alt="Bildschirmfoto 2023-11-21 um 15 48 00" src="https://github.com/emmablvy/FEM11213---Data-Science-and-HR-Analytics/assets/149567541/3d29a9bd-3ab9-4ece-bbfe-0be2379d24a0">
- Assign the first point to the nearest cluster
- Repeat the same thing for the next point and assign it to the nearest cluster. Again for third point etc. 

<img width="611" alt="Bildschirmfoto 2023-11-21 um 15 49 17" src="https://github.com/emmablvy/FEM11213---Data-Science-and-HR-Analytics/assets/149567541/b9e0d590-6459-43a9-aad1-e1a150db1759">

<img width="611" alt="Bildschirmfoto 2023-11-21 um 15 49 43" src="https://github.com/emmablvy/FEM11213---Data-Science-and-HR-Analytics/assets/149567541/bce122ef-baec-4a5a-9979-f30d014ab0a8">

<img width="611" alt="Bildschirmfoto 2023-11-21 um 15 50 00" src="https://github.com/emmablvy/FEM11213---Data-Science-and-HR-Analytics/assets/149567541/ddc7c0cc-4905-4a1b-ace1-aa88c5b13f5b">

- Can asses the quality of the clustering by adding up the variation within each cluster.

<img width="611" alt="Bildschirmfoto 2023-11-21 um 15 52 16" src="https://github.com/emmablvy/FEM11213---Data-Science-and-HR-Analytics/assets/149567541/3b8c780c-9800-4e4c-ad09-e5b7f255ac9b">

<img width="611" alt="Bildschirmfoto 2023-11-21 um 15 52 44" src="https://github.com/emmablvy/FEM11213---Data-Science-and-HR-Analytics/assets/149567541/82b7608d-17c9-4907-8e26-6179d85bd351">

<img width="611" alt="Bildschirmfoto 2023-11-21 um 15 53 12" src="https://github.com/emmablvy/FEM11213---Data-Science-and-HR-Analytics/assets/149567541/7b69e265-df6d-4d5c-8cbd-5068f75a7ca8">

How to choose K? 
- Sometimes (as in the example) it's clear to use K =3, because there are 3 colors, but often unclear. So best to try with different K's.
- Try K = 1, this is the worst case so we compute the variation of this as a reference for other K's.
- For different K's compare the total variation of each other. The more K, the smaller the variation. And in the extreme case, where there is only one point per cluster, the variation is 0.
- But in this example one can see that the reduction in Variation is the stringest at K=3:
<img width="611" alt="Bildschirmfoto 2023-11-21 um 15 56 08" src="https://github.com/emmablvy/FEM11213---Data-Science-and-HR-Analytics/assets/149567541/1003f38a-414c-4719-88ac-f4e85686da7e">

This is the elbow, and the K at this elbow is the optimal K. 


## Theory: Cross Validation Video
https://www.youtube.com/watch?v=fSytzGwwBVw
Notes: 
Example - want to use Sypmtoms as Chest pain, Good Blood Circulation, Blocked Arteries, Weight to predict whether someone (new) has a heart disease. 
- Need to decide which ML Method is best. i.e. Logistic Regression, K-nearest Neighbors, Support vector Machines etc. --> How to decide which to use? 
- Cross validation: allows to compare different ML methods and get a sense of how well they'll work in practice.
- Use data of people who had and didn't had a heart disease and do two things:  (1) Estimate the parameters for the ML methods (also called: Training the algorithm), (2) Evaluate how well the machine learning methods work. --> i.e. need to find if a logistic regression does a good job in predicting new data. Also called: Testing the algorithm. 
- Hence: (1) train ML methods and (2) test ML methods.
- Cannot use same data for training and testing! Better to use first 75% for training and last 25% for testing i.e.
- How to know how to divide up the data? what about using the first 25% instead of last 25%. --> cross validation uses them all, uses the first three blocks to train and last to test. Then in does the same, but takes first, third and fourth for training and second for testing etc. At the end it compares which did the best job.
- In the case of 4 Blocks, we have a 4 fold cross Validation. In an extreme case you could use each sample individually. In practice the most common is ten --> ten-fold cross validation. 








Compute the R-Squared
```
SC <-read.csv("Desktop/HS23/Data Science and HR Analytics/Data/semiconductor.csv")
full <- glm(FAIL ~ ., data=SC, family=binomial) #regress failure on all covariates
1 - full$deviance/full$null.deviance #compute the R-squared

```

The three steps: 
1. Define functions for the Deviance and in R.
2. Partition data into k-folds and then run experiment for each fold.
3. Plot the results.

In R: 
```

SC <-read.csv("Desktop/HS23/Data Science and HR Analytics/Data/semiconductor.csv")
full <- glm(FAIL ~ ., data=SC, family=binomial) #regress failure on all covariates
1 - full$deviance/full$null.deviance #compute the R-squared


deviance <- function(y, pred, family=c("gaussian","binomial")){
  family <- match.arg(family)
  if(family=="gaussian"){
    return( sum( (y-pred)^2 ) )
  }else{
    if(is.factor(y)) y <- as.numeric(y)>1
    return( -2*sum( y*log(pred) + (1-y)*log(1-pred) ) )
  }
}
## get null devaince too, and return R2
R2 <- function(y, pred, family=c("gaussian","binomial")){
  fam <- match.arg(family)
  if(fam=="binomial"){
    if(is.factor(y)){ y <- as.numeric(y)>1 }
  }
  dev <- deviance(y, pred, family=fam)
  dev0 <- deviance(y, mean(y), family=fam)
  return(1-dev/dev0)
}

# setup the experiment
n <- nrow(SC) # the number of observations
K <- 10 # the number of `folds'
# create a vector of fold memberships (random order)
foldid <- rep(1:K,each=ceiling(n/K))[sample(1:n)]
# create an empty dataframe of results
Out <- data.frame(full=rep(NA,K))
# use a for loop to run the experiment
for(k in 1:K){
  train <- which(foldid!=k) # train on all but fold `k'
  ## fit regression on full sample
  rfull <- glm(FAIL~., data=SC, subset=train, family=binomial)
  ## get prediction: type=response so we have probabilities
  predfull <- predict(rfull, newdata=SC[-train,], type="response")
  ## calculate and log R2
  Out$full[k] <- R2(y=SC$FAIL[-train], pred=predfull, family="binomial")
  ## print progress
  cat(k, " ")
}
boxplot(Out, col="plum", ylab="R2")

```

## Regularization Paths  
Video: LASSO Regression https://www.youtube.com/watch?v=NGf0voTMlcs  

<img width="611" alt="Bildschirmfoto 2023-11-21 um 16 22 09" src="https://github.com/emmablvy/FEM11213---Data-Science-and-HR-Analytics/assets/149567541/be08eac3-a983-4ad4-a240-3ecbd47a813d">

- Lasso Regression results in a little bit of Bias but with less variance than in least squares.
- Lasso makes prediction of size less sensitive
- Lasso can be applied to complicated models that combine different types of data.
- LASSO does not choose the model for you, in other words. Instead, it gives you a set of candidate models to choose from.














