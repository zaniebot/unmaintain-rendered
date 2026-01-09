---
number: 4719
title: "`CONTRIBUTING.md` `MkDocs` incomplete: adding that `cargo` is required"
type: issue
state: closed
author: jamesbraza
labels: []
assignees: []
created_at: 2023-05-30T01:46:59Z
updated_at: 2023-05-30T17:44:20Z
url: https://github.com/astral-sh/ruff/issues/4719
synced_at: 2026-01-07T13:12:14-06:00
---

# `CONTRIBUTING.md` `MkDocs` incomplete: adding that `cargo` is required

---

_Issue opened by @jamesbraza on 2023-05-30 01:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I was trying to follow the `MkDocs` section of `CONTRIBUTING.md`: https://github.com/charliermarsh/ruff/blob/main/CONTRIBUTING.md#mkdocs

On a clean machine without `cargo`, after making a new Python 3.10.11 `venv`:

```sh
> pip install -r docs/requirements.txt
> python scripts/generate_mkdocs.py
Traceback (most recent call last):
  File "/path/to/ruff/scripts/generate_mkdocs.py", line 161, in <module>
    main()
  File "/path/to/ruff/scripts/generate_mkdocs.py", line 86, in main
    subprocess.run(["cargo", "dev", "generate-docs"], check=True)
  File "/path/to/.pyenv/versions/3.10.11/lib/python3.10/subprocess.py", line 503, in run
    with Popen(*popenargs, **kwargs) as process:
  File "/path/to/.pyenv/versions/3.10.11/lib/python3.10/subprocess.py", line 971, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "/path/to/.pyenv/versions/3.10.11/lib/python3.10/subprocess.py", line 1863, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'cargo'
```

After installing Rust:
1. `brew install rustup && rustup-init`
2. Entering `1` for normal install
3. Restarting my terminal

I was able to run `python scripts/generate_mkdocs.py` and then `mkdocs serve`.

The request: having the `MkDocs` section mention a Rust installation is required

---

_Referenced in [astral-sh/ruff#4733](../../astral-sh/ruff/pulls/4733.md) on 2023-05-30 17:30_

---

_Closed by @charliermarsh on 2023-05-30 17:39_

---

_Comment by @jamesbraza on 2023-05-30 17:44_

Thanks @charliermarsh for your dedication!

---
