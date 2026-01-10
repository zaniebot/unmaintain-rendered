```yaml
number: 19080
title: "[`flake8-pyi`] Make example error out-of-the-box (`PYI059`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-07-01T22:48:37Z
updated_at: 2025-07-02T15:56:45Z
url: https://github.com/astral-sh/ruff/pull/19080
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-pyi`] Make example error out-of-the-box (`PYI059`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-01 22:48_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [generic-not-last-base-class (PYI059)](https://docs.astral.sh/ruff/rules/generic-not-last-base-class/#generic-not-last-base-class-pyi059)'s example error out-of-the-box

> [!WARNING]
> This PR has more extensive and possibly wrong changes than my usual "Make example error out-of-the-box" PRs
>
> I made fairly lengthy modifications so that the example is valid code (adding the `collections.abc` imports and `TypeVar`s), but all `PYI059` actually needs to trigger is a `from typing import Generic`. I'm not sure if this is right choice, since I'm uncertain if the examples being working code is favored vs verbosity.
>
> I also changed the `Tuple` to `tuple` since in modern python that would require importing the deprecated alias, but it's also not nessacary for `PYI059` to trigger.


[Old example](https://play.ruff.rs/e3f75bc9-8dc4-4421-a2b8-ff0e5bb1fd8e)
```py
class LinkedList(Generic[T], Sized):
    def push(self, item: T) -> None:
        self._items.append(item)

class MyMapping(
    Generic[K, V],
    Iterable[Tuple[K, V]],
    Container[Tuple[K, V]],
):
    ...
```

[New example](https://play.ruff.rs/5cb546a0-5fb6-4d5a-a94d-f78f5b3e96d2)
```py
from collections.abc import Container, Iterable, Sized
from typing import Generic, TypeVar


T = TypeVar("T")
K = TypeVar("K")
V = TypeVar("V")


class LinkedList(Generic[T], Sized):
    def push(self, item: T) -> None:
        self._items.append(item)


class MyMapping(
    Generic[K, V],
    Iterable[tuple[K, V]],
    Container[tuple[K, V]],
):
    ...
```

The "use instead" section was also modified similarly

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Review requested from @AlexWaygood by @MeGaGiGaGon on 2025-07-01 22:48_

---

_Comment by @github-actions[bot] on 2025-07-01 22:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @AlexWaygood on 2025-07-02 07:22_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:37 on 2025-07-02 10:30_

Stylistic nit: let's put all the TypeVar declarations together at the top

```suggestion
/// T = TypeVar("T")
/// K = TypeVar("K")
/// V = TypeVar("V")
///
///
/// class LinkedList(Generic[T], Sized):
///     def push(self, item: T) -> None:
///         self._items.append(item)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/generic_not_last_base_class.rs`:63 on 2025-07-02 10:30_

```suggestion
/// T = TypeVar("T")
/// K = TypeVar("K")
/// V = TypeVar("V")
///
///
/// class LinkedList(Sized, Generic[T]):
///     def push(self, item: T) -> None:
///         self._items.append(item)
```

---

_@AlexWaygood reviewed on 2025-07-02 10:30_

Thanks, this makes sense to me!

---

_@AlexWaygood approved on 2025-07-02 15:49_

---

_Merged by @AlexWaygood on 2025-07-02 15:49_

---

_Closed by @AlexWaygood on 2025-07-02 15:49_

---

_Branch deleted on 2025-07-02 15:50_

---
