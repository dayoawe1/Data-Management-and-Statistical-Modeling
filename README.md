
# "Data-Management-and-Statistical-Modeling"

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## R Markdown
## Load data into R

```{r import}
setwd('/Users/dayoawe1/Desktop/School Work/new/SPRING 2023/Advanced Data Analysis/Assignment/HW 1')
```

## Read the data into R in both txt and csv format

```{r text}
ShipData = read.table('ShipAccidents.txt',header=TRUE)
```
```{r csv}
ShipData = read.csv('ShipAccidents.csv',header=TRUE)
```

## Confirm missing values have been accounted for correctly

```{r confirm}
ShipData = na.omit(ShipData)
attach(ShipData)
ShipData
```

<img width="839" alt="DF" src="https://github.com/user-attachments/assets/ed496a8c-c36e-431f-94b3-c5463e509be2">


## Descriptive statistics to confirm the data has been read in correctly

```{r descriptive}
hist(accidents)
boxplot(accidents)
mean(accidents)
var(accidents)
summary(accidents)
table(accidents)
```

<img width="561" alt="Hist" src="https://github.com/user-attachments/assets/31d6088b-202c-4cdb-8738-2fd8f43ae5eb">

<img width="693" alt="Box plot" src="https://github.com/user-attachments/assets/372474cb-d7f8-45d7-ad9b-ca0694013861">

## Present an appropriate normal linear regression model

```{r regression}
NormalModel = lm(accidents~exposure+construction1+construction2+construction3)
summary(NormalModel)
```
<img width="934" alt="Lm res" src="https://github.com/user-attachments/assets/2ce3f08d-37b2-46c1-9e3e-6e1864a827f8">

accidents$_i$ = -50.7824 + 7.9985 exposure + 8.0606 construction$_1$ + 7.2704 construction$_2$ + 2.0311 construction$_3$ + $\epsilon_i$

## List the assumptions of the model (Linear Regression)

- The normal linear regression model has the four standard assumptions:

1. The linear relationship is appropriate.

2. The responses are independent.

3. The responses are normally distributed.

4. The responses have equal variation.

## Discussion of all the assumptions of the model, including tests of the assumptions

- The assumptions have the following interpretations:

