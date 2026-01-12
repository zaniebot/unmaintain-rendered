```yaml
number: 18608
title: Adding flake8-future-annotations for deprecated typing features
type: issue
state: closed
author: joshlk
labels:
  - question
assignees: []
created_at: 2025-06-10T09:26:53Z
updated_at: 2025-06-10T10:18:38Z
url: https://github.com/astral-sh/ruff/issues/18608
synced_at: 2026-01-12T15:54:56Z
```

# Adding flake8-future-annotations for deprecated typing features

---

_@joshlk_

### Summary

Hi, would it be possible to add [flake8-future-annotations](https://pypi.org/project/flake8-modern-annotations/) which fixes the use of deprecated typing features such as using `Tuple`, `List`, `Dict`, `Optional`, `Union` (instead you can now use `tuple`, `list`, `dict`, `| None`, `|`)?

The rules in flake8-future-annotations are `MDA100` to `MDA501`. The other rules are also useful.

Thanks

---

_Comment by @MichaReiser on 2025-06-10 09:28_

I think this is already covered by some of the UP rules, e.g. https://docs.astral.sh/ruff/rules/non-pep604-annotation-union/ or what's missing from those rules to replace `flake8-future-annoations`?

---

_Label `question` added by @MichaReiser on 2025-06-10 09:28_

---

_Renamed from "Adding flake8-future-annotations for depricated typing features" to "Adding flake8-future-annotations for deprecated typing features" by @AlexWaygood on 2025-06-10 10:01_

---

_Comment by @joshlk on 2025-06-10 10:18_

Ah sorry - I didn't see those rules! I did a fair bit of Googling before submitting this request so it seems Google haven't picked up on it. Thanks!

---

_Closed by @joshlk on 2025-06-10 10:18_

---
