```yaml
number: 13323
title: "Update installation.md: Add x-cmd method to install ruff"
type: pull_request
state: closed
author: lunrenyi
labels: []
assignees: []
base: main
head: patch-1
created_at: 2024-09-11T08:17:06Z
updated_at: 2024-09-12T00:38:35Z
url: https://github.com/astral-sh/ruff/pull/13323
synced_at: 2026-01-12T15:55:43Z
```

# Update installation.md: Add x-cmd method to install ruff

---

_@lunrenyi_

## Summary

Add x-cmd method to install ruff

- Hi, we have implemented a lightweight [package manager using shell and awk](https://www.x-cmd.com/pkg/). It helps you download ruff release packages from the internet and extract them into a unified directory for management, without requiring root permissions.

- **I mean, can the installation method provided by x-cmd be added to the ruff installation.md?**[The installation method for the x command](https://www.x-cmd.com/start/) and [ruff demo](https://www.x-cmd.com/pkg/ruff)
  ```sh
  x env use ruff
  ```


## Test Plan

Nothing.

---

_Comment by @MichaReiser on 2024-09-11 19:03_

Hi

Thanks for your contribution. 

I've never heard of X-CMD. It looks interesting. 

We only want to have a limited set of explicitly listed installation methods to keep the section brief. Because of that, we only list the most commonly used and recommended installation methods and we want to avoid that the section is used to promote other tools. IMO, X-CMD doesn't meet that bar yet. We list many more installation methods in the [table below](https://repology.org/project/ruff-python-linter/versions). Have you considered exploring if you can X-CMD to that table (I'm sorry, but I do no know how you would do that). 

I hope you understand the decision and I hope you're successful in adding X-CMD to repology. 

---

_Closed by @MichaReiser on 2024-09-11 19:03_

---

_Comment by @lunrenyi on 2024-09-12 00:38_

Okay, thank you for your reply.

---
