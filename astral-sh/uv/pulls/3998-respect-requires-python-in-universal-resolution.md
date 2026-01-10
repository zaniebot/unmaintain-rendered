```yaml
number: 3998
title: "Respect `Requires-Python` in universal resolution"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/req-python
created_at: 2024-06-03T21:28:39Z
updated_at: 2024-06-04T13:59:05Z
url: https://github.com/astral-sh/uv/pull/3998
synced_at: 2026-01-10T13:54:02Z
```

# Respect `Requires-Python` in universal resolution

---

_Pull request opened by @charliermarsh on 2024-06-03 21:28_

## Summary

Closes #3982.


---

_Label `preview` added by @charliermarsh on 2024-06-03 21:28_

---

_Marked ready for review by @charliermarsh on 2024-06-03 22:42_

---

_@charliermarsh reviewed on 2024-06-03 23:09_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:994 on 2024-06-03 23:09_

Not a particularly clear message but it's roughly the same as `echo "pygls>=1.1.0" | cargo run pip compile --python-version 3.7 -` before any of my changes.

---

_Review requested from @zanieb by @charliermarsh on 2024-06-03 23:10_

---

_Review requested from @konstin by @charliermarsh on 2024-06-03 23:10_

---

_@charliermarsh reviewed on 2024-06-04 01:17_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:994 on 2024-06-04 01:17_

Here's Poetry:

```
The current project's Python requirement (>=3.7) is not compatible with some of the required packages Python requirement:
  - pygls requires Python >=3.7.9,<4, so it will not be satisfied for Python >=3.7,<3.7.9 || >=4
  - pygls requires Python >=3.7.9,<4, so it will not be satisfied for Python >=3.7,<3.7.9 || >=4
  - pygls requires Python >=3.7.9,<4, so it will not be satisfied for Python >=3.7,<3.7.9 || >=4
  - pygls requires Python >=3.7.9,<4, so it will not be satisfied for Python >=3.7,<3.7.9 || >=4
  - pygls requires Python >=3.7.9,<4, so it will not be satisfied for Python >=3.7,<3.7.9 || >=4
  - pygls requires Python >=3.8, so it will not be satisfied for Python >=3.7,<3.8
  - pygls requires Python >=3.8, so it will not be satisfied for Python >=3.7,<3.8

Because no versions of pygls match >1.1.0,<1.1.1 || >1.1.1,<1.1.2 || >1.1.2,<1.2.0 || >1.2.0,<1.2.1 || >1.2.1,<1.3.0 || >1.3.0,<1.3.1 || >1.3.1
 and pygls (1.1.0) requires Python >=3.7.9,<4, pygls is forbidden.
And because pygls (1.1.1) requires Python >=3.7.9,<4, pygls is forbidden.
And because pygls (1.1.2) requires Python >=3.7.9,<4
 and pygls (1.2.0) requires Python >=3.7.9,<4, pygls is forbidden.
And because pygls (1.2.1) requires Python >=3.7.9,<4
 and pygls (1.3.0) requires Python >=3.8, pygls is forbidden.
So, because pygls (1.3.1) requires Python >=3.8
 and poetry-test depends on pygls (>=1.1.0), version solving failed.

  • Check your dependencies Python requirement: The Python requirement can be specified via the `python` or `markers` properties

    For pygls, a possible solution would be to set the `python` property to ">=3.7.9,<4"
    For pygls, a possible solution would be to set the `python` property to ">=3.7.9,<4"
    For pygls, a possible solution would be to set the `python` property to ">=3.7.9,<4"
    For pygls, a possible solution would be to set the `python` property to ">=3.7.9,<4"
    For pygls, a possible solution would be to set the `python` property to ">=3.7.9,<4"
    For pygls, a possible solution would be to set the `python` property to ">=3.8"
    For pygls, a possible solution would be to set the `python` property to ">=3.8"
```

---

_Review comment by @konstin on `crates/uv-resolver/src/python_requirement.rs`:85 on 2024-06-04 07:54_

Added the missing try_from conversion method for that in https://github.com/astral-sh/uv/pull/4010

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:994 on 2024-06-04 08:00_

We should add a hint here that "requested Python version" refers to the version you declared in your pyproject.toml and that - unlike everything else - that version needs to stricter than the one in your dependencies.

---

_@konstin approved on 2024-06-04 08:36_

---

_@charliermarsh reviewed on 2024-06-04 13:44_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:994 on 2024-06-04 13:44_

Will do in a separate PR.

---

_Merged by @charliermarsh on 2024-06-04 13:56_

---

_Closed by @charliermarsh on 2024-06-04 13:56_

---

_Branch deleted on 2024-06-04 13:56_

---

_@charliermarsh reviewed on 2024-06-04 13:59_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/python_requirement.rs`:85 on 2024-06-04 13:59_

Thx.

---
