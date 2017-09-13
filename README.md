---
title: "2 and 3 Level Power Analysis Example"
output: html_document
---
In this example, I am going to demonstrate how to use the PowerUp R package for clustered in data in two levels.  Let us assume first, that we want to measure change from pre to post test scores on outcome for some intervention in students who are nested in schools and schools are randomly assigned to treatment and control groups.  One way to estimate the power for this type of study  
```{r}
install.packages("PowerUpR")
library(PowerUpR)
mrss.cra2r2(mdes=.25, power=.80, alpha=.05, two.tail=FALSE,
gm=2, ncase=10, constrain="power", J = 10, J0=10, tol=.10, rho2 = 0, g2=0, P=.50, R12=0, R22=0)
```



