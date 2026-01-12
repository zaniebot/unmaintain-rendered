```yaml
number: 7176
title: Rule F403 behaves weirdly
type: issue
state: closed
author: Saransh-cpp
labels: []
assignees: []
created_at: 2023-09-06T02:27:12Z
updated_at: 2023-09-06T02:36:18Z
url: https://github.com/astral-sh/ruff/issues/7176
synced_at: 2026-01-12T15:54:46Z
```

# Rule F403 behaves weirdly

---

_@Saransh-cpp_

Taking the example from ruff's [docs](https://beta.ruff.rs/docs/rules/undefined-local-with-import-star/) -

If I run ruff over the following code snippet -

```py
from math import *


def area(radius):
    return pi * radius**2
```

it errors out with -
```
F405 `pi` may be undefined, or defined from star imports
```

Shouldn't this error out with rule F403?

Moreover, I want ruff to not error out at all  if I do the following -
```py
from math import *  # noqa: F403


def area(radius):
    return pi * radius**2
```

but ruff instead errors out with the following -
```
RUF100 [*] Unused `noqa` directive (unused: `F403`)
F405 `pi` may be undefined, or defined from star imports
```

---

command used:
`pre-commit run ruff --all-files`

pre-commit config:
```yaml
- repo: https://github.com/astral-sh/ruff-pre-commit
    rev: "v0.0.282"
    hooks:
      - id: ruff
        args: []
```
Am I missing something here? Thanks!
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @Saransh-cpp on 2023-09-06 02:36_

False alarm, this was an issue with my ruff config. Sorry!

---

_Closed by @Saransh-cpp on 2023-09-06 02:36_

---
