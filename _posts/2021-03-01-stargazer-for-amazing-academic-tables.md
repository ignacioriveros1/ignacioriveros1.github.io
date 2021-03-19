---
title:  "Stargazer: A solution to produce amazing academic tables"
layout: post
# TODO: Actualizar fecha
date:   "2021-03-01"
categories: ["Coding"]
---

## Introduction
- Importance of maintaining the tables automatized
  - Avoid typing errors
  - Help your future self: numbers changes while working on projects, 
  - Reproducible Research

- My experience producing tables
  - Academic Research in Economics <->  Regressions Tables
  - Stata + Latex

- Some difficulties that I have found with my past workflows 
  - Million .tex files with the tables
  - Tricky to customize

- Discover `stargazer`:
  - Your table is produced with the model objects that are outputs of your estimations.
  - Almost every detail of the tables are customizable

## Applied Example

```r
stargazer::stargazer(model_1, type = "html")
```

<table style="text-align:center"><tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"></td><td><em>Dependent variable:</em></td></tr>
<tr><td></td><td colspan="1" style="border-bottom: 1px solid black"></td></tr>
<tr><td style="text-align:left"></td><td>Fertility</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Catholic</td><td>0.121<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.038)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">Infant.Mortality</td><td>1.483<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(0.537)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td style="text-align:left">Constant</td><td>35.598<sup>***</sup></td></tr>
<tr><td style="text-align:left"></td><td>(10.662)</td></tr>
<tr><td style="text-align:left"></td><td></td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left">Observations</td><td>47</td></tr>
<tr><td style="text-align:left">R<sup>2</sup></td><td>0.331</td></tr>
<tr><td style="text-align:left">Adjusted R<sup>2</sup></td><td>0.301</td></tr>
<tr><td style="text-align:left">Residual Std. Error</td><td>10.448 (df = 44)</td></tr>
<tr><td style="text-align:left">F Statistic</td><td>10.881<sup>***</sup> (df = 2; 44)</td></tr>
<tr><td colspan="2" style="border-bottom: 1px solid black"></td></tr><tr><td style="text-align:left"><em>Note:</em></td><td style="text-align:right"><sup>*</sup>p<0.1; <sup>**</sup>p<0.05; <sup>***</sup>p<0.01</td></tr>
</table>

## References
- [`stargazer`: beautiful LATEX, HTML and ASCII tables from R statistical output](https://cran.r-project.org/web/packages/stargazer/vignettes/stargazer.pdf){:target="_blank"}
- [A Stargazer Cheatsheet](https://www.jakeruss.com/cheatsheets/stargazer/){:target="_blank"}
- [`stargazer` function - RDocumentation](https://www.rdocumentation.org/packages/stargazer/versions/5.2.2/topics/stargazer){:target="_blank"}