```yaml
number: 4318
title: "Request: `SIM105` autofix including (and not removing) inline comments"
type: issue
state: closed
author: jamesbraza
labels:
  - question
assignees: []
created_at: 2023-05-09T17:50:34Z
updated_at: 2023-05-09T21:30:58Z
url: https://github.com/astral-sh/ruff/issues/4318
synced_at: 2026-01-12T15:54:44Z
```

# Request: `SIM105` autofix including (and not removing) inline comments

---

_@jamesbraza_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Please see the below code snippet:

```python
try:
    _ = 0
except KeyError:
    pass  # Some reason for the suppression
```

With `ruff==0.0.265`, and running `ruff --fixable=SIM105 --select=SIM105 --fix a.py`, this is autofixed to:

```python
import contextlib
with contextlib.suppress(KeyError):
    _ = 0
  # Some reason for the suppression
```

The autofix is great, but the explanatory comment isn't moved too, so the comment now becomes misplaced.

I think it is desirable for the autofix to move the comment as well, like so:

```python
import contextlib
with contextlib.suppress(KeyError):
    # Some reason for the suppression
    _ = 0

# or

import contextlib
with contextlib.suppress(KeyError):  # Some reason for the suppression
    _ = 0
```

Thank you for your consideration!

---

_Renamed from "Request: `SIM105` autofix including inline comments" to "Request: `SIM105` autofix including (and not removing) inline comments" by @jamesbraza on 2023-05-09 17:52_

---

_Comment by @jamesbraza on 2023-05-09 17:55_

Similarly, please see this snippet:

```python
try:
    _ = 0
except KeyError:  # Some reason for the suppression
    pass
```

`ruff --fixable=SIM105 --select=SIM105 --fix a.py,` autofixes to the below, which actually removes the explanatory comment entirely:

```python
import contextlib
with contextlib.suppress(KeyError):
    _ = 0
```

I think it's not desirable for an autofix to remove inline comments.  Thanks again!

---

_Comment by @charliermarsh on 2023-05-09 18:45_

It's difficult to retain comments in these cases, since it's not always clear what the comment is meant to be attached to. In general, we often avoid fixes if the fix would result in a lost comment, so perhaps we should extend that logic to this rule.

---

_Label `question` added by @charliermarsh on 2023-05-09 18:45_

---

_Closed by @charliermarsh on 2023-05-09 21:30_

---
