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

2. Partition data into k-folds and then run experiment for each fold.


















