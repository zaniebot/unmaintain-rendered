```yaml
number: 21189
title: "[`flake8-bugbear`] Flag mutable defaults passed via named variables, excluding UPPER_CASE (`B006`)"
type: pull_request
state: open
author: danparizher
labels:
  - rule
assignees: []
base: main
head: fix-21142
created_at: 2025-11-01T19:16:11Z
updated_at: 2025-11-29T05:31:09Z
url: https://github.com/astral-sh/ruff/pull/21189
synced_at: 2026-01-12T15:57:18Z
```

# [`flake8-bugbear`] Flag mutable defaults passed via named variables, excluding UPPER_CASE (`B006`)

---

_@danparizher_

## Summary

Fixes #21142

B006 now flags mutable defaults passed via module-level named constants. Previously, it only flagged inline mutable defaults (e.g., `{}`, `[]`, `set()`), missing cases where the default references a module-level constant bound to a mutable object (e.g., `MY_SET = {"ABC", "DEF"}`).

## Problem Analysis

B006’s detection was syntax-local and didn’t resolve identifiers to check mutability. When a default used a name like `MY_SET`, it wasn’t resolved to the bound mutable object, causing false negatives.

**Example:**
```python
MY_SET = {"ABC", "DEF"}  # module-level mutable constant

def func_A(s: set[str] = MY_SET):  # Not flagged (should be flagged)
    return s

def func_B(s: set[str] = {"ABC", "DEF"}):  # Correctly flagged
    return s
```

This is a false negative because `func_A` and `func_B` share the same mutable default object.

## Approach

Extended `is_guaranteed_mutable_expr` (used in preview mode) to handle `Expr::Name`:

1. Resolve the identifier using `semantic.only_binding()` to get the binding.
2. Require an assignment binding (not imports, parameters, etc.).
3. Extract the assigned value via `find_binding_value()`.
4. Recursively check if that value is mutable using `is_guaranteed_mutable_expr()`.

This enables detecting cases like:
- `MY_SET = {"ABC", "DEF"}` → flagged when used as a default
- `MY_LIST = [1, 2, 3]` → flagged when used as a default  
- `MY_DICT = {"key": "value"}` → flagged when used as a default

The fix is enabled only in preview mode. Non-preview mode preserves existing behavior (only flags inline mutable defaults).


---

_Comment by @github-actions[bot] on 2025-11-01 19:27_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ docs/docs/genai/flavors/dspy/notebooks/dspy_quickstart.ipynb:cell 23:1:58: B006 Do not use mutable data structures for argument defaults
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B006 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>





---

_Comment by @BlindChickens on 2025-11-04 13:56_

This brings the rule more in line with the pylint rule it is supposed to represent/replace. `dangerous-default-value (W0102)`
Lets get it merged.

---

_Comment by @MichaReiser on 2025-11-10 13:10_

Hmm, I'm not sure I agree with this change. The `UPPER_CASE` indicates a constant:

> Constants are usually defined on a module level and written in all capital letters with underscores separating words. Examples include MAX_OVERFLOW and TOTAL.
> https://peps.python.org/pep-0008/#constants


I think it makes sense to assume that those variables are read-only by default.

---

_Label `rule` added by @MichaReiser on 2025-11-10 13:13_

---

_@MichaReiser reviewed on 2025-11-10 13:15_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:159 on 2025-11-10 13:15_

This is interesting. That means using `a = imported.MY_LIST` is fine but using `MY_LIST` in the module itself isn't.

---

_Comment by @MichaReiser on 2025-11-10 13:26_

I'm not sure if I agree with this change. Especially because the proposed fix doesn't really help.

`ALL_UPPERCASE` names in Python are considered constants. That's why I think using them in a default value position is fine. The only risk really is that the function body might change the value, which in turn changes the constant. 

```py
A = [1, 2, 3]

def test(a = A): a.append(3) # changes `A`
```

But this is still the case even when applying the proposed fix:

```py
>>> def test(a):
...     if a is None:
...         a = A
...     a.append(4) # changes `A`
...
```

We should at least update the fix to something that actually fixes the underlying problem, e.g. by changing the type annotation to `typing.Sequence` or making a deep copy (but that also seems undesired). Overall, this seems to require more design



---

_@MichaReiser requested changes on 2025-11-10 13:26_

See my other comment

---

_Comment by @danparizher on 2025-11-10 21:27_

Thanks for the feedback, I've updated the implementation to exclude `UPPER_CASE` constants from being flagged,

Since we're no longer flagging that, no fix is suggested for them, which avoids the underlying mutation problem. For inline mutable defaults, the current fix works because it creates a new object per call.

Maybe what we can do is when flagging a constant like `MY_LIST`, we could suggest using `MY_LIST.copy()` or `copy.deepcopy(MY_LIST)` to create a new instance, or suggest changing the type annotation to `Sequence[int]` to indicate immutability. 

We could also analyze the function body to detect if the argument is mutated and suggest appropriate fixes—if it's only read, suggest `Sequence`/`Mapping` types; if it's mutated, suggest copies or immutable alternatives like `tuple` or `frozenset`. 

This would require more design work as you noted, but could be a good follow-up. For now, I think excluding `UPPER_CASE` constants seems like the right pragmatic approach.


---

_Review requested from @MichaReiser by @amyreese on 2025-11-26 01:54_

---

_Review requested from @ntBre by @amyreese on 2025-11-26 01:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:153 on 2025-11-26 08:38_

I think this comment here is inaccurate as it doesn't just resolve module level constants:

* It resolves constants and non constants
* It resolves any name in the current scope

We should add at least one test demonstrating the behavior with a nested function that reads a value from the outer scope.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:159 on 2025-11-26 08:39_

Can you explain the motivation for this restriction?

We should also add tests that demonstrate this behavior

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B006_10.py`:6 on 2025-11-26 08:40_

The comment here is a bit misleading. PEP8 doesn't specify that this rule should exclude constants. Instead, PEP8 specifies that UPPER_CASE variables are considered constants.

---

_@MichaReiser reviewed on 2025-11-26 08:40_

I've a few more inline comments. Could you also update the PR title and summary to reflect your most recent changes?

---

_Comment by @danparizher on 2025-11-29 05:07_

The assignment restriction ensures we only flag cases where mutable objects are explicitly assigned at definition time (which are then shared across calls), avoiding false positives from imports (external dependencies) and function parameters (runtime values evaluated per call).

---

_Renamed from "[`flake8-bugbear`] Flag mutable defaults passed via named constants (`B006`)" to "[`flake8-bugbear`] Flag mutable defaults passed via named variables, excluding UPPER_CASE (`B006`)" by @danparizher on 2025-11-29 05:07_

---

_Review requested from @MichaReiser by @danparizher on 2025-11-29 05:31_

---
