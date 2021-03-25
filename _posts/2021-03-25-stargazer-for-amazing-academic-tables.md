---
title:  "Stargazer: A solution to produce amazing academic tables"
layout: post
date:   "2021-03-25"
categories: ["Coding"]
---

## Introduction
As an economist working as a research assistant, I would hold that producing tables with estimation results is one of the job's main tasks. As the research project flows, the results change, and as a consequence, the tables. With this in mind, the importance of maintaining the tables pipeline automatized is a crucial task. In the first place, you avoid typing errors due to manual typing, and most importantly, you prevent manually typing a table hand. Second, when you have a code to make the results for you, your results are reproducible. This issue is important to share the code with other researchers, as well as your future self.

 In this post, I will show how to achieve amazing automatized regression tables using the R package called `stargazer`. This fantastic package  (which name is also an incredible song written by [Rainbow](https://youtu.be/rVXy1OhaERY){:target="_blank"}) produce table outputs with the data objects that are outputs of your estimations. Almost every detail of the tables is customizable!


## Using `stargazer`
### Data set
According to the documentation, `swiss` data contains "*Standardized fertility measure and socio-economic indicators for each of 47 French-speaking provinces of Switzerland at about 1888*". You can find more details [here](https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/swiss.html){:target="_blank"}.


```r
data('swiss')
head(swiss)
```

```
##              Fertility Agriculture Examination Education Catholic
## Courtelary        80.2        17.0          15        12     9.96
## Delemont          83.1        45.1           6         9    84.84
## Franches-Mnt      92.5        39.7           5         5    93.40
## Moutier           85.8        36.5          12         7    33.77
## Neuveville        76.9        43.5          17        15     5.16
## Porrentruy        76.1        35.3           9         7    90.57
##              Infant.Mortality
## Courtelary               22.2
## Delemont                 22.2
## Franches-Mnt             20.2
## Moutier                  20.3
## Neuveville               20.6
## Porrentruy               26.6
```

### Basic Usage
We will use a Linear Regression model to explain how Fertility is affected by several variables for explanatory purposes. Of course, `stargazer` supports a broader set of packages, including Instrumental Variables, Fixed Effects Models, among many others. Check the [supported models](https://rdrr.io/cran/stargazer/man/stargazer_models.html){:target="_blank"} to see all the possibilities. Finally, I will format all the tables in `html` to show the outputs. In the last part of this post, I will show how to use `LaTeX` outputs.  
**Disclaimer**: I intend to show how to draw tables using `stargazer`, so do not expect too much from these models.


```r
model_1 <- lm(Fertility ~ Catholic + Infant.Mortality, data = swiss)
model_2 <-
  lm(Fertility ~ Catholic + Infant.Mortality + Agriculture, data = swiss)
model_3 <-
  lm(Fertility ~ Catholic + Infant.Mortality + Agriculture + Examination,
     data = swiss)

# Checking a summary
summary(model_1)
```

```
## 
## Call:
## lm(formula = Fertility ~ Catholic + Infant.Mortality, data = swiss)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -32.406  -4.336   2.036   5.317  20.421 
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)   
## (Intercept)      35.59794   10.66161   3.339  0.00172 **
## Catholic          0.12071    0.03752   3.217  0.00243 **
## Infant.Mortality  1.48317    0.53719   2.761  0.00837 **
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 10.45 on 44 degrees of freedom
## Multiple R-squared:  0.3309,	Adjusted R-squared:  0.3005 
## F-statistic: 10.88 on 2 and 44 DF,  p-value: 0.0001447
```


The package is straightforward to use. As we see in the example, just passed the objects with the estimation parameters to the table, and you will have an excellent output.


```r
# install.packages('stargazer')
library(stargazer)

stargazer(model_1, model_2, model_3, type = "html")
```


<table style="text-align:center"><tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="3" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="3">Fertility</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Catholic</td><td>0.121<sup>***</sup></td><td>0.088<sup>**</sup></td><td>0.026</td></tr>
<tr><td style="text-align:left"></td><td>(0.038)</td><td>(0.040)</td><td>(0.038)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Infant.Mortality</td><td>1.483<sup>***</sup></td><td>1.633<sup>***</sup></td><td>1.396<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.537)</td><td>(0.526)</td><td>(0.463)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Agriculture</td><td></td><td>0.142<sup>*</sup></td><td>-0.048</td></tr>
<tr><td style="text-align:left"></td><td></td><td>(0.073)</td><td>(0.080)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Examination</td><td></td><td></td><td>-0.968<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td>(0.253)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>35.598<sup>***</sup></td><td>26.748<sup>**</sup></td><td>59.603<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(10.662)</td><td>(11.274)</td><td>(13.042)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>47</td><td>47</td><td>47</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.331</td><td>0.386</td><td>0.545</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.301</td><td>0.343</td><td>0.501</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>10.448 (df = 44)</td><td>10.125 (df = 43)</td><td>8.820 (df = 42)</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>10.881<sup>***</sup> (df = 2; 44)</td><td>9.007<sup>***</sup> (df = 3; 43)</td><td>12.565<sup>***</sup> (df = 4; 42)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="3" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

### Editing Headers
Of course, your requirements will vary depending on which results do you want to show. Within this function, you have many options to format the header of your table. Here I present some features that you can modify (`dep.var.caption`, `dep.var.labels`, `column.labels`, `dep.var.labels.include`, `model.numbers`), but there are many more!



```r
stargazer(
  model_1,
  model_2,
  model_3,
  type = "html",
  dep.var.caption  = "My new caption",
  dep.var.labels   = "Fertility (Live births per 1,000 inhabitants)",
  column.labels = c("OLS", "OLS", "OLS"),
  dep.var.labels.include = TRUE,
  # By default is TRUE, change to FALSE to supress the dep var labels,
  model.numbers = TRUE
  # By default is TRUE, change to FALSE to supress models' numbers.
)
```


<table style="text-align:center"><tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3">My new caption</td></tr>
<tr><td></td><td colspan="3" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="3">Fertility (Live births per 1,000 inhabitants)</td></tr>
<tr><td style="text-align:left"></td><td>OLS</td><td>OLS</td><td>OLS</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Catholic</td><td>0.121<sup>***</sup></td><td>0.088<sup>**</sup></td><td>0.026</td></tr>
<tr><td style="text-align:left"></td><td>(0.038)</td><td>(0.040)</td><td>(0.038)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Infant.Mortality</td><td>1.483<sup>***</sup></td><td>1.633<sup>***</sup></td><td>1.396<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.537)</td><td>(0.526)</td><td>(0.463)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Agriculture</td><td></td><td>0.142<sup>*</sup></td><td>-0.048</td></tr>
<tr><td style="text-align:left"></td><td></td><td>(0.073)</td><td>(0.080)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Examination</td><td></td><td></td><td>-0.968<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td>(0.253)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>35.598<sup>***</sup></td><td>26.748<sup>**</sup></td><td>59.603<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(10.662)</td><td>(11.274)</td><td>(13.042)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>47</td><td>47</td><td>47</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.331</td><td>0.386</td><td>0.545</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.301</td><td>0.343</td><td>0.501</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>10.448 (df = 44)</td><td>10.125 (df = 43)</td><td>8.820 (df = 42)</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>10.881<sup>***</sup> (df = 2; 44)</td><td>9.007<sup>***</sup> (df = 3; 43)</td><td>12.565<sup>***</sup> (df = 4; 42)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="3" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

#### Multicolumns
You can arrange the `column.labels` array to work with multicolumn using `column.separate`.
For instance,  in econometrics, it is common to use the column labels to reference the estimation strategy used to estimate a set of coefficients. Imagine that you are working on the same three models presented above, with the difference that you are estimating the third one with *Instrumental Variables* (IV) approach instead of *Ordinary Least Squares* (OLS).


```r
stargazer(
  model_1,
  model_2,
  model_3,
  type = "html",
  column.labels   = c("OLS", "IV"),
  column.separate = c(2, 1) 
  # First label for the first two columns and the second for the third one
)
```


<table style="text-align:center"><tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="3" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="3">Fertility</td></tr>
<tr><td style="text-align:left"></td><td colspan="2">OLS</td><td>IV</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Catholic</td><td>0.121<sup>***</sup></td><td>0.088<sup>**</sup></td><td>0.026</td></tr>
<tr><td style="text-align:left"></td><td>(0.038)</td><td>(0.040)</td><td>(0.038)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Infant.Mortality</td><td>1.483<sup>***</sup></td><td>1.633<sup>***</sup></td><td>1.396<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.537)</td><td>(0.526)</td><td>(0.463)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Agriculture</td><td></td><td>0.142<sup>*</sup></td><td>-0.048</td></tr>
<tr><td style="text-align:left"></td><td></td><td>(0.073)</td><td>(0.080)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Examination</td><td></td><td></td><td>-0.968<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td>(0.253)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>35.598<sup>***</sup></td><td>26.748<sup>**</sup></td><td>59.603<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(10.662)</td><td>(11.274)</td><td>(13.042)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>47</td><td>47</td><td>47</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.331</td><td>0.386</td><td>0.545</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.301</td><td>0.343</td><td>0.501</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>10.448 (df = 44)</td><td>10.125 (df = 43)</td><td>8.820 (df = 42)</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>10.881<sup>***</sup> (df = 2; 44)</td><td>9.007<sup>***</sup> (df = 3; 43)</td><td>12.565<sup>***</sup> (df = 4; 42)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="3" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

