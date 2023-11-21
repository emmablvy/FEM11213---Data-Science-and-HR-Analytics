## 04 Regularization
```
SC <-read.csv("Desktop/HS23/Data Science and HR Analytics/Data/semiconductor.csv")
full <- glm(FAIL ~ ., data=SC, family=binomial) #regress failure on all covariates
1 - full$deviance/full$null.deviance #compute the R-squared

```





















