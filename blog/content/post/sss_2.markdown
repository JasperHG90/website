---
title: "Shiny server series part 2: running shiny on multiple ports"
layout: post
categories: ["tutorial"]
date: "2017-04-15T22:39:00"
tag: ["R", "shiny", "shiny server", "rstudio server"]
author: Jasper Ginn
blog: true
description: In this part, we’ll configure shiny server to run on two ports; one on 3838 and one on 4949 (although this is arbitrary and you could use any port you'd like). This way, we can set up our server to service a publicly accessible shiny server instance as well as an instance that will be password protected.
---

*This guide is part of a series on setting up your own private server running shiny apps. There are many guides with great advice on how to set up an R shiny server and related software. I try to make a comprehensive guide based in part on these resources as well as my own experiences. I always aim to properly attribute information to their respective sources. If you notice an issue, please contact me.*

*I noticed there were some errors in the first part of this series. These are now fixed. The revised post is available [here](https://www.jasperginn.nl/shiny-server-series-pt1/)*

Part 1 of this series is available [here](https://www.jasperginn.nl/shiny-server-series-pt1/)

Part 3 of this series is available [here](https://www.jasperginn.nl/shiny-server-series-pt3/)

In [part 1](https://www.jasperginn.nl/shiny-server-series-pt1/) of this series, we set up shiny server on a VPS. Our setup currently looks like this:

* Rstudio server is running on port 8787
* Shiny server is running on port 3838
* We have a custom domain name pointing to the DigitalOcean VPS

In this part, we’ll configure shiny server to run on two ports; one on 3838 and one on 4949 (although this is arbitrary and you could use any port you'd like). This way, we can set up our server to service a publicly accessible shiny server instance as well as an instance that will be password protected. This is useful if you want to share an app with a select group of people or just for development purposes.

## Resources used for this part
This tutorial draws from  [Rstudio’s documentation](https://support.rstudio.com/hc/en-us/articles/219045167-Shiny-Server-Quick-Start-Run-Shiny-Server-on-multiple-ports)  about shiny server configuration.

## Running shiny on multiple ports
These steps are fairly straightforward. First, log into your VPS, switch to the shiny user and back up the shiny configuration file in case something goes wrong:

```shell
# Log into the server
ssh root@<your-VPS-ip>
# Switch to shiny user
su shiny
# Copy the config file
sudo cp /etc/shiny-server/shiny-server.conf /etc/shiny-server/shiny-server-backup.conf
```

Open the configuration file:

```shell
sudo nano /etc/shiny-server/shiny-server.conf
```

This configuration governs the port on which shiny runs, as well as the location where it stores error logs and where shiny can find your apps.

Firstly, add the following line under ‘run_as shiny’ (denoted by ‘1’ in the image below). This ensures that shiny outputs error logs in this folder:

```text
# Log all Shiny output to files in this directory
log_dir /var/log/shiny-server;
```

Then, add ‘127.0.0.1’ next to ‘listen 3838’ (denoted by ‘2’ in the image below).

Finally, add the following lines below the last ‘}’ (denoted by ‘3’ in the image below):

```text
# Define a server that listens on port 4949
server {
  listen 4949 127.0.0.1;

  # Define a location at the base URL
  location / {
    # Host the directory of Shiny Apps stored in this directory
    site_dir /srv/private-server;

    # When a user visits the base URL rather than a particular application,
    # an index of the applications available in this directory will be shown.
    directory_index on;
  }
}
```

Hit `control+x` and `Y` and `enter` to save your changes.

![](img/blog/shiny_series_pt2/SSS_prt2_1.png)

This configuration ensures that shiny listens on both ports 3838 and 4949. As you can see in the config file, the private shiny server does not look for shiny apps in the ‘/srv/shiny-server’ folder, but rather in the ‘/srv/private-server’ folder. We need to create this folder and give the shiny user read and write permissions:

```shell
# Navigate to the /srv/ folder
cd /srv
# Create a folder for the private shiny server
sudo mkdir private-server
# Enter the folder
cd private-server
# Give shiny read/write permissions
sudo chown -R shiny:shiny-apps .
sudo chmod g+w .
sudo chmod g+s .
```

Now, we just need to restart shiny server:

```shell
sudo service shiny-server restart
```

You are now running shiny server on both port 3838 and 4949!

Now, we simply need to arrange a new route to the nginx configuration. To do this, open up the config file:

```shell
sudo nano /etc/nginx/sites-enabled/default
```

Then, copy paste the following lines just below the editor route. Indent the lines so they line up with the other routes, and remember to only use spaces, no tabs.

```text
location /private-apps/ {
    proxy_pass http://127.0.0.1:4949/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
}
```

It should look like the configuration in the image below:

![](img/blog/shiny_series_pt2/SSS_prt2_2.png)

Hit `control+x` and `Y` and `enter` to save your changes. Then, restart nginx:

```shell
sudo service nginx restart
```

You can now access your second shiny server on `http://www.<yourdomainname>.<extension>/private-apps/`

In the next part, we’ll add an SSL security certificate to the server.
