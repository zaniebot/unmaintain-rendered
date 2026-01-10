```yaml
number: 100
title: "`dict` function-based initialization is rejected"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - overloads
assignees: []
created_at: 2025-05-03T14:34:55Z
updated_at: 2025-05-19T15:45:42Z
url: https://github.com/astral-sh/ty/issues/100
synced_at: 2026-01-10T02:34:09Z
```

# `dict` function-based initialization is rejected

---

_Issue opened by @charliermarsh on 2025-05-03 14:34_

The following gives: No overload of bound method `__init__` matches arguments

```python
SESSION_OPTIONS: dict[str, Any] = dict(
    # Allow models to be read after transactions are closed
    expire_on_commit=False,
    # Do not allow implicit re-use of sessions after their context closes
    close_resets_only=False,
)
```

See: https://types.ruff.rs/30b74134-d2d1-42d5-8fa5-e22768dad640


---

_Assigned to @dhruvmanila by @AlexWaygood on 2025-05-03 14:42_

---

_Label `bug` added by @AlexWaygood on 2025-05-03 14:42_

---

_Comment by @AlexWaygood on 2025-05-03 14:42_

(Not sure if this is a known issue or not -- @dhruvmanila?)

---

_Comment by @dhruvmanila on 2025-05-03 14:49_

I think this requires support for variadic and keyword-variadic arguments i.e., resolving the following TODO:

https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/src/types/call/bind.rs#L1135-L1138

---

_Comment by @dhruvmanila on 2025-05-03 15:00_

Hmm, maybe not. Let me look at it more closely when I'm at my desk.

---

_Label `bug` added by @MichaReiser on 2025-05-07 15:17_

---

_Label `overloads` added by @AlexWaygood on 2025-05-10 18:02_

---

_Comment by @dhruvmanila on 2025-05-17 18:35_

I think this is the same issue as https://github.com/astral-sh/ty/issues/370.

Here are the overloads for `dict`:

```
info: Possible overloads for bound method `__init__`:
info:   (self) -> None
info:   (self: dict[str, _VT], **kwargs: _VT) -> None
info:   (self, map: SupportsKeysAndGetItem[_KT, _VT], /) -> None
info:   (self: dict[str, _VT], map: SupportsKeysAndGetItem[str, _VT], /, **kwargs: _VT) -> None
info:   (self, iterable: Iterable[tuple[_KT, _VT]], /) -> None
info:   (self: dict[str, _VT], iterable: Iterable[tuple[str, _VT]], /, **kwargs: _VT) -> None
info:   (self: dict[str, str], iterable: Iterable[list[str]], /) -> None
info:   (self: dict[bytes, bytes], iterable: Iterable[list[bytes]], /) -> None
```

Out of which, this is the overload that should match given the parameters above:

```
info:   (self: dict[str, _VT], **kwargs: _VT) -> None
```

which has `self` annotated which is causing an issue in the manner similar to the linked issue.

---

_Unassigned @dhruvmanila by @AlexWaygood on 2025-05-17 18:36_

---

_Comment by @MichaReiser on 2025-05-17 18:39_

So cool that our diagnostics now surface enough information to debug issues :)

---

_Closed by @dcreager on 2025-05-19 15:45_

---

_Closed by @dcreager on 2025-05-19 15:45_

---
