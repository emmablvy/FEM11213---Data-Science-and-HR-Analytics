## Classification
#### Classification example from class: 
```
pred <- predict(credscore$gamlr, credx, type="response")
pred <- drop(pred) # remove the sparse Matrix formatting
boxplot(pred ~ default, xlab="default", ylab="prob of default", col=c("pink","dodgerblue"))

```
We want to see a result, where the probability of default is higher, when default is 1 compared to when it's 0. 
However, if we look at the resulted boxplot, we can see that there's a lot of overlap between non-defaults and the true defaults. This suggests that there's a hard classification problem. 

#### Classification rule: 
Suggest, we say that we predict y to be 0, if p is below a certain treshold p and Y is 1 if it is above. This can potentially lead to false positives and negatives. 
- True positive: Label is 1 (positive) and the Signal reflects that too by showing 1.
- False positive: Label is 1 but Signal does not reflect that and shows 0.
- True negative: Label is 0 (negative) and the Signal reflects that by showing 0 too.
- False negative: Label is 0, but signal shows 1. 

