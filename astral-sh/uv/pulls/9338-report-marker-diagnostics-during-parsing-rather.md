```yaml
number: 9338
title: Report marker diagnostics during parsing, rather than evaluation
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/eval
created_at: 2024-11-21T22:27:20Z
updated_at: 2024-11-26T14:15:34Z
url: https://github.com/astral-sh/uv/pull/9338
synced_at: 2026-01-10T12:00:00Z
```

# Report marker diagnostics during parsing, rather than evaluation

---

_Pull request opened by @charliermarsh on 2024-11-21 22:27_

## Summary

I want to move towards a more normalized marker representation within the marker tree, which means that the things we warn against will disappear by the time we get to evaluation. I think it makes more sense to show these warnings when we create the tree, rather than when we evaluate it.


---

_Review requested from @konstin by @charliermarsh on 2024-11-21 22:27_

---

_Label `error messages` added by @charliermarsh on 2024-11-21 22:27_

---

_Comment by @charliermarsh on 2024-11-25 03:43_

Ah shoot, I accidentally merged https://github.com/astral-sh/uv/pull/9341 into this branch.

---

_Comment by @charliermarsh on 2024-11-25 03:43_

Please review the first commit, but ignore the second.

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/parse.rs`:114 on 2024-11-26 10:13_

nit: `?` to remove the inspect

---

_Review comment by @konstin on `crates/uv-pep508/src/verbatim_url.rs`:12 on 2024-11-26 10:14_

I forgot to comment that, but this one is an explicit import to make exporting and rewriting to https://github.com/konstin/pep508_rs/blob/d155787317425e8ab897beef55a947e8ef28f9d8/src/verbatim_url.rs#L12-L13 easier

---

_@konstin approved on 2024-11-26 10:15_

That's the much better place

---

_@charliermarsh reviewed on 2024-11-26 14:06_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/marker/parse.rs`:114 on 2024-11-26 14:06_

Is that better? Then I have to assign and return the value

---

_Merged by @charliermarsh on 2024-11-26 14:15_

---

_Closed by @charliermarsh on 2024-11-26 14:15_

---

_Branch deleted on 2024-11-26 14:15_

---