1. The linear relationship is appropriate: This states that the linear function relating the parameters (the $\beta's$) to the mean of the response is reasonable. This is often evaluated by considering residual plots: a pattern in the plot may suggest that the proposed relationship is not appropriate.

2. The responses are independent: This states that the responses are distributed independently of each other. This is typically evaluated by considering the sampling technique. Tests are available in special cases.

3. The responses are normally distributed: This states that the shape of the distribution of all responses is normal. This is typically tested by classical normality tests applied to the residuals, along with normal probability plots.

4. The responses have equal variation: This states the the responses come from distributions with the same variation. This is typically tested in regression using White's Test or the Breusch-Pagan test, both of which consider the effects of individual predictors on response variation within the model. Residual plots are
also considered.

## Tests of the assumptions

- a. Linearity of regression function 

```{r linearity non formal}
library(ggplot2)
qplot(NormalModel$fitted.values,NormalModel$residuals)
``` 

<img width="727" alt="ggplot1" src="https://github.com/user-attachments/assets/a58237dd-09cc-4f06-8c5f-a92a4a968744">

conclusion: Since their is no bouncing of the dots randomly, it is not conclusive enough to conclude linearity in the regression functions.

```{r linearity formal}
NormalModel = lm(accidents~exposure+construction1+construction2+construction3)
summary(NormalModel)
```

Hypothesis:

 $H_0: \beta_1 = 0$ (no linear relationship)
 
 $H_A: \beta_1 ≠ 0$ (there is linear relationship between Y and X)
 
Conclusion: there is a significant linear relationship between target variable 'accidents' (Y) and the predictors or features (X's) since the p-value is 5.957e-08 which is less than $\alpha$ = 0.05, we therefore reject the null hypothesis and conclude, linearity in the regression function

- b. Normality of variables

```{r normality}
shapiro.test(NormalModel$residuals)
```

<img width="744" alt="Shapiro" src="https://github.com/user-attachments/assets/de4bc5f9-bd84-4bb1-93ca-9d6e67bf0bc1">


 Hypothesis:
 
 $H_o$: Normal
 
 $H_a$: Not normal

conclusion: Since the p value is 0.0729 which is greater than alpha 0.05, we fail to reject the null and conclude normality of variables.

- c. Non-constant Variance

```{r nonconstant}
library(car)
ncvTest(NormalModel)
```

<img width="468" alt="Non CNS" src="https://github.com/user-attachments/assets/27c880f6-78b2-4b8c-ba26-78760013db49">


 Hypothesis:
 
 $H_o$: constant variance
 
 $H_a$: Non constant variance

conclusion: Since the p value is 0.0065455 which is less than alpha 0.05, we reject the null and conclude constant variance in the error terms.
 
- d. Independence of error terms 

conclusion: Since their is no time order given to the data, we assume independence of error terms. 

## Describe the meaning of Homogeneity of Variance
Homogeneity of variance: Describes a situation in which the error term is 
the same across all values of the independent variables which are exposure, construction 1,construction 2 and construction 3.

consequences of ignoring violations of this assumption

We cannot apply the formula of the variance of the coefficient to conduct tests of significance and construct confidence intervals.

If μ (error term) is heteroscedastic, the OLS estimates will be inefficient in small samples

The coefficient estimates would still be statistically unbiased even if the μ‘s are heteroscedastic

The prediction of Y for a given value of X based on the estimates from the original
data, would have a high variance.

OR

This assumption means that each observed response comes from a distribution with the same level of variation, regardless of the specific response. In other words, while each response has some inherent variability, this variability remains constant across all observations. Although it doesn’t explicitly state that the variances across groups are equal, this is a natural consequence of the assumption. In this context, it implies that the "number of accidents" is drawn from a distribution with consistent, unchanging variation.

When applying the ncvTest to the proposed normal model, the results provide evidence to reject the assumption of constant variance (X² = 4.6339, p = 0.0313). Furthermore, the residuals versus predicted values plot suggests increasing variance as predicted values rise. This plot indicates additional problems, such as a potential misfit in the relationship or missing predictors. Overall, the test shows signs of non-constant variation. If this violation is ignored, the actual distribution of the F-statistic becomes uncertain, making hypothesis tests potentially misleading.

## Process of Using Transformation 

- Identify the type of transformation that is most effective  

- After transformation, rerun the ANOVA on the transformed data  

 - Recheck the transformed data against the assumptions for the ANOVA  
 
 - Lastly, if the transformation is significant, then keep it. If not, disregard it  
 
## Purpose of Transformation

Transformations are applied so that the data appear to more closely meet the assumptions of statistical inference procedure that is to be applied or to adjust/improve the interpretability of appearance of graphs. OR Transformations are chosen to stabilize variance and are commonly known as "variance-stabilizing transformations." The goal is to apply a power or other nonlinear transformation to the response variable, ensuring that fluctuations in outcomes remain consistent across different values of the independent variables. Residual plots are often used to help identify appropriate variance-stabilizing transformations by providing visual clues about the pattern of variability.

## How are they selected?

- If variance increases linearity, then we use the square root transformation on the response variable i,e sqrt(y)

- If variance increases then decreases parabolically, we use arcsin(sqrt(y))

- If variance increases quadratically, then we use ln(y)

## Descriptive statistics to investigate possible transformation of the response variable

Although formal methods exist for selecting appropriate transformations, one approach is to examine the shape of the residual plot(s) to make an educated guess about a suitable transformation. In the residual plot above, the variation appears to increase linearly with the predicted values, suggesting that a square root transformation of the response variable might be a reasonable option to try.

## Variance-stabilizing transformation of the outcome of interest

As stated above, the residual plot suggests that a square root transformation could be effective. Additionally, a logarithmic transformation might also be worth considering.

```{r transformation}
sqrt_accidents = sqrt(accidents)
```

## Using the transformed response, re-fit the linear model and evaluate the model assumptions

```{r Sqrt}
SQRTModel = lm(sqrt_accidents~exposure+construction1+construction2+construction3)
summary(SQRTModel)
```

<img width="566" alt="SQRT" src="https://github.com/user-attachments/assets/a6396c0f-7347-4e64-8a5f-7d9b8ff62285">

Hypothesis:

$H_0$: $\beta_1$ = 0 (no linear relationship)

H$_A$: $\beta_1$ ≠ 0 (there is linear relationship between y and x)

Conclusion: there is a significant linear relationship between Y and the Xs since the p-value is 6.219e-11 which is less than alpha 0.05, we therefore reject the null hypothesis and conclude, linearity in the regression function

```{r transformation normality }
library(car)
ncvTest(SQRTModel)
```

<img width="468" alt="Non CNS" src="https://github.com/user-attachments/assets/3227f8a9-0390-4f95-b3f4-e5bded75bacc">


Using the square root of the number of accidents as the new outcome variable, the test for non-constant variance shows no significant evidence of deviations from homoscedasticity (X² = 0.3813; p = 0.5369). The residual plot now resembles random noise, indicating that the transformation has addressed some model issues, including the previously observed changing variation in the outcome.

```{r non formal}
library(ggplot2)
qplot(SQRTModel$fitted.values,SQRTModel$residuals)
``` 

<img width="737" alt="SQgplot" src="https://github.com/user-attachments/assets/24c5c56b-5d59-42ea-90b1-decce65a459f">

## Process of weighted Least Squares to accommodate departures from constant variance

 - Fit the regression model by un-weighted least squares and analyze the residuals. 
 
 - Estimate the variance function or the standard deviation function by regressing either the squared residuals or the absolute residuals on the appropriate predictor(s). 
 
 - Use the fitted values from the estimated variance or standard deviation function to obtain the weights. 
 
 - Estimate the regression coefficients using these weights.

## Purpose of weighted Least Squares

Weighted Least Square is a remedy for non constant variance that is, it helps to reduce or eliminate
unequal variances of the error terms. That is, Weighted Least Squares (WLS) is a technique that adjusts both the estimation process and standard errors to account for varying levels of response variability across observations. Observations with higher variability are assigned "less weight" in parameter estimation and standard error calculations, while those with lower variability are given "greater weight". 

## What decision must be made by the researcher

The researcher has to choose the weights. In this approach, the researcher determines the weights for each observation, typically by applying a transformation to an independent variable in the model.

## Using descriptive statistics, investigate possible weights to be used in fitting the normal regression model

Scatterplots are commonly used to examine predictors linked to variation in the response variable. These plots typically display either the response values or residuals from a normal model against transformations of potential weighting variables. Shown are plots of residuals versus the square root of months in service and the square root of exposure, though other variables can also be explored.

```{r weights descriptive}
qplot(service_months,NormalModel$residuals)
qplot(service_months^(1/2),NormalModel$residuals)
```

```{r exposure descriptive}
qplot(exposure,NormalModel$residuals)
qplot(exposure^(1/2),NormalModel$residuals)
```

## Select and justify appropriate weights for fitting the proposed linear model.

```{r weights}
qplot(service_months^(1/2),NormalModel$residuals)
qplot(exposure^(1/2),NormalModel$residuals)
```

<img width="740" alt="serv" src="https://github.com/user-attachments/assets/a256675a-d4d8-4671-a828-a3bf248fa3bc">

<img width="717" alt="exp" src="https://github.com/user-attachments/assets/f8eec6ad-7c70-47df-8721-71be775d15ba">

Both predictors seem to explain some of the varying residual patterns. To address heteroscedasticity, the square root of the months in service will be used as weights in the regression analysis. 

## Using your weights, re-fit the linear model and evaluate the model assumptions.

```{r linearity of weights}
WModel = lm(accidents~exposure+construction1+construction2+construction3,weights=(service_months^(1/2)))
summary(WModel)
```

<img width="626" alt="Wmodell" src="https://github.com/user-attachments/assets/1c3a3521-113b-499c-b71e-426215833d4b">

## Re-evaluate model assumptions

- a. Non constant Variance

```{r weights non constant}
library(car)
ncvTest(WModel)
```

<img width="603" alt="NcvT" src="https://github.com/user-attachments/assets/27afe859-e4b1-4e8a-9fd1-f15a39a0e58c">

- b. Normality of variables

```{r weights normality}
shapiro.test(WModel$residuals)
```

- c. Linearity of regression function 

```{r linearity2}
library(ggplot2)
qplot(WModel$fitted.values,WModel$residuals)
```

<img width="732" alt="WmodelQplot" src="https://github.com/user-attachments/assets/d7df2ddc-9427-4628-8a8d-8bbb2597d0ed">

Despite using the square root of service months as weights, the constant variance test still fails to detect significant heteroscedasticity (χ² = 1.3252, p = 0.2497). However, the residual plot continues to indicate potential heteroscedasticity. This suggests that other transformations of service months or additional predictors might be necessary to achieve a better-fitting model.
