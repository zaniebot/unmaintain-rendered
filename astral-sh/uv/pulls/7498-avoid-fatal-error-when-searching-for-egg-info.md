```yaml
number: 7498
title: Avoid fatal error when searching for egg-info with missing directory
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cache
assignees: []
merged: true
base: main
head: charlie/c
created_at: 2024-09-18T13:24:28Z
updated_at: 2024-09-18T13:33:12Z
url: https://github.com/astral-sh/uv/pull/7498
synced_at: 2026-01-10T12:53:48Z
```

# Avoid fatal error when searching for egg-info with missing directory

---

_Pull request opened by @charliermarsh on 2024-09-18 13:24_

## Summary

Closes https://github.com/astral-sh/uv/issues/7485.

## Test Plan

```
$ cargo run cache clean
$ cargo run venv
$ cargo run pip install django-allauth==0.51.0
$ cargo run venv
$ cargo run pip install django-allauth==0.51.0
```


---

_Label `bug` added by @charliermarsh on 2024-09-18 13:24_

---

_Label `cache` added by @charliermarsh on 2024-09-18 13:24_

---

_@zanieb approved on 2024-09-18 13:25_

---

_Merged by @charliermarsh on 2024-09-18 13:33_

---

_Closed by @charliermarsh on 2024-09-18 13:33_

---

_Branch deleted on 2024-09-18 13:33_

---
