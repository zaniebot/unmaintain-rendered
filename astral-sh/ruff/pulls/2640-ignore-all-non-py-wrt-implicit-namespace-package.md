```yaml
number: 2640
title: "Ignore all non-`.py` wrt. implicit namespace package"
type: pull_request
state: merged
author: scop
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/ns-package-scripts
created_at: 2023-02-07T22:06:06Z
updated_at: 2023-02-08T06:25:28Z
url: https://github.com/astral-sh/ruff/pull/2640
synced_at: 2026-01-12T15:55:09Z
```

# Ignore all non-`.py` wrt. implicit namespace package

---

_@scop_

It's not only `.pyi` that should be exempt for this, but also for example scripts which don't have an extension, explicitly passed in command line args.

---

_Review comment by @scop on `crates/ruff/resources/test/fixtures/flake8_no_pep420/test_pass_script/script`:1 on 2023-02-07 22:08_

Note: it seems this fixture never gets tested, I guess because it doesn't have a `.py` or `.pyi` extension. Haven't researched what could/should be done about that yet.

---

_@scop reviewed on 2023-02-07 22:08_

---

_@scop reviewed on 2023-02-07 22:15_

---

_Review comment by @scop on `crates/ruff/resources/test/fixtures/flake8_no_pep420/test_pass_script/script`:1 on 2023-02-07 22:15_

Nevermind, found it (and fixed related bugs!)

---

_Label `bug` added by @charliermarsh on 2023-02-07 23:21_

---

_Comment by @charliermarsh on 2023-02-07 23:21_

Seems reasonable. (We don't run Ruff over files that lack an extension unless they're passed in explicitly.)

---

_Merged by @charliermarsh on 2023-02-07 23:22_

---

_Closed by @charliermarsh on 2023-02-07 23:22_

---

_Branch deleted on 2023-02-08 06:17_

---

_Comment by @scop on 2023-02-08 06:17_

Yeah, that behavior of not running over extensionless is ok, too. But for example pre-commit does pass them to ruff, because it decides pythonness also based on file shebangs, which is reasonable as well, and wouldn't be unreasonable for ruff to do at some point, too I think :)

---
