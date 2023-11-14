# Week 2
## 02_Uncertainty
### BH Algorithm Example 1 
Code used: 
```R
# dataset "web_browser.csv" used
spendy <- glm(log(spend) ~ . -id, data=web_browsers)
round(summary(spendy)$coef,2)
pval <- summary(spendy)$coef[-1, "Pr(>|t|)"]
pvalrank <- rank(pval)
reject <- ifelse(pval< (0.1/9)*pvalrank, 2, 1)
print(pvalrank)
print(pval)
print(reject)
```
Result:
```markdown
```R
# R code block
print(pvalrank)
anychildren   broadband    hispanic   raceblack   raceother   racewhite    regionNE     regionS 
          5           1           3           6           8           7           2           9 
    regionW 
          4

print(pval)
anychildren    broadband     hispanic    raceblack    raceother    racewhite     regionNE 
1.122431e-02 1.323047e-32 1.696801e-05 1.594329e-01 1.878053e-01 1.740374e-01 6.440516e-07 
regionS      regionW 
8.977425e-01 5.322114e-04


print(reject)
anychildren   broadband    hispanic   raceblack   raceother   racewhite    regionNE     regionS 
          2           2           2           1           1           1           2           1 
    regionW 
          2 

```
Comment on output:
- We keep the hypotheses of "raceblack", "raceother", "racewhite" and "regionS"
- This is in line with the ranking of the p-values. The smallest four p-values (6, 7, 8, 9) are the same as those that are not rejected.

Comment on code:  
I first didn't understand the line "reject <- ifelse(pval< (0.1/9)*pvalrank, 2, 1)", especially why exactly we need to divide by 9 because of the increased likelihood of false positives in hypothesis testing. After doing some research, I found the answer. With multiple hypothesis testing, the number of statistical tests conducted automatically increases and hence the probability of obtaining at least one statistically significant result by chance also increases. 
*Chat GTP: Inflation of Type I Error Rate: The more tests you conduct, the higher the chance of observing at least one statistically significant result, even if there are no true effects. This phenomenon is known as the "multiple testing problem" or "familywise error rate inflation."*

## 03_Regression
### Generalized linear regression
Command:
```R
reg1 <- glm(log(sales) ~ brand + log(price), data=oj)
View(reg1)

```

Output: 

| Variable           | Coefficient | Std. Error |
|--------------------|-------------|------------|
| brandminute.maid   | 0.870^***    | (0.013)    |
| brandtropicana     | 1.530^***    | (0.016)    |
| log(price)         | -3.139^***   | (0.023)    |
| Constant           | 10.829^***   | (0.015)    |

**Note:**
- Observations: 28,947
- Log Likelihood: -34,378.400
- Akaike Inf. Crit.: 68,764.800

*Note:*
- $^{*}p<0.1$
- $^{**}p<0.05$
- $^{***}p<0.01$

Interpretation:
Negative coefficient on log(price) <=> negative elasticity 1% increase in price, decreases sales by -3.139%. 


### Deviance and Likelihood
### Define functions for the Deviance and in R.

#### Deviance
Code:

```
deviance <- function(y, pred, family=c("gaussian","binomial")){ family <- match.arg(family)
if(family=="gaussian"){
return( sum( (y-pred)^2 ) ) }else{
if(is.factor(y)) y <- as.numeric(y)>1
return( -2*sum( y*log(pred) + (1-y)*log(1-pred) ) ) }

}
```
Components of code explained:
```
deviance <- function(y, pred, family=c("gaussian","binomial")){ family <- match.arg(family)
```
This line defines a function named deviance
```
return( sum( (y-pred)^2 ) ) }else
```
This line calculates the deviance for a Gaussian distribution by summing the squared differences between the observed y values and the predicted pred values.

```
if(is.factor(y)) y <- as.numeric(y)>1
return( -2*sum( y*log(pred) + (1-y)*log(1-pred) ) ) }
```
This line calculates the deviance for a binomial distribution using the log-likelihood formula. It involves the observed values y, the predicted probabilities pred, and their complements. The result is multiplied by -2, as is common in the context of likelihood-based statistics.


#### R-squared: 
```
R2 <- function(y, pred, family=c("gaussian","binomial")){ fam <- match.arg(family)
```
This line defines a function named R2. 
It uses match.arg() to ensure that the family argument takes one of the allowed values ("gaussian" or "binomial"). If a different value is provided, it will choose the closest match from the allowed values and assign it to the variable fam.


```
if(fam=="binomial"){
if(is.factor(y)){ y <- as.numeric(y)>1 }
}
```
if (fam == "binomial") {: This is an if statement that checks if the distributional assumption is "binomial." If it is, it further checks whether the observed values y are a factor. If they are, it converts them to numeric using as.numeric(y) > 1.

```
dev <- deviance(y, pred, family=fam)
dev0 <- deviance(y, mean(y), family=fam)
return(1-dev/dev0)
}
```
dev <- deviance(y, pred, family = fam): This line calculates the deviance for the provided model predictions and observed values based on the specified distributional assumption (fam).
dev0 <- deviance(y, mean(y), family = fam): This line calculates the deviance for a null model where the predicted values are the mean of the observed values. This is often used as a baseline for comparison.
return(1 - dev/dev0): This line calculates and returns the R-squared value. R-squared is computed as 1 minus the ratio of the deviance of the model (dev) to the deviance of the null model (dev0). It provides a measure of the proportion of deviance explained by the model.



















