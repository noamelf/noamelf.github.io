---
date: "2015-08-29T16:38:46Z"
title: Optimization Tale
---

I was asked to optimize our web API service. This was the first time I ever experienced anything of that sort so I was pretty excited about it. I read a lot of blog posts and Stack Overflow questions about optimization but still wasted some time on optimizing the wrong parts. Following is the lessons I learned, and some pitfalls that you can avoid on your next optimization task.

#### Locust vs Jmeter
The first you want to do when optimizing anything is to be able to measure it's performance. I looked for a Python based (my favorite language) load testing solution and found [Locust](http://locust.io/). Locust is a quick and easy package to run a threaded load testing programs with nothing but Python code. Everything was great until I sadly realized that Locust aggregates the results, meaning: I couldn't get a *raw* CSV to store and analyze later on using graph tools of sorts.

I turned to the industry standard: [Jmeter](http://jmeter.apache.org/). It's website felt 90'ish and *Java* isn't really my cup of tea, but nonetheless I found it to be a great piece of software.  After investing some time in reading the docs and understanding Jmeter inner logic and ugly GUI, you can make stuff happen really fast and in a very modular "Lego bricks" way.

Jmeter has built in result *listeners*, which means you can view real time results with multiple graph/table listener of your choice. Further more, it is based on a GUI to configure the test itself, but after that, there is no problem running it in a non-GUI mode that writes the results to a CSV file and analyze it later in Jmeter or great services like [LoadSophia](https://loadosophia.org/). **Tip**: if your looking to create a stress test for your application, go for the no-GUI mode and analyze the results later since generating real time graphs is heavy on resources. 

#### Load testing
Before we continue, some information about the setup: we are using AWS Elastic Load Balancing (ELB) in front of 2 m3.medium identical instances. Each instance has Ngnix in front of a Uwsgi server running a flask application.   

Back to our story, I created the Jmeter load test configuration on my local machine to simulate practical usage scenarios and [sshfs](https://wiki.archlinux.org/index.php/Sshfs) it to a designated server, wrote a bash script to automatically run test of different loads and run the tests. Getting analytics from LoadSophia, I sent a proud report to my coworkers with graphs and tables explaining the current API performance.

#### Optimizations ?!?
My next step, optimization, would be easy (or so I thought). I already read about some possible configurations to Nginx and Uwsgi like David Cramer's blog post ( [You Should Be Using Nginx + UWSGI](http://cramer.io/2013/06/27/serving-python-web-applications/)) or the Uwsgi docs about [integration with Nginx](http://uwsgi-docs.readthedocs.org/en/latest/Nginx.html). Happily following their advices, I started tweaking the configuration and testing the results:

- Changed the communication between Nginx to Uwsgi to the Uwsgi protocol which has lower overhead than HTTP.
- Changed socket type from TCP to Unix. Since both process are running from the same machine and a Unix socket is lighter, hence faster.
- Experimented with different numbers of processes/threads.

At that point I faced a frustrating situation, all of the changes I made had nearly no impact on the setup performance - the servers Transaction Per Second (TPS) and average response time remained more or less the same as before. Imagine my disappointment. 

#### Eureka
I started thinking there might be something wrong with the DB performance. Puzzled by the case, I turned to the [scientific method](http://goo.gl/K1gH7Z) or put otherwise: *stop and think!* I thought about the facts I was seeing, and remembered that through all my optimization tweaks, the servers CPU level was pretty high. I wrote 2 hypothesis about that, both of which seemed unlikely. In short (you can find the long version [here](https://goo.gl/irxrcs)): 

1. Uwsgi is consuming tons of resources while trying to load balance it's processes and threads.
2. Our Flask application is CPU intensive.

Testing my assumptions I saw clearly that the Uwsgi processes running our Flask app takes almost all of the CPU capacity. This was thrilling to find as well as disappointing since I spent much of my time barking up the wrong tree. **The application bottleneck was the processes high CPU consumption, thus, improving the rate Uwsgi and Nginx passes requests wouldn't improve performance.**

With  that revelation, we turned into the simplest, most immediate solution: increasing CPU power. In our case this meant upgrading the machines to an m3.large instances (2 CPU's instead of 1). This change immediately doubled our TPS and enabled us to serve twice more concurrent users. I really wanted to profile the application and see which part is gobbling the CPU, alas I run out of time for the task. 

#### Lesson learned
When testing applications performance, and trying to figure out how to optimize it, always start with the most basic suspects: it's memory and CPU consumption. Write a simple load test, and understand what `top` (I used `htop` with process filtering, very convenient) is showing you. If the servers memory or CPU are at full capacity, you can be certain that thats the bottleneck and invest your well worth time there. 