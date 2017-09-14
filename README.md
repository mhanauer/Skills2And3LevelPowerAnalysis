---
title: "2 and 3 Level Power Analysis Examples in R"
output: html_document
---
In this example, I am going to demonstrate how to use the PowerUp R package for clustered data with two and in the next example three levels.  Let us assume first, that we want to measure change from pre to post test scores on outcome for some intervention in students who are nested in schools and schools are randomly assigned to treatment and control groups.  One way to estimate the power for this type of study is to use the following function from the PowerUp R package:  mrss.cra2r2

mrss = find the sample given specified parameters
cra = random assignment according to clusters
first 2 = 2 levels (students in classes)
second 2 = level 2 (i.e. clusters) are randomly assigned to treatment and control

This is model is assuming random intercepts and fixed slopes.

Next we need to specify several parameters in order for PowerUp to estimate the effect for our design.  First is the mdes, which is the minimal detectable effect size.  This is the smallest effect that we would be able to detect given a sample size given the other parameter that were specified.  This is the amount of change we would expect to see between two groups in standard deviation units.  Therefore, the effect size, we have chosen, .25 means that we would expect a .25 standard deviation increase in the outcome of interest for the intervention group's outcome.  Alpha is the largest value of type 1 error that you are tolerant of, which I set to the convention of .05.  two.tail = I selected one, because I am assuming the intervention will have a positive effect therefore, I only need one tail (i.e. just measuring whether the intervention is having a larger effect on the treatment than the control group).  gm = is how wide the program searches for combinations of power, clusters, and n's for each cluster.  Larger values indicate a larger search.  2 is the default which I selected here.  J = is the number of clusters.  Therefore, 10 means that I am anticipating 10 schools.  tol is the cut off for the search algorithm and I used the default of .1.  rho2 is the inter correlation class (ICC) coefficient, which is the proportion of variance associated with between cluster variation.  g2 is the number of level 2 covariates, which in this case is none, because we only have one level one variable which is the intervention variable.  R12 is the proportion of level one variance explained by the level covariates.  Since I am not including any level one covariates, I am using zero here.  This is the same for R22, because I am not including any level two covariates therefore none (i.e. zero) of the variance is being explained by level two covariates.  P is the probability of being selected into the treatment or control. In this design I am assuming that each school has a 50%/50% chance of being selected into the treatment or control group.  ncase is the number of cases or possible combinations of clusters and n’s with particular power that are displayed, which in this case I have chosen ten.  Finally, there is the constraint function, which in this case we want to specify power, because that provides us with the power for specific sample sizes.   
```{r, message=FALSE, warning=FALSE}
library(PowerUpR)
mrss.cra2r2(mdes=.25, power=.80, alpha=.05, two.tail=FALSE,
gm=2, ncase=10, constrain="power", J = 10, tol=.10, rho2 =0, g2=0, P=.50, R12=0, R22= 0)
```
After running the program, we can see that PowerUp provides an overall value with 48 people per 10 clusters totaling 480 people with a power of .803.  PowerUp also provides 10 options, as specified by the ncase value of 10, for us to explore further.

PowerUp also has the option to extend this model to three levels.  Let us now assume that now we have a longitudinal model, where we are looking at student's outcomes on some intervention over four time points in classrooms.  So level one would be time, level two would be students (since we have multiple time points for each student), and level three would be classrooms.  Let us still assume that classrooms are randomly assigned to the treatment and control groups.  Now we will use the mrss.cra3r3 function from the PowerUp package.  There are only few differences from the previous function.  First is the additional of the rho3 which the proportion of variance due to the differences in level 3 (i.e. classrooms clusters) and the R32 which is the proportion of variance at level three that is accounted for by three level covariates, which is zero because we do not include any level three covariates in this design.
```{r}
mrss.cra3r3(mdes=.25, power=.80, alpha=.05, two.tail=FALSE, gm=2, ncase=10, constrain="power",rho2 = 0, rho3 = 0,J =4, K=10, tol=.10, P=.50, g3=0, R12=0, R22=0, R32=0)
```
One point I need to make about adding time is that random intercept models assumes that the correlation between time points is the same (Donohue et al., 2016).  This seems like a reasonable assumption for most studies; however, if this is not a reasonable assumption then a package like longPower maybe more appropriate.

We can see that 12 students per classroom over 4 time points across ten clusters means that we would need 12*4 = 48 total students for this particular study to achieve a power of .8 to detect an effect size of .25.

References: Donohue, M. C., Edland, S. D., & Gamst, A. C. (2016). Power for linear models of longitudinal data with applications to Alzheimer’s Disease Phase II study design.
