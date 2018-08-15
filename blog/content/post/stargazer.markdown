---
title: "Making regression tables with R and Stargazer"
layout: post
categories: ["Tutorial"]
date: "2014-07-14T20:33:00"
author: Jasper Ginn
tag: ["Stargazer", "R", "R Package"]
blog: true
description: One large advantage of using R over other statistical software programs, is that its open-source community gets to develop packages to add to R's basic functionalities.
---

<em>You can find the CPI2006 dataset and R-script used in this post <a href="https://github.com/JasperHG90/stargazer_tutorial" target="_blank">here</a></em>

One large advantage of using <a href="http://www.r-project.org/" target="_blank">R</a> over other statistical software programs, is that its open-source community gets to develop packages to add to R's basic functionalities. One package which is particularly helpful is the <a href="http://cran.r-project.org/web/packages/stargazer/stargazer.pdf" target="_blank">“stargazer” package</a>, which produces beautiful (regression) tables in HTML and LaTeX format.

Suppose we take the <a href="http://archive.transparency.org/policy_research/surveys_indices/cpi/2006" target="_blank">2006 Corruption Perception Index</a> (CPI) data which we downloaded in <a href="http://jasperginn.nl/data-scraping-with-python-and-beautifulsoup/" target="_blank">this</a> post and create some tables with stargazer. I'll be using <a href="http://www.r-project.org/" target="_blank">R base package</a> and <a href="http://www.rstudio.com/" target="_blank">Rstudio</a>, both of which are free to download.

```R
# Clean workspace
#rm(list=ls())
# Set working directory
setwd("E:\\Documents\\Private\\Wordpres\\Blog3_Stargazer\\")
# Install packages (if needed)
require(stargazer)
# load CPI data
CPI<-read.csv("CPI2006.txt",sep=";",header=TRUE)
```

Say we want to investigate the relationship between the CPI score and the range between the lower and upper error rate. The rationale being that countries with a higher CPI score (i.e. less corruption) deliver more accurate survey results than countries with lower CPI scores. Naturally, this relationship may not hold for any of a number of reasons. For example, when a country suffers from corruption but corruption isn't widespread, then it may be more difficult to observe, thus increasing the error rate. We'll first create a LaTeX table containing the summary statistics.

```R
# Plot summary statistics
stargazer(CPI,summary.logical=TRUE,type="html",median=TRUE,no.space=FALSE,
	single.row=FALSE,digits=1,title="Summary Statistics")
```

<table style="text-align: center;"><caption><strong>Summary Statistics</strong></caption>
<tbody>
<tr>
<td style="border-bottom: 1px solid black;" colspan="7"></td>
</tr>
<tr>
<td style="text-align: left;">Statistic</td>
<td>N</td>
<td>Mean</td>
<td>St. Dev.</td>
<td>Min</td>
<td>Median</td>
<td>Max</td>
</tr>
<tr>
<td style="border-bottom: 1px solid black;" colspan="7"></td>
</tr>
<tr>
<td style="text-align: left;">Rank</td>
<td>163</td>
<td>80.1</td>
<td>46.2</td>
<td>1</td>
<td>79</td>
<td>163</td>
</tr>
<tr>
<td style="text-align: left;">CPI_score</td>
<td>163</td>
<td>4.1</td>
<td>2.2</td>
<td>1.8</td>
<td>3.2</td>
<td>9.6</td>
</tr>
<tr>
<td style="text-align: left;">Number_surveys</td>
<td>163</td>
<td>5.8</td>
<td>1.8</td>
<td>3</td>
<td>6</td>
<td>10</td>
</tr>
<tr>
<td style="text-align: left;">Conf_Range_low</td>
<td>163</td>
<td>3.7</td>
<td>2.1</td>
<td>1.6</td>
<td>2.7</td>
<td>9.5</td>
</tr>
<tr>
<td style="text-align: left;">Conf_Range_high</td>
<td>163</td>
<td>4.5</td>
<td>2.2</td>
<td>1.8</td>
<td>3.6</td>
<td>9.7</td>
</tr>
<tr>
<td style="text-align: left;">Conf_Range_Difference</td>
<td>163</td>
<td>0.8</td>
<td>0.5</td>
<td>0.1</td>
<td>0.7</td>
<td>3.2</td>
</tr>
<tr>
<td style="border-bottom: 1px solid black;" colspan="7"></td>
</tr>
</tbody>
</table>

The table above is printed in HTML format. If you were using <a href="http://www.stat.uni-muenchen.de/~leisch/Sweave/" target="_blank">Sweave</a> or <a href="http://yihui.name/knitr/" target="_blank">Knitr</a>, you would change the 'type'-option to 'latex' and then turn it into a PDF file, as you can observe <a href="https://dl.dropboxusercontent.com/u/38011066/wp/Blog3_Stargazer.pdf" target="_blank">by viewing this post in PDF format</a>. You can also print the above table in plain text:

```R
# Plot summary statistics
stargazer(CPI,summary.logical=TRUE,type="text",median=TRUE,no.space=FALSE,
	single.row=FALSE,digits=1,title="Summary Statistics")
```

<pre><code>##
## Summary Statistics
## ======================================================
## Statistic              N  Mean St. Dev. Min Median Max
## ------------------------------------------------------
## Rank                  163 80.1   46.2    1    79   163
## CPI_score             163 4.1    2.2    1.8  3.2   9.6
## Number_surveys        163 5.8    1.8     3    6    10
## Conf_Range_low        163 3.7    2.1    1.6  2.7   9.5
## Conf_Range_high       163 4.5    2.2    1.8  3.6   9.7
## Conf_Range_Difference 163 0.8    0.5    0.1  0.7   3.2
## ------------------------------------------------------
</code></pre>

