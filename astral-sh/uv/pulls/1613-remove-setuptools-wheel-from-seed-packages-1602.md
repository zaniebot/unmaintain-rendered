```yaml
number: 1613
title: "Remove setuptools & wheel from seed packages (#1602)"
type: pull_request
state: merged
author: arjunnn
labels:
  - compatibility
  - virtualenv
assignees: []
merged: true
base: main
head: arjun/remove-setuptools-wheel-from-seed
created_at: 2024-02-17T19:49:35Z
updated_at: 2024-02-18T21:20:21Z
url: https://github.com/astral-sh/uv/pull/1613
synced_at: 2026-01-10T15:33:24Z
```

# Remove setuptools & wheel from seed packages (#1602)

---

_Pull request opened by @arjunnn on 2024-02-17 19:49_

## Summary
Removed `wheel` and `setuptools` from seed packages list when creating a virtual environment

Closes https://github.com/astral-sh/uv/issues/1602

## Test Plan
Ran the command `cargo nextest run` :
<img width="564" alt="image" src="https://github.com/astral-sh/uv/assets/6116387/14ed2da6-1b3e-4598-a49f-29dd8c4cb19b">



---

_Label `compatibility` added by @zanieb on 2024-02-17 19:51_

---

_Comment by @zanieb on 2024-02-17 19:52_

Hi! Thanks for opening a pull request.

We'll only want to do this when the Python version is 3.12 or newer.

---

_@zanieb reviewed on 2024-02-17 19:52_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:167 on 2024-02-17 19:52_

This list should be conditional on the Python version being used

---

_@zanieb requested changes on 2024-02-17 19:53_

<!-- empty -->

---

_@arjunnn reviewed on 2024-02-17 21:07_

---

_Review comment by @arjunnn on `crates/uv/src/commands/venv.rs`:167 on 2024-02-17 21:07_

Thank you for reviewing this
I've updated it with a conditional check 

---

_Assigned to @zanieb by @zanieb on 2024-02-17 21:08_

---

_@zanieb reviewed on 2024-02-18 08:22_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:169 on 2024-02-18 08:22_

Could you use `interpreter.python_tuple()` and just do a numerical comparison instead of using the `VersionSpecifier`? It should be faster and simpler.

---

_@zanieb reviewed on 2024-02-18 08:23_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:166 on 2024-02-18 08:23_

Can you copy this seed test case for another Python version to show we preserve these?

---

_@arjunnn reviewed on 2024-02-18 11:17_

---

_Review comment by @arjunnn on `crates/uv/src/commands/venv.rs`:169 on 2024-02-18 11:17_

updated to use numerical comparison

---

_@arjunnn reviewed on 2024-02-18 11:17_

---

_Review comment by @arjunnn on `crates/uv/tests/venv.rs`:166 on 2024-02-18 11:17_

added a new test case for older python versions

---

_Review requested from @zanieb by @arjunnn on 2024-02-18 12:37_

---

_Comment by @zanieb on 2024-02-18 21:16_

Thank you!

---

_@zanieb approved on 2024-02-18 21:16_

---

_Label `virtualenv` added by @zanieb on 2024-02-18 21:17_

---

_Merged by @zanieb on 2024-02-18 21:20_

---

_Closed by @zanieb on 2024-02-18 21:20_

---
