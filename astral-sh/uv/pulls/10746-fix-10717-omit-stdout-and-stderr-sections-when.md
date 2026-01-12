```yaml
number: 10746
title: "fix(#10717): Omit stdout and stderr sections when empty in errors"
type: pull_request
state: merged
author: issokuos
labels:
  - error messages
assignees: []
merged: true
base: main
head: fix-10717
created_at: 2025-01-19T01:05:09Z
updated_at: 2025-01-22T07:34:45Z
url: https://github.com/astral-sh/uv/pull/10746
synced_at: 2026-01-12T16:09:28Z
```

# fix(#10717): Omit stdout and stderr sections when empty in errors

---

_@issokuos_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This change closes #10717 

## Test Plan

<!-- How was it tested? -->


---

_Review comment by @charliermarsh on `crates/uv-python/src/interpreter.rs`:575 on 2025-01-19 02:37_

Is `write` here vs. `writeln` above intentional?

---

_@charliermarsh reviewed on 2025-01-19 02:37_

---

_@charliermarsh reviewed on 2025-01-19 02:38_

---

_Review comment by @charliermarsh on `crates/uv-python/src/interpreter.rs`:572 on 2025-01-19 02:38_

Is `{}` vs. `{0}` below intentional?

---

_@charliermarsh reviewed on 2025-01-19 02:38_

Thanks!

---

_@issokuos reviewed on 2025-01-19 07:04_

---

_Review comment by @issokuos on `crates/uv-python/src/interpreter.rs`:572 on 2025-01-19 07:04_

This was intentional, I  replaced the positional  argument with a named  argument

---

_@issokuos reviewed on 2025-01-19 07:04_

---

_Review comment by @issokuos on `crates/uv-python/src/interpreter.rs`:575 on 2025-01-19 07:04_

No, it wasn't fixed now. Thank you

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-21 21:09_

---

_@charliermarsh approved on 2025-01-21 21:32_

---

_Label `error messages` added by @charliermarsh on 2025-01-21 21:32_

---

_Merged by @charliermarsh on 2025-01-21 21:45_

---

_Closed by @charliermarsh on 2025-01-21 21:45_

---

_Branch deleted on 2025-01-22 07:34_

---
