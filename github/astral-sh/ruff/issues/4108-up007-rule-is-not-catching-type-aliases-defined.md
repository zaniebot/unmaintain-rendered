---
number: 4108
title: "UP007 rule is not catching type aliases defined without the `TypeAlias` annotation"
type: issue
state: closed
author: ericbn
labels:
  - bug
assignees: []
created_at: 2023-04-26T00:32:36Z
updated_at: 2023-04-26T17:14:01Z
url: https://github.com/astral-sh/ruff/issues/4108
synced_at: 2026-01-07T13:12:14-06:00
---

# UP007 rule is not catching type aliases defined without the `TypeAlias` annotation

---

_Issue opened by @ericbn on 2023-04-26 00:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi! 👋 

The UP007 rule is not catching the scenarios where a type alias is defined without the `TypeAlias` annotation, which is likely to happen when upgrading from py39 to py310.
 
#### test.py
Minimal code snipped that reproduces the bug:
```
from typing import Literal, Optional, TypeAlias, Union

YesNo = Literal["yes", "no"]
OptionalYesNo = Optional[YesNo]
OptionalBlankYesNo = Union[OptionalYesNo, Literal[""]]
OptionalYesNoAlias: TypeAlias = Optional[YesNo]
OptionalBlankYesNoAlias: TypeAlias = Union[OptionalYesNo, Literal[""]]
```

#### Ruff output

The command I've invoked: `ruff --isolated --select UP --target-version py310 test.py`
```
test.py:6:33: UP007 [*] Use `X | Y` for type annotations
test.py:7:38: UP007 [*] Use `X | Y` for type annotations
Found 2 errors.
[*] 2 potentially fixable with the --fix option.
```

I was expecting UP007 to have reported lines 4 and 5 too. When using `--fix` I was expecting the fixed code to be:
```
from typing import Literal, TypeAlias

YesNo = Literal["yes", "no"]
OptionalYesNo = YesNo | None
OptionalBlankYesNo = OptionalYesNo | Literal[""]
OptionalYesNoAlias: TypeAlias = YesNo | None
OptionalBlankYesNoAlias: TypeAlias = OptionalYesNo | Literal[""]
```

#### ruff --version
```
ruff 0.0.262
```


---

_Label `bug` added by @charliermarsh on 2023-04-26 00:42_

---

_Comment by @charliermarsh on 2023-04-26 00:48_

Hmm yeah, I was confused too here, but looking back at the code, I think this is by design... We have to be somewhat careful about transforming runtime unions as those transformations can lead to subtle bugs. This was one example: https://github.com/charliermarsh/ruff/issues/3215. It also came up here: https://github.com/charliermarsh/ruff/issues/2981.

I need to see whether this is safe to do in more contexts. We could also consider flagging those usages even if we don't autofix them.

(We explicitly special-case the RHS of an assignment annotated with `TypeAlias`.)

---

_Comment by @ericbn on 2023-04-26 01:09_

Fair enough. I cannot even start to wonder all the possible edge cases were transforming type aliases can lead to bugs after looking at the issues you've mentioned.

Sorry, I should have researched more on past issues before opening this one.

Unless you still want to consider researching on possible safe scenarios, this issue can be closed.

---

_Comment by @ericbn on 2023-04-26 01:11_

FYI I didn't check pyupgrade's behavior before opening this issue and doing `pyupgrade --py310-plus test.py` actually does not affect the file, not even lines 6 and 7. (which is kind of plot twist to the original issue I've reported)  :- )

---

_Comment by @charliermarsh on 2023-04-26 17:14_

Haha yeah, I think we take slightly more liberties when `TypeAlias` is added explicitly. I'm going to close for now, though I would like to revisit this at some point.

---

_Closed by @charliermarsh on 2023-04-26 17:14_

---

_Referenced in [astral-sh/ruff#4170](../../astral-sh/ruff/pulls/4170.md) on 2023-05-01 17:07_

---

_Referenced in [astral-sh/ruff#4843](../../astral-sh/ruff/issues/4843.md) on 2023-06-03 23:07_

---
