---
number: 4159
title: Is it possible to version documentation?
type: issue
state: closed
author: JonathanPlasse
labels:
  - documentation
assignees: []
created_at: 2023-04-30T10:54:29Z
updated_at: 2024-01-14T03:21:43Z
url: https://github.com/astral-sh/ruff/issues/4159
synced_at: 2026-01-10T01:22:43Z
---

# Is it possible to version documentation?

---

_Issue opened by @JonathanPlasse on 2023-04-30 10:54_

https://pypi.org/project/mike/
> **mike** is a Python utility to easily deploy multiple versions of your [MkDocs](http://www.mkdocs.org/)-powered docs to a Git branch, suitable for deploying to Github via `gh-pages`. To see an example of this in action, take a look at the documentation for [bfg9000](https://jimporter.github.io/bfg9000).

---

_Label `documentation` added by @charliermarsh on 2023-05-02 01:02_

---

_Renamed from "Use `mike` in order to have versionned documentation" to "Use `mike` in order to have versioned documentation" by @charliermarsh on 2023-05-10 03:00_

---

_Comment by @charliermarsh on 2023-06-28 21:37_

This looks reasonable if anyone is ever interested in it. The main problem is that we don't use a `gh-pages` branch.

---

_Comment by @zanieb on 2023-06-28 22:53_

@charliermarsh we did this over at Prefect and opted for a separate repository to contain all of the built documentation rather than using a branch in the primary repository. On each release of the primary repository, a job is triggered in the documentation repository which pulls the release source, builds the documentation, checks it into the repository, and opens an auto-merged pull request for the version. On merge to main, it's deployed via Netlify to production. Unfortunately the documentation repository is not public at this time so I don't have an example to point to (cc @desertaxle changing it to public was something I was working on if you have the time to finish the process ❤️).

---

_Renamed from "Use `mike` in order to have versioned documentation" to "Is it possible to version documentation?" by @JonathanPlasse on 2023-12-17 19:16_

---

_Referenced in [astral-sh/ruff#9507](../../astral-sh/ruff/issues/9507.md) on 2024-01-14 02:04_

---

_Comment by @zanieb on 2024-01-14 03:21_

Closing in favor of https://github.com/astral-sh/ruff/issues/9507

---

_Closed by @zanieb on 2024-01-14 03:21_

---
