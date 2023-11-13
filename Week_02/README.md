#Week 2
## BH Algorithm Example 1 
Code used: 
```R
# dataset "web_browser.csv" used
spendy <- glm(log(spend) ~ . -id, data=web_browsers)
round(summary(spendy)$coef,2)
pval <- summary(spendy)$coef[-1, "Pr(>|t|)"]
pvalrank <- rank(pval)
reject <- ifelse(pval< (0.1/9)*pvalrank, 2, 1)
print(pval)
```
Result
```markdown
```R
# R code block
 print(pval)
anychildren    broadband     hispanic    raceblack    raceother    racewhite     regionNE 
1.122431e-02 1.323047e-32 1.696801e-05 1.594329e-01 1.878053e-01 1.740374e-01 6.440516e-07 
regionS      regionW 
8.977425e-01 5.322114e-04

```
