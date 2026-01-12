```yaml
number: 10637
title: "[`pylint`] Don't recommend decorating staticmethods with `@singledispatch` (`PLE1519`, `PLE1520`)"
type: pull_request
state: merged
author: alex-700
labels:
  - bug
assignees: []
merged: true
base: main
head: latyshev/PLE1520-fix
created_at: 2024-03-27T20:05:39Z
updated_at: 2025-06-10T20:47:15Z
url: https://github.com/astral-sh/ruff/pull/10637
synced_at: 2026-01-12T15:55:32Z
```

# [`pylint`] Don't recommend decorating staticmethods with `@singledispatch` (`PLE1519`, `PLE1520`)

---

_@alex-700_

…andling

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes #10619 

The behaviour, introduced in this PR, is different from original `pylint`'s one, but this one is more natural.

According to the docs of [staticmethod](https://docs.python.org/3/library/functions.html#staticmethod), the method marked with `@staticmethod` can be called via class (`C.foo()`) and via the instance (`C().foo()` (UFCS: `C.foo(C())`). Marking such method with `@singledispatch` allows only call via class, but not via the instance. Marking such method with `@singledispatchmethod` supports both `C.foo()` and `C().foo()`. The latter also is supported by [`singledispatchmethod` docs](https://docs.python.org/3/library/functools.html#functools.singledispatchmethod) via 
> @singledispatchmethod supports nesting with other decorators such as [@classmethod](https://docs.python.org/3/library/functions.html#classmethod). 
> ... 
> The same pattern can be used for other similar decorators: [@staticmethod](https://docs.python.org/3/library/functions.html#staticmethod), [@abstractmethod](https://docs.python.org/3/library/abc.html#abc.abstractmethod), and others.

<details><summary>Example</summary>
<p>

```python
class C:
    @singledispatchmethod
    @staticmethod
    def foo(x): raise NotImplementedError()

    @foo.register
    @staticmethod
    def _(x: int) -> int: return x


print(C.foo(1))
print(C().foo(1))
```
prints
```
1
1
```
but 
```python
class C:
    @singledispatch
    @staticmethod
    def foo(x): raise NotImplementedError()

    @foo.register
    @staticmethod
    def _(x: int) -> int: return x


print(C.foo(1))
print(C().foo(1))
```
fails with
```
C.foo() takes 1 positional argument but 2 were given
```
</p>
</details> 



<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
cargo test

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-03-27 20:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @MichaReiser on 2024-03-27 20:45_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-04-02 16:31_

---

_@AlexWaygood approved on 2024-04-02 16:39_

Thanks @alex-700!

---

_Renamed from "[`pylint`] Fix `singledispatchmethod[_function]` in `@staticmethod` h…" to "[`pylint`] Don't recommend decorating staticmethods with `@singledispatch` (`PLE1519`, `PLE1520`)" by @AlexWaygood on 2024-04-02 16:40_

---

_Merged by @AlexWaygood on 2024-04-02 16:47_

---

_Closed by @AlexWaygood on 2024-04-02 16:47_

---

_Label `bug` added by @dhruvmanila on 2024-04-11 14:53_

---

_Branch deleted on 2025-06-10 20:47_

---
