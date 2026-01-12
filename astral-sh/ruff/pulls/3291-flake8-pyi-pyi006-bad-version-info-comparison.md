```yaml
number: 3291
title: flake8-pyi PYI006 bad version info comparison
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: flake8-pyi-pyi006-bad-version-info-comparison
created_at: 2023-03-01T11:18:28Z
updated_at: 2023-03-01T17:59:01Z
url: https://github.com/astral-sh/ruff/pull/3291
synced_at: 2026-01-12T04:39:44Z
```

# flake8-pyi PYI006 bad version info comparison

---

_Pull request opened by @konstin on 2023-03-01 11:18_

This implements [flake8-pyi](https://github.com/PyCQA/flake8-pyi) Y006 as PYI006 to catch bad comparisons with `sys.version_info`. I've also snuck in a change to `generate_mkdocs.py`.

I'm pretty sure this kind of comparison is always a bad, independent of `.py` vs. `.pyi`, but it's active only in `.pyi` matching the other `PYI` checks

Ticks off one box in #848.

---

_Review requested from @charliermarsh by @konstin on 2023-03-01 11:18_

---

_Comment by @sasanjac on 2023-03-01 13:02_

Why not activate it for `.py` files also if we get the check for free?

---

_@charliermarsh reviewed on 2023-03-01 16:14_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:22 on 2023-03-01 16:14_

Nit: can we include here a prose explanation of why this is a problem? And then something like:

```
/// ## Example
/// ```python
/// import sys
///
/// if sys.version_info > (3, 8):
//     ...
/// ```
///
/// Use instead:
/// ```python
/// import sys
///
/// if sys.version_info >= (3, 9):
//     ...
/// ```

---

_@charliermarsh reviewed on 2023-03-01 16:14_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:243 on 2023-03-01 16:14_

Thank you :)

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:51 on 2023-03-01 16:16_

Nit: move this into the `if` body on line 59? Since it's only used in the event that we actually trigger the diagnostic.

---

_@charliermarsh reviewed on 2023-03-01 16:16_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:51 on 2023-03-01 16:33_

done

---

_@konstin reviewed on 2023-03-01 16:33_

---

_@konstin reviewed on 2023-03-01 16:33_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:22 on 2023-03-01 16:33_

good idea, thanks

---

_Comment by @charliermarsh on 2023-03-01 16:54_

@sasanjac - We may turn on the `flake8-pyi` rules for `.py` files, but I'd rather do it all-at-once. For now, this is consistent with `flake8-pyi` and with how the other rules work.


---

_@charliermarsh approved on 2023-03-01 16:59_

---

_@charliermarsh reviewed on 2023-03-01 17:00_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:12 on 2023-03-01 17:00_

I just removed the empty lines here for consistency with the other docs.

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:1 on 2023-03-01 17:45_

is there something other than `cargo fmt` i have to do here? i have "run rustfmt on save" on in pycharm but the precommit also didn't seem to have caught that

---

_@konstin reviewed on 2023-03-01 17:45_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:42 on 2023-03-01 17:46_

oops thanks


---

_@konstin reviewed on 2023-03-01 17:46_

---

_@konstin reviewed on 2023-03-01 17:46_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:12 on 2023-03-01 17:46_

ok i'll switch to that style

---

_@charliermarsh reviewed on 2023-03-01 17:54_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:1 on 2023-03-01 17:54_

This is PyCharm's "Optimize Imports" action. It's kind of a bad habit for me to run + commit it since it's not actually enforced in any automated way, but I often do run it when editing a new file 🙈 

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/bad_version_info_comparison.rs`:1 on 2023-03-01 17:54_

We used to enforce these via `rustfmt` but it depended on nightly Rust so we removed it.

---

_@charliermarsh reviewed on 2023-03-01 17:54_

---

_Merged by @konstin on 2023-03-01 17:58_

---

_Closed by @konstin on 2023-03-01 17:58_

---

_Branch deleted on 2023-03-01 17:59_

---
