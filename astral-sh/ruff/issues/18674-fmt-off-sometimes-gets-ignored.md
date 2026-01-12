```yaml
number: 18674
title: "fmt: off sometimes gets ignored"
type: issue
state: closed
author: mxmlnkn
labels:
  - question
assignees: []
created_at: 2025-06-14T15:22:49Z
updated_at: 2025-06-14T16:25:33Z
url: https://github.com/astral-sh/ruff/issues/18674
synced_at: 2026-01-12T15:54:56Z
```

# fmt: off sometimes gets ignored

---

_@mxmlnkn_

### Summary

Code example:

```python3
class FuseMount:
    def __init__(self, **kwargs):
        print("__init__")
        self.options = kwargs


def createFuseMount(args) -> None:
    with FuseMount(
        # fmt: off
        pathToMount = args.mount_source,
        encoding    = args.encoding,
        # ...
        # fmt: on
    ) as fuseOperationsObject:
        print("Do stuff")
```

I am calling ruff with:

```
ruff clean; ruff format --isolated ratarmount/Actions.py
```

and end up with:

```python3
class FuseMount:
    def __init__(self, **kwargs):
        print("__init__")
        self.options = kwargs


def createFuseMount(args) -> None:
    with FuseMount(
        # fmt: off
        pathToMount=args.mount_source,
        encoding=args.encoding,
        # ...
        # fmt: on
    ) as fuseOperationsObject:
        print("Do stuff")
```

The `fmt: off` works in other places of my code base, so this seems like a bug to me.

### Version

ruff 0.11.13

---

_Comment by @MeGaGiGaGon on 2025-06-14 15:33_

Per [the docs](https://docs.astral.sh/ruff/formatter/#format-suppression), format on/off/skip work at the statement level, and are ignored inside expressions, which is why they aren't working for you. I'd recommend [this instead](https://play.ruff.rs/99c0a872-8c25-479d-999c-43c3e8d7ffd6)
```py
class FuseMount:
    def __init__(self, **kwargs):
        print("__init__")
        self.options = kwargs


def createFuseMount(args) -> None:
    with FuseMount(
        pathToMount = args.mount_source,
        encoding    = args.encoding,
        # ...
    ) as fuseOperationsObject: # fmt: skip
        print("Do stuff" )

---

_Comment by @ntBre on 2025-06-14 15:34_

Thanks @MeGaGiGaGon, you beat me to it! Yes, you can see that [invalid-formatter-suppression-comment (RUF028)](https://docs.astral.sh/ruff/rules/invalid-formatter-suppression-comment/#invalid-formatter-suppression-comment-ruf028) triggers on these comments in the [playground](https://play.ruff.rs/f19a5b56-0e6d-4bf1-8654-63fde488885d), so I think this is the expected behavior.

---

_Label `question` added by @ntBre on 2025-06-14 15:35_

---

_Comment by @mxmlnkn on 2025-06-14 16:23_

Thank you for the fast answers! This was unexpected behavior because this worked for a long time with black. For completeness, ~~this also works~~:

```python3
    # fmt: off
    with FuseMount(
        pathToMount=args.mount_source,
        encoding=args.encoding,
        # ...
    ) as fuseOperationsObject:
    # fmt: on
```

this would give an `E115 expected an indented block (comment)` error with pytype.

---

_Closed by @mxmlnkn on 2025-06-14 16:23_

---
