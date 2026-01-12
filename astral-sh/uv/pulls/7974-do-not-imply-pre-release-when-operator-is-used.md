```yaml
number: 7974
title: "do not imply pre-release when `!=` operator is used"
type: pull_request
state: merged
author: Czaki
labels:
  - bug
  - breaking
assignees: []
merged: true
base: tracking/050
head: fix_ne_pre
created_at: 2024-10-07T15:08:21Z
updated_at: 2024-10-21T18:27:23Z
url: https://github.com/astral-sh/uv/pull/7974
synced_at: 2026-01-12T16:08:06Z
```

# do not imply pre-release when `!=` operator is used

---

_@Czaki_

## Summary

closes #6640

## Test Plan

Could you suggest how I should test it? 

(already tested locally)


---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-10-09 22:29_

---

_Comment by @charliermarsh on 2024-10-09 22:29_

This looks right to me! It will need to be part of 0.5.0 so I'll add it to that milestone.

---

_@charliermarsh approved on 2024-10-09 22:29_

---

_Comment by @charliermarsh on 2024-10-09 22:31_

I think you could write a test like `prerelease_upper_bound_exclude`... `flask<2.0.1, !=2.0.0rc1` and ensure that `2.0.0rc2` is _not_ selected.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-09 22:31_

---

_Label `bug` added by @charliermarsh on 2024-10-09 22:31_

---

_Label `breaking` added by @charliermarsh on 2024-10-09 22:31_

---

_@Czaki reviewed on 2024-10-10 09:28_

---

_Review comment by @Czaki on `crates/uv-resolver/src/prerelease.rs`:87 on 2024-10-10 09:28_

@charliermarsh I just spot that there is `NotEqualStar` operator also defined. Should I add it here?  Is `flask!=2.0.0rc*` valid inequality? 

---

_@charliermarsh reviewed on 2024-10-10 13:56_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/prerelease.rs`:87 on 2024-10-10 13:56_

Yeah, I think you should add that here too.

---

_@charliermarsh reviewed on 2024-10-10 14:08_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/prerelease.rs`:89 on 2024-10-10 14:08_

You might be interested in something like: `!matches!(spec.operator(), Operator::NotEqual | Operator::NotEqualStar)`

---

_@Czaki reviewed on 2024-10-10 15:26_

---

_Review comment by @Czaki on `crates/uv-resolver/src/prerelease.rs`:89 on 2024-10-10 15:26_

thanks

---

_Comment by @zanieb on 2024-10-21 17:05_

@Czaki it looks like this has some unrelated changes in it now.

---

_Comment by @charliermarsh on 2024-10-21 17:46_

I backed them out. @zanieb, want to merge into 0.5 branch?

---

_@zanieb approved on 2024-10-21 17:53_

---

_Merged by @charliermarsh on 2024-10-21 18:25_

---

_Closed by @charliermarsh on 2024-10-21 18:25_

---

_Branch deleted on 2024-10-21 18:27_

---
