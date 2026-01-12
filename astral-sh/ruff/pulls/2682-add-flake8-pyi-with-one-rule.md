```yaml
number: 2682
title: Add flake8-pyi with one rule
type: pull_request
state: merged
author: sbdchd
labels:
  - rule
assignees: []
merged: true
base: main
head: steve-flake8-pyi-init
created_at: 2023-02-09T03:53:04Z
updated_at: 2023-02-10T00:03:12Z
url: https://github.com/astral-sh/ruff/pull/2682
synced_at: 2026-01-12T04:52:00Z
```

# Add flake8-pyi with one rule

---

_Pull request opened by @sbdchd on 2023-02-09 03:53_

Add basic scaffold for [flake8-pyi](https://github.com/PyCQA/flake8-pyi) and the first rule, Y001

rel: https://github.com/charliermarsh/ruff/issues/848

---

_@sbdchd reviewed on 2023-02-09 03:54_

---

_Review comment by @sbdchd on `crates/ruff/src/rules/flake8_pyi/mod.rs`:15 on 2023-02-09 03:54_

I wasn't sure how to have multiple test files, in this case I want to make sure we ignore `.py` files since they don't have the same import/export rules -- although maybe it should apply to all files anyways? not sure when you'd want to export a type var

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/mod.rs`:15 on 2023-02-09 03:58_

Grep for `#[test_case(Rule::InvalidEscapeSequence, Path::new("W605_0.py"))]` as an example -- that's our standard convention.

---

_@charliermarsh reviewed on 2023-02-09 03:58_

---

_@charliermarsh reviewed on 2023-02-09 14:25_

---

_Review comment by @charliermarsh on `README.md`:1149 on 2023-02-09 14:25_

I think we should consider using `PYI` for this instead of `Y`, which is generic enough that it may well conflict with other plugins.

---

_Review requested from @charliermarsh by @sbdchd on 2023-02-09 23:35_

---

_@charliermarsh reviewed on 2023-02-09 23:38_

---

_Review comment by @charliermarsh on `README.md`:1149 on 2023-02-09 23:38_

Now we pray that there's no plugin using `PYI` already.

---

_@charliermarsh reviewed on 2023-02-09 23:39_

---

_Review comment by @charliermarsh on `README.md`:1149 on 2023-02-09 23:39_

I can't find any such references on GitHub Code Search.

---

_Label `rule` added by @charliermarsh on 2023-02-10 00:03_

---

_Merged by @charliermarsh on 2023-02-10 00:03_

---

_Closed by @charliermarsh on 2023-02-10 00:03_

---