### Changing Covariates' Labels
Change the covariate labels is also super easy. Just pass an array with your desired titles to the parameter `covariates.labels`.  My suggestion is that you ALWAYS must check the order of the features used in an estimation model because you could wrongly label a variable with another name.


```r
labels <- c(
  'Catholic (% as opposed to protestant)',
  'Infant Mortality	*live births who live less than 1 year)',
  'Agriculture	(% of males involved in agriculture as occupation',
  'Examination	(% draftees receiving highest mark on army examination)'
)

stargazer(model_1,
          model_2,
          model_3,
          type = "html",
          covariate.labels = labels)
```


<table style="text-align:center"><tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="3" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="3">Fertility</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Catholic (% as opposed to protestant)</td><td>0.121<sup>***</sup></td><td>0.088<sup>**</sup></td><td>0.026</td></tr>
<tr><td style="text-align:left"></td><td>(0.038)</td><td>(0.040)</td><td>(0.038)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Infant Mortality	*live births who live less than 1 year)</td><td>1.483<sup>***</sup></td><td>1.633<sup>***</sup></td><td>1.396<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.537)</td><td>(0.526)</td><td>(0.463)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Agriculture	(% of males involved in agriculture as occupation</td><td></td><td>0.142<sup>*</sup></td><td>-0.048</td></tr>
<tr><td style="text-align:left"></td><td></td><td>(0.073)</td><td>(0.080)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Examination	(% draftees receiving highest mark on army examination)</td><td></td><td></td><td>-0.968<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td>(0.253)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>35.598<sup>***</sup></td><td>26.748<sup>**</sup></td><td>59.603<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(10.662)</td><td>(11.274)</td><td>(13.042)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>47</td><td>47</td><td>47</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.331</td><td>0.386</td><td>0.545</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.301</td><td>0.343</td><td>0.501</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>10.448 (df = 44)</td><td>10.125 (df = 43)</td><td>8.820 (df = 42)</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>10.881<sup>***</sup> (df = 2; 44)</td><td>9.007<sup>***</sup> (df = 3; 43)</td><td>12.565<sup>***</sup> (df = 4; 42)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="3" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

### Styles
This package has a parameter to configure the table's custom according to several academic journals' aesthetics. For example, in the model below, I present a table with *American Economic Review* style. You can check all the options at this [link](https://rdrr.io/cran/stargazer/man/stargazer_style_list.html){:target="_blank"}.


```r
stargazer(model_1, model_2, model_3, type = "html", style='aer')
```


<table style="text-align:center"><tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3">Fertility</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Catholic</td><td>0.121<sup>***</sup></td><td>0.088<sup>**</sup></td><td>0.026</td></tr>
<tr><td style="text-align:left"></td><td>(0.038)</td><td>(0.040)</td><td>(0.038)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Infant.Mortality</td><td>1.483<sup>***</sup></td><td>1.633<sup>***</sup></td><td>1.396<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.537)</td><td>(0.526)</td><td>(0.463)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Agriculture</td><td></td><td>0.142<sup>*</sup></td><td>-0.048</td></tr>
<tr><td style="text-align:left"></td><td></td><td>(0.073)</td><td>(0.080)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Examination</td><td></td><td></td><td>-0.968<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td>(0.253)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>35.598<sup>***</sup></td><td>26.748<sup>**</sup></td><td>59.603<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(10.662)</td><td>(11.274)</td><td>(13.042)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Observations</td><td>47</td><td>47</td><td>47</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.331</td><td>0.386</td><td>0.545</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.301</td><td>0.343</td><td>0.501</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>10.448 (df = 44)</td><td>10.125 (df = 43)</td><td>8.820 (df = 42)</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>10.881<sup>***</sup> (df = 2; 44)</td><td>9.007<sup>***</sup> (df = 3; 43)</td><td>12.565<sup>***</sup> (df = 4; 42)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Notes:</em></td><td colspan="3" style="text-align:left"><sup>***</sup>Significant at the 1 percent level.</td></tr>
<tr><td style="text-align:left"></td><td colspan="3" style="text-align:left"><sup>**</sup>Significant at the 5 percent level.</td></tr>
<tr><td style="text-align:left"></td><td colspan="3" style="text-align:left"><sup>*</sup>Significant at the 10 percent level.</td></tr>
</table>


### Customizing Stats of the Table
You can customize how to present the coefficients and the statistical inference using the `report` parameter. In my opinion, the syntax of this parameter a little tricky. Here are two examples of how to modify this feature:  


```r
# rep_format <- "vcp*" # variable name, coefficient, p value and stars
rep_format <- "vct" # variable name, coefficient, t statistic without stars

stargazer(model_1,
          model_2,
          model_3,
          type = "html",
          report = rep_format) 
```


<table style="text-align:center"><tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="3" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="3">Fertility</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Catholic</td><td>0.121</td><td>0.088</td><td>0.026</td></tr>
<tr><td style="text-align:left"></td><td>t = 3.217</td><td>t = 2.192</td><td>t = 0.679</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Infant.Mortality</td><td>1.483</td><td>1.633</td><td>1.396</td></tr>
<tr><td style="text-align:left"></td><td>t = 2.761</td><td>t = 3.104</td><td>t = 3.018</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Agriculture</td><td></td><td>0.142</td><td>-0.048</td></tr>
<tr><td style="text-align:left"></td><td></td><td>t = 1.962</td><td>t = -0.593</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Examination</td><td></td><td></td><td>-0.968</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td>t = -3.829</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>35.598</td><td>26.748</td><td>59.603</td></tr>
<tr><td style="text-align:left"></td><td>t = 3.339</td><td>t = 2.372</td><td>t = 4.570</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>47</td><td>47</td><td>47</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.331</td><td>0.386</td><td>0.545</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.301</td><td>0.343</td><td>0.501</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>10.448 (df = 44)</td><td>10.125 (df = 43)</td><td>8.820 (df = 42)</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>10.881<sup>***</sup> (df = 2; 44)</td><td>9.007<sup>***</sup> (df = 3; 43)</td><td>12.565<sup>***</sup> (df = 4; 42)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="3" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

### Using personalized standard errors and p-values
In general, a best practice while estimating linear models is to use robust standard errors since the good asymptotic properties allow us to improve the estimation's statistical inference.
For `Stata` users, the option `, r` at the end of every regression performs this task. Unfortunately, in R, the [approach](https://www.r-econometrics.com/methods/hcrobusterrors/){:target="_blank"} is not as direct as in `Stata` to use these errors. Nonetheless, `stargazer` allow us to use customized standard errors and p-values for our tables.


```r
# install.packages('lmtest')
# install.packages('sandwich')

library("lmtest") # coeftest
library("sandwich") # vcovHC

# Robust standard errors: 
# Check https://www.r-econometrics.com/methods/hcrobusterrors/ for more details/
inference_m1 <-
  coeftest(model_1, vcov = vcovHC(model_1, type = "HC0"))
inference_m2 <-
  coeftest(model_2, vcov = vcovHC(model_2, type = "HC0"))
inference_m3 <-
  coeftest(model_3, vcov = vcovHC(model_3, type = "HC0"))

stargazer(
  model_1,
  model_2,
  model_3,
  se = list(NULL, inference_m2[, 2], inference_m3[, 2]), # If NULL use model_* errors
  p = list(NULL, inference_m2[, 4], inference_m3[, 4]), # If NULL use model_* pvalues
  omit.stat = "f",
  type = "html"
) 
```


<table style="text-align:center"><tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="3" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="3">Fertility</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Catholic</td><td>0.121<sup>***</sup></td><td>0.088<sup>**</sup></td><td>0.026</td></tr>
<tr><td style="text-align:left"></td><td>(0.038)</td><td>(0.037)</td><td>(0.036)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Infant.Mortality</td><td>1.483<sup>***</sup></td><td>1.633<sup>***</sup></td><td>1.396<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.537)</td><td>(0.433)</td><td>(0.471)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Agriculture</td><td></td><td>0.142</td><td>-0.048</td></tr>
<tr><td style="text-align:left"></td><td></td><td>(0.090)</td><td>(0.067)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Examination</td><td></td><td></td><td>-0.968<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td>(0.232)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>35.598<sup>***</sup></td><td>26.748<sup>**</sup></td><td>59.603<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(10.662)</td><td>(11.190)</td><td>(12.262)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>47</td><td>47</td><td>47</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.331</td><td>0.386</td><td>0.545</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.301</td><td>0.343</td><td>0.501</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>10.448 (df = 44)</td><td>10.125 (df = 43)</td><td>8.820 (df = 42)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="3" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>


### Customizing Footer Stats
Of course, you don't need to show always the same indicators in every table. With the option `keep.stat`, you can specify an array of statistics to show in your table. In this example, I only show the sample size and the R-squared. You can check the [list of statistics](https://www.rdocumentation.org/packages/stargazer/versions/5.2.2/topics/stargazer_stat_code_list){:target="_blank"} here!

```r
stargazer(
  model_1,
  model_2,
  model_3,
  type = "html",
  keep.stat = c("n", "rsq")
) 
```


<table style="text-align:center"><tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="3" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="3">Fertility</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Catholic</td><td>0.121<sup>***</sup></td><td>0.088<sup>**</sup></td><td>0.026</td></tr>
<tr><td style="text-align:left"></td><td>(0.038)</td><td>(0.040)</td><td>(0.038)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Infant.Mortality</td><td>1.483<sup>***</sup></td><td>1.633<sup>***</sup></td><td>1.396<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.537)</td><td>(0.526)</td><td>(0.463)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Agriculture</td><td></td><td>0.142<sup>*</sup></td><td>-0.048</td></tr>
<tr><td style="text-align:left"></td><td></td><td>(0.073)</td><td>(0.080)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Examination</td><td></td><td></td><td>-0.968<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td>(0.253)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>35.598<sup>***</sup></td><td>26.748<sup>**</sup></td><td>59.603<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(10.662)</td><td>(11.274)</td><td>(13.042)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>47</td><td>47</td><td>47</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.331</td><td>0.386</td><td>0.545</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="3" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

