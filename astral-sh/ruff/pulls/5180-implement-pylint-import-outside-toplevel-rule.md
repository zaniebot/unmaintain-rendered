```yaml
number: 5180
title: Implement pylint import-outside-toplevel rule (C0415)
type: pull_request
state: merged
author: ashanbrown
labels:
  - rule
assignees: []
merged: true
base: main
head: import-outside-toplevel
created_at: 2023-06-19T14:17:47Z
updated_at: 2023-10-29T16:50:23Z
url: https://github.com/astral-sh/ruff/pull/5180
synced_at: 2026-01-12T15:55:17Z
```

# Implement pylint import-outside-toplevel rule (C0415)

---

_@ashanbrown_

## Summary

Implements pylint C0415 (import-outside-toplevel) — imports should be at the top level of a file.

The great debate I had on this implementation is whether "top-level" is one word or two (`toplevel` or `top_level`).  I opted for 2 because that seemed to be how it is used in the codebase but the rule string itself uses one-word "toplevel."  🤷   I'd be happy to change it as desired.

I suppose this could be auto-fixed by moving the import to the top-level, but it seems likely that the author's intent was to actually import this dynamically, so I view the main point of this rule is to force some sort of explanation, and auto-fixing might be annoying.

For reference, this is what "pylint" reports:
```
> pylint crates/ruff/resources/test/fixtures/pylint/import_outside_top_level.py
************* Module import_outside_top_level
...
crates/ruff/resources/test/fixtures/pylint/import_outside_top_level.py:4:4: C0415: Import outside toplevel (string) (import-outside-toplevel)
```

ruff would now report:

```
import_outside_top_level.py:4:5: PLC0415 `import` should be used only at the top level of a file
  |
3 | def import_outside_top_level():
4 |     import string # [import-outside-toplevel]
  |     ^^^^^^^^^^^^^ PLC0415
  |
```

Contributes to https://github.com/astral-sh/ruff/issues/970.

## Test Plan

<!-- How was it tested? -->
Snapshot test.


---

_Renamed from "Add pylint import-outside-toplevel rule" to "Implement pylint import-outside-toplevel rule (C0415)" by @ashanbrown on 2023-06-19 14:25_

---

_Comment by @github-actions[bot] on 2023-06-19 14:37_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected linter changes**. (+10 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/a013e976fd650a78aa7786e69eb0e5e6a1e4e859/rotkehlchen/assets/resolver.py#L127'>rotkehlchen/assets/resolver.py:127:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/rotki/rotki/blob/a013e976fd650a78aa7786e69eb0e5e6a1e4e859/rotkehlchen/assets/resolver.py#L128'>rotkehlchen/assets/resolver.py:128:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/rotki/rotki/blob/a013e976fd650a78aa7786e69eb0e5e6a1e4e859/rotkehlchen/assets/resolver.py#L158'>rotkehlchen/assets/resolver.py:158:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/rotki/rotki/blob/a013e976fd650a78aa7786e69eb0e5e6a1e4e859/rotkehlchen/assets/resolver.py#L159'>rotkehlchen/assets/resolver.py:159:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/rotki/rotki/blob/a013e976fd650a78aa7786e69eb0e5e6a1e4e859/rotkehlchen/assets/resolver.py#L70'>rotkehlchen/assets/resolver.py:70:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/rotki/rotki/blob/a013e976fd650a78aa7786e69eb0e5e6a1e4e859/rotkehlchen/assets/resolver.py#L71'>rotkehlchen/assets/resolver.py:71:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/rotki/rotki/blob/a013e976fd650a78aa7786e69eb0e5e6a1e4e859/rotkehlchen/assets/resolver.py#L96'>rotkehlchen/assets/resolver.py:96:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/rotki/rotki/blob/a013e976fd650a78aa7786e69eb0e5e6a1e4e859/rotkehlchen/assets/resolver.py#L97'>rotkehlchen/assets/resolver.py:97:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/rotki/rotki/blob/a013e976fd650a78aa7786e69eb0e5e6a1e4e859/rotkehlchen/tests/conftest.py#L133'>rotkehlchen/tests/conftest.py:133:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/rotki/rotki/blob/a013e976fd650a78aa7786e69eb0e5e6a1e4e859/tools/pyinstaller_hooks/pre_find_module_path/hook-distutils.py#L40'>tools/pyinstaller_hooks/pre_find_module_path/hook-distutils.py:40:5:</a> PLC0415 `import` should be at the top-level of a file
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

</p>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0415 | 10 | 10 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/import_outside_top_level.rs`:30 on 2023-06-19 16:11_

Will this rule need special handling to allow typing only imports?

---

_@MichaReiser reviewed on 2023-06-19 16:12_

---

_@ashanbrown reviewed on 2023-06-19 16:51_

---

_Review comment by @ashanbrown on `crates/ruff/src/rules/pylint/rules/import_outside_top_level.rs`:30 on 2023-06-19 16:51_

I imagine so.  I'll revert this to draft while I sort this out.

---

_Converted to draft by @ashanbrown on 2023-06-19 16:51_

---

_Marked ready for review by @ashanbrown on 2023-06-19 19:04_

---

_@ashanbrown reviewed on 2023-06-19 19:05_

---

_Review comment by @ashanbrown on `crates/ruff/src/rules/pylint/rules/import_outside_top_level.rs`:30 on 2023-06-19 19:05_

@MichaReiser I've excluded anything in a type-checking block.  Ideally, we might be able to detect "top-level type-checking blocks" and treat those differently, but I suspect it isn't that important as type-checking blocks are generally at the top-level.

---

_Review requested from @charliermarsh by @MichaReiser on 2023-06-19 20:34_

---

_@charliermarsh reviewed on 2023-06-20 03:07_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/import_outside_top_level.rs`:39 on 2023-06-20 03:07_

As far as I can tell, Pylint only flags this for imports outside of the module scope:

```python
if True:
  import sys  # Pylint doesn't flag this.

def foo():
  import sys  # It does flag this!
```

I think this should perhaps be `!checker.scope().kind.is_module()`?


---

_Comment by @ashanbrown on 2023-06-20 05:28_

@charliermarsh I've gone with your suggestion of just checking the semantic scope kind.  I had misinterpreted one of the tests from pylint and it does look like they considered nested conditionals to be at the top-level (and that's why pylint needs no special handling for type-checking).

