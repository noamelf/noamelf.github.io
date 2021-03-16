---
date: "2021-03-15T00:00:00Z"
title: Kanban is simply great!
tags:
- management
---

Transitioning from team to group management, I wanted to pass some of the techniques I gathered to the team leaders I'm managing, and one of them is to manage ongoing work with a [Kanban board](https://en.wikipedia.org/wiki/Kanban_board). It serves 2 related objectives:

- It makes work items status highly visible
- It emphasizes important steps in work items delivery

This simple objectives may not sound like a big deal but it has great implications on the team's ability to deliver work predictably and reduce communication and process burden.

Kanban can have different columns for different teams. In the team I lead, we had a few iterations until we reached this structure:

| Backlog | Blocked | In progress | On staging | Tested on staging | Deployed | Done |
|---------|---------|-------------|------------|-------------------|----------|------|

This setup represents what is important for our team, particularly these columns:

- Blocked - a key to streamlining development work is to, well, streamline work. Blocked tickets are tasks that cannot progress due to an external dependency. For example, waiting for the infrastructure team to make changes so development can continue, or a missing specification from the ticket requester. Having blocked items highly visible allows the team - and mostly the team leader - to reconcile it quickly and "unstuck" the task. If the task can't be "unstack" at the time being, it can go back to the backlog, making it clear that it won't be delivered at this time.

- On staging / tested on staging - there is a tendency to feel like the work is done once the code is merged. I think it's because a lot of the time testing it works - i.e. provides value to the users/system, can be a difficult and tedious job. Most devs prefer to leave that job for the QA team. I don't like that approach since there certainly some issues that the developer can find in a quick check and save the QA->dev->QA cycle, and also, it can leave the staging environment barely functional if something serious broke. Having the developer aware of that, and consciously move the ticket to the "tested" column, avoid these issues. The same goes for deployed / done columns, the developer needs to do his best to see that everything works well, before signing off the ticket as "done".

Having a tight Kanban board team methodology makes life much easier for the developers and team lead as it makes it clear to everyone what work is being delivered and how it is being delivered, releasing the developers and the team leader to focus on the work itself, rather than on the process. A bit like having an automated code formatter ([gofmt](https://blog.golang.org/gofmt), [Black](https://black.readthedocs.io/en/stable/)) that removes the need to discuss code formatting for each change, having a clear Kanban board reduces the need to check and discuss the status for each task.
