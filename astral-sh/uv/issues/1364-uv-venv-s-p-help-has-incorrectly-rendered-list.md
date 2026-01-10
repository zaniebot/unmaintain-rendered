```yaml
number: 1364
title: "`uv venv`'s `-p` help has incorrectly rendered list"
type: issue
state: closed
author: trag1c
labels:
  - bug
  - documentation
  - good first issue
assignees: []
created_at: 2024-02-15T21:48:37Z
updated_at: 2024-02-16T14:36:38Z
url: https://github.com/astral-sh/uv/issues/1364
synced_at: 2026-01-10T05:40:31Z
```

# `uv venv`'s `-p` help has incorrectly rendered list

---

_Issue opened by @trag1c on 2024-02-15 21:48_

```
λ ~ :: uv help venv
Create a virtual environment

Usage: uv venv [OPTIONS] [NAME]

Arguments:
  [NAME]
          The path to the virtual environment to create

          [default: .venv]

Options:
  -p, --python <PYTHON>
          The Python interpreter to use for the virtual environment.

          Supported formats: * `-p 3.10` searches for an installed Python 3.10 (`py --list-paths` on Windows, `python3.10` on Linux/Mac). Specifying a patch version is not supported. * `-p python3.10` or `-p python.exe` looks for a binary in `PATH`. * `-p /home/ferris/.local/bin/python3.10` uses this exact Python.

          Note that this is different from `--python-version` in `pip compile`, which takes `3.10` or `3.10.13` and doesn't look for a Python interpreter on disk.
```
I'd expect it to be:
```
λ ~ :: uv help venv
Create a virtual environment

Usage: uv venv [OPTIONS] [NAME]

Arguments:
  [NAME]
          The path to the virtual environment to create

          [default: .venv]

Options:
  -p, --python <PYTHON>
          The Python interpreter to use for the virtual environment.

          Supported formats:
            * `-p 3.10` searches for an installed Python 3.10 (`py --list-paths` on Windows, `python3.10` on Linux/Mac). Specifying a patch version is not supported.
            * `-p python3.10` or `-p python.exe` looks for a binary in `PATH`.
            * `-p /home/ferris/.local/bin/python3.10` uses this exact Python.

          Note that this is different from `--python-version` in `pip compile`, which takes `3.10` or `3.10.13` and doesn't look for a Python interpreter on disk.
```

---

_Label `bug` added by @MichaReiser on 2024-02-15 21:49_

---

_Comment by @charliermarsh on 2024-02-15 21:50_

Oops, thanks.

---

_Comment by @zanieb on 2024-02-15 21:50_

Thanks! I'm not sure how to fix this rendering. We'll need to look into Clap's documentation.

---

_Label `documentation` added by @zanieb on 2024-02-15 21:50_

---

_Comment by @MichaReiser on 2024-02-15 22:13_

You can try `#[clap(verbatim_doc_comment)]` on the option. But it turns off some other comment transformations

---

_Label `good first issue` added by @charliermarsh on 2024-02-16 05:06_

---

_Closed by @MichaReiser on 2024-02-16 14:36_

---
