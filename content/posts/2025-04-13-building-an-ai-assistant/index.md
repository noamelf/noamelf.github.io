---
date: "2025-04-13"
title: "Building an LLM Wiki for Work"
tags:
  - management
  - hacks
  - ai
---

A couple of weeks ago, Andrej Karpathy [tweeted](https://x.com/karpathy/status/2039805659525644595) about using LLMs to build personal knowledge bases -- what he calls "LLM Wikis." He followed it up with a [GitHub gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) laying out the full pattern: raw sources go into a directory, an LLM reads them, and incrementally builds a structured wiki of interlinked Markdown pages. The LLM does the tedious bookkeeping -- cross-references, summaries, contradiction detection -- while you do the thinking.

The tweet resonated with me immediately. I'd been building a personal AI assistant for my day job, and the stateless-session problem Karpathy describes was exactly the bottleneck I was hitting. So I took his pattern and ran with it.

## The problem: every session starts from zero

I manage several engineering teams at [Bluevine](https://www.bluevine.com/). My day-to-day involves a lot of context that doesn't exist in any single document: reporting lines, deployment processes, which Slack channels are used for what, tribal knowledge around internal tools, and small decisions made in 1:1s that never get written down.

I've been building a personal AI assistant (using [OpenCode](https://opencode.ai), an open-source AI agent that runs in the terminal) that automates recurring workflows -- Slack, email, Jira, GitHub, calendar, HR timesheets. The problem is that AI sessions are stateless. Every new session, the assistant knows nothing about my org. I kept re-explaining who my team leads are, how to file tickets, which custom fields to use. It felt like onboarding a new employee every morning.

This is the exact problem Karpathy identifies:

> Most people's experience with LLMs and documents looks like RAG: you upload a collection of files, the LLM retrieves relevant chunks at query time, and generates an answer. This works, but the LLM is rediscovering knowledge from scratch on every question. There's no accumulation.

## The wiki as persistent memory

After reading the gist, I built a persistent wiki following his pattern: ~70 interlinked Markdown pages across categories like people, teams, projects, decisions, processes, and technical reference. Every page has YAML frontmatter with confidence levels, sources, and staleness dates. Pages cross-reference each other with `[[wiki-links]]`.

When I ask "who owns service X?" or "what's the process for Y?", the assistant reads the index, follows links, and gives me an answer with citations -- no re-discovery needed.

## Applying it to organizational knowledge

Karpathy's use case is research -- papers and articles, building an evolving thesis. Mine is organizational knowledge, which shifts the pattern in a few ways.

My sources are ephemeral. A Slack conversation about a decision, a meeting where we agreed on something, a Jira ticket that revealed how a process actually works. The raw source might not be worth keeping, but the extracted knowledge is. And the wiki gets written during work, not as a separate activity -- when I hit an undocumented edge case during a workflow, the assistant updates the wiki right there.

This is the business/team use case that Karpathy mentions but doesn't go deep on. He writes about "an internal wiki maintained by LLMs, fed by Slack threads, meeting transcripts, project documents." That's exactly what I have, except it's personal -- the organizational knowledge that makes me effective, now persistent and queryable.

This is also what made Jack Dorsey and Roelof Botha's [From Hierarchy to Intelligence](https://block.xyz/inside/from-hierarchy-to-intelligence) click for me. Their core idea is a "company world model" -- the context that managers typically convey about operations and strategy, made consumable by LLMs. When I first read it I thought it was interesting but abstract. Now I really get it. A huge chunk of what I do as a manager is communication work: making sure the right context reaches the right people, translating between teams, repeating decisions so they stick. An LLM wiki that updates in real time -- as decisions happen, as things change -- can shift a lot of that burden. My wiki is a personal-scale version of what they're describing, and even at this small scale, the reduction in repetitive communication work is noticeable.

## What makes it work

Karpathy nails the core insight:

> The tedious part of maintaining a knowledge base is not the reading or the thinking — it's the bookkeeping. Updating cross-references, keeping summaries current, noting when new data contradicts old claims. Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass.

This matches my experience. I've tried personal knowledge systems before -- Notion, Obsidian, plain text files. They all decay because the filing overhead is too high. With an LLM-maintained wiki, I say "ingest this," and the assistant figures out which pages to create, update, and cross-reference.

It's only been about a week and it's already genuinely useful. I prep for 1:1s by asking about someone's recent context. I file tickets in projects I don't use daily because the wiki knows the field mappings. I pull up decisions with context instead of digging through Slack history.

The thing that surprised me is how the wiki compounds in ways I didn't plan for. I ingested notes from a planning meeting. Separately, I ingested a team's structure. Later, when I asked about capacity for an initiative, the assistant connected the two on its own. Grove writes in [High Output Management](/posts/high-output-management/) about variable inspection -- going deep on a limited number of tasks because insights carry forward. The wiki gives that idea more leverage: every deep dive gets encoded, and the next time the assistant needs that context, it's already there.

## Getting started

If you want to try this, the barrier is low. You need an LLM agent that can read and write files ([OpenCode](https://opencode.ai), Claude Code, Codex, Cursor -- any of them work), and a folder for your wiki. Copy [Karpathy's gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) and paste it to your agent as a starting point.
