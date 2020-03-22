---
layout: post
comments: true
title:  Executing Stata in R using RStata package.
categories: Stata  R
---

Try to migrate from one statistical software to another is, most of the time, an arduous task. It is common to hear people who use one statistical software (i.e., Stata) and try to learn another one (i.e., R) desists because of the lack of time to learn it appropriately. On the other hand, several people use both languages depending on the task to solve but have to switch between programs and interfaces, losing valuable time.

The main advantage of learning and using both languages is that you can match your programming skills to what is more comfortable for you.  In my opinion, I think that Stata is better handy for data cleaning and working with a single data frame. On the other side, R is far better for plotting using [`ggplot2`](https://ggplot2.tidyverse.org/) package and connecting with other API (e.g., Google Maps).

Nevertheless, using both languages and switching between them is time costly. While I used both programs separately, I lost a lot of time and disk space writing files in Stata and loading them in R to make my plots. Fortunately, I found a solution to this issue.

The [`RStata`](https://github.com/lbraglia/RStata) package provides and excelent solution to use both languages in the same interface (in this case R). Check the documentation [here](https://cran.r-project.org/web/packages/RStata/README.html). First of all, we have to install the package from CRAN and activate it.

```r
install.packages('RStata')
library(RStata)
```


Secondly, we have to indicate to R wich one is our Stata path and version. For example, my path and version of Stata are:

```r
options("RStata.StataPath" = "/Applications/Stata/StataMP.app/Contents/MacOS/stata-mp")
options("RStata.StataVersion" = 14)
```
In the case of Windows, we have to delete the `.exe` from the path.


Now we are able to run Stata commands from a R session. Let's see some examples:

**Running a single line commands**
```r
stata('di "Hello World"')
stata('di 2+2')
stata('clear all')
```
**Running several lines  command**
```r
command <- "sysuse auto, clear
  sum mpg, d"
stata(command)
```

**Running an entire `.do` file.**
```r
stata("my_dofile.do")
```
Although at this point, nothing is shocking about this package, the most potent usability of the tool comes when passing or receiving arguments from it. Let me see some examples. Imagine the following situation: you want to clean some data in Stata to nextly plot it in R. The workflow that I follow for months to tackle this task was to write the data as a `.dta` file an then read it with R. This procedure filled my Google Drive with a lot of folders and files, making it difficult to track them appropriately.

To avoid this, you can use the `data.out` parameter built inside the package. This option returns an R data frame object.
```r
data = stata("data_creation.do",
      data.out = TRUE)
```

You also can have a situation where you have to pass Stata some data that you have in R to execute commands on it. In this case, we use `data.in` parameter:

```r
random_df = data.frame('column1'= rnorm(100))
stata("sum column1, d", data.in = random_df)
```

Finally, you can combine these two operations:
```r
random_df = data.frame('column1'= rnorm(100))
random_df2 = stata("gen column2 = 2*column1 if column1>0 ", data.in = random_df, data.out = TRUE)
```

I hope this post will help Stata and/or R users' to do more convenient the use of both languages at the same time. 
