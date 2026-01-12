```yaml
number: 70
title: Crash if no pyproject.toml file is found
type: issue
state: closed
author: HallerPatrick
labels: []
assignees: []
created_at: 2022-09-01T12:48:58Z
updated_at: 2022-09-01T13:15:19Z
url: https://github.com/astral-sh/ruff/issues/70
synced_at: 2026-01-12T15:54:40Z
```

# Crash if no pyproject.toml file is found

---

_@HallerPatrick_

Hey! 

Great project! I just found a bug that occurs if `ruff` cannot find a `pyproject.toml` file. It will assume that
there is a `pyproject.toml` file at `$HOME/.ruff`. When trying to parse it, the program crashes (silently) with a file not found error.

To reproduce:

Try running `ruff` in a location where it cannot find a `pyproject.toml`file at all.

I already forked and fixed the bug :) 



---

_Closed by @charliermarsh on 2022-09-01 13:15_

---

_Comment by @charliermarsh on 2022-09-01 13:15_

Much appreciated!

---
