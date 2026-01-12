```yaml
number: 1933
title: "Add compatibility for deprecated `python_implementation` marker"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/impl
created_at: 2024-02-23T19:18:57Z
updated_at: 2024-02-23T19:47:59Z
url: https://github.com/astral-sh/uv/pull/1933
synced_at: 2026-01-12T16:04:47Z
```

# Add compatibility for deprecated `python_implementation` marker

---

_@charliermarsh_

## Summary

Like `platform.python_implementation`, we should support the `python_implementation` "alias" for `platform_python_implementation`.

Closes https://github.com/astral-sh/uv/issues/1906.

## Test Plan

```shell
❯ cargo run pip install "pynacl==1.4.0"
    Finished dev [unoptimized + debuginfo] target(s) in 7.02s
     Running `target/debug/uv pip install pynacl==1.4.0`
Resolved 4 packages in 9ms
   Built pynacl==1.4.0                                                                                                                                                                                                                        Downloaded 1 package in 31.51s
Installed 4 packages in 3ms
 + cffi==1.16.0
 + pycparser==2.21
 + pynacl==1.4.0
 + six==1.16.0
```


---

_Label `bug` added by @charliermarsh on 2024-02-23 19:19_

---

_Label `compatibility` added by @charliermarsh on 2024-02-23 19:19_

---

_Merged by @charliermarsh on 2024-02-23 19:47_

---

_Closed by @charliermarsh on 2024-02-23 19:47_

---

_Branch deleted on 2024-02-23 19:47_

---
