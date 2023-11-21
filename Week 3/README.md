## 04 Regularization

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

1. Obtain the predicted value, which corresponds to beta-hat*X

```
deviance <- function(y, pred, family=c("gaussian","binomial")){
  family <- match.arg(family)
  if(family=="gaussian"){
    return( sum( (y-pred)^2 ) )
  }else{
    if(is.factor(y)) y <- as.numeric(y)>1
    return( -2*sum( y*log(pred) + (1-y)*log(1-pred) ) )
  }
}
```

Get the deviance over the null deviance too

```
R2 <- function(y, pred, family=c("gaussian","binomial")){
fam <- match.arg(family)
if(fam=="binomial"){
if(is.factor(y)){ y <- as.numeric(y)>1 }
}
dev <- deviance(y, pred, family=fam)
dev0 <- deviance(y, mean(y), family=fam)
return(1-dev/dev0)
```



















