---
title: "Distilling Event Data from News Articles"
layout: post
date: "2015-01-18T14:20:00"
categories: ["Analysis"]
tags: ["EL:DIABLO", "Event data", "GDELT"]
author: Jasper Ginn
blog: true
description: Before 2000, researchers coded small datasets based on reputable news sources to extract events. Since then, increasingly sophisticated algorithms and computing standards allow us to expand this process to an unprecedented scale
---

Whereas general readers consume news to remain informed, social scientists use the same news to extract data on the events reported in order to study their meaning. Before 2000, researchers coded small datasets based on reputable news sources to extract events. Since then, increasingly sophisticated algorithms and computing standards (e.g. <a href="https://www.informs.org/ORMS-Today/Public-Articles/June-Volume-41-Number-3/DOING-UNNATURAL-THINGS-WITH-NATURAL-LANGUAGE" target="_blank">Natural Language Processing</a> and Machine Learning) allow us to expand this process to an unprecedented scale, turning event data into an invaluable tool to monitor e.g. protests and violent events at near real-time speed.

In the land of event data, there are currently three well-known event datasets: the <em><a href="http://www.lockheedmartin.com/us/products/W-ICEWS.html">World-Wide Integrated Crisis Early Warning System</a></em> (ICEWS), developed by Lockheed Martin for the US government. ICEWS is somewhat regarded as the <a href="http://mdwardlab.com/sites/default/files/GDELTICEWS_0.pdf">'gold standard'</a> of event data, although very few people currently have access to it. The second dataset is the <em><a href="http://gdeltproject.org/">The Global Database on Events, Language and Tone</a></em> (GDELT), which was released in 2013. Finally, we have the somewhat unpronounceable <em><a href="http://openeventdata.github.io/">Event/Location: Dataset In A Box, Linux-Option or event data in a box, basically</a></em> (EL:DIABLO),[^1] created by the Open Event Data Alliance. Whereas ICEWS is coded by hand, the latter two are entirely coded by computers.

I have been working a lot with EL:DIABLO in the past months, and it looks extremely promising. In this post, I will go into how event data is collected, how EL:DIABLO and GDELT differ from one another, and I'll discuss some of the pitfalls of event data in general.

<strong>Event Data: How does it work?</strong>

Both GDELT and EL:DIABLO create events out of news articles. Basically, the software looks for 'actors' (e.g. countries, heads of states, religious figures etc.) who perform some sort of 'action' (e.g. cooperate, respond, attack etc.). These data are then combined with further attributes. For example, an article can refer to particular ethnic groups, non-state actors or locations.

At this point, you may be wondering how the software identifies these actors, actions and attributes. Essentially, these are all versions of <a href="http://en.wikipedia.org/wiki/Statistical_classification">classification problems</a>. The software uses <a href="https://github.com/openeventdata/Dictionaries/blob/master/Phoenix.International.actors.txt">'dictionaries'</a> to identify e.g. actors. Most commonly, actions are coded into <em><a href="http://en.wikipedia.org/wiki/Conflict_and_Mediation_Event_Observations" target="_blank">'Conflict and Mediation Event Observations'</a></em> (CAMEO Codes). The 'tone' of an article (the intensity of conflict or cooperation) is measured by a metric called the <em><a href="http://web.pdx.edu/~kinsella/jgscale.html" target="_blank">'Goldstein Scale'</a></em>. The event extraction process is schematically outlined below.

<div>
  <img src="https://www.jasperginn.nl/assets/images/blog/distilling_event_data_articles/GDELT_Overview.png">
</div>

Let's illustrate this with an example from <a href="http://www.themoscowtimes.com/news/article/onslaught-of-ousted-spies-indicates-new-rules-of-global-espionage/511309.html">this Moscow Times article</a>. It contains the following sentence:  

<em>'Russia expelled a German diplomat from the Moscow embassy after a Russian diplomat working in Bonn was sent home amid speculation that he was a spy'</em>

This sentence would generate an event where the 'source actor' (Russia) expelled the 'target actor' (German citizen) from Russia. The article further mentions attributes such as the fact that the target actor is a diplomat, and reveals that Germany had earlier also expelled a Russian diplomat who was an alleged spy. This sentence would therefore contain two events.

