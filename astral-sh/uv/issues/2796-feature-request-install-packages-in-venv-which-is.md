---
number: 2796
title: "[feature request] install packages in venv which is in the other dir"
type: issue
state: closed
author: gmankab
labels:
  - question
assignees: []
created_at: 2024-04-03T03:35:14Z
updated_at: 2024-04-03T04:20:14Z
url: https://github.com/astral-sh/uv/issues/2796
synced_at: 2026-01-10T01:23:22Z
---

# [feature request] install packages in venv which is in the other dir

---

_Issue opened by @gmankab on 2024-04-03 03:35_

[feature request] install packages in venv which is in the other dir

let's create a venv in any dir which is not cwd

```shell
mkdir /tmp/myproj
uv venv /tmp/myproj/.venv
```

now to install packages in this venv i should activate it or cd into it

please provide a way to install packages to it without cd and without activating it

my suggestions

```shell
mkdir /tmp/myproj
uv venv /tmp/myproj/.venv --with-uv
/tmp/myproj/.venv/bin/uv pip install rich
```

```shell
mkdir /tmp/myproj
uv venv /tmp/myproj/.venv
uv pip install rich --venv=/tmp/myproj/.venv
```


---

_Comment by @charliermarsh on 2024-04-03 03:35_

You can try: `uv pip install rich --python=/tmp/myproj/.venv/bin/python`. Does that work?

---

_Label `question` added by @charliermarsh on 2024-04-03 03:35_

---

_Comment by @gmankab on 2024-04-03 03:53_

thank you, it works

can you please add info about `--python` argument to `--help`?

feel free to close issue


---

_Comment by @charliermarsh on 2024-04-03 04:03_

Thanks, I think it's in there already!

```
  -p, --python <PYTHON>
          The Python interpreter into which packages should be installed.

          By default, `uv` installs into the virtual environment in the current working directory or
          any parent directory. The `--python` option allows you to specify a different interpreter,
          which is intended for use in continuous integration (CI) environments or other automated
          workflows.

          Supported formats:
          - `3.10` looks for an installed Python 3.10 using `py --list-paths` on Windows, or
            `python3.10` on Linux and macOS.
          - `python3.10` or `python.exe` looks for a binary with the given name in `PATH`.
          - `/home/ferris/.local/bin/python3.10` uses the exact Python at the given path.
```


---

_Closed by @charliermarsh on 2024-04-03 04:03_

---

_Comment by @gmankab on 2024-04-03 04:18_

@charliermarsh

```shell
uv --version
# uv 0.1.28
uv --help | grep '\--python'
# no output
uv pip --help | grep '\--python'
# no output
```


---

_Comment by @charliermarsh on 2024-04-03 04:18_

`--help` needs to be specified on a subcommand, like `uv pip install --help`.

---

_Comment by @gmankab on 2024-04-03 04:19_

got it, thank you

---

_Comment by @charliermarsh on 2024-04-03 04:20_

(The arguments you see when you run `uv --help` are "global" arguments that apply to all commands, but `--python` isn't quite one of them.)

---