I've also updated the documentation for this check as I don't think [PEP8](https://peps.python.org/pep-0008/#imports) completely offers a rationale for this rule.  The [pylint PR](https://github.com/pylint-dev/pylint/pull/3079) for this rule also doesn't provide a lot of explanation, so I've done my best to offer a more complete rationale for this check, but I'd welcome suggestions.

---

_Comment by @ashanbrown on 2023-06-20 05:45_

For the record, this is why ChatGPT thinks we should place our imports at the top-level of a module:

> why should I place my imports at the top-level of a module in python?
ChatGPT
In Python, it is generally recommended to place your imports at the top-level of a module. Here are some reasons why this is a good practice:
>
> Readability: Placing imports at the top of your module makes it easier for other developers (including yourself) to understand and navigate your code. When imports are at the top, it provides a clear and concise view of the dependencies required by your module.
>
> Avoiding Confusion: Placing imports at the top-level helps prevent potential conflicts and confusion. It allows you to easily see which external modules or libraries your code relies on, reducing the chances of accidentally redefining or overriding names used in other parts of your code.
>
>Consistency: Placing imports at the top-level is a widely followed convention in the Python community. By adhering to this convention, you make your code more familiar and consistent with other Python projects, which can facilitate collaboration and code maintenance.
>
>Dependency Analysis: When imports are located at the top, it becomes easier to analyze the dependencies of a module. Tools like static analyzers or linters can quickly scan the imports and identify any potential issues or unused imports.
>
>Performance: Import statements at the top-level are executed only once when the module is first imported, and subsequent imports of the same module do not execute those statements again. Placing imports within functions or conditional blocks may lead to repeated imports, which can impact performance.
>
>However, it's worth noting that there are a few exceptions or alternative approaches to consider. For instance, if an import is only needed within a specific function or block of code, it can be placed there to improve the performance of the module as a whole. Additionally, there may be cases where conditional imports are required based on runtime conditions. But as a general guideline, placing imports at the top-level is considered a good practice in Python.

---

_@MichaReiser approved on 2023-06-21 05:32_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-25 22:42_

---

_Comment by @sbrugman on 2023-07-17 15:06_

The autofix for this could be https://github.com/astral-sh/ruff/issues/1251

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-27 13:56_

---

_Label `rule` added by @charliermarsh on 2023-10-29 16:36_

---

_Merged by @charliermarsh on 2023-10-29 16:40_

---

_Closed by @charliermarsh on 2023-10-29 16:40_

---
