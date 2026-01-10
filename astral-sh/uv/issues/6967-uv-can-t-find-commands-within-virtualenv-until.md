---
number: 6967
title: "uv can't find commands within virtualenv until executed once with uv run <command>"
type: issue
state: closed
author: klaytron
labels:
  - question
assignees: []
created_at: 2024-09-03T12:43:26Z
updated_at: 2024-09-03T12:53:55Z
url: https://github.com/astral-sh/uv/issues/6967
synced_at: 2026-01-10T01:24:08Z
---

# uv can't find commands within virtualenv until executed once with uv run <command>

---

_Issue opened by @klaytron on 2024-09-03 12:43_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When invoking a command (.e.g. `pytest`) within a uv venv, the command is not found until first run with the explicit `uv run <command>` one time, at which point the package installations take place, and running commands with the venv without `uv run` works.

DESIRED: package installations already take place upon creating or entering the venv.

```
$ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.4.3 x86_64-unknown-linux-gnu
installing to /home/clayton/.cargo/bin
  uv
  uvx
everything's installed!

$ uv sync --frozen
Audited 94 packages in 0.19ms

$ uv venv
Using Python 3.12.3 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ source .venv/bin/activate

(venv) $ pytest
Command 'pytest' not found, but can be installed with:
sudo apt install python3-pytest

(venv) $ uv run pytest
Installed 94 packages in 41ms
# pytest output

(venv) $ pytest
# pytest output
```

platform linux -- Python 3.12.3
uv 0.4.3


---

_Comment by @charliermarsh on 2024-09-03 12:49_

`uv venv` replaces the virtual environment that was created by `uv sync`.

---

_Label `question` added by @charliermarsh on 2024-09-03 12:50_

---

_Comment by @charliermarsh on 2024-09-03 12:50_

(You should remove `uv venv` from the above and just activate it.)

---

_Comment by @klaytron on 2024-09-03 12:53_

thank you for quick reply!

---

_Closed by @klaytron on 2024-09-03 12:53_

---
