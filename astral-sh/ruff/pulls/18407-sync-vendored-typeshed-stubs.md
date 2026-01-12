```yaml
number: 18407
title: Sync vendored typeshed stubs
type: pull_request
state: merged
author: github-actions
labels:
  - internal
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-06-01T00:38:42Z
updated_at: 2025-06-01T14:36:49Z
url: https://github.com/astral-sh/ruff/pull/18407
synced_at: 2026-01-12T15:56:18Z
```

# Sync vendored typeshed stubs

---

_@github-actions_

Close and reopen this PR to trigger CI

---

_Label `internal` added by @github-actions[bot] on 2025-06-01 00:38_

---

_Review requested from @carljm by @github-actions[bot] on 2025-06-01 00:38_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-06-01 00:38_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-06-01 00:38_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-06-01 00:38_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-06-01 00:38_

---

_Closed by @AlexWaygood on 2025-06-01 06:32_

---

_Reopened by @AlexWaygood on 2025-06-01 06:32_

---

_Comment by @github-actions[bot] on 2025-06-01 06:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
+ error[unknown-argument] src/_pytest/assertion/rewrite.py:735:55: Argument `lineno` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] src/_pytest/assertion/rewrite.py:735:70: Argument `col_offset` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] src/_pytest/assertion/rewrite.py:739:21: Argument `lineno` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] src/_pytest/assertion/rewrite.py:740:21: Argument `col_offset` does not match any known parameter of bound method `__init__`
- Found 882 diagnostics
+ Found 886 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ error[unknown-argument] ddtrace/appsec/_iast/_ast/visitor.py:424:21: Argument `lineno` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] ddtrace/appsec/_iast/_ast/visitor.py:425:21: Argument `col_offset` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] ddtrace/appsec/_iast/_ast/visitor.py:439:21: Argument `lineno` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] ddtrace/appsec/_iast/_ast/visitor.py:440:21: Argument `col_offset` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] ddtrace/appsec/_iast/_ast/visitor.py:454:21: Argument `lineno` does not match any known parameter of bound method `__init__`
+ error[unknown-argument] ddtrace/appsec/_iast/_ast/visitor.py:455:21: Argument `col_offset` does not match any known parameter of bound method `__init__`
- Found 6854 diagnostics
+ Found 6860 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-06-01 14:21_

The primer hits are from https://github.com/python/typeshed/pull/14103. It's interesting that there was no primer fallout in the typeshed PR there. I think this is because typeshed just runs primer with mypy on Python 3.13, so it didn't see any effect from a change that only effects `sys.version_info < (3, 10)` branches. But in our primer runs on this repo, we use whatever the project has in their pyproject.toml for the configured Python version, and pytest [says](https://github.com/pytest-dev/pytest/blob/261e7f15721a02cdc7361c60183b86fa36d0f72f/pyproject.toml#L26) they support 3.9+ -- so we assumed Python 3.9 when type-checking pytest in the mypy_primer run.

The change to the attribute annotations in that typeshed PR seem definitely correct, since `ast.alias` objects created by the interpreter won't ever have those attributes, so you cannot guarantee that any arbitrary `ast.alias` node will have those attributes on Python <3.10. But it's the changes to `__init__` that are causing the primer hits here. I'm not totally convinced that those changes are useful, since `ast.alias.__init__` doesn't actually raise an exception if you pass those keywords when you're manually constructing `ast.alias` nodes on Python <3.10 (it even sets the attributes in the way you'd expect), and the change makes it harder to write code that type-checks on multiple Python versions. 

```pycon
~/dev % uv run -p3.9 --no-project python
Python 3.9.6 (default, Mar 12 2025, 20:22:46) 
[Clang 17.0.0 (clang-1700.0.13.3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import ast
>>> ast.alias("foo", col_offset=39)
<ast.alias object at 0x10429deb0>
>>> _.col_offset
39
```

Cc. @JelleZijlstra -- curious for your thoughts on this!

---

_Merged by @AlexWaygood on 2025-06-01 14:21_

---

_Closed by @AlexWaygood on 2025-06-01 14:21_

---

_Branch deleted on 2025-06-01 14:21_

---

_Comment by @JelleZijlstra on 2025-06-01 14:36_

I think it would be confusing if we allowed those attributes to be passed to `__init__` but not accessed as attributes.

---
