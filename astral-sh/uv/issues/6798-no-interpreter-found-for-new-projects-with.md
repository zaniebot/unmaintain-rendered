---
number: 6798
title: "No interpreter found for new projects with `requires-python = \">=3.13\"`"
type: issue
state: closed
author: deckstose
labels:
  - bug
  - uv python
assignees: []
created_at: 2024-08-29T09:17:22Z
updated_at: 2024-08-30T15:44:15Z
url: https://github.com/astral-sh/uv/issues/6798
synced_at: 2026-01-10T01:24:06Z
---

# No interpreter found for new projects with `requires-python = ">=3.13"`

---

_Issue opened by @deckstose on 2024-08-29 09:17_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`uv`-actions like `run` or `add` fail to find Python-3.13. However `uv python list` lists Python-3.13.

```sh
uv init example
uv --directory=example add rich
# error: No interpreter found for Python >=3.13 in managed installations or system path
uv --directory=example run hello.py
# error: No interpreter found for Python >=3.13 in managed installations or system path
uv python list
# cpython-3.13.0-linux-x86_64-gnu     /usr/lib/python-exec/python3.13/python3 -> ../../../bin/python3.13
# cpython-3.13.0-linux-x86_64-gnu     /usr/lib/python-exec/python3.13/python -> python3
# cpython-3.13.0-linux-x86_64-gnu     /usr/bin/python3.13
# cpython-3.12.5-linux-x86_64-gnu     /usr/bin/python3.12
# cpython-3.12.5-linux-x86_64-gnu     <download available>
# ...
cat example/pyproject.toml
# [project]
# name = "example"
# version = "0.1.0"
# description = "Add your description here"
# readme = "README.md"
# requires-python = ">=3.13"
# dependencies = []
```

Is there a check that fails for release candidates, maybe?

```sh
python3 -V
# Python 3.13.0rc1
uv --version
# uv 0.4.0
```


---

_Label `bug` added by @charliermarsh on 2024-08-29 12:59_

---

_Comment by @charliermarsh on 2024-08-29 12:59_

Yeah it's possible `3.13.0rc1` technically does not satisfy `>=3.13`, but we make an effort to ignore prerelease segments there. We might be missing one. I'll take a look today.

---

_Label `uv python` added by @zanieb on 2024-08-29 13:42_

---

_Referenced in [astral-sh/uv#6813](../../astral-sh/uv/pulls/6813.md) on 2024-08-29 14:22_

---

_Closed by @zanieb on 2024-08-29 16:45_

---

_Comment by @cclauss on 2024-08-30 15:33_

`uv python install 3.13.0rc1` fails?

---

_Comment by @charliermarsh on 2024-08-30 15:44_

3.13 is not yet supported in by Python builds -- need to bring your own. We're working on it though.

---
