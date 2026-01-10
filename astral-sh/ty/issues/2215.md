---
number: 2215
title: "Improve diagnostics in cases where we infer `object` due to possibility of subclassing"
type: issue
state: closed
author: ooopus
labels:
  - diagnostics
  - narrowing
assignees: []
created_at: 2025-12-25T02:08:54Z
updated_at: 2025-12-26T18:02:55Z
url: https://github.com/astral-sh/ty/issues/2215
synced_at: 2026-01-10T01:52:52Z
---

# Improve diagnostics in cases where we infer `object` due to possibility of subclassing

---

_Issue opened by @ooopus on 2025-12-25 02:08_

### Summary

## Summary

When using the common guard pattern:

```py
if hasattr(obj, "attr") and obj.attr:
    ...
````

ty incorrectly infers `obj.attr` as `~AlwaysFalsy`, producing a false-positive

`error[unresolved-attribute]` when accessing attributes on `obj.attr`.

This code runs correctly at runtime; the issue appears to be in ty's flow-sensitive narrowing.

## Environment

* OS: Windows 11

* Python: 3.13.9

* ty: 0.0.7 (cf82a04b5 2025-12-24)

## Command

```sh
ty check test_ty_always_falsy.py
```

(No special `pyproject.toml` settings; default config.)

## Reproduction

```py
from __future__ import annotations

class Editor:
    def __init__(self) -> None:
        self.note: Note | None = Note()

class Note:
    def __init__(self) -> None:
        self.fields: list[str] = ["field1", "field2"]

class WebView:
    pass

class EditorWebView:
    def __init__(self) -> None:
        self.editor: Editor | None = Editor()

def test(webview: WebView | EditorWebView) -> list[str] | None:
    if hasattr(webview, "editor") and webview.editor:
        reveal_type(webview)
        reveal_type(webview.editor)

        note = webview.editor.note # ty: error[unresolved-attribute]

        if note:
            return note.fields

    return None
```

## Expected Behavior

Inside:

```py
if hasattr(webview, "editor") and webview.editor:
```

`webview.editor` should be narrowed to a truthy `Editor` type, so

`webview.editor.note` should be valid.

## Actual Behavior

ty reports a false positive similar to:

```
error[unresolved-attribute]: Object of type `~AlwaysFalsy` has no attribute `note`
```

## Workarounds

Both of the following avoid the false positive:

* `isinstance()`-based narrowing

* `getattr(webview, "editor", None)` + truthiness check

### Version

ty 0.0.7 (cf82a04b5 2025-12-24)

---

_Label `narrowing` added by @mtshiba on 2025-12-25 03:39_

---

_Comment by @mtshiba on 2025-12-25 03:55_

In fact, I think this is the intended behavior, because `WebView` can be inherited and may have an attribute `editor`, so even if we can see that the parameter `webview` has an attribute `editor`, it doesn't say much about its type.

```python
from __future__ import annotations

class Editor:
    def __init__(self) -> None:
        self.note: Note | None = Note()

class Note:
    def __init__(self) -> None:
        self.fields: list[str] = ["field1", "field2"]

class WebView:
    pass

class WebViewSub(WebView):
    def __init__(self) -> None:
        self.editor = 0  # You can assign any type of object here.

class EditorWebView:
    def __init__(self) -> None:
        self.editor: Editor | None = Editor()

def test(webview: WebView | EditorWebView) -> list[str] | None:
    if hasattr(webview, "editor"):
        reveal_type(webview)  # It may be of type `WebViewSub`
        reveal_type(webview.editor)  # It may be of type `int`!

    return None

test(WebViewSub())
```

If you want to avoid this, you can use `typing.final`, and ty will behave exactly as you intended.

```python
from __future__ import annotations
from typing import final

class Editor:
    def __init__(self) -> None:
        self.note: Note | None = Note()

class Note:
    def __init__(self) -> None:
        self.fields: list[str] = ["field1", "field2"]

@final
class WebView:
    pass

class EditorWebView:
    def __init__(self) -> None:
        self.editor: Editor | None = Editor()

def test(webview: WebView | EditorWebView) -> list[str] | None:
    if hasattr(webview, "editor") and webview.editor:
        reveal_type(webview)
        reveal_type(webview.editor)

        note = webview.editor.note # OK
        reveal_type(note)  # Note | None

        if note:
            return note.fields

    return None
```

---

_Comment by @mtshiba on 2025-12-25 04:03_

However, it's certainly not clear why narrowing failed, and diagnostic messages could be improved - for example, if the user accesses a non-existent attribute using an expression that appears inside `cond` of `if cond: ...`, it could explain the fact that narrowing failed (or is incomplete) and why.

---

_Closed by @ooopus on 2025-12-25 04:32_

---

_Comment by @carljm on 2025-12-26 17:47_

I had mentioned the idea in https://github.com/astral-sh/ty/issues/1578#issuecomment-3542936632 that we could improve diagnostics for some similar cases. I will reopen this issue to track this idea. Thanks for the report!

---

_Reopened by @carljm on 2025-12-26 17:47_

---

_Label `diagnostics` added by @carljm on 2025-12-26 17:48_

---

_Renamed from "hasattr() + truthiness guard incorrectly infers ~AlwaysFalsy for attribute access" to "Improve diagnostics in cases where we infer `object` due to possibility of subclassing" by @carljm on 2025-12-26 17:48_

---

_Comment by @carljm on 2025-12-26 18:02_

Oh, I just saw #2221 -- we can use that issue instead.

---

_Closed by @carljm on 2025-12-26 18:02_

---
