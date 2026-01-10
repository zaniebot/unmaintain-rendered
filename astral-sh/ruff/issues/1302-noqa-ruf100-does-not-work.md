```yaml
number: 1302
title: "`# noqa: RUF100` does not work"
type: issue
state: closed
author: actionless
labels:
  - bug
assignees: []
created_at: 2022-12-20T20:31:24Z
updated_at: 2022-12-20T22:40:11Z
url: https://github.com/astral-sh/ruff/issues/1302
synced_at: 2026-01-10T12:05:25Z
```

# `# noqa: RUF100` does not work

---

_Issue opened by @actionless on 2022-12-20 20:31_

```
RUF100 Unused `noqa` directive for: RUF100
```

---

_Label `bug` added by @charliermarsh on 2022-12-20 20:34_

---

_Comment by @charliermarsh on 2022-12-20 20:34_

Oh good call, thanks.

---

_Comment by @charliermarsh on 2022-12-20 22:08_

@actionless - I have a fix in #1305, but could you post an example of how you expect this to work?

A few examples (`F401` is unused-import; `E501` is long-too-long):

```py
# This line doesn't trigger RUF100, so `noqa: RUF100` is an error
import os  # noqa: RUF100

---

# This line doesn't trigger RUF100, so `noqa: RUF100` is an error
import os  # noqa: F401, RUF100

---

# This line _would_ trigger RUF100, so `noqa: RUF100` suppresses it
import os  # noqa: E501, RUF100
```

---

_Comment by @actionless on 2022-12-20 22:15_

in my case i just have a warning which i want to ignore in `flake`, but it's not raised by `ruff`, so it's raising RUF100

---

_Comment by @charliermarsh on 2022-12-20 22:21_

Is it an error code that doesn't exist in Ruff? You could set `external = ["E301"]` or whatever in `pyproject.toml`.

---

_Comment by @charliermarsh on 2022-12-20 22:22_

But, in that case, I should probably just do the simple thing and suppress `RUF100` whenever it's provided explicitly.

---

_Comment by @actionless on 2022-12-20 22:29_

the situation even more complicated 😺
it exists in both flake and ruff, but behaves slightly differently (flake have a bug similar to https://github.com/charliermarsh/ruff/issues/1303 but only inside string types, ie `'Callable[[Any, DefaultNamedArg(bool | None, name="some_prop_name")], None]'` (note the quotes!)
so this will be failing the CI until it will start behaving same in both flake and ruff

---

_Comment by @charliermarsh on 2022-12-20 22:31_

Hahaha ok!

---

_Closed by @charliermarsh on 2022-12-20 22:40_

---
