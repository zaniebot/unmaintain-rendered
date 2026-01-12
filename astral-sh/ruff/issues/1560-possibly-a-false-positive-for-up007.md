```yaml
number: 1560
title: Possibly a false positive for UP007
type: issue
state: closed
author: rassie
labels:
  - bug
assignees: []
created_at: 2023-01-02T20:02:00Z
updated_at: 2023-01-02T21:33:53Z
url: https://github.com/astral-sh/ruff/issues/1560
synced_at: 2026-01-12T15:54:41Z
```

# Possibly a false positive for UP007

---

_@rassie_

Here is a bit of code I adapted [from StackOverflow](https://stackoverflow.com/questions/61753056/partial-update-in-fastapi/71135795#71135795) to make Pydantic model's fields optional:

```python
def convert_to_optional(schema: type[Base]) -> dict:
    return {k: Optional[v] for k, v in schema.__annotations__.items()} 
```

This triggers UP007, proposing to replace `Optional[v]` with `v | None`, which the compiler denies (`TypeError: unsupported operand type(s) for |: 'str' and 'NoneType'`). Since `v` is a value and not a type, I suppose this is a false positive.

That being said: I consider this issue edge-casey and thus simply informative, both for developers and people searching the web for the problem. I'm fully  content with just adding `# noqa: UP007` to that line and be done with it.

---

_Comment by @charliermarsh on 2023-01-02 21:02_

Let me see if pyupgrade handles this case correctly.

---

_Comment by @charliermarsh on 2023-01-02 21:27_

I think this is actually just a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-02 21:27_

---

_Label `bug` added by @charliermarsh on 2023-01-02 21:27_

---

_Comment by @charliermarsh on 2023-01-02 21:33_

It looks like pyupgrade avoids these, presumedly for this reason.

---

_Closed by @charliermarsh on 2023-01-02 21:33_

---
