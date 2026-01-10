```yaml
number: 13803
title: Better error message for version specifier with missing operator
type: pull_request
state: merged
author: jmwoliver
labels: []
assignees: []
merged: true
base: main
head: jmw/missing-operator-hint-error
created_at: 2025-06-03T12:36:52Z
updated_at: 2025-06-03T19:18:19Z
url: https://github.com/astral-sh/uv/pull/13803
synced_at: 2026-01-10T11:10:42Z
```

# Better error message for version specifier with missing operator

---

_Pull request opened by @jmwoliver on 2025-06-03 12:36_

## Summary

When missing an operator for version parsing, it would give an error that was hard to know how to fix if you were not familiar with version specifiers / PEP-440:

```
Unexpected end of version specifier, expected operator
```

Now, it will attempt to provide a more useful hint if it can parse the version from the remaining scanner:

```
Unexpected end of version specifier, expected operator. Did you mean `==3.12`?
```
## Test Plan

Unit tests in `version_specifier.rs` were added/updated for the following cases:
- `test_parse_specifier_missing_operator_error`
- `test_parse_specifier_missing_operator_invalid_version_error`
- `test_invalid_word`
- `test_invalid_specifier`
- `error_message_version_specifiers_parse_error`

A test in `edit.rs` for failing to parse the `pyproject.toml` when using `add` was also included to match the request in the original issue:
- `add_invalid_requires_python`

I didn't add cases where no version specifier is provided because it seemed like it doesn't get to the point of parsing in that case, so it should not happen.


## Reference

Fixes #13126

---

_@joao-krauchenco-mottu approved on 2025-06-03 12:38_

---

_Assigned to @konstin by @zanieb on 2025-06-03 12:51_

---

_Review comment by @jmwoliver on `crates/uv/tests/it/pip_compile.rs`:6964 on 2025-06-03 13:02_

This seems like it is parsing wrong (`3.12` vs `12`) but I don't understand the full context and maybe that is the expected behavior in this case. I updated it to get the tests passing.

---

_@jmwoliver reviewed on 2025-06-03 13:03_

---

_Comment by @jmwoliver on 2025-06-03 13:33_

I'm not sure why the `CI / check system | python on opensuse` action is failing. Would someone mind rerunning that? It looks like it just timed out.

---

_Comment by @zanieb on 2025-06-03 14:33_

Yeah, we'll disable that until we can fix it.

---

_@zanieb reviewed on 2025-06-03 14:34_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:6964 on 2025-06-03 14:34_

The wheel _does_ have `12` in it

```
❯ cat validation-3.0.0.dist-info/METADATA
Metadata-Version: 2.1
Name: validation
Version: 3.0.0
Requires-Python: 12
```

---

_@jmwoliver reviewed on 2025-06-03 14:43_

---

_Review comment by @jmwoliver on `crates/uv/tests/it/pip_compile.rs`:6964 on 2025-06-03 14:43_

Ah thanks for the clarification! I saw this:
```rust
let context = TestContext::new("3.12");
```

And wasn't sure if it was just parsing that wrong.

---

_Renamed from "fix: better error message when missing operator in version parsing (#13126)" to "Better error message when missing operator in version parsing" by @konstin on 2025-06-03 17:55_

---

_Renamed from "Better error message when missing operator in version parsing" to "Better error message for missing operator in version parsing" by @konstin on 2025-06-03 17:55_

---

_Renamed from "Better error message for missing operator in version parsing" to "Better error message for version specifier with missing operator" by @konstin on 2025-06-03 17:55_

---

_Comment by @konstin on 2025-06-03 17:56_

I changed the diagnostic building so we don't drop a `.*` if present and adjusted the message style for the uv conventions.

---

_@konstin approved on 2025-06-03 17:57_

---

_Merged by @konstin on 2025-06-03 19:18_

---

_Closed by @konstin on 2025-06-03 19:18_

---
