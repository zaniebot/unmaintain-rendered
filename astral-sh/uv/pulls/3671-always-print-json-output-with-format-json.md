```yaml
number: 3671
title: Always print JSON output with --format json
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/list
created_at: 2024-05-20T16:28:55Z
updated_at: 2024-05-20T16:39:32Z
url: https://github.com/astral-sh/uv/pull/3671
synced_at: 2026-01-12T16:05:47Z
```

# Always print JSON output with --format json

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/3670.

## Test Plan

```
uv on  charlie/list:main
❯ cargo run pip list --editable
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uv pip list --editable`
uv on  charlie/list:main
❯ cargo run pip list --editable --format json
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.16s
     Running `target/debug/uv pip list --editable --format json`
[]
uv on  charlie/list:main
❯ cargo run pip list --editable --format freeze
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uv pip list --editable --format freeze`
uv on  charlie/list:main
❯ cargo run pip list --editable --format columns
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uv pip list --editable --format columns`
```


---

_Label `bug` added by @charliermarsh on 2024-05-20 16:28_

---

_@zanieb approved on 2024-05-20 16:35_

---

_Merged by @charliermarsh on 2024-05-20 16:39_

---

_Closed by @charliermarsh on 2024-05-20 16:39_

---

_Branch deleted on 2024-05-20 16:39_

---
