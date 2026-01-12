```yaml
number: 10808
title: "[`flake8-slots`] Flag subclasses of call-based `typing.NamedTuple`s as well as subclasses of `collections.namedtuple()` (`SLOT002`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: typing-namedtuple-slots
created_at: 2024-04-06T22:06:01Z
updated_at: 2024-04-06T23:16:08Z
url: https://github.com/astral-sh/ruff/pull/10808
synced_at: 2026-01-12T15:55:33Z
```

# [`flake8-slots`] Flag subclasses of call-based `typing.NamedTuple`s as well as subclasses of `collections.namedtuple()` (`SLOT002`)

---

_@AlexWaygood_

## Summary

Fixes #10805.

While `__slots__` are automatically added to this class by the `typing` module...

```py
class Foo(typing.NamedTuple):
    x: int
```

...they are not automatically added to this class...

```py
class Foo(typing.NamedTuple("foo", [("x", int)])):
    pass
```

...so it seems reasonable for `SLOT002` to emit a lint on that class just the same as it does for this class:

```py
class Foo(collections.namedtuple("foo", ["x"])):
    pass
```

## Test Plan

`cargo test`


---

_Label `rule` added by @AlexWaygood on 2024-04-06 22:06_

---

_Comment by @github-actions[bot] on 2024-04-06 22:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-04-06 23:12_

---

_Merged by @AlexWaygood on 2024-04-06 23:16_

---

_Closed by @AlexWaygood on 2024-04-06 23:16_

---

_Branch deleted on 2024-04-06 23:16_

---
