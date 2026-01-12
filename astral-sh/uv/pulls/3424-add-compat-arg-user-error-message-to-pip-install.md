```yaml
number: 3424
title: add compat arg --user error message to pip install
type: pull_request
state: merged
author: blueraft
labels:
  - error messages
assignees: []
merged: true
base: main
head: compatarg-pip-install
created_at: 2024-05-07T14:50:15Z
updated_at: 2024-05-07T15:28:48Z
url: https://github.com/astral-sh/uv/pull/3424
synced_at: 2026-01-12T16:05:37Z
```

# add compat arg --user error message to pip install

---

_@blueraft_

Resolves [#3419](https://github.com/astral-sh/uv/issues/3419)

## Summary

Add compatargs to pip install command and hint the user to create a venv for --user arg.

## Test Plan

Tested it locally.

```bash
cargo run pip install --user flask
   Compiling uv v0.1.39 (/home/ahmedilyas/uv/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 8.96s
     Running `target/debug/uv pip install --user flask`
error: pip install's `--user` is unsupported (use a virtual environment instead).
```


---

_@charliermarsh approved on 2024-05-07 14:51_

---

_@zanieb approved on 2024-05-07 14:56_

---

_Comment by @zanieb on 2024-05-07 14:56_

So fast! Thank you :)

---

_Label `error messages` added by @zanieb on 2024-05-07 14:56_

---

_Merged by @zanieb on 2024-05-07 14:59_

---

_Closed by @zanieb on 2024-05-07 14:59_

---

_Branch deleted on 2024-05-07 15:28_

---
