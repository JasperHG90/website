---
title: "The gender gap in director compensation at S&P500 firms"
layout: post
date: "2014-11-29T14:03:00"
categories: ["analysis"]
tags: ["Analysis", "Boardex", "Director compensation", "S&P500"]
author: Jasper Ginn
blog: true
description: After reading several articles on remuneration across gender in the boards of S&P500 firms, I wondered if it was possible to get a closer look at it
---

<em>Data and scripts available upon request</em>

After reading several articles on remuneration across gender in the boards of S&amp;P500 firms (see <a href="http://www.bloomberg.com/news/2013-08-13/best-paid-women-in-s-p-500-settle-for-less-with-18-gender-gap.html" target="_blank">here</a>, <a href="http://www.bloomberg.com/news/2013-08-13/best-paid-women-in-s-p-500-settle-for-less-with-18-gender-gap.html" target="_blank">here</a> and <a href="http://fortune.com/tag/ceo-compensation/" target="_blank">here</a>), I wondered if it was possible to get a closer look at it.

I happened to have access to <a href="https://www.boardex.com/" target="_blank">BoardEx</a>, a high-quality database that contains a lot of information on corporate boards and their board members, so I wrote a little script to scrape all the data about S&amp;P500 board members. In the image below, each board member's age is plotted against the compensation that board member received. Here, compensation is logged because it is heavily skewed to the right (meaning that there are directors who get a much higher compensation compared to other directors in the dataset). The colouring signifies gender.

<div>
	<https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/gender_gap_director_compensation/Gender_BoardEx.png">
</div>

At this point, you may be tempted to either conclude that male directors earn a lot more than their female counterparts, or you may be wondering why there appear to be two 'clusters' in the image above.

Corporate boards generally consist of executive directors and supervisory (or independent) directors. Executive directors are managers at the firm (eg. the Chief executive officer), while supervisory directors oversee the executive directors, and are 'independent' in the sense that they have no other ties to the company other than supervising the managers of the firm.

It won't surprise you to hear that executive directors generally earn a lot more than the independent directors. The image below shows this divide in terms of compensation.

<div>
	<https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/gender_gap_director_compensation/Directorship_BoardEx.png">
</div>

So, what can we say about the difference in compensation for male and female directors? Well, they seem to lie very close to one another. The median compensation for female independent directors is 180.000 EUR versus 190.000 EUR for male independent directors. Conversely, the median compensation for female executive directors is 9.3 million EUR, while the median compensation for male executive directors is 'only' 8.5 million EUR. Overall, this looks quite equal.

We can use a basic linear regression model to see whether Gender is predicted to have an effect on director compensation. The model also gives us information about possible data issues. In the table below, we can see the effect of each of the input variables (Age, Gender, Directorship and an <a href="http://www.theanalysisfactor.com/interpreting-interactions-in-regression/" target="_blank">interaction effect</a> between gender and director type) on the directorship compensation. This model serves as an illustration, so don't worry if you don't get what's going on here. The important takeaway from the model is that gender is not predicted to affect compensation.[^1] What matters irrespective of gender is the directorship type; if you are an executive director, you will earn more. Moreover, age (surprise, surprise) is a predictor of director compensation.

<table style="text-align: center;"><caption><strong>Regression Table for Compensation predicters (standard errors in parentheses)</strong></caption>
<tbody>
<tr>
<td style="border-bottom: 1px solid black;" colspan="2"></td>
</tr>
<tr>
<td style="text-align: left;"></td>
<td><em>Dependent variable:</em></td>
</tr>
<tr>
<td></td>
<td style="border-bottom: 1px solid black;" colspan="1"></td>
</tr>
<tr>
<td style="text-align: left;"></td>
<td>Compensation (log)</td>
</tr>
<tr>
<td style="border-bottom: 1px solid black;" colspan="2"></td>
</tr>
<tr>
<td style="text-align: left;">Age (Centered)</td>
<td>0.005<sup>***</sup> (0.002)</td>
</tr>
<tr>
<td style="text-align: left;">Gender (Male)</td>
<td>0.051 (0.147)</td>
</tr>
<tr>
<td style="text-align: left;">Director Type (Indep. Director)</td>
<td>-3.759<sup>***</sup> (0.146)</td>
</tr>
<tr>
<td style="text-align: left;">Gender (Male) * Director Type (Indep. Director)</td>
<td>-0.026 (0.150)</td>
</tr>
<tr>
<td style="text-align: left;">Constant</td>
<td>8.955<sup>***</sup> (0.144)</td>
</tr>
<tr>
<td style="border-bottom: 1px solid black;" colspan="2"></td>
</tr>
<tr>
<td style="text-align: left;">Observations</td>
<td>4,174</td>
</tr>
<tr>
<td style="text-align: left;">R<sup>2</sup></td>
<td>0.747</td>
</tr>
<tr>
<td style="text-align: left;">Adjusted R<sup>2</sup></td>
<td>0.747</td>
</tr>
<tr>
<td style="text-align: left;">Residual Std. Error</td>
<td>0.719 (df = 4169)</td>
</tr>
<tr>
<td style="text-align: left;">F Statistic</td>
<td>3,081.736<sup>***</sup> (df = 4; 4169)</td>
</tr>
<tr>
<td style="border-bottom: 1px solid black;" colspan="2"></td>
</tr>
<tr>
<td style="text-align: left;"><em>Note:</em></td>
<td style="text-align: right;"><sup>*</sup>p&lt;0.1; <sup>**</sup>p&lt;0.05; <sup>***</sup>p&lt;0.01</td>
</tr>
</tbody>
</table>

In summary, it seems as if the existing female directors in S&P500 firms don't earn less money than their male counterparts. Of course, this is not the whole story. Female directors are still vastly underrepresented in the boards of firms, let alone top-tier firms such as those represented in the S&P500 index. At these firms, only 741 out of roughly 3800 independent board members are female, and only 25(!) out of 489 executive board members are female. This can affect our results because we simply don't have enough data points (and thus not enough statistical power) to properly extrapolate from the data. An example of this is the interaction effect between gender and directorship type in the model above; because we only have 25 female executive directors, they may have some special 'attributes' (like above-average negotiation skills) which leads to e.g. self-selection. In any case, it would seem to me that the difference in director compensation is less pressing than the lack of a gender balance across these firms.

<h1>Notes</h1>
[^1]: Although gender is not statistically significant in the model, we could argue that the difference that it does predict (namely a 5.23% higher compensation for male directors) is important from a substantive point of view. However, this difference is not extremely high, and the standard deviation for the indicator is so large that it suggests some methodological issues. I'll get back to that at the end of this post.