<strong>Some issues with event data . . . </strong>

Using event data is not without pitfalls. Essentially, we are aggregating information originating from several news sources. These sources are affected by various biases, are unlikely to capture all events, and are likely to be 'noisy'. We can divide these issues into several categories, illustrated in the image below. We start out with four hypothetical 'real' events, some of which are reported on, some of which are not. The stories that are picked up by the media are subject to, for example, <em><a href="https://civic.mit.edu/blog/kanarinka/how-close-to-home-crisis-attention-and-geographic-bias" target="_blank">geography bias</a></em>, alluding to the fact that stories about the United States tend to be better reported on than stories about, say, rural China. There are 'article selection biases', arising from our ability to be much better at processing English-language texts than texts written in other languages, and issues of so-called '<a href="http://www.parusanalytics.com/eventdata/papers.dir/EWER.pdf" target="_blank">media fatigue</a>', where media tend to become less interested in e.g. conflicts over time and thus tend to report less on these conflicts (think Syria).

<div>
  <img src="https://www.jasperginn.nl/assets/images/blog/distilling_event_data_articles/GDELT_ISSUES.png">
</div>

Other issues are related to the fact that both the <a href="http://www.ibmbigdatahub.com/infographic/four-vs-big-data" target="_blank">volume (the size of data) and velocity (the speed at which data grows)</a> of the data grow at increasing rates because (a) we get better at capturing and storing events, and (b) the total amount of news articles increases over time. The image below shows the growth of observations in the GDELT database over the period 2000-2012. This throws up all sorts of road blocks when analysing temporal dynamics.

<div>
  <img src="https://www.jasperginn.nl/assets/images/blog/distilling_event_data_articles/figure1.png">
</div>

Although we can somewhat mitigate such issues by normalising the data, as is done in the figure below for GDELT observations concerning violent events in Afghanistan between 2000 and 2012, it remains difficult to assess <em>what exactly is driving the dynamics of reported violent events</em>. Does the drop reported events post-2001 resemble a lack of interest in violent events in Afghanistan? Does it resemble the actual lack of violence? Or does it show us the growth of the number of articles over time? Ultimately, we can deal with such issues in various ways, but it does illustrate how 'noisy' event data can be.

<div>
  <img src="https://www.jasperginn.nl/assets/images/blog/distilling_event_data_articles/figure3.png">
</div>

<strong>Comparing GDELT and EL:DIABLO</strong>

When <a href="http://gdeltproject.org/about.html" target="_blank">GDELT</a> was released back in 2013, it was initially heralded as the next big thing in computational social sciences before it was shut down due to concerns about <a href="http://andrewspath.com/ibnmonkey/gdelt-suspended/" target="_blank">the underlying data</a>.[^2] Around about May 2014, however, it was back online. Currently, the project is backed by Google Ideas and integrated into <a href="http://googlecloudplatform.blogspot.nl/2014/05/worlds-largest-event-dataset-now-publicly-available-in-google-bigquery.html" target="_blank">Google BigQuery</a>.

One of the biggest problems with GDELT is that it does not seem to de-duplicate aggregated news articles. This means that highly shocking events, which tend to capture a lot of media attention, will generate an absurd amount of data points.[^3] Ultimately, then, this means that GDELT does not actually measure 'events', but rather measures 'reports', which is a crucial difference.

Other issues relate to <a href="http://badhessian.org/2014/02/assessing-gdelt-with-handcoded-protest-data/" target="_blank">the large number of false positives</a> that people have stumbled on while playing around with the dataset, as well as the fact that GDELT data is basically a 'black box'; we don't know which dictionaries are used, we don't know exactly which sources are used, and we cannot asses the methods by which it generates events properly.[^4]

After the initial problems with GDELT, several of the project's contributors went ahead and started EL:DIABLO, which is currently spearheaded by the <a href="http://openeventdata.github.io/" target="_blank">Open Event Data Alliance</a>. In contrast to GDELT, EL:DIABLO is completely open-source; meaning that we can pick the whole process apart and see how events are created exactly.[^5]

