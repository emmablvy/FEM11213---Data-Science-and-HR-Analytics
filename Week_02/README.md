#Week 2
## BH Algorithm Example 1 
Code used: 
```R
# This is an R code block
spendy <- glm(log(spend) ~ . -id, data=web_browsers)
round(summary(spendy)$coef,2)
pval <- summary(spendy)$coef[-1, "Pr(>|t|)"]
pvalrank <- rank(pval)
reject <- ifelse(pval< (0.1/9)*pvalrank, 2, 1)
print(pval)
