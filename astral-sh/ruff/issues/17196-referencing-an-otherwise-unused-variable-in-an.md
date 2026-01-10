```yaml
number: 17196
title: "Referencing an otherwise unused variable in an argument to `Annotated` falsely triggers F841"
type: issue
state: closed
author: bkis
labels:
  - bug
assignees: []
created_at: 2025-04-04T10:03:17Z
updated_at: 2025-04-04T13:17:41Z
url: https://github.com/astral-sh/ruff/issues/17196
synced_at: 2026-01-10T11:09:58Z
```

# Referencing an otherwise unused variable in an argument to `Annotated` falsely triggers F841

---

_Issue opened by @bkis on 2025-04-04 10:03_

### Summary

The following code shows how `ruff check --isolated` (`0.8.2` through `0.11.3`) falsely interprets variables used in arguments to `Annotated` as "unused" (exact same code in the [playground](https://play.ruff.rs/421bd4d2-02be-4f52-bb94-1349fd6f7ef0)):

```py
from typing import Annotated, Literal


def type_annotations_from_tuple():
    annos = (str, "foo", "bar")  # unused? really?
    return Annotated[annos]


def type_annotations_from_filtered_tuple():
    annos = (str, None, "foo", None, "bar")  # still unused?
    return Annotated[tuple([a for a in annos if a is not None])]


def literals_from_tuple():
    string_literals = ("foo", "bar")
    return Literal[string_literals]  # but this counts as "used"?
```

### Version

at least >=0.8.2, <=0.11.3

---

_Comment by @Daverball on 2025-04-04 11:44_

This should be a simple fix. This happens because Ruff always expects a tuple slice in `typing.Annotated`, which it generally should be, since passing it a different kind of expression would make it an invalid type expression. But that ignores the possibility of dynamically creating these objects in a way that would get ignored by type checkers.

Instead of `debug!` in the else branch we should just visit the slice, like we do with `typing.Literal`.

Edit: I'm working on this.

---

_Label `bug` added by @dhruvmanila on 2025-04-04 12:48_

---

_Closed by @dhruvmanila on 2025-04-04 13:17_

---