### Adding new lines
In some context, it is helpful to add lines at the end of the table. For example, when using Fixed Effects, it is common to see an array indicating if an estimation is controlling for some Fixed Effect or not. From my perspective, the best way to achieve this is to use the `add.lines` option. Here, you specify a list of arrays with all the lines that you want to add.

```r
stargazer(
  model_1,
  model_2,
  model_3,
  type = "html",
  keep.stat = c("n", "rsq"),
  add.lines = list(
    c("County Fixed Effect", "No", "Yes", 'Yes'),
    c("Time Fixed Effect", "Yes", "No", 'Yes')
  )
) 
```


<table style="text-align:center"><tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="3" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="3">Fertility</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Catholic</td><td>0.121<sup>***</sup></td><td>0.088<sup>**</sup></td><td>0.026</td></tr>
<tr><td style="text-align:left"></td><td>(0.038)</td><td>(0.040)</td><td>(0.038)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Infant.Mortality</td><td>1.483<sup>***</sup></td><td>1.633<sup>***</sup></td><td>1.396<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.537)</td><td>(0.526)</td><td>(0.463)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Agriculture</td><td></td><td>0.142<sup>*</sup></td><td>-0.048</td></tr>
<tr><td style="text-align:left"></td><td></td><td>(0.073)</td><td>(0.080)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Examination</td><td></td><td></td><td>-0.968<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td>(0.253)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>35.598<sup>***</sup></td><td>26.748<sup>**</sup></td><td>59.603<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(10.662)</td><td>(11.274)</td><td>(13.042)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">County Fixed Effect</td><td>No</td><td>Yes</td><td>Yes</td></tr>
<tr><td style="text-align:left">Time Fixed Effect</td><td>Yes</td><td>No</td><td>Yes</td></tr>
<tr><td style="text-align:left">Observations</td><td>47</td><td>47</td><td>47</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.331</td><td>0.386</td><td>0.545</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td colspan="3" style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

