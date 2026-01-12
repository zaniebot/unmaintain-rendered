```yaml
number: 4864
title: "New Rule to rewrite `pathlib.Path().joinpath` with `/`"
type: issue
state: open
author: mosauter
labels:
  - rule
  - type-inference
  - needs-decision
assignees: []
created_at: 2023-06-05T13:32:39Z
updated_at: 2025-03-13T13:09:52Z
url: https://github.com/astral-sh/ruff/issues/4864
synced_at: 2026-01-12T15:54:45Z
```

# New Rule to rewrite `pathlib.Path().joinpath` with `/`

---

_@mosauter_

In many projects I came around something like this:

```python
from pathlib import Path

path_a = Path(".").joinpath("somewhere.xml")
path_b = Path(".") / "somewhere.xml"
```

I would like to have a rule that (optimally rewrites) one into another. I don't really care which one it is, all I want is consistency.

I could also imagine that this should be realized as a setting-variable, with either the one or other way to be preferred.

The main problem for an implementation that I see is that we don't have type recognition in ruff, and therefore I would just apply it onto any `joinpath` method that I find. 

(And this is the only direction this could currently work with, as the reverse, meaning '/' to 'joinpath' is not really possible without getting the types of an expression)

---

_Comment by @mosauter on 2023-06-05 13:33_

I could also see myself implementing this, but with the missing typing I don't know if it is sensible to start with it now or if it's better to wait until we have a more general solution for typing.

---

_Label `rule` added by @charliermarsh on 2023-06-05 18:30_

---

_Comment by @charliermarsh on 2023-06-05 18:30_

I don't think we're able to implement this right now -- the number of cases we could enforce it in would be pretty limited.

---

_Label `type-inference` added by @charliermarsh on 2023-06-22 20:11_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:27_

---

_Comment by @vivodi on 2025-03-13 13:09_

I advocate for this.

---
