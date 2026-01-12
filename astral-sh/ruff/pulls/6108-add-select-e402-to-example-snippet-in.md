```yaml
number: 6108
title: "Add \"--select E402\" to example snippet in `CONTRIBUTING.md`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - documentation
assignees: []
merged: true
base: main
head: update-contributing
created_at: 2023-07-26T22:17:06Z
updated_at: 2023-07-27T21:04:57Z
url: https://github.com/astral-sh/ruff/pull/6108
synced_at: 2026-01-12T03:23:56Z
```

# Add "--select E402" to example snippet in `CONTRIBUTING.md`

---

_Pull request opened by @LaBatata101 on 2023-07-26 22:17_

## Summary
In Ruff only a subset of rules are enabled by default. This change change aims to clarify that when adding a new rule, you must explicitly use the `--select name_of_rule` command to ensure the rule gets executed.

This was talked about on Discord a while back.

## Test Plan
Checked docs via mkdocs

---

_Label `documentation` added by @charliermarsh on 2023-07-26 22:40_

---

_Comment by @charliermarsh on 2023-07-26 22:41_

Great, thanks!

---

_Merged by @charliermarsh on 2023-07-26 22:48_

---

_Closed by @charliermarsh on 2023-07-26 22:48_

---

_Branch deleted on 2023-07-27 21:04_

---
