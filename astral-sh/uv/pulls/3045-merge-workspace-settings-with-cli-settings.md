```yaml
number: 3045
title: Merge workspace settings with CLI settings
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/workspace-ii
created_at: 2024-04-15T22:41:35Z
updated_at: 2024-04-17T17:03:31Z
url: https://github.com/astral-sh/uv/pull/3045
synced_at: 2026-01-12T16:05:23Z
```

# Merge workspace settings with CLI settings

---

_@charliermarsh_

## Summary

This PR adds the structs and logic necessary to respect settings from the workspace. It's a ton of code, but it's mostly mechanical. And, believe it or not, I pulled out a few refactors in advance to trim down the code and complexity.

The highlights are:

- All CLI arguments are now `Option`, so that we can detect whether they were provided (i.e., we can't let Clap fill in the defaults).
- We now have a `*Settings` struct for each command, which merges the CLI and workspace options (e.g., `PipCompileSettings`).

I've only implemented `PipCompileSettings` for now. If approved, I'll implement the others prior to merging, but it's very mechanical and I both didn't want to do the conversion prior to receiving feedback _and_ realized it would make the PR harder to review.


---

_Review requested from @zanieb by @charliermarsh on 2024-04-15 22:41_

---

_Review requested from @konstin by @charliermarsh on 2024-04-15 22:41_

---

_Label `configuration` added by @charliermarsh on 2024-04-15 22:41_

---

_Comment by @charliermarsh on 2024-04-15 23:14_

Ah shoot. Misunderstood how `Option<bool>` works. Need to reconsider this.

---

_Marked ready for review by @charliermarsh on 2024-04-16 00:23_

---

_@konstin approved on 2024-04-16 08:47_

---

_Comment by @charliermarsh on 2024-04-16 19:05_

Ok taking this from here to merge is gonna be a lot of tedious work, to add settings structs for all commands.

---

_Comment by @charliermarsh on 2024-04-17 01:13_

A mere 1,200 lines...

---

_Merged by @charliermarsh on 2024-04-17 17:03_

---

_Closed by @charliermarsh on 2024-04-17 17:03_

---

_Branch deleted on 2024-04-17 17:03_

---
