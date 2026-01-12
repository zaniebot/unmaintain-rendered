```yaml
number: 8407
title: "Fix bug where `PLE1307` was raised when formatting `%c` with characters"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zanie/ple-char
created_at: 2023-11-01T16:13:08Z
updated_at: 2023-11-02T04:45:29Z
url: https://github.com/astral-sh/ruff/pull/8407
synced_at: 2026-01-10T23:40:55Z
```

# Fix bug where `PLE1307` was raised when formatting `%c` with characters

---

_Pull request opened by @zanieb on 2023-11-01 16:13_

Closes https://github.com/astral-sh/ruff/issues/8406


---

_@zanieb reviewed on 2023-11-01 16:13_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/pylint/bad_string_format_type.py`:29 on 2023-11-01 16:13_

Okay Ruff formatter :D will revert

---

_Comment by @github-actions[bot] on 2023-11-01 16:35_

## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @zanieb on 2023-11-01 16:44_

---

_Label `bug` added by @zanieb on 2023-11-01 21:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_string_format_type.rs`:127 on 2023-11-01 23:30_

Could this also be a byte string or fstring?

---

_@MichaReiser approved on 2023-11-01 23:31_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pylint/rules/bad_string_format_type.rs`:127 on 2023-11-02 01:00_

Hm good idea, I checked:

```python
print("%c" % b"x") # Runtime error
print("%c" % f"x")  # OK
```

I'm hesitant to support the format string though it's a weird use.




---

_@zanieb reviewed on 2023-11-02 01:00_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pylint/bad_string_format_type.py`:65 on 2023-11-02 04:03_

Probably a typo?
```suggestion
"%c" % "œ"
```

---

_@dhruvmanila approved on 2023-11-02 04:04_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/pylint/bad_string_format_type.py`:65 on 2023-11-02 04:28_

Definitely, thanks!

---

_@zanieb reviewed on 2023-11-02 04:28_

---

_Merged by @zanieb on 2023-11-02 04:36_

---

_Closed by @zanieb on 2023-11-02 04:36_

---

_Branch deleted on 2023-11-02 04:36_

---
