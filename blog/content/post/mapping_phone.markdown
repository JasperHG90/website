---
title: "Mapping your phone's location history, part 1"
layout: post
categories: ["tutorial"]
author: Jasper Ginn
date: "2014-12-06T10:31:00"
tag: ["ggmap", "ggplot2", "R", "Python"]
blog: true
description: It's no secret that phone manufacturers log your geolocation at all times, but at least they're open about it these days. I had no doubts that my Google Nexus 5 phone logged every move I made, and I was interested to see how I could map these geolocations
---

It's no secret that phone manufacturers log your geolocation at all times, but at least they're open about it these days. I had no doubts that my Google Nexus 5 phone logged every move I made, and I was interested to see how I could map these geolocations.

If you're using Android, you can download your location history from your <a href="https://maps.google.com/locationhistory/b/0">gmail account</a>. Once in the location history menu, you can select and view at most 30 days worth of history. This is slightly annoying if you want an oversight of, say, the past six months.

In order to work around this limitation, you can simply download the data for each month. The data is downloaded as a 'KML' file. Chances are that you (like me) are not that familiar with KML files. However, a quick look suggests that it is some form of XML file, which means that we can use BeautifulSoup to extract the data. You can download the script I used for this process <a href="https://github.com/JasperHG90/ConvertKML">here</a>.

Once you have extracted the data from the KML files, you are ready to load the data into R:

{% highlight R %}
# Remove objects stored in global environment
rm(list=ls())
# set working directory
setwd("/Users/Jasper/Documents/Private/Wordpres/Old/Blog9_GoogleTracking/Setup/")

# Load libraries
require(ggplot2) ## --> For plotting beautiful figures
require(ggmap) ## --> Plotting maps
require(raster) ## --> To get administrative maps
require(rgdal) ## --> basic spatial functions
require(dplyr) ## --> data management
require(lubridate) ## --> working with date formats

# load data
data <- tbl_df(read.csv(paste0(getwd(),"/data/","location_data.txt"), sep=";", header=T))
{% endhighlight %}

First, I would like to see roughly how many locations are logged daily on my phone. The <a href="http://cran.r-project.org/web/packages/lubridate/index.html">lubridate</a> package allows us to easily convert raw dates into more workable formats. We then use <a href="http://cran.r-project.org/web/packages/dplyr/index.html">dplyr</a> to group and summarize the data by date. Essentially, this simply leaves us with counts of logged locations for each day.[^1] We can then plot these counts using <a href="http://cran.r-project.org/web/packages/ggplot2/index.html">ggplot2</a>.

{% highlight R %}
# Convert the date stamps into Year-Month-Day format
data$Datestamp <- as.Date(ymd(data$Datestamp))

# Summarize
locsum <- data %>%
	group_by(Datestamp) %>%
	summarize(count=n())

# Plot

ggplot(locsum, aes(x=Datestamp, y=count)) +
	geom_line(size=2, alpha=0.8) +
	theme_bw() +
	scale_x_date(breaks="months", minor_breaks="weeks", labels=date_format("%b %d"), name="Date") +
	scale_y_continuous(name="Number of logged locations") +
	theme(legend.text = element_text(size = 13),
	legend.title = element_text(size=15, face="bold"),
	axis.title.x = element_text(face="bold",size=20),
	axis.title.y = element_text(face="bold",size=20),
	axis.text.x = element_text(size="15"),
	axis.text.y = element_text(size="15"),
	panel.background = element_rect(fill = "transparent",colour = NA),
	panel.grid.minor = element_blank(),
	panel.grid.major = element_blank(),
	panel.border = element_blank(),
	axis.line = element_line(color = 'black'))
{% endhighlight %}

In the image below, you see the number of logged locations (y-axis) for each given day (x-axis). On average, my phone logs 556 locations per day. The exception here being April 6th, where I may have switched off my mobile data service.

<div>
	<img src="https://www.jasperginn.nl/assets/images/blog/phone_location_history/Counts.png">
</div>

