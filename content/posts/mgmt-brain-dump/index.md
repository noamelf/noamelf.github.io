---
date: "2020-10-07T00:00:00Z"
title: Team Management Best Practices Brain-Dump
tags:
- management
---

Due to a change in Bluevine's R&D structure, I'm receiving another team to manage directly. So I'll be acting as a direct manager for both the Data Engineering team I've been managing this far and the new Core Backend team. This practically doubles the load I have right now and although I'm a bit concerned over my work/life balance and meeting the quality standard I inspire to in each of the teams, this is also an opportunity to have greater influence and learn new stuff.

{{< figure src="brain-dump.jpg" class="mid">}}

I figured that to do good work - while staying sane - I need to streamline my management practices. Make it so that ad-hoc communication and tasks are minimized and long-term predefined work is the main focus. I gathered a few best practices from the last 2 years of management experience, that in theory should help. I'm writing them here so that I try to accustom the new team to work by these principles from the get-go:

- Single "in progress" task - make sure each team member is working on a single task at each given point in time to avoid confusion and context switch overhead.

- Clean backlog - have 1-3 ready tasks in the backlog to consume if stuck, allows not to have to ask the manager for priority. Have a backlog grooming meeting every 2 weeks to keep the tasks in check.

- Clear bottlenecks - part of the reason for constant context switching is tasks being stuck due to reasons the developer doesn't control. Avoid this by planning in advance and sharing with the other teams. Be rigorous about this.

- On-call rotation - make sure to have the on-call person deal with all interrupts the team receives from external resources. This makes it more organized and allows for a less casual method for interrupting the team. All communications should be in the on-call channel in threads. This also allows me to review the interaction, and nudge it in the right direction.

- Async updates - make sure all team members update progress on cards. This allows me to know the status of tasks without interrupting work and creates a history of the task. Also, it creates a good snapshot of the status of all the work that is being done, so I can easily reply to status checks from superiors/neighboring teams.

- Domain responsibilities - define team domains and have a certain owner for each domain. This allows developers to develop a sense of intimacy and ownership of a domain, so they want to groom and improve it.

- Repeating team-building meetings -
  - Daily - updates, priorities, and notifications.
  - Weekly - tickets reviews, long term goals, backlog cleanup.
  - 1on1 - 1 hour time for each team member to update on personal and work-related issues. Also good for building personal relationships.

- Project management -
  - Define stakeholders
  - Prepare a road-map with milestones and other team dependencies
  - Set a weekly sync meeting with stakeholders to review and update the roadmap
  - Gather clear requirements
  - Prepare design doc
  - Review design and roadmap with team, stakeholders, and management
  
- Releases -
  - Appoint a release manager
  - Tickets to be released are tagged "release"
  - Each release has a doc containing planned changes
  - In the weekly meeting the release tickets are reviewed
