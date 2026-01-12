```yaml
number: 9490
title: Failed to create cache file when trying to run ruff
type: issue
state: closed
author: golgor
labels: []
assignees: []
created_at: 2024-01-12T11:58:02Z
updated_at: 2024-01-12T16:11:07Z
url: https://github.com/astral-sh/ruff/issues/9490
synced_at: 2026-01-12T15:54:49Z
```

# Failed to create cache file when trying to run ruff

---

_@golgor_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I'm using pyenv and had created a new virtual for Python 3.12.0. I installed ruff using pip the regular way (`pip install ruff`) in a new project folder, but I got an error when trying to run anything:
```sh
$ ruff check .            
ruff failed
  Cause: Failed to create cache file '/home/golgor/Code/phoenix-test-bench/.ruff_cache/0.1.12/1189904080403456301'
  Cause: No such file or directory (os error 2)
```
I am running zsh on Fedora Linux, but I also tried in bash, both in VS Code and system terminal. I only installed ruff and tried to run it immediately so no configurations or anything.

I did some tests and noticed that it seems to only be the latest release.

```sh
$ pip install ruff==0.1.10
$ ruff --version
ruff 0.1.10
$ ruff check .
pbt/hardware/nina.py:222:33: E711 Comparison to `None` should be `cond is None`
pbt/hardware/rs485_interface.py:123:12: E721 Do not compare types, use `isinstance()`
Found 2 errors.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```
```sh
$ pip install ruff==0.1.11
$ ruff --version
ruff 0.1.11
$ ruff check .
pbt/hardware/nina.py:222:33: E711 Comparison to `None` should be `cond is None`
pbt/hardware/rs485_interface.py:123:12: E721 Do not compare types, use `isinstance()`
Found 2 errors.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```
```sh
$ pip install ruff==0.1.12
$ ruff --version
ruff 0.1.12
$ ruff check .            
ruff failed
  Cause: Failed to create cache file '/home/golgor/Code/phoenix-test-bench/.ruff_cache/0.1.12/1189904080403456301'
  Cause: No such file or directory (os error 2)
```

I tried to run it with the --isolation option, and then it worked fine and after this the regular command works fine as well. Not sure if this is a bug or just some wierd environment issue for me. Unfortunately I can't reproduce it anymore, maybe someone else has had a similar issue?

---

_Comment by @jonasgeiler on 2024-01-12 12:05_

Same problem here, my GitHub Action runs started failing since yesterday... I can provide the log archive if needed, error is pretty much the same though:
```
ruff failed
  Cause: Failed to create cache file '/home/runner/work/genai/genai/.ruff_cache/0.1.12/12636055692318189605'
  Cause: No such file or directory (os error 2)
Error: Process completed with exit code 2.
```

---

_Comment by @trag1c on 2024-01-12 12:06_

I believe this is a duplicate of #9489

---

_Comment by @charliermarsh on 2024-01-12 16:11_

Should be fixed now via [v0.1.13](https://github.com/astral-sh/ruff/releases/tag/v0.1.13) -- please let me know if not!

---

_Closed by @charliermarsh on 2024-01-12 16:11_

---
