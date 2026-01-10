```yaml
number: 1719
title: "Respect `--index-url` provided via requirements.txt"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/inde
created_at: 2024-02-19T20:45:09Z
updated_at: 2024-02-20T00:02:27Z
url: https://github.com/astral-sh/uv/pull/1719
synced_at: 2026-01-10T15:33:24Z
```

# Respect `--index-url` provided via requirements.txt

---

_Pull request opened by @charliermarsh on 2024-02-19 20:45_

## Summary

When we read `--index-url` from a `requirements.txt`, we attempt to respect the `--index-url` provided by the CLI if it exists. Unfortunately, `--index-url` from the CLI has a default value... so we _never_ respect the `--index-url` in the requirements file.

This PR modifies the CLI to use `None`, and moves the default into logic in the `IndexLocations `struct.

Closes https://github.com/astral-sh/uv/issues/1692.


---

_Label `bug` added by @charliermarsh on 2024-02-19 20:45_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-19 20:45_

---

_Converted to draft by @charliermarsh on 2024-02-19 21:24_

---

_Marked ready for review by @charliermarsh on 2024-02-19 21:26_

---

_Converted to draft by @charliermarsh on 2024-02-19 21:45_

---

_Marked ready for review by @charliermarsh on 2024-02-19 23:13_

---

_Merged by @charliermarsh on 2024-02-20 00:02_

---

_Closed by @charliermarsh on 2024-02-20 00:02_

---

_Branch deleted on 2024-02-20 00:02_

---
