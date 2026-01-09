---
number: 14874
title: "`uv run --with` can't find installed ruff in subprocess"
type: issue
state: open
author: charlesnicholson
labels:
  - bug
assignees: []
created_at: 2025-07-24T16:40:29Z
updated_at: 2025-10-23T06:51:36Z
url: https://github.com/astral-sh/uv/issues/14874
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv run --with` can't find installed ruff in subprocess

---

_Issue opened by @charlesnicholson on 2025-07-24 16:40_

### Summary

The following Python script `test.py`:
```
import subprocess
import sys
subprocess.run([sys.executable, "-m", "ruff", "-h"], check=True)
```
fails when run with this invocation:
```
❯ uv run -p python3.13 --with ruff test.py
```

The error message is this:
```
❯ uv run -p python3.13 --with ruff test.py
Installed 1 package in 3ms
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/Users/charles/.cache/uv/archive-v0/sn6gCRNvu9pVNdW-aD0pf/lib/python3.13/site-packages/ruff/__main__.py", line 81, in <module>
    ruff = find_ruff_bin()
  File "/Users/charles/.cache/uv/archive-v0/sn6gCRNvu9pVNdW-aD0pf/lib/python3.13/site-packages/ruff/__main__.py", line 77, in find_ruff_bin
    raise FileNotFoundError(scripts_path)
FileNotFoundError: /Users/charles/.cache/uv/builds-v0/.tmpaiwJLs/bin/ruff
Traceback (most recent call last):
  File "/Users/charles/src/uv-run-test/test.py", line 4, in <module>
    subprocess.run([sys.executable, "-m", "ruff", "-h"], check=True)
    ~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/homebrew/Cellar/python@3.13/3.13.5/Frameworks/Python.framework/Versions/3.13/lib/python3.13/subprocess.py", line 577, in run
    raise CalledProcessError(retcode, process.args, output=stdout, stderr=stderr)
subprocess.CalledProcessError: Command '['/Users/charles/.cache/uv/builds-v0/.tmpaiwJLs/bin/python', '-m', 'ruff', '-h']' returned non-zero exit status 1.
```

It's certainly a wrinkle that I'm trying to launch ruff via a subprocess of the current python, but it also feels like it should work? I'm not sure if this is a bug or me holding it wrong.

Thanks for reading!

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.8.0 (0b2357294 2025-07-17)

### Python version

Python 3.13.5

---

_Label `bug` added by @charlesnicholson on 2025-07-24 16:40_

---

_Comment by @charlesnicholson on 2025-07-24 16:41_

Also, sorry- i'm not sure if this is a `uv` bug or a `ruff` bug but I figured I'd start with `uv` because of the subprocess approach I'm trying.

---

_Comment by @zanieb on 2025-07-24 17:20_

I assume this is an interaction with the environment layering we do for `--with` requirements. I presume it's fixable, but I'll have to see where it's going wrong.

---

_Assigned to @zanieb by @zanieb on 2025-07-24 17:20_

---

_Comment by @charlesnicholson on 2025-07-24 19:02_

Sorry, forgot to mention: uv 0.5.23 does support this use case, so if it's an intended feature then this might be a regression.

---

_Comment by @zanieb on 2025-07-24 20:02_

This looks like a regression from 0.8

```
❯ uvx uv@0.7 run --with ruff python -c 'import subprocess, sys; subprocess.run([sys.executable, "-m", "ruff"])'
Installed 1 package in 7ms
Ruff: An extremely fast Python linter and code formatter.

...

❯ uvx uv@0.7.22 run --with ruff python -c 'import subprocess, sys; subprocess.run([sys.executable, "-m", "ruff"])'
Installed 1 package in 8ms
Installed 1 package in 2ms
Ruff: An extremely fast Python linter and code formatter.

...

❯ uvx uv@0.8 run --with ruff python -c 'import subprocess, sys; subprocess.run([sys.executable, "-m", "ruff"])'
Installed 1 package in 8ms
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/Users/zb/.cache/uv/archive-v0/t9IrbjSm57DWh1Dy5qrYt/lib/python3.13/site-packages/ruff/__main__.py", line 81, in <module>
    ruff = find_ruff_bin()
  File "/Users/zb/.cache/uv/archive-v0/t9IrbjSm57DWh1Dy5qrYt/lib/python3.13/site-packages/ruff/__main__.py", line 77, in find_ruff_bin
    raise FileNotFoundError(scripts_path)
FileNotFoundError: /Users/zb/.cache/uv/builds-v0/.tmp3PcDUA/bin/ruff
```

---

_Referenced in [astral-sh/uv#14877](../../astral-sh/uv/pulls/14877.md) on 2025-07-24 20:18_

---

_Comment by @charliermarsh on 2025-07-24 20:26_

Feels like this is partly just a result of Ruff's limited path discovery. Unsure if it should be fixed in uv, hmm.

---

_Comment by @zanieb on 2025-07-24 20:34_

Yeah I'm not sure either.

---

_Comment by @charlesnicholson on 2025-07-30 19:38_

FWIW we just worked around this via
```
./uv venv -p ${{env.OUR_PYTHON_VERSION}} out/venv
./uv pip install -p out/venv/bin/python $(grep '^ruff' build/constraints-linux.txt)
```
before running our script as a separate command. Not quite as elegant as a `./uv run --with` one-liner but whatevs. :)

---

_Comment by @ofek on 2025-10-23 06:51_

I also encountered this today. As explained in Discord you can easily reproduce like so:

```
❯ docker run --rm -it ghcr.io/astral-sh/uv:debian bash
$ git clone -qqq https://github.com/ofek/dotslash-python && cd dotslash-python
$ DOTSLASH_VERSION="v0.5.8" uv run --with pytest python -c "import shutil,sys,sysconfig;print('proj bin',shutil.which('dotslash'));print('dep bin',shutil.which('pyte
st'));print(f'{sysconfig.get_path('scripts')=}');print(f'{sysconfig.get_path('scripts', vars={'base': sys.base_prefix})=}')"
```

I get the following output, showing that the project binary gets placed at a different path than the one used for dependencies and `sysconfig` appears to only have knowledge of the latter:

```
proj bin /dotslash-python/.venv/bin/dotslash
dep bin /root/.cache/uv/builds-v0/.tmpd8mffh/bin/pytest
sysconfig.get_path('scripts')='/root/.cache/uv/builds-v0/.tmpd8mffh/bin'
sysconfig.get_path('scripts', vars={'base': sys.base_prefix})='/usr/bin'
```

---
