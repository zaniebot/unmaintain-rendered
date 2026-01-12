```yaml
number: 4661
title: F401 fix leaves empty space behind
type: issue
state: closed
author: radeklat
labels:
  - question
assignees: []
created_at: 2023-05-25T19:29:22Z
updated_at: 2023-05-28T23:55:15Z
url: https://github.com/astral-sh/ruff/issues/4661
synced_at: 2026-01-12T15:54:44Z
```

# F401 fix leaves empty space behind

---

_@radeklat_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

When unused imports (F401) are auto-fixed and no imports are remaining in the import group, empty space is left behind. In the example below, `\n` is used to denote empty new lines as GitHub markdown removed them.

I appreciate the code can be reformatted later by other tools like `black`. But it would be great if `ruff` could take care of the imports completely and remove the extra white space left behind as well. This would bring `ruff` closer to being a replacement for `isort`, which it already seems to be aiming to be.

### Initial code (`test.py`)

```python
from collections import defaultdict  # unused import
from typing import Any  # unused import
\n
import requests  # used import
\n
requests.get("https://github.com/charliermarsh/ruff")
```

### Command

```shell
ruff check --fix --isolated test.py
```

Outputs:

```text
Found 2 errors (2 fixed, 0 remaining).
```

### Expected output

```python
import requests  # used import
\n
requests.get("https://github.com/charliermarsh/ruff")
```

### Actual output

```python
\n
import requests  # used import
\n
requests.get("https://github.com/charliermarsh/ruff")
```

### Ruff settings

 None (no config file)

### Ruff version

0.0.270

---

_Comment by @charliermarsh on 2023-05-25 19:31_

So in Ruff, we delegate this responsibility to the import-sorting rules, rather than having the unused-import rules take care of that kind of formatting . Have you tried adding the `"I"` rules to your configuration? E.g., `ruff check --fix --extend-select I test.py`, or with:

```toml
[tool.ruff]
select = ["E", "F", "I"]
```

---

_Label `question` added by @charliermarsh on 2023-05-25 19:56_

---

_Comment by @radeklat on 2023-05-25 20:00_

Delegation to the `I` rules makes sense. However, none of them fixes that either (not even on repeated runs) :( I tried:

- `ruff check --fix --isolated --select=I,E test.py`
- `ruff check --fix --isolated --select=ALL test.py`

---

_Comment by @charliermarsh on 2023-05-25 20:14_

I’ll test it out myself in a bit - it’s possible we’re missing some case related to removing the first line in a file!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-26 02:59_

---

_Comment by @charliermarsh on 2023-05-26 03:01_

So it does seem that we leave the empty line at the top of the file, but isort does too. (Generally, isort only modifies the newlines _after_ an import or set of imports, but not before.) So I'm torn on whether we should actually change anything here. My instinct is to leave our behavior as-is, and wait for the Ruff autoformatter to handle this anyway.

---

_Comment by @radeklat on 2023-05-28 23:55_

I didn't know an auto-formatter was planned! I presume you mean https://github.com/charliermarsh/ruff/issues/1904 ? That would be acceptable solution. As mentioned initially, this issue can be already solved by `black`. So that is a reasonable workaround in the mean time.

---

_Closed by @radeklat on 2023-05-28 23:55_

---
