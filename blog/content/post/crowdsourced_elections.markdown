---
title: "What can crowdsourced data tell us about the conduct of elections?"
layout: post
date: "2015-09-25T12:45:00"
tags: ["Crowd-sourced data","Nigeria","Elections","Nigerian elections"]
categories: ["analysis"]
author: Johanna Renz
description: What does the crowdsourcing website reclaimnaija reveal about irregularities that obstruct the electoral process during the Nigerian elections of 2011 and 2015?
---

<em>This article was written by <a href="https://uk.linkedin.com/in/johanna-renz-a886a895" target="_blank">Johanna Renz</a> around about the time of the 2015 Nigerian Elections. The scripts used to download the data from reclaimnaija can be found <a href="https://github.com/JasperHG90/reclaimnaija-data" target="_blank">on GitHub</a>.</em>

So far, not many social scientists have paid attention to crowd sourced data, which is essentially all data that is collected through the contributions of a crowd. In contrast to many ‘conventional’ data sources, crowd sourced data can offer information almost instantly and on a high level of detail. At this point, however, we cannot fully judge the validity of crowd sourced information or whether it accurately (and completely) reflects events in the ‘real’ world.

For my thesis at <a href="http://www.lucthehague.nl/" target="_blank">Leiden University College The Hague</a>, I explored crowd sourced data and how such data can help us understand electoral processes. This is a brief summary showing some of my thought processes and findings. Specifically, I want to find out what the crowdsourcing website <a href="http://www.reclaimnaija.net/" target="_blank">reclaimnaija.net</a> reveals about irregularities that obstruct the electoral process during the recent Nigerian elections (2011 and 2015).

The website ReclaimNaija was launched for the 2011 elections in Nigeria. It uses <a href="http://www.ushahidi.com/" target="_blank">Ushahidi</a> software, which was developed during the 2009 Kenyan post-election crisis citizens to allow citizens to report on acts of violence. Since then, websites using Ushahidi software have been launched in multiple countries. In Nigeria, the website allows citizens to become election observers and submit reports to the website – for example using SMS or email. These reports are gathered and displayed on a map on the website.

Here are some quick facts about the Nigerian elections (mostly obtained from European Union Observation Mission Reports and Human Rights Watch).

Both general elections were comprised of three elections each, spread out over a couple of weeks: those to vote for a Nigerian President, the National Assembly and most of the 36 Governors. In 2011, there were some instances of reported fraud. The incumbent Goodluck Jonathan of the People’s Democratic Party was re-elected. After the presidential elections, violence broke out, killing around 800 people.

In contrast, the 2015 elections were reported to be less violent, but some instances of fraud also obstructed the election process. The results of the 2015 elections were extraordinary: It was the first time since the end of military rule in 1999 that elections brought about a change in power. The incumbent Jonathan accepted the results, so that Muhammadu Buhari of the All Progressive Congress could become President.

To get an overview of the ReclaimNaija reports, I created two maps (using ArcGIS), showing the number of reports submitted in 2011 and 2015. Comparing these two maps allows us to make some observations, which enable us to draw a couple of tentative conclusions about the usefulness of crowd sourced data.

Let’s look at the geographical distribution of the reports: In both elections, citizens submitted reports from all Nigerian states. This may seem like a minor observation, but it does mean that even in the most conflict-ridden states of Nigeria (think of the North-Eastern region that has been under attack from Boko Haram for years now), citizens know about the website, observe the elections, and have access to the relevant technology to submit a report. Thus, crowd sourced reports could become a useful addition to traditional election observers, who cannot be present in all areas of a country.

<div>
  <img src="https://blog.jasperginn.nl/assets/img/blog/crowdsourced_elections/map_totrep2011.png">
  <figcaption class="caption">Map of total number of Reports in the Nigerian 2011 elections</figcaption>
</div>

<div>
  <img src="https://blog.jasperginn.nl/assets/img/blog/crowdsourced_elections/map_totrep2015.png">
  <figcaption class="caption">Map of total number of reports in the Nigerian 2015 elections</figcaption>
</div>

To criticise crowd sourced information, one could argue that the number of reports to ReclaimNaija do not reflect the number of election irregularities at all, but only reflect the number of people in an area who have access to mobile phones and internet and who know about the website. These maps suggest something else: the geographical distribution of reports changed between 2011 and 2015. If one assumes, that the distribution of mobile phone users did not change entirely within four years, the change in report distribution suggests that the number of reports does reflect the election events, at least to some extent.

This has been a mere exploration of the total report numbers using simple cloropleth maps. It can be used as a starting point to investigate the data further and more systematically. There has already been a significant increase of reports to ReclaimNaija between 2011 and 2015 (more than double). This suggests that an increasing number of people have access to the website. Therefore, this type of data may have potential to serve as an information source to understand the conduct of elections better. Furthermore, as crowd sourcing grows more popular, it may support democratisation, empowering citizens to collectively check on their government and their electoral processes.
