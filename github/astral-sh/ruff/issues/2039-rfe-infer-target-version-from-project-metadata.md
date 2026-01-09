---
number: 2039
title: "RFE: infer `target-version` from project metadata"
type: issue
state: closed
author: scop
labels:
  - core
assignees: []
created_at: 2023-01-20T21:14:11Z
updated_at: 2023-03-13T19:42:57Z
url: https://github.com/astral-sh/ruff/issues/2039
synced_at: 2026-01-07T13:12:14-06:00
---

# RFE: infer `target-version` from project metadata

---

_Issue opened by @scop on 2023-01-20 21:14_

`target-version` could in many cases be inferred from project metadata: 
- https://packaging.python.org/en/latest/specifications/declaring-project-metadata/#requires-python
- https://packaging.python.org/en/latest/specifications/core-metadata/#core-metadata-requires-python

Would be great to see ruff do that to eliminate one maintenance chore.

Black is moving toward doing that, too. The implementation over there is obviously somewhat different, because they still take a list of target versions whereas ruff takes the minimum one (which I think is the better, yet less flexible approach). Perhaps some of that work can be reused in ruff: https://github.com/psf/black/pull/3219

---

_Label `core` added by @charliermarsh on 2023-01-20 21:15_

---

_Referenced in [astral-sh/ruff#2099](../../astral-sh/ruff/pulls/2099.md) on 2023-01-30 21:06_

---

_Comment by @charliermarsh on 2023-01-30 22:58_

Perhaps we can use https://github.com/konstin/pep440-rs from @konstin to power the version parsing?

---

_Comment by @ilkersigirci on 2023-02-03 08:50_

FYI: [black#3219](https://github.com/psf/black/pull/3219) was merged and shipped with black [23.1.0](https://github.com/psf/black/releases/tag/23.1.0)

---

_Referenced in [astral-sh/ruff#2519](../../astral-sh/ruff/issues/2519.md) on 2023-02-03 09:11_

---

_Referenced in [astral-sh/ruff#2857](../../astral-sh/ruff/issues/2857.md) on 2023-02-13 14:49_

---

_Referenced in [astral-sh/ruff#3093](../../astral-sh/ruff/issues/3093.md) on 2023-02-21 19:40_

---

_Comment by @manueljacob on 2023-02-24 21:52_

Black’s target version inference is currently broken: https://github.com/psf/black/issues/3581. I’m currently working on a fix and would be happy to port the solution to ruff. However, before that, #2519 should be decided.

---

_Comment by @charliermarsh on 2023-02-24 23:12_

Nice! I'm happy to support target ranges, it's just not totally clear to me what rules they would affect and how.

---

_Comment by @JonathanPlasse on 2023-02-24 23:14_

Black have different behaviors depending on the versions for example. Maybe ruff formatter should do the same.

---

_Comment by @konstin on 2023-02-25 09:56_

one thing to consider is that the behavior in https://github.com/psf/black/issues/3581 (`>3.7` still implying 3.7 code styles) is most likely unintended and imho `>3.7` (as opposed to `>3.7.1` or `>=3.7`) should raise a warning (or you could emit linter errors for ruff's own configuration :D) EDIT: See also Y006 in [flake8-pyi](https://github.com/PyCQA/flake8-pyi) ("Use only `<` and `>=` for version comparisons. Comparisons involving `>` and `<=` may produce unintuitive results when tools do use the full `sys.version_info` tuple.")

---

_Referenced in [astral-sh/ruff#3332](../../astral-sh/ruff/issues/3332.md) on 2023-03-04 04:04_

---

_Referenced in [astral-sh/ruff#3470](../../astral-sh/ruff/pulls/3470.md) on 2023-03-12 19:28_

---

_Comment by @JonathanPlasse on 2023-03-13 18:01_

Should we close this issue?

---

_Comment by @konstin on 2023-03-13 19:35_

yep

---

_Closed by @konstin on 2023-03-13 19:35_

---

_Referenced in [astral-sh/ruff#3487](../../astral-sh/ruff/issues/3487.md) on 2023-03-13 19:42_

---

_Comment by @charliermarsh on 2023-03-13 19:42_

I created https://github.com/charliermarsh/ruff/issues/3487 to track inferring this info at runtime.

---

_Referenced in [scientific-python/cookie#201](../../scientific-python/cookie/issues/201.md) on 2023-07-02 19:22_

---
