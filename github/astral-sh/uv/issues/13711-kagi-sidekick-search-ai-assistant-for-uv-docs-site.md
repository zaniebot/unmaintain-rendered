---
number: 13711
title: Kagi Sidekick (search + AI assistant) for uv docs site?
type: issue
state: open
author: rsyring
labels: []
assignees: []
created_at: 2025-05-28T22:20:08Z
updated_at: 2025-06-02T10:10:25Z
url: https://github.com/astral-sh/uv/issues/13711
synced_at: 2026-01-07T13:12:18-06:00
---

# Kagi Sidekick (search + AI assistant) for uv docs site?

---

_Issue opened by @rsyring on 2025-05-28 22:20_

Discoverability of information for uv can be challenging given the volume of issues here in GH and how much there is in the documentation.  Given how fast uv has moved, posts from even a few months ago can be outdated which means AI isn't always the most helpful.  I've generally found ai struggles with uv anyway for some reason.

I ran across [Kagi Sidekick](https://help.kagi.com/kagi/sidekick/) today (no affiliation) which is an AI + search tool designed for websites/projects.  I wonder if it would be useful for uv and other astral projects?

Bonus points if it can tie into the GH issues for it's data and results.

---

_Comment by @konstin on 2025-06-02 10:10_

So far, I found that LLMs more often than not make up answers to user questions, even when given the uv docs as reference, so I'm hesitant to include something LLM based in our official docs. Pointing the model to https://docs.astral.sh/uv/ can get it on the right track though. The situation may improve as more resources around uv on the internet, from blog posts over the docs to projects making use of uv, move into the training sets.

While the uv docs search has room for improvement, there's a much lower barrier to working with mkdocs to improve the search if we have specific case where the search didn't work than to replace it with an external service.

Something I'd be very interested in is a model that can index all GitHub issues while handling GitHub's rate limits (minimally) as well as discord discussions, release notes and the docs (optionally) and point users to similar issues/pages. From the linked docs page, it's unclear how much of that kagi can provide without going through their sales process.

---
