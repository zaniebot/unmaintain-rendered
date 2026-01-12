```yaml
number: 3230
title: "[flake8-pyi]: PYI009, PYI010, PYI021"
type: pull_request
state: merged
author: sbdchd
labels:
  - rule
assignees: []
merged: true
base: main
head: steve-more-flake8-pyi-rules
created_at: 2023-02-26T00:17:13Z
updated_at: 2023-02-26T03:29:04Z
url: https://github.com/astral-sh/ruff/pull/3230
synced_at: 2026-01-12T04:39:44Z
```

# [flake8-pyi]: PYI009, PYI010, PYI021

---

_Pull request opened by @sbdchd on 2023-02-26 00:17_

PYI009 and PYI010 are very similar, always use `...` in function and class bodies in stubs.

PYI021 bans doc strings in stubs.


I think all of these rules should be relatively straightforward to implement auto fixes for but can do that later once we get all the other rules added.

rel: https://github.com/charliermarsh/ruff/issues/848

---

_Renamed from "flake8-pyi: PYI009, PYI010, PYI021" to "[flake8-pyi]: PYI009, PYI010, PYI021" by @sbdchd on 2023-02-26 00:21_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:3354 on 2023-02-26 00:32_

Maybe this should instead be colocated with the other `let definition = docstrings::extraction::extract(...)` calls, on `FunctionDef` and `ClassDef`, to ensure they're "real" docstrings? (Does `flake8-pyi` flag this for _any_ standalone string? Or only actual docstrings?)

---

_@charliermarsh reviewed on 2023-02-26 00:32_

---

_@sbdchd reviewed on 2023-02-26 00:34_

---

_Review comment by @sbdchd on `crates/ruff/src/checkers/ast.rs`:3354 on 2023-02-26 00:34_

oh good point

flake8-pyi flags any standalone string: https://github.com/PyCQA/flake8-pyi/blob/4212bec43dbc4020a59b90e2957c9488575e57ba/pyi.py#L1128-L1132

which probably isn't what we really want

---

_@charliermarsh reviewed on 2023-02-26 00:36_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:3354 on 2023-02-26 00:36_

The other thing you could do, if you want, is put this logic in `check_definitions`, which iterates over all module, function, and class definitions, and you can check `definition.docstring.is_none()`. So you wouldn't need to put this in three places.

---

_@sbdchd reviewed on 2023-02-26 00:46_

---

_Review comment by @sbdchd on `crates/ruff/src/checkers/ast.rs`:3354 on 2023-02-26 00:46_

even better!

---

_Label `rule` added by @charliermarsh on 2023-02-26 00:48_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/ban_doc_strings_in_stubs.rs`:10 on 2023-02-26 00:55_

I may rename these to adhere to our new rule naming convention (or feel free to do so if you're up for it): https://github.com/charliermarsh/ruff/blob/main/CONTRIBUTING.md#rule-naming-convention

Basically this would be: `DocstringInStub` (as in, "Allow docstring in stub").

---

_@charliermarsh reviewed on 2023-02-26 00:55_

---

_@sbdchd reviewed on 2023-02-26 01:39_

---

_Review comment by @sbdchd on `crates/ruff/src/rules/flake8_pyi/rules/ban_doc_strings_in_stubs.rs`:10 on 2023-02-26 01:39_

done!

---

_Merged by @charliermarsh on 2023-02-26 03:29_

---

_Closed by @charliermarsh on 2023-02-26 03:29_

---
