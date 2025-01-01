---
date: "2025-01-01"
title: Managing GitHub Teams at Scale with Terraform
tags:
  - terraform
  - management
---

During a recent reorganization, I was tasked with updating GitHub to reflect the changes. Doing the changes manually was out of the question due to the scope of the updates. Initially, I considered writing a custom script to handle the task, but I realized this was a golden opportunity to transition the tedious job of GitHub management to code. Enter Terraform.

{{< figure src="gh-tf.webp" align="center" height="400px" >}}

## The Problem

Managing GitHub teams and repositories at scale involves considerable manual effort, including:

- Maintaining consistent team structures
- Managing member access permissions
- Tracking repository permissions
- Handling team membership changes

Complicating factors include:

- GitHub requires teams listed as Codeowners to be explicitly added to repositories. Adding a general group like "developers" isn't sufficient.
- GitHub's team structures don't always align with standard organizational hierarchies, as some unique teams exist for specific purposes. This makes syncing with LDAP solutions problematic.
- Reorganizations frequently necessitate updates, creating additional work, like in this case.
- GitHub's default settings allow anyone to create teams, leading to inconsistent structures (e.g., teams without descriptions or parent teams).
- Change requests often lack sufficient detail (e.g., GitHub usernames), leading to back-and-forth communications.

## The Solution

I'm somewhat familiar with Terraform, as it's the tool we use to manage our cloud resources. Given Terraform's strengths in managing state and its GitHub provider offering the necessary functionality, it seemed like a great fit.

### Initial Challenges

The Terraform GitHub provider requires a high level of granularity. For instance, each team-repo relationship needs to be defined as a separate resource. This approach becomes unwieldy when managing many repositories and teams. Additionally, I wanted the Terraform repository to be self-service, allowing team members to submit PRs for changes without requiring them to learn Terraform.

Initially, I attempted to use an LLM (Claude) to generate Terraform configurations, but I quickly realized that without familiarity with Terraform, LLMs could produce weird patterns. Instead, I found an excellent [blog post](https://developer.hashicorp.com/terraform/tutorials/configuration-language/provider-versioning) by the Terraform education team (discovered on the second page of Google search results 🤯) that provided the guidance I needed.

### Implementing the Solution

Following the blog post, I created CSV files to define the desired GitHub configuration. Terraform then loads these CSVs and **dynamically** creates the necessary resources. This approach simplifies the process and allows team members to contribute without deep knowledge of Terraform.

Since this wasn't a greenfield setup, I needed to import existing resources into Terraform's state. I wrote a bash script to loop through the resources and run `terraform import`. The great thing about Terraform is that if you don't import a resource, it ignores it by default, allowing you to gradually transition resources to be managed by Terraform rather than forcing an all-or-nothing approach.

### Handling Codeowners

A unique challenge was managing the Codeowners feature, which requires teams to be explicitly added to repositories. To address this, I wrote a custom Terraform provider in Go, as Python, my go-to language, doesn't have first-class support in Terraform. While Claude helped with the initial code, the tricky part was installing it locally from my GitHub repo. There were different methods used in past versions, and Claude's guidance was off the mark here, so I had to go old school and RTFD.

## Results

With this setup, PRs are now submitted, reviewed, and merged, and Terraform handles updates to GitHub repositories and teams automatically. Good riddance to manual GitHub updates!