### Customizing Notes
I wouldn't say I like this option very much because the notes are introduced in a multicolumn, affecting the table's width depending on the notes' length. Nevertheless, I leave some examples of how to customize them.

```r
stargazer(
  model_1,
  model_2,
  model_3,
  type = "html",
  keep.stat = c("n", "rsq"),
  add.lines = list(
    c("County Fixed Effect", "No", "Yes", 'Yes'),
    c("Time Fixed Effect", "Yes", "No", 'Yes')
  ),
  notes.label = 'Comments', # Edit here the label
  notes = 'Own elaboration based on swiss data. Significance levels: * 90%, ** 95%, *** 99%.',
  notes.append = FALSE, # TRUE append the significance levels
  notes.align = 'l' # c center, r right
) 
```


<table style="text-align:center"><tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td colspan="3"><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="3" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td colspan="3">Fertility</td></tr>
<tr><td style="text-align:left"></td><td>(1)</td><td>(2)</td><td>(3)</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Catholic</td><td>0.121<sup>***</sup></td><td>0.088<sup>**</sup></td><td>0.026</td></tr>
<tr><td style="text-align:left"></td><td>(0.038)</td><td>(0.040)</td><td>(0.038)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Infant.Mortality</td><td>1.483<sup>***</sup></td><td>1.633<sup>***</sup></td><td>1.396<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.537)</td><td>(0.526)</td><td>(0.463)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Agriculture</td><td></td><td>0.142<sup>*</sup></td><td>-0.048</td></tr>
<tr><td style="text-align:left"></td><td></td><td>(0.073)</td><td>(0.080)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Examination</td><td></td><td></td><td>-0.968<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td>(0.253)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>35.598<sup>***</sup></td><td>26.748<sup>**</sup></td><td>59.603<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(10.662)</td><td>(11.274)</td><td>(13.042)</td></tr>
<tr><td style="text-align:left"></td><td></td><td></td><td></td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">County Fixed Effect</td><td>No</td><td>Yes</td><td>Yes</td></tr>
<tr><td style="text-align:left">Time Fixed Effect</td><td>Yes</td><td>No</td><td>Yes</td></tr>
<tr><td style="text-align:left">Observations</td><td>47</td><td>47</td><td>47</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.331</td><td>0.386</td><td>0.545</td></tr>
<tr><td colspan="4" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Comments</td><td colspan="3" style="text-align:left">Own elaboration based on swiss data. Significance levels: * 90%, ** 95%, *** 99%.</td></tr>
</table>

