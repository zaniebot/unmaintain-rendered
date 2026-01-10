```yaml
number: 1823
title: "[question/request] processing stdin?"
type: issue
state: closed
author: FrancescElies
labels: []
assignees: []
created_at: 2023-01-12T17:28:26Z
updated_at: 2023-01-13T05:29:43Z
url: https://github.com/astral-sh/ruff/issues/1823
synced_at: 2026-01-10T11:09:44Z
```

# [question/request] processing stdin?

---

_Issue opened by @FrancescElies on 2023-01-12 17:28_

With other tools like black one can pass a file via stdin `cat myfile.py | black -` and it will be formatted.

Could ruff do something similar `cat myfile.py | ruff -`, or does ruff need to know the path of the file?

This could be helpful in the case where an editor would pass the contents of a buffer via stdin and get those contents ruffed.

Thanks in advance

---

_Comment by @charliermarsh on 2023-01-12 17:34_

Yeah this exists!

```
❯ cat setup.py| ruff -
-:1:8: F401 `os` imported but unused
Found 1 error(s).
1 potentially fixable with the --fix option.
ruff on  autofix-flake8-quotes:main [$!⇡] is 📦 v0.0.219 via 🐍 v3.11.0 via 🦀 v1.68.0-nightly
❯ cat setup.py| ruff - --stdin-filename=setup.py
setup.py:1:8: F401 `os` imported but unused
Found 1 error(s).
1 potentially fixable with the --fix option
❯ cat setup.py| ruff - --stdin-filename=setup.py --fix
import sys

from setuptools import setup

sys.stderr.write(
    """
===============================
Unsupported installation method
===============================
ruff no longer supports installation with `python setup.py install`.
Please use `python -m pip install .` instead.
"""
)
sys.exit(1)


# The below code will never execute, however GitHub is particularly
# picky about where it finds Python packaging metadata.
# See: https://github.com/github/feedback/discussions/6456
#
# To be removed once GitHub catches up.

setup(name="ruff", install_requires=[])
```

---

_Closed by @charliermarsh on 2023-01-12 17:34_

---

_Comment by @charliermarsh on 2023-01-12 17:42_

(So broadly speaking: we output the errors, unless you pass `--fix`, in which case we pass you the fixed contents.)

---

_Comment by @FrancescElies on 2023-01-13 05:29_

aha, ok thanks adding `--fix` flag does the trick 🎊 `cat setup.py | ruff - --fix` 

---
