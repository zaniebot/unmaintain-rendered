```yaml
number: 8661
title: "Regression in Ruff's ordering of import aliases with `force-sort-within-sections`"
type: issue
state: closed
author: cmsetzer
labels:
  - bug
  - isort
assignees: []
created_at: 2023-11-13T18:25:24Z
updated_at: 2023-11-14T00:52:28Z
url: https://github.com/astral-sh/ruff/issues/8661
synced_at: 2026-01-10T11:09:50Z
```

# Regression in Ruff's ordering of import aliases with `force-sort-within-sections`

---

_Issue opened by @cmsetzer on 2023-11-13 18:25_

It looks like #7963 introduced a regression in Ruff's ordering of certain import aliases when `force-sort-within-sections` is enabled. In version 0.1.3 and prior, Ruff and isort produced identical output under this setting.

## Example

* Ruff version 0.1.5
* isort version 5.12.0

Given `pyproject.toml`:
```toml
[tool.isort]
force_sort_within_sections = true

[tool.ruff.isort]
force-sort-within-sections = true
```

And `example.py`:
```py
import encodings
from datetime import timezone as tz
from datetime import timedelta
import datetime as dt
import datetime
```

isort produces the following:
```console
$ cat example.py | isort -

import datetime
import datetime as dt
from datetime import timedelta
from datetime import timezone as tz
import encodings
```

...while Ruff now places `import datetime as dt` out of order:
```console
$ cat example.py | ruff check --fix --no-cache --select="I001" --stdin-filename="-"

import datetime
from datetime import timedelta
from datetime import timezone as tz
import datetime as dt
import encodings

Found 1 error (1 fixed, 0 remaining).
```

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

---

_Comment by @charliermarsh on 2023-11-13 18:26_

Thanks! @bluthej, are you able to take a look at this? If not, I understand and can investigate myself, but thought I would ask first since it's likely related to your recent refactor.

---

_Label `bug` added by @charliermarsh on 2023-11-13 18:26_

---

_Label `isort` added by @charliermarsh on 2023-11-13 18:26_

---

_Comment by @charliermarsh on 2023-11-13 18:27_

Regardless, @cmsetzer, will make sure this is fixed for the next release. Thanks for the clear write-up.

---

_Comment by @bluthej on 2023-11-13 18:27_

@charliermarsh sure I'll take a look at it! 

---

_Comment by @charliermarsh on 2023-11-13 18:30_

@bluthej - Thank you!

---

_Comment by @bluthej on 2023-11-13 19:13_

Ok so what happens is the from imports don't have an "asname", so they have a `None` that places them before the `import ... as`.

Looks like inverting the last two fields of `ModuleKey` fixes it, and all the tests pass, but before submitting a PR I'd like to look at the code before my refactor to make sure that this is how the ordering took place.

---

_Closed by @charliermarsh on 2023-11-13 23:27_

---
