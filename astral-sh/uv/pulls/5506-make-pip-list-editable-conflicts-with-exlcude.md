```yaml
number: 5506
title: "Make `pip list --editable` conflicts with `--exlcude-editable`"
type: pull_request
state: merged
author: j178
labels:
  - cli
assignees: []
merged: true
base: main
head: arg
created_at: 2024-07-27T09:09:40Z
updated_at: 2024-07-27T13:01:53Z
url: https://github.com/astral-sh/uv/pull/5506
synced_at: 2026-01-12T16:06:51Z
```

# Make `pip list --editable` conflicts with `--exlcude-editable`

---

_@j178_

## Summary

I think it makes no sense to allow `--editable` and `--exclude-editable` at the same time.

## Test Plan
```console
$ cargo run -- pip list --editable --exclude-editable
error: the argument '--editable' cannot be used with '--exclude-editable'

Usage: uv.exe pip list --editable

For more information, try '--help'.

```


---

_@charliermarsh approved on 2024-07-27 12:26_

---

_Label `cli` added by @charliermarsh on 2024-07-27 12:26_

---

_Merged by @charliermarsh on 2024-07-27 12:26_

---

_Closed by @charliermarsh on 2024-07-27 12:26_

---

_Comment by @charliermarsh on 2024-07-27 12:26_

Thanks!

---

_Branch deleted on 2024-07-27 13:01_

---
