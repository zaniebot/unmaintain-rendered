---
number: 17794
title: "Lint the `__all__` variable for validity"
type: issue
state: closed
author: hwittenborn
labels:
  - question
assignees: []
created_at: 2025-05-02T17:59:31Z
updated_at: 2025-05-12T08:24:00Z
url: https://github.com/astral-sh/ruff/issues/17794
synced_at: 2026-01-07T13:12:16-06:00
---

# Lint the `__all__` variable for validity

---

_Issue opened by @hwittenborn on 2025-05-02 17:59_

### Summary

Ruff doesn't complain about this non-existent `wow` import, even though it doesn't exist:

```python
__all__ = ["ai", "env", "tools", "endpoints", "wow"]
from . import ai, env, tools, endpoints
```

Would it be possible to lint `__all__` for non-existent members, in the same way it checks for members that _aren't_ present?

---

_Renamed from "Lint the `__all__` for validity" to "Lint the `__all__` variable for validity" by @hwittenborn on 2025-05-02 19:52_

---

_Comment by @Daverball on 2025-05-02 20:48_

It already does, it emits an [`undefined-name`](https://docs.astral.sh/ruff/rules/undefined-name/) (`F822`) for your example. If it doesn't you either have that diagnostic disabled or your stripped down example isn't enough on its own to reproduce your problem.

---

_Label `question` added by @MichaReiser on 2025-05-02 20:52_

---

_Comment by @hwittenborn on 2025-05-06 13:35_

Following the example I gave above, I get the following from running `ruff check` (without a configuration file being present):

```
$ ruff check
All checks passed!
```

Is it possible that the lint isn't enabled by default? Or are you seeing differently when you run things on your end?

---

_Comment by @Daverball on 2025-05-06 13:50_

`F` should be part of the default configuration, so it should be enabled. Does `ruff check --isolated` give you the same output? If not there may be a config file somewhere that's getting picked up by ruff. You can use `--show-settings` to see what the individual settings currently resolve to when checking any given file.

See [Config File Discovery](https://docs.astral.sh/ruff/configuration/#config-file-discovery) for where those settings could come from.

---

_Closed by @MichaReiser on 2025-05-12 08:24_

---
