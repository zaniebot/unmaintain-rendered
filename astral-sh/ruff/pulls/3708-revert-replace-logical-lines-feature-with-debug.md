```yaml
number: 3708
title: "Revert \"Replace `logical_lines` feature with `debug_assertions` (#3648)\""
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: revert-replace-logical-lines
created_at: 2023-03-24T03:14:21Z
updated_at: 2023-03-24T03:44:31Z
url: https://github.com/astral-sh/ruff/pull/3708
synced_at: 2026-01-12T04:39:45Z
```

# Revert "Replace `logical_lines` feature with `debug_assertions` (#3648)"

---

_Pull request opened by @JonathanPlasse on 2023-03-24 03:14_

This reverts commit 27903cdb11cfa5988f5c704837e77ec5d7d4422e.

I tried different configurations with `debug_assertions` and `logical_lines` together but could not make it work.
I think the easiest solution is to revert and run with `--all-features` when working on `logical_lines`.

- Close #3686

- This is needed for #3707 too

---

_Merged by @charliermarsh on 2023-03-24 03:42_

---

_Closed by @charliermarsh on 2023-03-24 03:42_

---

_Branch deleted on 2023-03-24 03:44_

---
