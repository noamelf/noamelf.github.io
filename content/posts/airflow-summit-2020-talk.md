---
date: "2020-07-27"
title: Airflow Summit 2020 talk
tags:
- airflow
- bluevine
- talks
markup:
  goldmark:
    renderer:
      unsafe: true
---

Inline with the Corona pandemic, I gave my first online conference talk! The Airflow Summit 2020 was wonderfully organized and I must say I liked the ability to participate in a conference without leaving home. Giving a talk to a computer is a very different experience then giving it live, a lot less interaction with the audience but on the plus side, it's much less stressful as well.

At Bluevine we use Airflow to drive our ML platform. In the talk, I presented the challenges and gains we had at transitioning from a single server running Python scripts with cron to a full blown Airflow setup. Some of the points that I've covered are:

- Supporting multiple Python versions
- Event driven DAGs
- Airflow Performance issues and how we circumvented them
- Building Airflow plugins to enhance observability
- Monitoring Airflow using Grafana
- Patching Airflow scheduler

Here are the slides:

<iframe src="//www.slideshare.net/slideshow/embed_code/key/bqoKn6z6ci9fcX" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
