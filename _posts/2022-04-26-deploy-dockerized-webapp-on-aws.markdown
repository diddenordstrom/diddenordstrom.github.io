---
layout: post
title:  "Deploy Dockerized web-app on AWS Lightsail"
date:   2022-04-26 10:00:36 +0200
categories: tutorial
---

## introduction

This guide will cover the basics of deploying a web app to an Ubuntu instance on AWS lightsail.

## prerequisites

This guide will assume you have a complete application that runs via docker while a using a docker-compose.yaml file.
The app I deployed contained a basic node.js application that worked on port 8080, along with a database the application communicated with to store and retrive data.
Note, the complete application started up using `docker-compose build && docker-compose up`.


## AWS Lightsail

To do deploy using AWS you first need an account.
This does require your credit card information, but it will not cost you anything if you choose the free tiers we are going to in this guide.

Once your account is created, navigate to Amazon Lightsail. 

- Create a new __instance__

- Select __Linux/Unix__ as you platform

- Under Select blueprint, choose __OS Only__ and take the latest Ubuntu LTS version (currently 20.04 LTS)

- We could under Launch script add all of our dependencies, but for me it did not work properly so we are going to skip this step.

- Under Choose your instance plan, select any of the _3 months free!_ tiers. I just choose the first model.

- Next, choose an instance name. I just went with Ubuntu, but name it something descriptive.

- Then, click create instance.


After the instance is created you can click the Amazon Ligtsail logo to go back to your home page.
If all went well you should see your instance listed under __instances__.

Now we need to connect to our instance.
Click the three dot meny on your instance and then __Connect__.
This will connect us to the instance using a ssh-client in the browser. It is rather limited, so it is better to connect using ssh in your own terminal.
If you are on a Unix system, press the three dot meny again and press __Manage__.
Now under __Connect__, there should be a link for Downloading a default key.
Download it and remember where you stored it.
Connect to your Ubuntu instance from your own terminal with the following command:

```bash
ssh -I path/to/key-file instance_name@instance_ip
# example
ssh -I ~/Downloads/key.pem ubuntu@11.11.11.11
```

Now we are connected to our instance. Run the obligatory

```bash
sudo apt update && sudo apt upgrade -y
``` 
to make sure our system is up to date.
Now, we need the docker client to run our app. 
[Here is an official guide for Ubuntu-based systems](https://docs.docker.com/engine/install/ubuntu/)
Do not forget to also install docker-compose via `sudo apt install docker-compose`

After everything is installed, run another 

```bash
sudo apt update && sudo apt upgrade -y
``` 
just to make sure all packages are up to date.
A reboot here could be good, but should not be needed.


## Setting up our dockerized git project

Now that we have everyhting we need we simply clone our project with `git clone REPO_URL`.
Go into the project-folder with `cd FOLDERNAME` and run

```bash
docker-compose build && docker-compose up
```

Make sure you know which port your project runs on, as we need to port forward that port within Amazon Lighsail.
It could also be good to create a static ip-address for your instance, so the ip-address does not change once we reboot our system.
On your homepage, go to the __Networking__ tab. Under __IPv4 Firewall__ we need to create some rules.

- Firstly, if you want to be able to ping your server, click __Add rule__, choose _Ping_ under application and press _Create_.

- To port forward the port our application is listening to, click __Add rule__ chose _Custom_ under application, _TCP_ under Protocol and the correct port number under _Port or range_.
In my case, 8080.

Now that should be all.
Access your server via `http://SERVER_IP:PORT_NR/`




