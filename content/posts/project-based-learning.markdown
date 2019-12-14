---
date: "2015-09-23T17:29:00Z"
tags:
- python
- teaching
title: Project-based learning
---

#### Context
The [Python](https://en.wikipedia.org/wiki/Python_%28programming_language%29) course I'm instructing at Avratech (see [earlier post](http://thespoon.ghost.io/ravtech-and-cnn/) and [CNN story](http://edition.cnn.com/2015/07/08/world/israel-ultra-orthodox-jews-high-tech/index.html#)) is advancing and the students are already a month and a half into their group projects. This post is about my personal experience with shifting my class from *teacher-led* learning to *project-based* (learning), but first, have a look at the 5 beautiful projects they're working on (you might want to use [Google to translate](https://translate.google.com/translate?hl=en&sl=iw&tl=en&u=jewishtimes.avratech.co.il) the websites if your Hebrew reading skills aren't in shape &#9786;):

- [Jewish Times](jewishtimes.avratech.co.il) - Jewish prayer times notification system. You register, choose the prayers you wish to be alerted on, and  receive notifications by email.
- [Kindergarten](kindergarten.avratech.co.il) - A kindergarten management cloud application. Enables to keep track of payments, contact parents, etc.  
- [The Swamp](theswamp.avratech.co.il) - A social media hub for the Ultra-Orthodox community. It uses Twitter and Facebook to collect updates from the community's leaders.
- [Kosher Apps](kosherapps.avratech.co.il) - A "kosher" app store, meaning it collects applications that are suitable for the ultra-orthodox crowd from Google Play.      
- [Easy Hotel](easyhotel.avratech.co.il) - An hotel management cloud application. From room reservations financial reports, this application answers every hotel needs (okay, maybe not all, but most). 

![Jewish times](http://i.imgur.com/JkyDgPt.png)
[Jewish Times screenshot]

#### Choosing a path
After 2 months of teacher-led classes, we reached the end of the basic course. Having a few more month of training time, we wanted to give the students some "real life" experience though we weren't sure how to go about it. First, we thought about inventing a project ourselves. We started writing our makeup project until at some point it felt like it will be too boring for the students and too time consuming for us. 

We decided to give the students freedom to come up with a project that they will be interested in. Although there were some worries that without direct, homework like, instructions the students might not take these projects seriously and slack off but we decided to give it a go.      


![Kindergarten](http://i.imgur.com/JXuCVat.png)
[kindergarten screenshot]

#### Grouping the students
The class (21 students) was split into 5 teams while trying to keep them as equal as possible skill wise. Each team was supposed to pick a team leader. I explained the class what responsibilities a team leader has, and gave them a few minutes to choose one for the team. For my surprise, none of the groups picked a leader. I stepped in and appointed the team member who seemed to me like the best fit. It was a little disappointing for me because I wanted the team leader to be given his "mandate" from his fellows and not from me. 

The next phase was for each team to choose a project they're interested in. I showed them some examples of real applications which I think are good and they started coming up with their own ideas. It didn't go easy since as the saying goes: *as many Jews as many opinions*, but by the end of the day all the teams had an idea or was closing in on one.

![TheSwamp](http://i.imgur.com/yDzqkgq.png)
[The swamp screenshot]
#### Agility
We wanted the teams to adopt an [agile](https://en.wikipedia.org/wiki/Agile_software_development) development work flow. We set an iteration (development cycle) planning meeting in the beginning of the week (each iteration constitutes one week). There was an emphasis on getting something to work by the end of each iteration. The teams spoke about what features they want to add to the project and together we analyzed the effort it will take them to get there and the amount of working days they have in the upcoming iteration. After the meeting was over, the team leaders updated their teams [Trello](https://trello.com/) boards with what is to be accomplished in the following iteration and by whom. 

The teams adopted the development flow rather easily but we're finding it difficult to start working since they weren't familiar with the way a web application works. Having team leaders was tremendously helpful at that point. I set a meeting with them discussing the way a web application is built and they transfered that knowledge and help direct their team members in their tasks.
     
After an iteration ended, each team had a retrospective meeting where we compared what's been done with what we hoped to accomplish. There was some interesting insights about how each team functioned. For example, a team that failed miserably in his iteration goals recognized that they had a communication problem with their team leader that caused him to "just do the job himself". Another nice example is that all the teams complained they were having a hard time utilizing [Git](https://en.wikipedia.org/wiki/Git_%28software%29), which made me realize I need to run a Git workshop. 

![Easy Hotel](http://i.imgur.com/qu1Q4l0.png)
[Easy Hotel screenshot]

##### Technological stack (for the techy reader)
We tried to keep the projects technological stack as simple as possible, so the transition between simple, homework Python programs, and a fullstack web application will be easier. 

**Flask** was an easy pick as a web framework since getting familiar with it is as easy as it gets, but it still provides some nice, advanced functionality (with it's extensions). **SQLAlchemy** and **SQLite3** are also a perfect couple for the job as they are very easy to set up, and doesn't require much SQL knowledge (at least at the beginning). Since they didn't get a real frontend - HTML/CSS - training, **Bootstrap** provided a quick and easy solution (although, when they wanted to customize the look and feel of their application, they had to dig into HTML/CSS anyway). The full list:

- Python3
- Flask
- SQLAlchemy
- SQLite3
- Nginx + Uwsgi emperor (I configured this part)
- Bootstrap
- HTML/CSS
- Git

Using a single web stack for all the groups was a really good decision since they could help each other and I was able to gather everyone together and explain a certain subject once. 

As funny (or obvious) as it may sound, getting them to use **Git** was by far the most challenging part of the project. I enjoy using Git very much, but I also remember how I struggled with it at the beginning. I'm considering using a different VCS system for my next classes but I'm not sure it will serve the students since Git is so ubiquitous these days.

![Kosher apps](http://i.imgur.com/0S7FoQf.png)
[Kosher apps screenshot]

#### Project-based learning is far more meaningful!
Having the students pick out their own projects and lead them was a tremendous success. It completely changed the value they are receiving from the course. The teacher-led part taught them Python alright but didn't really got them excited about software development (beside a few) and the endless possibilities it entails. Most just wanted to get good grades. Now, I can practically see the excitement in most, if not all, of their eyes and I believe that that is the best grantee for a successful programming career.  

To finish off, I want to thank Eli Gur, the school technical lead/adviser, for giving me priceless advices and helping the students with their projects, and to my colleague [Udi Oron](https://twitter.com/nonZero) who influenced me towards project-based learning and shared his experience with me. 