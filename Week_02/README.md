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

