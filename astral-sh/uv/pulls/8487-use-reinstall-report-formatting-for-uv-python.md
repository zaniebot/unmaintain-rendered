```yaml
number: 8487
title: "Use reinstall report formatting for `uv python install --reinstall`"
type: pull_request
state: merged
author: j178
labels:
  - enhancement
assignees: []
merged: true
base: main
head: python-reinstall
created_at: 2024-10-23T05:23:42Z
updated_at: 2024-10-24T14:29:21Z
url: https://github.com/astral-sh/uv/pull/8487
synced_at: 2026-01-12T16:08:20Z
```

# Use reinstall report formatting for `uv python install --reinstall`

---

_@j178_

## Summary

Resolves #8456

## Test Plan

```console
$ cargo run -- python install 3.13
$ cargo run -- python install --reinstall 3.13
Searching for Python versions matching: Python 3.13
Found existing installation for Python 3.13: cpython-3.13.0-macos-aarch64-none
Installed Python 3.13.0 in 7.39s
 ~ cpython-3.13.0-macos-aarch64-none
```


---

_Comment by @charliermarsh on 2024-10-23 18:36_

Awesome, thanks as always!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-23 18:36_

---

_Label `enhancement` added by @charliermarsh on 2024-10-23 18:36_

---

_@charliermarsh approved on 2024-10-23 18:40_

---

_Merged by @charliermarsh on 2024-10-23 18:53_

---

_Closed by @charliermarsh on 2024-10-23 18:53_

---

_Branch deleted on 2024-10-24 14:29_

---
