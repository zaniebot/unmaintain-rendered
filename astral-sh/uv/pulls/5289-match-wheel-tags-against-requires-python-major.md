```yaml
number: 5289
title: "Match wheel tags against `Requires-Python` major-minor"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/req-python-wheels
created_at: 2024-07-22T14:13:25Z
updated_at: 2024-07-22T14:33:55Z
url: https://github.com/astral-sh/uv/pull/5289
synced_at: 2026-01-12T16:06:44Z
```

# Match wheel tags against `Requires-Python` major-minor

---

_@charliermarsh_

## Summary

Given `Requires-Python: ">=3.12.3"`, we were rejecting wheels like `dearpygui-1.11.1-cp312-cp312-win_amd64.whl`, since `3.12.0` is not included in `>=3.12.3`. We instead need to test against the major-minor version of `Requires-Python`.

The easiest way to do this, I think, is the use the `RequiresPython` struct, which has a single bound that we can truncate the major-minor. This also means that we now allow `dearpygui-1.11.1-cp312-cp312-win_amd64.whl` for specifiers like `Requires-Python: "==3.10.*"`. This is incorrect on the surface, but it does match our semantics for `Requires-Python` elsewhere: we treat it as a lower bound.

Closes https://github.com/astral-sh/uv/issues/5287.


---

_Review requested from @konstin by @charliermarsh on 2024-07-22 14:13_

---

_Label `bug` added by @charliermarsh on 2024-07-22 14:13_

---

_@charliermarsh reviewed on 2024-07-22 14:13_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:174 on 2024-07-22 14:13_

This is strange but I think correct...

---

_Review comment by @konstin on `crates/uv-resolver/src/requires_python.rs`:174 on 2024-07-22 14:16_

Yeah `>3.10` is a footgun because it does actually include `3.10` version, this is basically PYI006

---

_Comment by @konstin on 2024-07-22 14:24_

> This also means that we now allow dearpygui-1.11.1-cp312-cp312-win_amd64.whl for specifiers like Requires-Python: "==3.10.*"

For warehouse with `==3.11.*`, this increases the lockfile from 4161 lines to 4466 lines (+7%).

---

_@konstin approved on 2024-07-22 14:24_

---

_Comment by @charliermarsh on 2024-07-22 14:25_

> For warehouse with ==3.11.*, this increases the lockfile from 4161 lines to 4466 lines (+7%).

Yeah, unfortunately I think we kinda have to do this, unless we want to change our `Requires-Python` semantics. But we should do that holistically if so.

---

_Comment by @konstin on 2024-07-22 14:33_

The most correct solution is probably computing an upper bound the same way we compute the lower bound, incl. widening to the next full major-minor version/discarding the patch version, and then only applying the lower bound for matching dependencies' requires-python, but applying the lower and the upper bound for selecting their wheels. (Giving us requires-python semantic #3, i think).

Improving this isn't schema-breaking or badly breaking backwards compatibility (the wheels can never be selected anyway), so we can move this post-release-ready.

---

_Merged by @charliermarsh on 2024-07-22 14:33_

---

_Closed by @charliermarsh on 2024-07-22 14:33_

---

_Branch deleted on 2024-07-22 14:33_

---
