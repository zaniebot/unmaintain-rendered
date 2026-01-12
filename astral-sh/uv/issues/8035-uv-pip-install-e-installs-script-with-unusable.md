```yaml
number: 8035
title: "`uv pip install -e .` installs script with unusable shebang"
type: issue
state: closed
author: salty-horse
labels: []
assignees: []
created_at: 2024-10-09T07:00:18Z
updated_at: 2024-10-09T09:57:22Z
url: https://github.com/astral-sh/uv/issues/8035
synced_at: 2026-01-12T15:59:19Z
```

# `uv pip install -e .` installs script with unusable shebang

---

_@salty-horse_

Using uv 0.4.18 on Linux.

Given a directory with this `setup.py`:
```python
from setuptools import setup

setup(
   name="test",
   scripts=(
      "scripts/test_script",
   ),
)
```
And this `scripts/test_script`:
```
#!/usr/bin/env python
print('hello')
```

And running these commands:
```
uv venv
uv pip install -e .
```

The script `test_script` was copied into `.venv/bin/test_script` and its shebang changed to `#!/home/myuser/.cache/uv/builds-v0/.tmpl3hQKg/bin/python`

This python path does not exist, which means the script is not executable.

Is uv behaving incorrectly, or is something wrong in the `setup.py` configuration?

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-10-09 09:34_

I think this _might_ be the same as https://github.com/astral-sh/uv/issues/2744?

---

_Comment by @salty-horse on 2024-10-09 09:50_

Looks like it. I missed it in my searches. Thanks!

---

_Closed by @salty-horse on 2024-10-09 09:50_

---

_Comment by @charliermarsh on 2024-10-09 09:57_

Np!

---
