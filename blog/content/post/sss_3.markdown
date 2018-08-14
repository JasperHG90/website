---
title: "Shiny server series part 3: adding SSL encryption"
layout: post
categories: ["tutorial"]
date: "2017-04-23T19:16:00"
tag: ["R", "shiny", "shiny server", "rstudio server"]
author: Jasper Ginn
description: In part 1 and part 2 of this series, we set up an ubuntu 16.04 server to host shiny applications. Thus far, we configured shiny server to listen on port 3838 (for public apps) and 4949 (for private apps). In this part, we will set up SSL encryption on the server for additional security
blog: true
---

*This guide is part of a series on setting up your own private server running shiny apps. There are many guides with great advice on how to set up an R shiny server and related software. I try to make a comprehensive guide based in part on these resources as well as my own experiences. I always aim to properly attribute information to their respective sources. If you notice an issue, please contact me.*

Part 1 of this series is available [here](https://www.jasperginn.nl/shiny-server-series-pt1/)

Part 2 of this series is available [here](https://www.jasperginn.nl/shiny-server-series-pt2/)

In part 1 and part 2 of this series, we set up an ubuntu 16.04 server to host shiny applications. Thus far, we configured shiny server to listen on port 3838 (for public apps) and 4949 (for private apps). In this part, we will set up SSL encryption on the server for additional security.

## Resources used for this part
This guide is largely based on [this](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-14-04)  tutorial.

## Adding SSL encryption to your server
We’re going to use [certbot](https://certbot.eff.org)  and [Let’s encrypt](https://letsencrypt.org) to set up the SSL certificate.

Log into your server and switch to the Shiny user:

```shell
# Log into the server
ssh root@<your-VPS-ip>
# Switch to shiny user
su shiny
```

Go to the `sbin` folder on your server and download certbot-auto:

```shell
cd /usr/local/sbin
sudo wget https://dl.eff.org/certbot-auto
```

Make the script executable:

```shell
sudo chmod a+x /usr/local/sbin/certbot-auto
```

Now, open up the nginx configuration:

```shell
sudo nano /etc/nginx/sites-available/default
```

Take note of the root location, shown in the image below surrounded by the blue box.  For the remainder of this tutorial, I’ll assume that your root location is located at `/var/www/html`. If it is not, make sure to switch your root location with mine when executing the commands below.

Then, add the contents below to the nginx configuration (surrounded by the red box in the image)

```text
location ~ /.well-known {
    allow all;
}
```

![](img/blog/shiny_series_pt3/setuppartIII.png)

Restart nginx:

```shell
sudo service nginx restart
```

Take your root location and your domain name (with www. and without it) and fill them out in the and <your-domain-name> parts in the command below. Don’t forget to change <.extension> to your extension (e.g. .nl, .com, .eu). Then, execute this command:

```shell
certbot-auto certonly -a webroot --webroot-path=/var/www/html -d <your-domain-name>.<extension> -d www.<your-domain-name>.<extension>
```

Next, we generate a strong [Diffie–Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)  group for extra security:

```shell
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```

SSL certificates expire every couple of months or so, so it’s a good idea to refresh our certificate regularly. We’ll set up a cron job that does this every week.  Access cron by executing the following:

```shell
sudo crontab -e
```

Add the following lines:

```text
30 2 * * 1 /usr/local/sbin/certbot-auto renew >> /var/log/le-renew.log
35 2 * * 1 /etc/init.d/nginx reload
```

![](img/blog/shiny_series_pt3/crontab.png)

Hit `control+x` and then `Y` and `enter`, and your changes will be saved. Congratulations, you have now successfully set up SSL encryption on your server! Note that SSL encryption is not yet operational; we'll take care of that in the next part, when we’ll add user authentication to our private shiny server using [Auth0](https://auth0.com)
