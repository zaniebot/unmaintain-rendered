```yaml
number: 8278
title: Update black tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: update-black-tests
created_at: 2023-10-27T10:34:58Z
updated_at: 2023-10-27T10:44:20Z
url: https://github.com/astral-sh/ruff/pull/8278
synced_at: 2026-01-12T15:55:26Z
```

# Update black tests

---

_@konstin_

Update black tests to https://github.com/psf/black/commit/c369e446f9dbff313ebb555bf461b4e7778ca78d

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/black/raw_docstring.py`:1 on 2023-10-27 10:39_

Should we add an option file that sets `preview: true`?

---

_@MichaReiser approved on 2023-10-27 10:39_

---

_Label `internal` added by @MichaReiser on 2023-10-27 10:39_

---

_@konstin reviewed on 2023-10-27 10:42_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/black/raw_docstring.py`:1 on 2023-10-27 10:42_

I don't think we support them for black tests yet

---

_@MichaReiser reviewed on 2023-10-27 10:43_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/black/raw_docstring.py`:1 on 2023-10-27 10:43_

We do, but only a single options file

https://github.com/astral-sh/ruff/blob/2d9b39871febdef1c5db75a2c9673dabcb86fbc1/crates/ruff_python_formatter/resources/test/fixtures/black/simple_cases/docstring.options.json#L1-L0

---

_Merged by @konstin on 2023-10-27 10:44_

---

_Closed by @konstin on 2023-10-27 10:44_

---

_Branch deleted on 2023-10-27 10:44_

---
