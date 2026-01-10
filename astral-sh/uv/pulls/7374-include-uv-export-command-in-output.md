```yaml
number: 7374
title: "Include `uv export` command in output"
type: pull_request
state: merged
author: dcwatson
labels:
  - compatibility
assignees: []
merged: true
base: main
head: 7159-export-command
created_at: 2024-09-13T19:16:44Z
updated_at: 2024-09-13T20:15:11Z
url: https://github.com/astral-sh/uv/pull/7374
synced_at: 2026-01-10T12:53:45Z
```

# Include `uv export` command in output

---

_Pull request opened by @dcwatson on 2024-09-13 19:16_

## Summary

Updates the output of `uv export` to include the command that produced it, similar to how `uv pip compile` does. This addresses #7159 - I had this same itch today, figured it was a good time to dive in!

## Test Plan

All the export unit tests were updated to test the new output format.


---

_Renamed from "Include uv export command in output (#7159)" to "Include uv export command in output" by @dcwatson on 2024-09-13 19:17_

---

_@charliermarsh approved on 2024-09-13 20:13_

Thank you! Looks great.

---

_Label `compatibility` added by @charliermarsh on 2024-09-13 20:13_

---

_Renamed from "Include uv export command in output" to "Include `uv export` command in output" by @charliermarsh on 2024-09-13 20:13_

---

_Merged by @charliermarsh on 2024-09-13 20:15_

---

_Closed by @charliermarsh on 2024-09-13 20:15_

---
