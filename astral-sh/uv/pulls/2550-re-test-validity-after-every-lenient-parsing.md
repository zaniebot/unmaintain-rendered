```yaml
number: 2550
title: Re-test validity after every lenient parsing change
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/lenient
created_at: 2024-03-19T19:21:19Z
updated_at: 2024-03-19T19:41:50Z
url: https://github.com/astral-sh/uv/pull/2550
synced_at: 2026-01-12T16:05:06Z
```

# Re-test validity after every lenient parsing change

---

_@charliermarsh_

## Summary

We had the right fixup for `torchsde`, but a subsequent fixup was making it invalid. In general, we should apply as few of these as we can, so lets stop as soon as we succeed.

Closes https://github.com/astral-sh/uv/issues/2546.

## Test Plan

`cargo run pip install torchsde==0.2.5 --verbose --reinstall -n --verbose`


---

_Label `bug` added by @charliermarsh on 2024-03-19 19:21_

---

_Marked ready for review by @charliermarsh on 2024-03-19 19:21_

---

_Merged by @charliermarsh on 2024-03-19 19:41_

---

_Closed by @charliermarsh on 2024-03-19 19:41_

---

_Branch deleted on 2024-03-19 19:41_

---
