```yaml
number: 3193
title: "Idea for TCH: for imports in `TYPE_CHECK` block"
type: issue
state: closed
author: hofrob
labels: []
assignees: []
created_at: 2023-02-23T23:48:18Z
updated_at: 2023-02-27T10:01:25Z
url: https://github.com/astral-sh/ruff/issues/3193
synced_at: 2026-01-12T15:54:43Z
```

# Idea for TCH: for imports in `TYPE_CHECK` block

---

_@hofrob_

Those two ideas are not part of `flake8-type-checking`, so I'm fully aware that this issue might not get a high priority. That's fine! I'm very thankful for this tool as it is already :slightly_smiling_face:.

It would be nice if ruff could highlight missing quotes for types that are in a `TYPE_CHECK` block. I missed a few while refactoring code using TCH001, and thought that this would be a great addition.

And regarding quotes, I have another suggestion that might be too opinionated. Let's see:

```py
import typing

if typing.TYPE_CHECKING:
    import datetime


def three_previous_years(present: "datetime.datetime") -> list["datetime.datetime"]:
    return [
        present.replace(year=present.year - 1),
        present.replace(year=present.year - 2),
        present.replace(year=present.year - 3),
    ]
```

The return type can be written as `list["datetime.datetime"]` and as `"list[datetime.datetime]"`. In this case it won't make a difference (afaik). When making the return type optional though, you'd have to wrap _everything_ in quotes. This `list["datetime.datetime"] | None` does not work:

```
TypeError: unsupported operand type(s) for |: 'str' and 'NoneType'
```

This is correct: `"list[datetime.datetime] | None"`

So for me personally, I'd prefer consistency and would force quotes for all types _if_ a type from the `TYPE_CHECK` block is involved. This might even be autofixable. I don't know of any linters/formatters that have this option.

Refactoring code using ruff is great. Thanks a lot!

---

_Comment by @hofrob on 2023-02-27 10:01_

I need to think about this a bit more. 

For example: `list["datetime.datetime"] | None` is valid, but `"list[datetime.datetime]" | None` isn't.

---

_Closed by @hofrob on 2023-02-27 10:01_

---
