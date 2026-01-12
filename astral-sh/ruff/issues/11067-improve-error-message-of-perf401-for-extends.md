```yaml
number: 11067
title: improve error message of PERF401 for extends
type: issue
state: closed
author: trim21
labels: []
assignees: []
created_at: 2024-04-21T07:10:35Z
updated_at: 2024-05-03T01:30:31Z
url: https://github.com/astral-sh/ruff/issues/11067
synced_at: 2026-01-12T15:54:50Z
```

# improve error message of PERF401 for extends

---

_@trim21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

```python
import os
from pathlib import Path
from typing import Any


directory: Any = []

files: list[Path] = []
for p in directory:
    if p.is_file():
        files.append(p)
    else:
        for dir, _, _files in os.walk(p):
            for _f in _files:
                files.append(Path(dir).joinpath(_f)) # PERF401 Use a list comprehension to create a transformed list

```


https://play.ruff.rs/cde69c3f-1d15-4089-bb62-5de79566898a

ruff version 0.4.1

(Is this even possible to re-write with list comprehension?)

---

_Comment by @JonathanPlasse on 2024-04-21 12:53_

You can rewrite it like this. Only the inner for loops need to be rewritten.
```python
import os
from pathlib import Path
from typing import Any


directory: Any = []

files: list[Path] = []
for p in directory:
    if p.is_file():
        files.append(p)
    else:
        files.extend(
            Path(dir).joinpath(_f) for dir, _, _files in os.walk(p) for _f in _files
        )
```

---

_Comment by @trim21 on 2024-04-21 13:00_

> You can rewrite it like this. Only the inner for loops need to be rewritten.
> 
> ```python
> import os
> from pathlib import Path
> from typing import Any
> 
> 
> directory: Any = []
> 
> files: list[Path] = []
> for p in directory:
>     if p.is_file():
>         files.append(p)
>     else:
>         files.extend(
>             Path(dir).joinpath(_f) for dir, _, _files in os.walk(p) for _f in _files
>         )
> ```

so the problem is error message not very clear…

---

_Comment by @JonathanPlasse on 2024-04-21 13:04_

Indeed

---

_Comment by @trim21 on 2024-04-21 13:21_

I'm not a fan of nested loop in list comprehension repr, I would just disable this rule and leave this issue open...

---

_Comment by @JonathanPlasse on 2024-04-21 13:28_

The [documentation](https://docs.astral.sh/ruff/rules/manual-list-comprehension/) does precise that `.extend` can be used for existing lists.

---

_Renamed from "false positive of PERF401 for complex append" to "improve error message of PERF401 for extends" by @trim21 on 2024-04-21 17:00_

---

_Comment by @charliermarsh on 2024-05-03 01:30_

I think this is actually okay because it _is_ included in the documentation, and it would be hard to know whether or not to recommend this in the error message itself.

---

_Closed by @charliermarsh on 2024-05-03 01:30_

---
