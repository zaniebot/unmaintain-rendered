```yaml
number: 17143
title: "[red-knot] Fix `_NotImplementedType` check for Python >=3.10"
type: pull_request
state: merged
author: cake-monotone
labels:
  - ty
assignees: []
merged: true
base: main
head: cake/fix_notimplemented_over_310
created_at: 2025-04-02T08:06:10Z
updated_at: 2025-04-02T10:03:00Z
url: https://github.com/astral-sh/ruff/pull/17143
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Fix `_NotImplementedType` check for Python >=3.10

---

_Pull request opened by @cake-monotone on 2025-04-02 08:06_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

from https://github.com/astral-sh/ruff/pull/17034#discussion_r2024222525

This is a simple PR to fix the invalid behavior of `NotImplemented` on Python >=3.10.


## Test Plan

I think it would be better if we could run mdtest across multiple Python versions in GitHub Actions.

<!-- How was it tested? -->


---

_Review requested from @carljm by @cake-monotone on 2025-04-02 08:06_

---

_Review requested from @AlexWaygood by @cake-monotone on 2025-04-02 08:06_

---

_Review requested from @sharkdp by @cake-monotone on 2025-04-02 08:06_

---

_Review requested from @dcreager by @cake-monotone on 2025-04-02 08:06_

---

_Comment by @github-actions[bot] on 2025-04-02 08:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:356:24: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:358:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:806:20: Object of type `_NotImplementedType` is not assignable to return type `SpecifierSet`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:846:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/markers.py:297:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/requirements.py:83:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:77:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:80:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:86:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:92:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:98:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:104:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:110:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- Found 110 diagnostics
+ Found 97 diagnostics

arrow (https://github.com/arrow-py/arrow)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1721:16: Object of type `_NotImplementedType` is not assignable to return type `Arrow`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1744:16: Object of type `_NotImplementedType` is not assignable to return type `timedelta | Arrow`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1750:16: Object of type `_NotImplementedType` is not assignable to return type `timedelta`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1768:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1774:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1780:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1786:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- Found 47 diagnostics
+ Found 40 diagnostics

black (https://github.com/psf/black)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:83:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- Found 191 diagnostics
+ Found 190 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/style.py:424:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/style.py:429:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/text.py:184:16: Object of type `_NotImplementedType` is not assignable to return type `Text`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/text.py:188:20: Object of type `_NotImplementedType` is not assignable to return type `bool`
- Found 411 diagnostics
+ Found 407 diagnostics

```
</details>


---

_Label `red-knot` added by @MichaReiser on 2025-04-02 08:09_

---

_Comment by @sharkdp on 2025-04-02 08:32_

> I think it would be better if we could run mdtest across multiple Python versions in GitHub Actions.

Well you can specify the version explicitly. And for some features where we know that they are version-dependent, we have tests for multiple versions. But it might make sense to think about running all tests for a set of versions, yes. Tests that specify an explicit version would still use that, of course.

---

_@sharkdp approved on 2025-04-02 08:34_

Thank you. It would be great if we could add a regression test with a `toml` configuration block and a target version of 3.10.

---

_@sharkdp reviewed on 2025-04-02 09:59_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/function/return_type.md`:275 on 2025-04-02 09:59_

Maybe
```suggestion
### Default Python version
```

---

_Merged by @sharkdp on 2025-04-02 10:03_

---

_Closed by @sharkdp on 2025-04-02 10:03_

---
