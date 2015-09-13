# Dokku. Build your own PaaS - Sergey Parkhomenko

## Introduction
Don’t like configuring and satisfying system requirements? Your dream is to deploy applications in one click? 
PaaS is the solution! During the year 2014, we all did a great dive into the world of development automatisation and 
optimisation. Riding this curve, there are a lot of cloud PaaS providers. The most famous are Heroku,
Google Apps Engine, etc. Each of them offers great opportunities for deploying, running and scaling your application,
but the first and the biggest issue is that they don’t offer enough flexibility in environment configuration.
Every developer had a situation when he needed something more from provider, who gives you limited bunch of features.
Another issue is cost. PaaS providers, mentioned above, cost money. This is not bad, because they do their job well,
but it is possible to avoid those expenses. And now is time to say one word: “Dokku”.

## Definitions
**Platform as a service (PaaS)** is a category of cloud computing services that provides a platform allowing customers
to develop, run, and manage Web applications without the complexity of building and maintaining the infrastructure
typically associated with developing and launching an app. PaaS can be delivered in two ways: as a public cloud
service from a provider, where the consumer controls software deployment and configuration settings, and the provider
provides the networks, servers, storage and other services to host the consumer's application; or as software
installed in private data centers or public infrastructure as a service and managed by internal IT departments.

**Heroku** is a cloud platform as a service (PaaS) supporting several programming languages.

**Dokku** is a Docker-powered PaaS implementation.

**Buildpack** is a bunch of scripts to set up stack on PaaS. In some areas of PaaS, buildpacks may become displaced by
Docker images and Dockerfiles

## PaaS use case
Let'ts imagine that you have a small blog written in Django which you need to deploy fast an easy. Also this blog is
under development and you are going to roll out updates every day during the upcoming month. Simultaneously, lets
imagine default actions which you do when you need to match such requirements. You purchase a new virtual or
dedicated server, or buy a new EC2 instance, set up a new environment for the project, or adopt your existing
infrastructure to requirements of your new blog, what can be especially painful if you have projects with other
system level requirements which can even use different languages. If you manage with very diverse technologies
stacks, sooner or later, your server becomes a trash can. But this is still not all pain which you experience in
such kind routine. If your changes to the code base require system configuration or packages modifications, you start
spending more time on deployment than on development. This is the right time to say: "Stop! I'm not a system
administrator! I don't want to configure and maintain, I want to write code!". And right after this phrase you become
a target audience for PaaS providers.

## Example
This guideline shows how fast and easy you can set up your own PaaS. Some points may differ depending on your OS. In
this example we will install Dokku on the server and deploy some application.

1. Install Dokku by running this on your server:

  ```
  server $ wget -qO- https://raw.github.com/progrium/dokku/master/bootstrap.sh | sudo bash
  ```
  
2. Add your domain to Dokku VHOST to make your applications accessible from subdomains:

  ```
  server $ cd /home/dokku
  server $ echo yourdomain.com >> VHOST
  ```
  
3. Install Dokku Postgres plugin to create a database for our application:

  ```
  server $ cd /var/lib/dokku/plugins
  server $ git clone https://github.com/Kloadut/dokku-pg-plugin
  server $ dokku plugins-install
  ```
  
4. Explore opportunities by:

  ```
  server $ dokku help
  ```
  
5. Wire database to our application:

  ```
  server $ dokku postgresql:create app_name
  ```
  
6. Deploy your application by adding git remote to the repository and making push:

  ```
  local $ git remote add name_for_remote dokku@your_domain.com:app_name
  local $ git push name_for_remote master
  ```
  
  After that you will see how Dokku picks a buildpack automatically or uses a custom one which you can set at `.env`
  file like:
  
  ```
  export BUILDPACK_URL=https://github.com/OShalakhin/heroku-buildpack-geodjango
  ```
  
  You can also just put a Dockerfile inside the project root and Dokku will pick it up and build an environment from it by default!

## Dokku advantages over competitors
1. You pay only for the server where Dokku is installed
2. Fully customisable
3. Supports build from Dockerfile
4. Has a great plugins infrastructure
5. Easy change number of workers per application

## Dokku disadvantages
1. Doesn't scale on many machines. If you want this, explore Deis

## Conclusion
If you have a number of small applications you need to deploy, Dokku is the right choice. It will provide you high
level of isolation per application and will make you forget pain you experience during applications deployment.

## References

1. [PaaS Wiki](https://en.wikipedia.org/wiki/Platform_as_a_service)
2. [Dokku Documentation](http://progrium.viewdocs.io/dokku/)
3. [Deis](http://deis.io/)