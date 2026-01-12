```yaml
number: 8485
title: "`uv python install`: remove the existing version only after the new installation is downloaded successfully"
type: pull_request
state: merged
author: j178
labels:
  - bug
assignees: []
merged: true
base: main
head: delay-remove
created_at: 2024-10-23T03:50:30Z
updated_at: 2024-10-24T14:30:06Z
url: https://github.com/astral-sh/uv/pull/8485
synced_at: 2026-01-12T16:08:20Z
```

# `uv python install`: remove the existing version only after the new installation is downloaded successfully

---

_@j178_

## Summary

This PR delays the removal of an existing version after downloading the new version when running `uv python install --reinstall`.

If the download fails, we can keep the existing version working.

## Test Plan

```console
$ cargo run -- python install 3.13
$ cargo run -- python install --reinstall 3.13 # when downloading, `ctrl-c` to interrupt
$ cargo run -- python list
```



---

_@charliermarsh approved on 2024-10-23 18:43_

---

_Label `bug` added by @charliermarsh on 2024-10-23 18:43_

---

_Merged by @charliermarsh on 2024-10-23 18:43_

---

_Closed by @charliermarsh on 2024-10-23 18:43_

---

_Branch deleted on 2024-10-24 14:30_

---