### Stargazer and LaTeX
`stargazer` and `LaTeX` are a great combination together! Change the parameter `type`  to `"latex"` and the package will do the work for you. You can also save the output in an external file specifying the parameter `out`. Additionally, if you don't want your table in the `table` environment (i.e. you just want the `tabular`), use the option `float`. In future posts, I will go deeper on this issue.

```r
stargazer(
  model_1,
  model_2,
  model_3,
  type = "latex",
  # out = 'path/of/your/table.tex'
  header = FALSE, # If TRUE, stargazer print the header with the citation and package info
  float = TRUE # If FALSE, the function don't returns the table in a table environment
) 
```

```
## 
## \begin{table}[!htbp] \centering 
##   \caption{} 
##   \label{} 
## \begin{tabular}{@{\extracolsep{5pt}}lccc} 
## \\[-1.8ex]\hline 
## \hline \\[-1.8ex] 
##  & \multicolumn{3}{c}{\textit{Dependent variable:}} \\ 
## \cline{2-4} 
## \\[-1.8ex] & \multicolumn{3}{c}{Fertility} \\ 
## \\[-1.8ex] & (1) & (2) & (3)\\ 
## \hline \\[-1.8ex] 
##  Catholic & 0.121$^{***}$ & 0.088$^{**}$ & 0.026 \\ 
##   & (0.038) & (0.040) & (0.038) \\ 
##   & & & \\ 
##  Infant.Mortality & 1.483$^{***}$ & 1.633$^{***}$ & 1.396$^{***}$ \\ 
##   & (0.537) & (0.526) & (0.463) \\ 
##   & & & \\ 
##  Agriculture &  & 0.142$^{*}$ & $-$0.048 \\ 
##   &  & (0.073) & (0.080) \\ 
##   & & & \\ 
##  Examination &  &  & $-$0.968$^{***}$ \\ 
##   &  &  & (0.253) \\ 
##   & & & \\ 
##  Constant & 35.598$^{***}$ & 26.748$^{**}$ & 59.603$^{***}$ \\ 
##   & (10.662) & (11.274) & (13.042) \\ 
##   & & & \\ 
## \hline \\[-1.8ex] 
## Observations & 47 & 47 & 47 \\ 
## R$^{2}$ & 0.331 & 0.386 & 0.545 \\ 
## Adjusted R$^{2}$ & 0.301 & 0.343 & 0.501 \\ 
## Residual Std. Error & 10.448 (df = 44) & 10.125 (df = 43) & 8.820 (df = 42) \\ 
## F Statistic & 10.881$^{***}$ (df = 2; 44) & 9.007$^{***}$ (df = 3; 43) & 12.565$^{***}$ (df = 4; 42) \\ 
## \hline 
## \hline \\[-1.8ex] 
## \textit{Note:}  & \multicolumn{3}{r}{$^{*}$p$<$0.1; $^{**}$p$<$0.05; $^{***}$p$<$0.01} \\ 
## \end{tabular} 
## \end{table}
```


## Final Thoughts
I found `stargazer` a terrific tool for scientific research. It is intuitive to use and is highly customizable.  There is also a `python` [version](https://pypi.org/project/stargazer/){:target="_blank"} under development! 

I hope this post will help you with your tables! :)

## References
- [`stargazer`: beautiful LATEX, HTML and ASCII tables from R statistical output](https://cran.r-project.org/web/packages/stargazer/vignettes/stargazer.pdf){:target="_blank"}
- [A Stargazer Cheatsheet](https://www.jakeruss.com/cheatsheets/stargazer/){:target="_blank"}
- [`stargazer` function - RDocumentation](https://www.rdocumentation.org/packages/stargazer/versions/5.2.2/topics/stargazer){:target="_blank"}
- [Heteroskedasticity Robust Standard Errors in R](https://www.r-econometrics.com/methods/hcrobusterrors/){:target="_blank"}
- [stargazer_models: `stargazer`: list of supported objects](https://rdrr.io/cran/stargazer/man/stargazer_models.html){:target="_blank"}
