```yaml
number: 3280
title: " Replace tuples with type union in isinstance or issubclass calls"
type: pull_request
state: merged
author: martinlehoux
labels:
  - rule
assignees: []
merged: true
base: main
head: github-2923
created_at: 2023-02-28T19:56:40Z
updated_at: 2023-03-03T21:52:06Z
url: https://github.com/astral-sh/ruff/pull/3280
synced_at: 2026-01-12T04:39:44Z
```

#  Replace tuples with type union in isinstance or issubclass calls

---

_Pull request opened by @martinlehoux on 2023-02-28 19:56_

- #2923 

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:86 on 2023-03-01 23:25_

Nit: check `checker.is_builtin(id)` here.

---

_@charliermarsh reviewed on 2023-03-01 23:25_

---

_@charliermarsh reviewed on 2023-03-01 23:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:78 on 2023-03-01 23:26_

Sorry to go back on my own suggestion, but on second thought, let's make this its own rule (`use_pep604_isinstance`). It is part of PEP 604, but it's not related to _annotations_.

---

_Marked ready for review by @charliermarsh on 2023-03-01 23:26_

---

_Comment by @martinlehoux on 2023-03-02 20:08_

Does it look better now? Maybe an idea for the violation message?

---

_Merged by @charliermarsh on 2023-03-02 20:59_

---

_Closed by @charliermarsh on 2023-03-02 20:59_

---

_Comment by @charliermarsh on 2023-03-02 20:59_

Looks good :)

---

_Label `rule` added by @charliermarsh on 2023-03-02 20:59_

---

_Branch deleted on 2023-03-02 21:30_

---

_Comment by @andersk on 2023-03-03 21:50_

This only works in Python 3.10+ and needs to be conditional on `target_version >= PythonVersion::Py310`.

---

_Comment by @charliermarsh on 2023-03-03 21:51_

Ah sorry, just an oversight on my part.

---

_Comment by @charliermarsh on 2023-03-03 21:52_

(Hasn't gone out yet, will fix before next release.)

---