We're going to use ggplot2 and ggmaps (part of the ggplot2 package) to plot our data. First, we fetch a static google map via the ggmap package. Then, we download a <a href="http://en.wikipedia.org/wiki/Shapefile">shapefile</a> with administrative regions, and transform the coordinate system to <a href="http://en.wikipedia.org/wiki/World_Geodetic_System">WGS84</a> such that we can put the layer on top of the google map. In order for ggmap to be able to 'understand' the geolocations, we have to use a function called 'fortify'. After that, we're all ready to go!

{% highlight R %}
#### Netherlands in total

# Fetch a google map
NL.map <- get_map(location = "the netherlands", maptype = "hybrid",
color = "bw", zoom = 8)
# Download shapefile with administrative regions
shpData <- getData('GADM', country='netherlands', level=1)
# Transform to WGS84 coordinate system
shpData <- spTransform(shpData, CRS("+proj=longlat +datum=WGS84"))
# Fortify shapefile so ggmap can read it
shpData <- fortify(shpData)
# plot map + shapefile
nlMap <- ggmap(NL.map) + geom_polygon(aes(x=long, y=lat, group=group),
data=shpData, color ="blue", fill ="blue", alpha = .1, size = .3)
# plot (map + shapefile) + datapoints
nlMap + geom_point(data = data, aes(x = longitude, y = latitude),
color = "darkred", alpha = 0.4, size = 3) +
scale_colour_manual(values=c("blue","red"))
{% endhighlight %}

The image below basically plots the latitude against the longitude values, and adds the google map, shapefile with administrative regions (blue) and my geolocations (red). I mostly travel by train, so you can actually make out the train tracks on this image.

<div>
	<img src="https://www.jasperginn.nl/assets/images/blog/phone_location_history/WholeNed.png">
</div>

What if we want to take a closer look at, say, locations in a specific municipality? Because ggmap works well with Google maps, you can basically specify any location name the same way you would on Google maps. In the example below, we're zooming in and plotting locations in Amsterdam only. Unfortunately, we cannot easily download shapefiles for administrative regions on the municipality level through R. However, the package <a href="http://cran.r-project.org/web/packages/rgdal/index.html">rgdal</a> allows us to import shapefiles downloaded from the internet and luckily, there are <a href="http://guides.library.upenn.edu/content.php?pid=324392&sid=2655132">many places</a> where we can download such shapefiles, and a quick Google search would give you even more.

{% highlight R %}
##### Only Amsterdam

# Fetch google map of amsterdam
AMS.map <- get_map(location = "amsterdam", maptype = "hybrid",
color = "bw", zoom = 12)
require(rgdal)
# Import shapefile
muni <- readOGR(".", "brtk2010_ind2005_region")
# Transform coordinate system to WGS84
muni <- spTransform(muni, CRS("+proj=longlat +datum=WGS84"))
# Transform so that ggplot can use the file
muni <- fortify(muni)
# Map + shapefile plot
amsMap <- ggmap(AMS.map) + geom_polygon(aes(x=long, y=lat, group=group),
data=muni, color ="orange", fill ="orange", alpha = .08, size = .1)
# Add points
amsMap + geom_point(data = data, aes(x = longitude, y = latitude),
	color = "darkred", alpha = 0.4, size = 3) +
	scale_colour_manual(values=c("blue","red"))
{% endhighlight %}

<div>
	<img src="https://www.jasperginn.nl/assets/images/blog/phone_location_history/ggmapAms.png">
</div>

This is one of the <a href="http://bcb.dfci.harvard.edu/~aedin/courses/R/CDC/maps.html">many ways</a> we can plot geospatial data in R. Although you could go even further and analyze the places you visit most often, it's mainly just a fun exercise. If you would like more information about plotting geospatial data in R, I would recommend going through <a href="http://stat405.had.co.nz/ggmap.pdf">this</a> short paper co-authored by <a href="http://had.co.nz/">Hadley Wickam</a>, the developer of ggplot2 and dplyr.

Part 2 will focus on creating more appealing visuals.

<h1>Notes</h1>
[^1]: I'll be using the chaining ('%>%') function from the dplyr package. This way, we can get to the summarized results in one go. If you're not familiar with dplyr, check out <a href="http://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html">its vignette</a> on CRAN. It is definitely worth it.