EL:DIABLO offers some major advantages over GDELT. Firstly, whereas GDELT uses the <a href="http://eventdata.parusanalytics.com/software.dir/tabari.html" target="_blank">TABARI</a> engine to distill events from news articles, EL:DIABLO makes use of the far more sophisticated <a href="https://github.com/openeventdata/petrarch" target="_blank">PETRARCH</a> engine. The difference between these pieces of software relate to their ability to interpret context; TABARI relies on <a href="http://en.wikipedia.org/wiki/Pattern_matching">pattern matching</a>, but PETRARCH <a href="http://petrarch.readthedocs.org/en/latest/" target="_blank">can process</a> full sentences that are parsed via Natural Language Processing. Furthermore, EL:DIABLO does a better job at capturing events because it actually <a href="https://andrewhalterman.files.wordpress.com/2014/11/halterman-beieler_encore-event_data_and_versioning.pdf" target="_blank">attempts to de-duplicate articles</a>, and is able to extract new actors from articles, basically allowing almost tailor-made updates on custom documents.

However, there are several drawbacks to using EL:DIABLO. Firstly, and somewhat unsurprisingly, the quality of the input[^6] heavily influences the quality of data resulting from it. As such, New York Times articles would generally yield more accurate data than, say, articles from Russia Today. Currently, the system scrapes roughly <a href="https://github.com/openeventdata/scraper/blob/master/whitelist_urls.csv" target="_blank">350 RSS feeds</a> of varying quality on an hourly basis, which results in a somewhat lesser quality dataset.

Secondly, EL:DIABLO does not collect historical data. While the current setup is interesting and allows for a potential near real-time updating system of event data, some projects require the monitoring of events over longer periods of time. Of course, collecting large numbers of articles is somewhat of a legal grey area, as scraping entire news paper archives usually violates end user agreements and intellectual property rights.[^7] However, getting historical event data is not completely impossible as you can write your own scrapers that get the article contents from various online newspapers, because PETRARCH will allow you to process any parsed sentences.

For now, it seems as if EL:DIABLO is a promising project. Its possibilities as an event monitoring system are quite large, and the fact that it is transparent and freely available allows us to peek under the hood of the system and assess how it goes about its business. This is truly its greatest advantage over GDELT. In adopting techniques such as machine learning to create event data at near real-time speed, we need to simultaneously adopt a mindset that allows for trial-and-error approaches, transparency and <a href="https://dartthrowingchimp.wordpress.com/2015/01/22/the-state-of-the-art-in-the-production-of-political-event-data/">human oversight</a>.

<h1>Notes</h1>

[^1]: Technically, EL:DIABLO is not a 'dataset', but rather a tool with which to create a dataset.
[^2]: Apparently, there were issues with the intellectual property rights of the historical backlog on which GDELT was based.
[^3]: One example is the kidnapping of 200 girls in Nigeria by Boko Haram. GDELT created several hundreds of data points out of this one event. See e.g. <a href="http://fivethirtyeight.com/datalab/nigeria-kidnapping/ " target="_blank">this</a> article, and <a href="http://badhessian.org/2014/05/it-is-time-to-get-rid-of-the-e-in-gdelt/" target="_blank">this</a> follow-up.
[^4]: Although there are some reports that compare GDELT to e.g. ICEWS which find that they <a href="http://mdwardlab.com/sites/default/files/GDELTICEWS_0.pdf" target="_blank">correlate quite well</a> with one another.
[^5]: EL:DIABLO is currently <a href="https://github.com/openeventdata/eldiablo" target="_blank">available from github</a> as a <a href="http://jasperginn.nl/?p=283" target="_blank">Vagrant box</a>.
[^6]: By 'input', I specifically refer to the quality of the written text (grammatically, syntax-wise etc.).
[^7]: Phil Schrodt wrote a <a href="https://asecondmouse.wordpress.com/2014/02/14/the-legal-status-of-event-data/" target="_blank">blog post</a> on the legal status of event data, reasoning that because the data is processed, it becomes in essence a new product.
