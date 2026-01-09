---
number: 10746
title: Ecosystem check race condition
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2024-04-03T01:34:38Z
updated_at: 2024-04-03T01:47:24Z
url: https://github.com/astral-sh/ruff/issues/10746
synced_at: 2026-01-07T13:12:15-06:00
---

# Ecosystem check race condition

---

_Issue opened by @zanieb on 2024-04-03 01:34_

The ecosystem checks perform a comparison between the branch `ruff` binary and a baseline `ruff` binary. We pull a baseline binary from the base branch of the pull request:

https://github.com/astral-sh/ruff/blob/814b26f82e1e6a678dc59ee4cd0fc1c277efb517/.github/workflows/ci.yaml#L248-L254

GitHub runs pull request CI against a "merge" commit of the pull request HEAD and the base branch. This commit is not guaranteed to be the same commit as we pulled a baseline for above. We see this manifest as incorrect ecosystem reports on pull requests — they include changes for "recently" merged pull requests.

I believe something like the following occurs:

- A commit is made to main and a baseline binary (1) is uploaded
- A commit is made to main and a baseline binary (2) build starts
- Open a pull request, a branch binary build starts — this includes the second commit made to main
- The pull request ecosystem checks start and pull latest baseline binary (1)
- Changes from (2) are incorrectly reported as coming from the pull request



---

_Referenced in [astral-sh/ruff#8745](../../astral-sh/ruff/issues/8745.md) on 2024-04-03 01:47_

---

_Comment by @zanieb on 2024-04-03 01:47_

Closing in favor of https://github.com/astral-sh/ruff/issues/8745

---

_Closed by @zanieb on 2024-04-03 01:47_

---
