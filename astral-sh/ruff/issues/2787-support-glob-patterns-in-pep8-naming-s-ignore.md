```yaml
number: 2787
title: "Support glob patterns in `pep8-naming`’s `ignore-names` option."
type: issue
state: closed
author: manueljacob
labels:
  - configuration
assignees: []
created_at: 2023-02-12T00:55:25Z
updated_at: 2023-06-13T15:37:15Z
url: https://github.com/astral-sh/ruff/issues/2787
synced_at: 2026-01-12T15:54:43Z
```

# Support glob patterns in `pep8-naming`’s `ignore-names` option.

---

_@manueljacob_

The original `pep8-naming` supports glob patterns in the `ignore-names` [option](https://pypi.org/project/pep8-naming/#options). Currently, Ruff supports only fixed strings.

---

_Label `configuration` added by @charliermarsh on 2023-02-12 16:58_

---

_Comment by @ergoithz on 2023-05-25 13:59_

Just wanted to point out this would be required to effectively support custom unittest-style `assert*` methods.

Without this feature, the only alternatives are either disabling N802 entirely or manually add every single custom assertion method name as `ignore-names`.

---

_Assigned to @Thomasdezeeuw by @Thomasdezeeuw on 2023-06-12 11:56_

---

_Closed by @Thomasdezeeuw on 2023-06-13 15:37_

---
