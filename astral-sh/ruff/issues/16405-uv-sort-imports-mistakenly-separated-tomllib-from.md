```yaml
number: 16405
title: "uv sort imports mistakenly separated `tomllib` from standard library group"
type: issue
state: closed
author: htv2012
labels:
  - question
assignees: []
created_at: 2025-02-26T18:34:12Z
updated_at: 2025-03-06T07:19:37Z
url: https://github.com/astral-sh/ruff/issues/16405
synced_at: 2026-01-10T11:09:57Z
```

# uv sort imports mistakenly separated `tomllib` from standard library group

---

_Issue opened by @htv2012 on 2025-02-26 18:34_

### Summary

Given a file with the following imports:

```python
import tomllib
import pathlib
import re
import subprocess
```

When I ran the following command to sort the imports:

	uv tool run ruff check --select I --fix .

The result is

```python
import pathlib
import re
import subprocess

import tomllib
```

Notice that `tomllib` was separated from the standard library group. It should be part of that group. That means, I expect to see this output:

```python
import pathlib
import re
import subprocess
import tomllib
```


### Platform

macOS Sequoia 15.3

### Version

uv 0.6.2 (Homebrew 2025-02-19)

### Python version

Python 3.13.2

---

_Label `bug` added by @htv2012 on 2025-02-26 18:34_

---

_Comment by @charliermarsh on 2025-02-26 19:47_

See, e.g., https://github.com/astral-sh/ruff-vscode/issues/465. My guess is that your Python version isn't set to the correct minimum.

---

_Comment by @charliermarsh on 2025-02-26 19:49_

Specifically, `tomllib` was added in Python 3.11. So if your `target-version` or `requires-python` includes versions lower than that (or isn't set at all), it won't be considered first-party.

---

_Label `bug` removed by @charliermarsh on 2025-02-26 19:49_

---

_Label `question` added by @charliermarsh on 2025-02-26 19:49_

---

_Closed by @MichaReiser on 2025-03-06 07:19_

---
