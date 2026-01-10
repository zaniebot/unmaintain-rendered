```yaml
number: 9952
title: "[`flake8-pyi`] Ignore 'unused' private type dicts in class scopes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/private
created_at: 2024-02-12T15:09:24Z
updated_at: 2024-02-12T17:11:08Z
url: https://github.com/astral-sh/ruff/pull/9952
synced_at: 2026-01-10T22:57:09Z
```

# [`flake8-pyi`] Ignore 'unused' private type dicts in class scopes

---

_Pull request opened by @charliermarsh on 2024-02-12 15:09_

## Summary

If these are defined within class scopes, they're actually attributes of the class, and can be accessed through the class itself.

(We preserve our existing behavior for `.pyi` files.)

Closes https://github.com/astral-sh/ruff/issues/9948.


---

_Review requested from @AlexWaygood by @charliermarsh on 2024-02-12 15:09_

---

_Marked ready for review by @charliermarsh on 2024-02-12 15:09_

---

_Label `rule` added by @charliermarsh on 2024-02-12 15:09_

---

_Renamed from "Ignore 'unused' private type dicts in class scopes" to "[`flake8-pyi`] Ignore 'unused' private type dicts in class scopes" by @charliermarsh on 2024-02-12 15:09_

---

_Comment by @github-actions[bot] on 2024-02-12 15:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-02-12 15:27_

I think maybe this rule should work differently for `.py` and `.pyi` files.

For a `.py` file, it makes sense not to flag something like this since, as you say, it's an attribute and maybe it's used elsewhere:

```py
class Foo:
    class _Unused(TypeDict):
        pass
```

But in a `.pyi` file, I'd definitely still want that flagged. (I'm not sure why you'd do something like that in a `.pyi` file... but I still feel like the principle is important 😄)

In a `.pyi` file, it's still highly likely that the `TypedDict` class there doesn't exist at runtime, even though it's inside the class scope. One of the heuristics these rules rely on (which in general is nearly always accurate) is the idea that TypedDicts, protocols, type aliases and protocols in stub files are nearly always "stub-only constructs" that don't exist at runtime, and are used purely for improving the annotations that we're able to supply in the stubs. I think that heuristic applies just as well to private definitions inside class scopes as it does to private definitions in the global scope.

---

_Comment by @charliermarsh on 2024-02-12 15:30_

Ok, I'll gate it by type. Makes sense.

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-02-12 15:44_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI049.pyi`:46 on 2024-02-12 15:50_

Nit: maybe get rid of the `print()` call in the `.pyi` fixture here; I'm not sure it adds anything

```suggestion
# Error
# (in .pyi files, we flag unused definitions in class scopes
# as well as the global scope, unlike in .py files)
class _CustomClass3:
    class _UnusedTypeDict4(TypedDict):
        pass
```

---

_Comment by @charliermarsh on 2024-02-12 15:57_

Fixed.

---

_@AlexWaygood approved on 2024-02-12 15:57_

LGTM.

I think there would still be a false positive if you did something like this in a `.pyi` file:

```py
class Foo:
    class _VeryPrivate(TypedDict):
        x: str

    bar: _VeryPrivate
```

But it seems pretty unlikely to actually come up. And I think this PR is probably an improvement as-is since it gets rid of the `.py`-file false positive (and I think it's probably correct to not worry about any of these rules inside class scopes in `.py` files). So I'm happy with this to go in as-is.

---

_Comment by @AlexWaygood on 2024-02-12 16:07_

> I think there would still be a false positive if you did something like this in a `.pyi` file:
> 
> ```python
> class Foo:
>     class _VeryPrivate(TypedDict):
>         x: str
> 
>     bar: _VeryPrivate
> ```

Hmm, no, it looks like that isn't flagged as being unused -- sorry, I should have checked. I think I might have slightly misunderstood the root cause of this issue here, in that case

---

_Comment by @AlexWaygood on 2024-02-12 16:10_

Ah, is the cause that the visitor recurses into class scopes when looking for references that might count as "usages", but does not recurse into method definitions _inside_ class scopes?

In that case, we should definitely be fine, since the bodies of methods in `.pyi` files _should_ always be empty except for `...`

---

_Comment by @charliermarsh on 2024-02-12 16:54_

The root cause is that given:

```python
class A:
    class _B(TypedDict):
        pass

    def f(self) -> None:
        print(A._B())
```

Our semantic model doesn't currently detect that `A._B()` is a usage of `_B`, since we don't follow attribute references.

But given:

```python
class Foo:
    class _VeryPrivate(TypedDict):
        x: str

    bar: _VeryPrivate
```

We _can_ resolve `_VeryPrivate` to the inner class correctly.


---

_Comment by @AlexWaygood on 2024-02-12 16:59_

Makes sense. I think this should be fine, then, since accessing private attributes in general is very rare in `.pyi` files, so the false-positive as it was reported in the issue is very unlikely to occur for `.pyi` files.

Sorry for the slightly confused review!

---

_Merged by @charliermarsh on 2024-02-12 17:06_

---

_Closed by @charliermarsh on 2024-02-12 17:06_

---

_Branch deleted on 2024-02-12 17:06_

---
