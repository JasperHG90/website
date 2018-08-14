---
title: "Shiny server series part 4: adding user authentication using Auth0"
layout: post
categories: ["tutorial"]
date: "2017-04-30T18:02:00"
tag: ["R", "shiny", "shiny server", "rstudio server"]
author: Jasper Ginn
description: We’re nearly at the end of the shiny server series, and that means that we’ll be getting into the really useful parts. In this tutorial, we’ll use Auth0 to set up user authentication to regulate access to our private shiny server.
blog: true
---

*This guide is part of a series on setting up your own private server running shiny apps. There are many guides with great advice on how to set up an R shiny server and related software. I try to make a comprehensive guide based in part on these resources as well as my own experiences. I always aim to properly attribute information to their respective sources. If you notice an issue, please contact me.*

Part 1 of this series is available [here](https://www.jasperginn.nl/shiny-server-series-pt1/)

Part 2 of this series is available [here](https://www.jasperginn.nl/shiny-server-series-pt2/)

Part 3 of this series is available [here](https://www.jasperginn.nl/shiny-server-series-pt3/)

We’re nearly at the end of the shiny server series, and that means that we’ll be getting into the really useful parts. In this tutorial, we’ll use [Auth0](http://auth0.com) to set up user authentication to regulate access to our private shiny server.

## Resources used for this part
This guide draws on [this](https://auth0.com/blog/adding-authentication-to-shiny-server/) excellent tutorial produced by Auth0.

## Where were we?
In the [previous part](https://www.jasperginn.nl/shiny-server-series-pt3/),  we set up SSL encryption on our server. This makes user authentication safer by encrypting traffic to and from the server. Remember, however, that we didn’t yet set up nginx to use this SSL certificate. So although we set up all the prerequisites, SSL encryption is not yet operational.

As always, the first thing we’ll do is log into the server

```shell
# Log into the server
ssh root@<your-VPS-ip>
```

Before you do anything else, take a note of the location where Let’s encrypt stored your certificate. It’s easiest to just navigate to the following folder

```shell
cd /etc/letsencrypt/live
```

And execute

```shell
ls
```

Write down the name of this folder, you’ll need it to complete the next step.

Now, we can switch to the shiny user:

```shell
su shiny
# Navigate to shiny home
cd
```

Back up the nginx configuration in case something goes wrong. Then, delete the config and open a new default config file:

```shell
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default2
# Delete default
sudo rm /etc/nginx/sites-available/default
# Make a new default config
sudo nano /etc/nginx/sites-available/default
```

The next part can be a bit tricky. Take a look at the config file below. You need to replace `<your-domain-name>` with the following:

1. In lines **19-20**, replace the text by the name of the folder you just wrote down.
2. In lines **39** and **91**, replace the text by your custom domain name, **with and without** ‘www’

I’ve added some helpful screenshots below for clarification. It’s easiest to just copy-paste this into a notepad file and replace the values before pasting it into the nano editor.

{% gist e5b72673858886e0d5772ac40da0ee06 %}

The image below shows what this looks like in the nano editor.

(here’s the top part of the config file)
![](img/blog/shiny_series_pt4/replacmeents.png)

(and here’s the bottom part)
![](img/blog/shiny_series_pt4/replacements2.png)

After pasting the new configuration, save the file by hitting `control+x` and then `Y` and `enter`. Then, restart the nginx service:

```shell
sudo service nginx restart
```

In your browser, navigate to your domain name. If all went well, you should now see the lock symbol indicating that the setup is a success!

![](img/blog/shiny_series_pt4/authenticated.png)

## What does this new config file do?
Glad you asked. This config file makes sure that we use the Let’s Encrypt SSL certificate we created earlier. We also keep the locations we’ve been using so far, but we changed the config files for the private-apps shiny server. This configuration file re-routs all traffic through the Auth0 authentication service we’ll set up in a second. If the user successfully authenticates, they will be forwarded to the private shiny environment.

## Setting up your Auth0 account
Navigate to the [Auth0 website](https://manage.auth0.com/login) and sign up for an account. A free account works with up to 7.000 users, so this should be more than enough.

Navigate to connections and remove all social logins. We won’t be needing them for this application as we’ll be setting up an email whitelist:

![](img/blog/shiny_series_pt4/auth0removesociallogins.png)

Go to ‘clients’ and click on ‘create client’

![](img/blog/shiny_series_pt4/auth0_step1.png)

Name your application and choose ‘regular web app’:

![](img/blog/shiny_series_pt4/auth0_step2.png)

Now go to ‘settings’, note down your domain, client ID and secret ID, and add a callback url in the following format:

```text
https://www.<your-domain-name>.<extension>/private-apps/callback,
https://<your-domain-name>.<extension>/private-apps/callback
```

![](img/blog/shiny_series_pt4/auth0_step3.png)

Scroll down and save your changes. Then, navigate to ‘rules’ and click on ‘create first rule’:

![](img/blog/shiny_series_pt4/auth0_step4.png)

From this list, select ‘whitelist for a specific app’:

![](img/blog/shiny_series_pt4/auth0_step5.png)

Change the name of the app (red box) to your application name, then add the email addresses that are allowed to make an account (gold box):

![](img/blog/shiny_series_pt4/auth0_step6.png)

## Setting up the Auth0 service
Go back to the terminal and clone my fork of the auth0 shiny-auth0 repository:

```shell
cd ~ && git clone https://github.com/JasperHG90/shiny-auth0.git
```

Install nodejs and npm

```shell
sudo apt-get install -y nodejs npm
```

Enter the shiny-auth0 repository and let npm install the dependencies

```shell
cd shiny-auth0 && npm install
```

Go to [this website](http://www.unit-conversion.info/texttools/random-string-generator/) and generate a random string of 40 characters. Copy-paste this string in a temporary document. You will need it a little later.

Go back to the terminal and create a file called ‘.env’ in the shiny-auth0 directory:

```shell
 nano .env
```

Copy the lines below into a temporary document and add the required information

```text
AUTH0_CLIENT_SECRET=yoursecretid
AUTH0_CLIENT_ID=yourclientid
AUTH0_DOMAIN=yourdomain
AUTH0_CALLBACK_URL=https://<your-domain-name>.<extension>/private-apps/callback
COOKIE_SECRET=random string generated in the previous step
SHINY_HOST=localhost
SHINY_PORT=4949
PORT=3000
```

Save the file by hitting `control+x` and then `Y` and `enter`. Make a symlink for nodejs so it can also be called with ‘node’:

```shell
sudo ln -s `which nodejs` /usr/bin/node
```

Run the app using:

```shell
npm start
```

Now, go to: `www.<your-domain-name>.<extension>/private-apps/` to see if it works.

Now that we know that user authentication works, we need to make sure that the Auth0 service can quietly run in the background like a regular service. Hit `control + C` in the terminal to stop the nodejs server. Then, execute

```shell
sudo nano /etc/systemd/system/shiny-auth0.service
```

and copy-paste the following:

```text
[Service]
ExecStart=/usr/bin/node /home/shiny/shiny-auth0/bin/www
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=shiny
User=shiny
Group=shiny
WorkingDirectory=/home/shiny/shiny-auth0/
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
```

Hit `Control + X`, `Y` and then `enter`. Then, execute the next two lines:

```shell
sudo systemctl enable shiny-auth0
sudo systemctl start shiny-auth0
```

This automatically starts up the auth0 server every time the server restarts.

## Wrapping up
Congratulations, user authentication is now set up! This wraps up part 4 of the shiny server series. In the last instalment, we’ll be adding a simple static website created using Jekyll.
