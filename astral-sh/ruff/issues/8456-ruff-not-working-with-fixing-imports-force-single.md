---
number: 8456
title: Ruff not working with fixing imports force-single-line=true
type: issue
state: closed
author: SanketDG
labels:
  - needs-info
assignees: []
created_at: 2023-11-03T02:54:47Z
updated_at: 2024-11-18T18:08:19Z
url: https://github.com/astral-sh/ruff/issues/8456
synced_at: 2026-01-10T01:22:48Z
---

# Ruff not working with fixing imports force-single-line=true

---

_Issue opened by @SanketDG on 2023-11-03 02:54_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:


* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

`test.py`
```python
from tests.helpers.db import (
    setup_db,
    setup_db2,
)

setup_db()
setup_db2()
```

`pyproject.toml`
```toml
[tool.ruff]
line-length = 120
exclude = ["entrypoint.py"]

[tool.ruff.isort]
force-single-line = true

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"
```

`ruff check` or `ruff format` does not output anything.
Also tried ` ruff check test.py --isolated --fix`

```
ruff --version                                  
ruff 0.1.3
```

---

_Comment by @charliermarsh on 2023-11-03 03:04_

I'm unable to reproduce this -- if you try it in the playground, `ruff check` works as expected: https://play.ruff.rs/c56a6c61-188a-4606-9b69-85fcd129acb3. So it may be an issue with your configuration. Are you sure your configuration is being applied to the file?


---

_Label `waiting-on-author` added by @charliermarsh on 2023-11-03 03:04_

---

_Comment by @charliermarsh on 2023-11-03 16:34_

I'm going to close as I think this is working as intended and so doesn't seem to be a _bug_, but if you have any follow-ups I'm happy to help work through here in the issue.

---

_Closed by @charliermarsh on 2023-11-03 16:34_

---

_Comment by @ishefi on 2023-12-26 11:11_

happens to me as well -- my `pyproject.toml` has the following section:

```
[tool.ruff.lint.isort]
force-single-line = true
```

Could it be that the issue is with reading configurations from `pyproject.toml`?

ruff version 0.1.9

EDIT: it works after I added the following to `pyproject.toml`:

```
[tool.ruff]
select = ["F", "I"]
```


---

_Comment by @skhaz on 2024-11-18 17:19_

I am now getting 

>  The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
>  - 'select' -> 'lint.select'
  
 When I add
 
 ```
 [tool.ruff]
select = ["F", "I"]
```

---

_Comment by @MichaReiser on 2024-11-18 17:22_

@skhaz can you try 

```
[tool.ruff.lint]
select = ["F", "I"]
```

---

_Comment by @skhaz on 2024-11-18 18:08_

Perfect, thank you so much!

---
