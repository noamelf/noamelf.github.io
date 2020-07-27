---
date: "2016-03-18T21:19:00Z"
tags:
- api
- docker
- talks
title: API and Microservices Management with Kong
---

Hi all,
At the last [PyWebIL meetup][meetup] I took the stand and gave a talk about one a very interesting open-source project - [Kong][kong].  Kong is an API and microservices management layer that serves as a reverse proxy to your API's while taking care of generic actions such as rate-limiting, authentication, monitoring and much more. One of it's key benefits is that it is very plugable, hence it is easy to add your own custom logic (I actually enhanced a plugin to fit my needs).

![Kong](http://i.imgur.com/iDcys5P.png)

The talk is constructed from a high level introduction to API's and the concept of microservices (here are the [slides][slides]) and a hands on demonstration of a demo microservice environment using Python/Flask, Docker and Kong as the management layer (here is the [code][code]). I had  real good time preparing for the talk! If you're interested in hearing in your meetup/company [reach out][about].   

[meetup]: http://www.meetup.com/PyWeb-IL/events/228958390/
[kong]: https://getkong.org/
[slides]: https://goo.gl/sS5spo
[code]: https://github.com/noamelf/kong-talk
[about]: http://noamelf.com/about