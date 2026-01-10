```yaml
number: 8038
title: Remove the newly created tool environment if sync failed
type: pull_request
state: merged
author: j178
labels:
  - bug
assignees: []
merged: true
base: main
head: tool-sync-fail
created_at: 2024-10-09T07:58:04Z
updated_at: 2024-10-09T15:31:20Z
url: https://github.com/astral-sh/uv/pull/8038
synced_at: 2026-01-10T12:54:01Z
```

# Remove the newly created tool environment if sync failed

---

_Pull request opened by @j178 on 2024-10-09 07:58_

## Summary

Resolves #8011

## Test Plan

```console
$ cargo run -- tool install pyenv
$ cargo run -- tool list
```


---

_Converted to draft by @j178 on 2024-10-09 08:25_

---

_Marked ready for review by @j178 on 2024-10-09 09:22_

---

_@charliermarsh approved on 2024-10-09 12:54_

---

_Label `bug` added by @charliermarsh on 2024-10-09 12:54_

---

_Merged by @charliermarsh on 2024-10-09 13:01_

---

_Closed by @charliermarsh on 2024-10-09 13:01_

---

_Branch deleted on 2024-10-09 15:31_

---
