```yaml
number: 10434
title: Add remaining Python type annotations to build backend
type: pull_request
state: merged
author: cthoyt
labels:
  - preview
assignees: []
merged: true
base: main
head: build-backend-type-annotations
created_at: 2025-01-09T15:50:27Z
updated_at: 2025-01-13T00:28:14Z
url: https://github.com/astral-sh/uv/pull/10434
synced_at: 2026-01-10T11:44:48Z
```

# Add remaining Python type annotations to build backend

---

_Pull request opened by @cthoyt on 2025-01-09 15:50_

## Summary

This PR add remaining Python type annotations to build backend, specifically in `python/uv/_build_backend.py`.

## Test Plan

- [x] Running `uvx ruff check --select ANN python/` checks that annotations are available everywhere.<br><br>Adding this into `ruff.toml` would cause all of the scripts to get checked, which I could update if requested, but currently is out of scope for this PR.
- [x] Running `uvx mypy --strict python/` checks that there are no typing issues


---

_Review requested from @konstin by @zanieb on 2025-01-09 17:39_

---

_Label `preview` added by @zanieb on 2025-01-09 17:39_

---

_Comment by @konstin on 2025-01-09 18:24_

Thanks!

Given that PEP 517 explicitly marks `config_settings` as escape hatch, with a SHOULD on a string-string multi-dict input, these dicts are either `dict[Any, str | list[str]]` or `dict[Any, Any]`. Since we don't read them, i'd go with `dict[Any, Any]` for simplicity.

---

_Comment by @cthoyt on 2025-01-09 18:59_

@konstin the only issue with `dict[Any, Any]` is that it requires globally importing (part of) the typing module, even if the type annotation itself is hiding in a string. 

Given uv is trying to be as performant as possible, this could be an issue. What do you think?

---

_Comment by @notatallshaw on 2025-01-09 19:38_

> Given uv is trying to be as performant as possible, this could be an issue. What do you think?

If your goal is to avoid importing typing at runtime I beleive this is accepted by all type checkers:

```
TYPE_CHECKING = False
if TYPE_CHECKING:
    from typing import Any

def foo() -> "dict[Any, Any]": ...
```


---

_Comment by @cthoyt on 2025-01-09 22:01_

Thanks @notatallshaw, I gave that a try and it seemed to work! The results are in 449fa4472.

---

_@konstin approved on 2025-01-10 12:27_

---

_Merged by @konstin on 2025-01-10 12:27_

---

_Closed by @konstin on 2025-01-10 12:27_

---

_Branch deleted on 2025-01-10 12:36_

---

_Comment by @jorenham on 2025-01-10 21:44_

Note that `dict[Any, Any]` doesn't accept `TypedDict` instances, because typeshed annotates it as a subtype of `Mapping[str, object]` (contradicting its runtime implementation): https://github.com/python/typeshed/blob/f26ad2059ef33e7aa55a15bf861f594e441de03b/stdlib/typing.pyi#L944

So in case `TypedDict` instances should also be allowed here, you could use something like `Mapping[Any, object]` instead.

---

_Comment by @cthoyt on 2025-01-13 00:28_

thanks for the suggestion @jorenham, I've made a PR for this in #10549 

---
