---
layout: post
title: Mastering the information flow
---

One of the daunting things in managing a team is being up to date with what is going on. On the one hand, it's crucial to be up to speed with the team's progress, but on the other hand, I hate standup meetings and/or continuously asking about status. I guess being an individual contributor up until now, I remember not enjoying this type of management practices. It took me a while to figure out a technique that works well for my needs and to master the team's information flow, and I can say it splits into 2 aspects:

1. Smart usage of communication channels
2. Forming team communication habits 

In following writeup I'll try to specify the exact way in which I try to achieve optimal communication.

## Smart usage of communication channels

The key here is utilizing the existing communication channels to the maximum without drowning in information. It can be accomplished by setting up smart notification and filters. If this sound too generic, here is the actual data sources and filtering mechanisms: 

### Emails 
In short, filters and inbox zero. Filters are mandatory, in order to avoid drowning in a sea of irrelevant information. As much as it sound anti-social, I'm not really interested in new recruits of other teams, that are I do not interact with, or birthdays of members of these teams. Also, error emails for unrelated applications aren't my cup of tea either. The simplest way to ignore these is to have accurate filters on recurring and not relevant emails.

Once the filters are set up, I'm using the [inbox zero](https://whatis.techtarget.com/definition/inbox-zero) methodology, so that anything that needs addressing is being addressed at that moment or snoozed to when it needs to be addressed. If it's an ongoing thing, I turn it into a ticket. 

### Tickets

We use Trello where I work. Set up as a Kanban board, it allows to have multiple lists that represent the status of stuff, e.g. backlog, in progress, etc. The tickets are always sorted by priority in their list where a key requirement is to have the minimum viable number of tickets in each list. Our team has a board that holds all the work that we do, and if there are complex sub projects that need their own board, the main board points to the sub-project board. 

Having a single board, allows me to understand at a glance what are we working on at the moment, in what priorities. Since Trello allows to subscribe to any action on the board, I get a notification when someone updates his progress. I find it more convenient to use Trello's in-app notification instead of receiving emails, since it allows to respond more easily. One of the challenges is forming team communication habits, and one is less likely to update his tickets without getting any feedback that someone is paying attention. Hence, a simple "+1" emoji can go a long way.

Each ticket gets assigned the relevant Pull Requests, so that we can easily see the code that is being developed, Trello integrates nicely and shows the PR status (open/closed/merged) and the CI status (fails/success) on the ticket. 

### Pull Requests 

Like with Trello, Github allows to subscribe to all activities on a certain repository, this is very convenient since I can be aware of what is going on in our code base, without being dependent on voluntary updates. If something is not interesting it's possible to unsubscribe specifically from it hence decrease the notifications load. Also, on repos that are not directly developed by us, I subscribe just to PRs of interest to me. Here also, I canceled the email notifications and use Github's in app notifications, since it is more capable. 


### Meetings
Meetings are a very important source for updates and planning, but too many of those kills productivity. Since so much of the manager's day is invested in meetings, to try to make them shorter and more effective. A few ideas I use:

- Start with emails, then move to meetings - try to avoid setting meetings when an email can do the same job. If things are unclear or there is a conflict or the project is just stuck for no reason, it's time to move to a meeting since these issues are easier to resolve that way.
- Ask for meetings agenda - helps to make the meeting more productive and shows if the meeting is really needed. e.g. "Discuss latency" meeting title doesn't say much.


## Forming team communication habits 
Now all these ideas are nice and dandy, but if your team doesn't cooperate with you, it will never work. From my experience, lack of cooperation is mostly a matter of changing habits. It's pretty easy to start working on something without moving the ticket to "in progress".

What I try to do is to convince the individuals in the team, why communication is important, and how if they don't answer a mail or move a ticket or open draft PRs, it creates more communication load later on. I also let them know that I understand that programming is not something that can be interrupted every few minutes and that as far as I'm considered, checking their communication channels twice a day is enough. Something I encounter often is that they don't have anything meaningful to say so they just don't reply. This makes some sense, but what I encourage them to do, is to reply "still in progress" or "will check later today", so that the other person knows that things are in check.

After the explanation part, comes the reminding part, for example - I ask a question on a ticket, once I don't get a response within say 4 hours, I ask that person: did you check Trello recently? Doing this enough time hopefully should encourage people  

Another thing that can work is asking one of the team members to be the "scrum master" and to manage the tickets, this makes it more con