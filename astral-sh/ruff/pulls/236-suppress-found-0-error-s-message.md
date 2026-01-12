```yaml
number: 236
title: Suppress “Found 0 error(s)” message
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: 0-errors
created_at: 2022-09-20T23:09:11Z
updated_at: 2022-09-20T23:34:57Z
url: https://github.com/astral-sh/ruff/pull/236
synced_at: 2026-01-12T15:55:04Z
```

# Suppress “Found 0 error(s)” message

---

_@andersk_

When ruff is run from a script alongside many other linters, the noisy default messages from each one saying that no errors were found can easily drown out a real error. Only print the error count if it’s nonzero or `--verbose` was specified.

(I’m open to making this opt-out rather than opt-in if you prefer, although I’m not sure what I’d call the option. `--quiet` already means something else, suppressing all output including errors.)

---

_Comment by @charliermarsh on 2022-09-20 23:32_

This seems reasonable to me.

---

_Merged by @charliermarsh on 2022-09-20 23:32_

---

_Closed by @charliermarsh on 2022-09-20 23:32_

---

_Branch deleted on 2022-09-20 23:34_

---
