```yaml
number: 4667
title: TCH002 false alarm when importing submodule with import..as
type: issue
state: closed
author: jakkdl
labels:
  - bug
assignees: []
created_at: 2023-05-26T09:23:32Z
updated_at: 2023-05-30T11:52:53Z
url: https://github.com/astral-sh/ruff/issues/4667
synced_at: 2026-01-12T15:54:44Z
```

# TCH002 false alarm when importing submodule with import..as

---

_@jakkdl_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
## not using import..as
```python
from __future__ import annotations

import libcst
import libcst.matchers

my_list: list[libcst.With] = []
dir(libcst.matchers)
```
```bash
$ ruff --select --isolated TCH002 foo.py
```
no errors, working as expected.

## using import..as
```python
from __future__ import annotations

import libcst
import libcst.matchers as m

my_list: list[libcst.With] = []
dir(m)
```

```bash
$ ruff --select --isolated TCH002 foo.py
foo.py:3:8: TCH002 Move third-party import `libcst` into a type-checking block
Found 1 error.
```

## version
```bash
$ ruff --version
ruff 0.0.269
$ python --version
Python 3.11.3
```

`flake8-type-checking` handles both cases, so this is solely a bug in ruff.
Popped up in https://github.com/Zac-HD/flake8-trio/pull/196#discussion_r1200414885

---

_Comment by @dhruvmanila on 2023-05-26 12:02_

This seems to be similar to the problem faced in https://github.com/charliermarsh/ruff/issues/4655. I'm guessing the submodule import (`import libcst.matchers`) is increasing the usage count of `libcst` import, thus not flagging for your first example. But, in your second example, the aliased import is not increasing the usage.

---

_Comment by @jakkdl on 2023-05-26 12:12_

Oh interesting, it's related but also the reverse. So maybe there's two bugs here that cancel out in the first example, where I don't care to create a dedicated `TYPE_CHECKING` block with no performance improvement (since importing `libcst.matchers` will also import `libcst`)

---

_Label `bug` added by @charliermarsh on 2023-05-26 12:16_

---

_Comment by @charliermarsh on 2023-05-26 12:57_

Guessing this is a bug in the "strictness" check, whereby we determine whether an import has some other runtime-required import of the same module.

---

_Comment by @charliermarsh on 2023-05-26 13:35_

@dhruvmanila - Do you wanna take a look? I'd start by looking at `is_implicit_import`.

---

_Comment by @dhruvmanila on 2023-05-26 13:52_

Yeah, sure!

---

_Closed by @charliermarsh on 2023-05-28 03:58_

---

_Comment by @jakkdl on 2023-05-30 11:52_

Thanks for the quick fix! :rocket: 

---
