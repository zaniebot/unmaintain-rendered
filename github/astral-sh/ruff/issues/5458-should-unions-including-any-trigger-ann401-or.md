---
number: 5458
title: "Should unions including `Any` trigger `ANN401` (or some other rule)?"
type: issue
state: closed
author: bersbersbers
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-07-01T20:10:01Z
updated_at: 2023-07-13T12:49:28Z
url: https://github.com/astral-sh/ruff/issues/5458
synced_at: 2026-01-07T13:12:15-06:00
---

# Should unions including `Any` trigger `ANN401` (or some other rule)?

---

_Issue opened by @bersbersbers on 2023-07-01 20:10_

```python
from typing import Any

def fun_ann401(abc: Any) -> None:
    print(abc)

def fun_no_error(abc: Any | None = None) -> None:
    print(abc)
```

gives

> bug.py:4:21: ANN401 Dynamically typed expressions (typing.Any) are disallowed in `abc`

I wonder why that error is not also emitted for `fun_no_error`. `Any | None` is at least as undefined as `Any` is.

Another option would be to add a new rule that says
> `Any | Foo` is redundant and should be replaced by `Any`.

---

_Referenced in [astral-sh/ruff#5459](../../astral-sh/ruff/pulls/5459.md) on 2023-07-01 20:23_

---

_Referenced in [astral-sh/ruff#5460](../../astral-sh/ruff/pulls/5460.md) on 2023-07-01 20:23_

---

_Comment by @charliermarsh on 2023-07-02 20:06_

I think it should be included, yeah.

---

_Label `rule` added by @charliermarsh on 2023-07-02 20:06_

---

_Comment by @charliermarsh on 2023-07-02 20:06_

Maybe we can reuse the logic that @dhruvmanila added for implicit optionals.

---

_Comment by @dhruvmanila on 2023-07-04 13:27_

> Maybe we can reuse the logic that @dhruvmanila added for implicit optionals.

If I understand correctly, do you mean to check if there's `typing.Any` instead of `None` in the `TypingTarget`? Or, do you just mean to use the `|` union iterator to check if there's `Any` in it?

---

_Comment by @charliermarsh on 2023-07-04 13:28_

The latter! Does that make sense?

---

_Comment by @dhruvmanila on 2023-07-04 13:36_

It does but then `Union[Any, None]` (which is same as `Any | None`) won't be triggered.

---

_Comment by @dhruvmanila on 2023-07-04 13:39_

Actually, this then brings up the question if we should look for `Any` in other places as well (`Optional[Any]`).

I believe the main question would be "does this type resolve to `Any`"?

---

_Comment by @charliermarsh on 2023-07-04 14:02_

Err, sorry, I may have misunderstood your question. We're just looking to understand whether the type resolves to `Any`, I think.

---

_Comment by @dhruvmanila on 2023-07-04 14:23_

Right, then I believe the following cases should also be considered although the original [plugin doesn't support them](https://github.com/sco1/flake8-annotations#dynamic-typing-caveats).
* `Any | ...`
* `Union[Any, ...]`
* `Optional[Any]`
* `Annotated[<any of the above variants>, ...]`
* Forward references?

Does this make sense?

I don't think `Any` in container types (e.g., `list[Any]`) should be considered, right?

---

_Comment by @charliermarsh on 2023-07-04 22:03_

Yes, agreed.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-07-05 03:16_

---

_Comment by @dhruvmanila on 2023-07-05 03:19_

It should be simple enough to extract out the logic from `implicit-optional` check and add another method to check for `Any`.

---

_Referenced in [astral-sh/ruff#5601](../../astral-sh/ruff/pulls/5601.md) on 2023-07-07 20:38_

---

_Label `accepted` added by @charliermarsh on 2023-07-10 01:28_

---

_Closed by @dhruvmanila on 2023-07-13 12:49_

---
