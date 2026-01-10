---
number: 756
title: "Suggestion: Do not print update information if `--format=json`"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
assignees: []
created_at: 2022-11-15T16:43:47Z
updated_at: 2022-11-15T20:29:11Z
url: https://github.com/astral-sh/ruff/issues/756
synced_at: 2026-01-10T01:22:38Z
---

# Suggestion: Do not print update information if `--format=json`

---

_Issue opened by @dhruvmanila on 2022-11-15 16:43_

When passing the flag to output in the JSON format, the update information is still being provided which leads to parsing error when passing the output to any JSON processor.

```console
❯ ruff --format=json .
[]

A new version of ruff is available: v0.0.117 -> v0.0.120
Run to update: pip3 install --upgrade ruff
```

Example using the `jq` command-line tool:

```console
❯ ruff data_structures/linked_list/__init__.py --format=json | jq
[]
parse error: Invalid numeric literal at line 3, column 2
```

This problem also persists when using the JSON output for editor integration.

---

_Label `bug` added by @charliermarsh on 2022-11-15 16:46_

---

_Comment by @charliermarsh on 2022-11-15 16:46_

Yeah this makes sense.

---

_Comment by @charliermarsh on 2022-11-15 16:54_

(You can pass `--quiet` alongside `--format=json` for now to suppress this.)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-15 17:58_

---

_Comment by @charliermarsh on 2022-11-15 19:03_

(But this will get fixed today.)

---

_Referenced in [astral-sh/ruff#760](../../astral-sh/ruff/pulls/760.md) on 2022-11-15 20:29_

---

_Closed by @charliermarsh on 2022-11-15 20:29_

---
