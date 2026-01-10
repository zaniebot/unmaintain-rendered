---
number: 1128
title: "Add a test to ensure that `--fix` is Black-compatible"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2022-12-07T19:39:11Z
updated_at: 2022-12-15T20:27:08Z
url: https://github.com/astral-sh/ruff/issues/1128
synced_at: 2026-01-10T01:22:38Z
---

# Add a test to ensure that `--fix` is Black-compatible

---

_Issue opened by @charliermarsh on 2022-12-07 19:39_

We introduced a bug whereby one of the docstring fixes was incompatible with Black, and led to continuous reformatting when running Ruff, then Black, then Ruff, etc.

We should include some kind of integration test to ensure that `ruff`, `black`, `ruff` is a stable sequence.

---

_Label `internal` added by @charliermarsh on 2022-12-07 19:39_

---

_Comment by @charliermarsh on 2022-12-15 20:27_

Closed by @squiddy via #1206.

---

_Closed by @charliermarsh on 2022-12-15 20:27_

---
