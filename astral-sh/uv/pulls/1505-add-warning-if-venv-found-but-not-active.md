```yaml
number: 1505
title: Add warning if venv found but not active
type: pull_request
state: closed
author: sanders41
labels: []
assignees: []
base: main
head: venv-warning
created_at: 2024-02-16T15:50:46Z
updated_at: 2024-02-28T10:20:36Z
url: https://github.com/astral-sh/uv/pull/1505
synced_at: 2026-01-10T14:54:43Z
```

# Add warning if venv found but not active

---

_Pull request opened by @sanders41 on 2024-02-16 15:50_

Closes #1408

I couldn't come up with a good way to add a test for this since all the test macros seem to have a virtual environment active. I'm happy to add one if pointed in the right direction.

---

_Review requested from @zanieb by @zanieb on 2024-02-16 17:09_

---

_Assigned to @zanieb by @zanieb on 2024-02-16 17:09_

---

_Unassigned @zanieb by @konstin on 2024-02-28 08:22_

---

_Comment by @konstin on 2024-02-28 08:51_

Hi, thanks for the PR and sorry the late reply! There are different workflows around virtual environments, some don't require activating it at all (i have e.g. written CI and docker workflow where i used `.venv/bin/<bin>` to avoid problems with environment variables, and i expect more tool to pick up `.venv` automatically in the future) and we also plan on extending uv to make venvs more managed, e.g. a `uv run` command. While there's definitely a gap where users don't realize that they need to activate the venv, i don't think we should unconditionally show a warning. 

---

_Closed by @konstin on 2024-02-28 08:51_

---

_Branch deleted on 2024-02-28 10:20_

---
