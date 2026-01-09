---
number: 9814
title: Highlight feature to sync a single package
type: issue
state: open
author: kevinsimper
labels:
  - documentation
assignees: []
created_at: 2024-12-11T15:01:38Z
updated_at: 2024-12-11T15:37:51Z
url: https://github.com/astral-sh/uv/issues/9814
synced_at: 2026-01-07T13:12:18-06:00
---

# Highlight feature to sync a single package

---

_Issue opened by @kevinsimper on 2024-12-11 15:01_

If you are working with UV codespaces, you can end up with a really big list of dependencies.

We have a repository with 6 GB of just dependencies.

Turborepo has a method called Prune which can remove other apps so you can build a minimal application

https://turbo.build/repo/docs/reference/prune

I looked and I think I found that UV has a similar feature and seems to work great.

```
$ uv sync --package=todos
$ du -sh .venv
352M	.venv
$ uv sync --package=projects
$ du -sh .venv
 34M	.venv
```
https://github.com/kevinsimper/fastapi-uv-workspaces-demo

So I am looking for feedback about what other things, should this feature to be highlighted more and how? 😄

---

_Label `documentation` added by @zanieb on 2024-12-11 15:37_

---
