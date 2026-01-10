```yaml
number: 499
title: Perform a single Git fetch when building source distributions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/precise
created_at: 2023-11-24T18:14:38Z
updated_at: 2023-11-25T23:29:43Z
url: https://github.com/astral-sh/uv/pull/499
synced_at: 2026-01-10T15:44:44Z
```

# Perform a single Git fetch when building source distributions

---

_Pull request opened by @charliermarsh on 2023-11-24 18:14_

## Summary

We need to pass in the distribution with the "precise" URL to avoid refetching.

## Test Plan

Ran `cargo run -p puffin-cli -- pip-compile requirements.in --verbose` with `flask @ git+https://github.com/pallets/flask.git` and verified that we only checked out Flask once.

---

_@charliermarsh reviewed on 2023-11-24 18:14_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/source_dist.rs`:156 on 2023-11-24 18:14_

Probably clearer to just remove this and return the precise URL from `self.git`.

---

_Label `bug` added by @charliermarsh on 2023-11-24 18:15_

---

_Comment by @charliermarsh on 2023-11-24 18:24_

Need to look into this test failure.

---

_Comment by @charliermarsh on 2023-11-24 18:25_

I suspect it has to do with using the precise vs. non-precise URL somewhere.

---

_Converted to draft by @charliermarsh on 2023-11-24 19:08_

---

_Comment by @charliermarsh on 2023-11-24 19:08_

I see the problem, will fix later and mark as ready...

---

_Marked ready for review by @charliermarsh on 2023-11-25 23:23_

---

_Merged by @charliermarsh on 2023-11-25 23:29_

---

_Closed by @charliermarsh on 2023-11-25 23:29_

---

_Branch deleted on 2023-11-25 23:29_

---
