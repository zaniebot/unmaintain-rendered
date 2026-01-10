---
number: 4561
title: "Unlike isort, ruff `I` rules do not remove unneeded line break commas"
type: issue
state: closed
author: johnthagen
labels:
  - isort
assignees: []
created_at: 2023-05-21T18:07:36Z
updated_at: 2023-05-21T18:33:29Z
url: https://github.com/astral-sh/ruff/issues/4561
synced_at: 2026-01-10T01:22:43Z
---

# Unlike isort, ruff `I` rules do not remove unneeded line break commas

---

_Issue opened by @johnthagen on 2023-05-21 18:07_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


Consider this code:

```py
from enum import (
    Enum,
    unique,
)
```

Running `isort main.py` results in:

```py
from enum import Enum, unique
```

But running `ruff check .\main.py --select I --fix` results in (unchanged):

```py
from enum import (
    Enum,
    unique,
)
```

Our workflow _used_ to be, run `isort` and then run `black`. What this would do was remove any unnecessary line break commas in imports, and then black would reflow any lines that needed changing.

- Example using nox: https://github.com/johnthagen/python-blueprint/blob/9e3c9a7ee7d89f46a2e0be0e8b77984abf974634/noxfile.py#L30

But with Ruff's implementation of isort rules, a single small import with a trailing comma will never be collapsed.

Imagine a scenario where you used to import 6 things from a module and that required breaking into new lines, but later refactor your code and use your editor to automatically remove the unused imports. If there is a single trailing comma, now that will never be nicely collapsed to a single line. Over a large code base, this can start to add up to a lot of extra vertical space consumed.

Is there a way to restore to isort's behavior?

### Environmenet

- Python: 3.10.7
- Ruff: 0.0.269
- isort: 5.12.0

---

_Comment by @charliermarsh on 2023-05-21 18:17_

You can set [`split-on-trailing-comma`](https://beta.ruff.rs/docs/settings/#split-on-trailing-comma) to `false`.

(There's more on this here: https://github.com/charliermarsh/ruff/issues/4153. I continue to believe this is a bug in isort: the documentation states that `split-on-trailing-comma` defaults to `true` for `profile = "black"`, but in practice, you have to explicitly enable it.)


---

_Comment by @charliermarsh on 2023-05-21 18:18_

(Closing preemptively, but let me know if this doesn't solve your problem.)

---

_Closed by @charliermarsh on 2023-05-21 18:18_

---

_Label `isort` added by @charliermarsh on 2023-05-21 18:18_

---

_Comment by @johnthagen on 2023-05-21 18:33_

Thanks @charliermarsh! That is very interesting about the isort history. I always assumed it was intentional to collapse extra lines for the imports (and deviate from black in this one specific way), but as you mention in the other thread, perhaps it was not?

---

_Referenced in [johnthagen/python-blueprint#172](../../johnthagen/python-blueprint/pulls/172.md) on 2023-05-21 18:50_

---
