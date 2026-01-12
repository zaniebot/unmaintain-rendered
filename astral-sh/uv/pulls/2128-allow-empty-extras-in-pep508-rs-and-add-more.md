```yaml
number: 2128
title: "Allow empty extras in `pep508-rs` and add more corner case to tests"
type: pull_request
state: merged
author: SigureMo
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: pep508-allow-empty-extras
created_at: 2024-03-02T09:49:11Z
updated_at: 2024-03-03T02:38:58Z
url: https://github.com/astral-sh/uv/pull/2128
synced_at: 2026-01-12T16:04:53Z
```

# Allow empty extras in `pep508-rs` and add more corner case to tests

---

_@SigureMo_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #2127, allow empty extras, and add more corner case to tests

## References

- [PEP 508 grammar](https://peps.python.org/pep-0508/#complete-grammar)

---

_Renamed from "Allow empty extras in PEP508 and add more corner case to tests" to "Allow empty extras in pep508-rs and add more corner case to tests" by @SigureMo on 2024-03-02 09:50_

---

_Renamed from "Allow empty extras in pep508-rs and add more corner case to tests" to "Allow empty extras in `pep508-rs` and add more corner case to tests" by @SigureMo on 2024-03-02 09:51_

---

_Review comment by @AlexWaygood on `crates/pep508-rs/src/lib.rs`:637 on 2024-03-02 12:10_

```suggestion
                        "Expected either alphanumerical character (starting the extra name) or ']' (ending the extras section), found ','".to_string()
```

---

_Review comment by @AlexWaygood on `crates/requirements-txt/src/lib.rs`:1208 on 2024-03-02 12:10_

```suggestion
            Expected either alphanumerical character (starting the extra name) or ']' (ending the extras section), found ','
```

---

_@AlexWaygood reviewed on 2024-03-02 12:11_

A small grammar nit regarding the error message:

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-02 14:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-02 14:04_

---

_Label `bug` added by @charliermarsh on 2024-03-02 14:04_

---

_Label `compatibility` added by @charliermarsh on 2024-03-02 14:04_

---

_@charliermarsh approved on 2024-03-02 20:27_

Thanks!

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:620 on 2024-03-02 20:28_

Moved this out from the start of the loop, because it's also at the end of the loop.

---

_@charliermarsh reviewed on 2024-03-02 20:28_

---

_Merged by @charliermarsh on 2024-03-02 20:36_

---

_Closed by @charliermarsh on 2024-03-02 20:36_

---

_Branch deleted on 2024-03-03 02:38_

---
