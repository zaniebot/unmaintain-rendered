```yaml
number: 11620
title: Ruff should follow imported notebooks via %run magic
type: issue
state: closed
author: djoshea
labels: []
assignees: []
created_at: 2024-05-30T15:25:19Z
updated_at: 2024-05-30T16:12:41Z
url: https://github.com/astral-sh/ruff/issues/11620
synced_at: 2026-01-12T15:54:51Z
```

# Ruff should follow imported notebooks via %run magic

---

_@djoshea_

I'm working on jupyter notebooks in VSCode, and I have been modularizing the long preambles by putting a bunch of imports into a `preamble.ipynb` notebook, and then running this in my `main.ipynb` notebook via

```python
%run preamble.ipynb
```

This works well, but Ruff doesn't look at the code in the preamble when it lints the main notebook. A simple example would be to have `preamble.ipynb` contain the cell:

```python
import sys
```

And `main.ipynb` contains two cells:

```python
%run preamble.ipynb
```
and 
```python
sys.path.append(...)
```

Then I get an "Undefined name `sys` Ruff (F821)" linting error.

Would it be possible to enable an option to have ruff follow these %run style imports? Or to avoid the X/Y problem, is there a better way to deal with modularizing a long list of imports in my development notebooks that plays well with Ruff?

Thanks!

Keywords: "%run magic" "follow imported notebook" "preamble"
Ruff version: v2024.22.0



---

_Comment by @MichaReiser on 2024-05-30 16:12_

There's some context on why supporting `run` isn't feasible today in https://github.com/astral-sh/ruff/issues/9644. We may support it in the future when we support multifile analysis. 

---

_Closed by @MichaReiser on 2024-05-30 16:12_

---
