---
number: 2238
title: "`uv pip` installs a broken package while `pip` works correctly"
type: issue
state: closed
author: stinodego
labels: []
assignees: []
created_at: 2024-03-06T11:34:24Z
updated_at: 2024-03-06T15:49:15Z
url: https://github.com/astral-sh/uv/issues/2238
synced_at: 2026-01-10T01:23:14Z
---

# `uv pip` installs a broken package while `pip` works correctly

---

_Issue opened by @stinodego on 2024-03-06 11:34_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

This is on Python 3.12.0 with a virtual environment with `uv` 0.1.15 installed:

#### `pip` works
```
$ pip install --force-reinstall xlsx2csv==0.8.2 && python -c 'import xlsx2csv'
Collecting xlsx2csv==0.8.2
  Using cached xlsx2csv-0.8.2-py3-none-any.whl
Installing collected packages: xlsx2csv
  Attempting uninstall: xlsx2csv
    Found existing installation: xlsx2csv 0.8.2
    Uninstalling xlsx2csv-0.8.2:
      Successfully uninstalled xlsx2csv-0.8.2
Successfully installed xlsx2csv-0.8.2
```

#### `uv pip` fails

Installation succeeds, but importing the package doesn't work.

```
$ uv --no-cache pip install --force-reinstall xlsx2csv==0.8.2 && python -c 'import xlsx2csv'
Resolved 1 package in 1ms
Installed 1 package in 7ms
 - xlsx2csv==0.8.2
 + xlsx2csv==0.8.2
/home/stijn/code/polars/.venv/lib/python3.12/site-packages/xlsx2csv.py:836: SyntaxWarning: invalid escape sequence '\['
  '.*\[.*[dmhys].*\]', format_str):
```

No idea what's going on here, but I thought I'd report it.

---

_Comment by @charliermarsh on 2024-03-06 13:43_

So, I believe this is explained here: https://github.com/astral-sh/uv/issues/1928#issuecomment-1968857514. Which is that pip does bytecode compilation by default, and they swallow all errors. So that warning is present in both cases, it's just swallowed by the `pip install`.

Relatedly, you can enable bytecode compilation with `uv` if you'd like as of the latest release:

```
❯ cargo run pip install --compile --force-reinstall xlsx2csv==0.8.2 && source .venv/bin/activate && python -c 'import xlsx2csv'
    Finished dev [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uv pip install --compile --force-reinstall xlsx2csv==0.8.2`
Resolved 1 package in 2ms
Installed 1 package in 8ms
Bytecode compiled 196 files in 65ms
 - xlsx2csv==0.8.2
 + xlsx2csv==0.8.2
```

(But again, it's a real warning in the package; Python just don't show them if bytecode already exists for the file, and pip swallows the warnings when compiling.)

---

_Closed by @charliermarsh on 2024-03-06 13:43_

---

_Comment by @stinodego on 2024-03-06 15:49_

Thanks, `--compile` indeed the resolves the issue. And I learned something new about `pip install`.

Thanks and keep up the great work!

---
