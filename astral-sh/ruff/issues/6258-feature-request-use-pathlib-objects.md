```yaml
number: 6258
title: "Feature request: Use `pathlib` objects"
type: issue
state: open
author: petermattia
labels:
  - needs-decision
assignees: []
created_at: 2023-08-01T21:38:07Z
updated_at: 2023-09-26T02:02:04Z
url: https://github.com/astral-sh/ruff/issues/6258
synced_at: 2026-01-10T11:09:48Z
```

# Feature request: Use `pathlib` objects

---

_Issue opened by @petermattia on 2023-08-01 21:38_

Low priority feature request: Use `pathlib` objects over the class.

Let's say I have a path like this:
````python
    my_path = Path.cwd() / "data.csv"
````

Instead of this:
````python
    from pathlib import Path

    with Path.open(my_path, "w") as file:
````

I'd like to enforce this:
````python
    with my_path.open("w") as file:
````
as it's more idiomatic, shorter, and uses the object (which would skip the import if, say, this path were accessed from another module).

---

_Label `needs-decision` added by @zanieb on 2023-08-02 03:27_

---

_Comment by @jamesbraza on 2023-08-09 06:25_

[`refurb`](https://github.com/dosisod/refurb) can flag this with [`FURB117`](https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb117-use-pathlib-open), fyi

---

_Comment by @petermattia on 2023-09-26 02:02_

Linking #1348.

---