In order to examine the relationship between the CPI variable and the error margin, let's create a scatterplot using the ggplot2 package:

```R
# Load the ggplot2 package
require(ggplot2)
# Plot a scratterplot between variables
qplot(CPI_score,Conf_Range_Difference,data=CPI)
```

<div>
	<https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/regression_tables_r_stargazer/scatter_1.png">
</div>

It looks like the data shows a curve where the confidence range is lowest at high and low scores, but becomes larger in the mid-level scores. Thus, the CPI scores of mid-range countries have larger error rates. This seems in line with the above theory that corruption is most difficult to observe when it's present, but not widespread. In order to deal with this non-linear relationship, we simply plot a polynomial regression smoother:

{% highlight R %}
# Plot a scratterplot between variables
qq<-qplot(CPI_score,Conf_Range_Difference,data=CPI)
plot(qq + geom_smooth(method="lm",formula=y~x + I(x^2)))
{% endhighlight %}

<div>
	<https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/regression_tables_r_stargazer/scatter_2.png">
</div>

All right, we now model the relationship using simple linear regression. Because the relationship appears to be non-linear, we'll include the polynomial relationship in the second model. The extra settings we're defining allow us to to further customize the output. Find a full overview of the features <a href="http://cran.r-project.org/web/packages/stargazer/stargazer.pdf" target="_blank">here</a>.

```R
# Model
CPI$CPI_score=as.numeric(CPI$CPI_score)
model1<-lm(Conf_Range_Difference~CPI_score,data=CPI)
model2<-lm(Conf_Range_Difference~CPI_score + I(CPI_score^2),data=CPI)
# Stargazer table
stargazer(model1,model2, type="html", column.labels=c("Linear Model","Polynomial Model"),
	title="Example regression table",covariate.labels=c("CPI score","CPI score-squared"),
	no.space=TRUE,single.row=TRUE,style="default",model.numbers=FALSE,multicolumn=FALSE,
	notes=c("These are some notes which replace the standard notes.",
	"Standard errors are reported in parentheses underneath the coefficients.",
	"Significance at p = 0.1, p = 0.5 and p = 0.01 are denoted by '*', '**'
	and '***' respectively"),notes.append=FALSE,notes.align="l")
```

<table style="text-align: center;"><caption><strong>Example regression table</strong></caption>
<tbody>
<tr>
<td style="border-bottom: 1px solid black;" colspan="3"></td>
</tr>
<tr>
<td style="text-align: left;"></td>
<td colspan="2"><em>Dependent variable:</em></td>
</tr>
<tr>
<td></td>
<td style="border-bottom: 1px solid black;" colspan="2"></td>
</tr>
<tr>
<td style="text-align: left;"></td>
<td>Conf_Range_Difference</td>
<td>Conf_Range_Difference</td>
</tr>
<tr>
<td style="text-align: left;"></td>
<td>Linear Model</td>
<td>Polynomial Model</td>
</tr>
<tr>
<td style="border-bottom: 1px solid black;" colspan="3"></td>
</tr>
<tr>
<td style="text-align: left;">CPI score</td>
<td>0.059<sup>***</sup> (0.018)</td>
<td>0.887<sup>***</sup> (0.076)</td>
</tr>
<tr>
<td style="text-align: left;">CPI score-squared</td>
<td></td>
<td>-0.077<sup>***</sup> (0.007)</td>
</tr>
<tr>
<td style="text-align: left;">Constant</td>
<td>0.559<sup>***</sup> (0.083)</td>
<td>-1.177<sup>***</sup> (0.169)</td>
</tr>
<tr>
<td style="border-bottom: 1px solid black;" colspan="3"></td>
</tr>
<tr>
<td style="text-align: left;">Observations</td>
<td>163</td>
<td>163</td>
</tr>
<tr>
<td style="text-align: left;">R<sup>2</sup></td>
<td>0.062</td>
<td>0.468</td>
</tr>
<tr>
<td style="text-align: left;">Adjusted R<sup>2</sup></td>
<td>0.056</td>
<td>0.462</td>
</tr>
<tr>
<td style="text-align: left;">Residual Std. Error</td>
<td>0.494 (df = 161)</td>
<td>0.373 (df = 160)</td>
</tr>
<tr>
<td style="text-align: left;">F Statistic</td>
<td>10.690<sup>***</sup> (df = 1; 161)</td>
<td>70.510<sup>***</sup> (df = 2; 160)</td>
</tr>
<tr>
<td style="border-bottom: 1px solid black;" colspan="3"></td>
</tr>
<tr>
<td style="text-align: left;"><em>Note:</em></td>
<td style="text-align: left;" colspan="2">These are some notes which replace the standard notes.</td>
</tr>
<tr>
<td style="text-align: left;"></td>
<td style="text-align: left;" colspan="2">Standard errors are reported in parentheses underneath the coefficients.</td>
</tr>
<tr>
<td style="text-align: left;"></td>
<td style="text-align: left;" colspan="2">Significance at p = 0.1, p = 0.5 and p = 0.01 are denoted by ‘*’, ‘**’ and</td>
</tr>
<tr>
<td style="text-align: left;"></td>
<td style="text-align: left;" colspan="2">‘***’ respectively</td>
</tr>
</tbody>
</table>

That's it! That's how you use stargazer in combination with R.
