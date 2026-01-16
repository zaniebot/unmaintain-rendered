```yaml
number: 2501
title: index-out-of-bounds error after numpy reshape
type: issue
state: open
author: ntjohnson1
labels:
  - needs-info
assignees: []
created_at: 2026-01-14T21:43:29Z
updated_at: 2026-01-16T21:26:23Z
url: https://github.com/astral-sh/ty/issues/2501
synced_at: 2026-01-16T22:14:35Z
```

# index-out-of-bounds error after numpy reshape

---

_@ntjohnson1_

### Summary

I didn't see discussion on numpy specific support or other index related edge cases.

```python
def example(data: int | Sequence[int | float]) -> None:
    if isinstance(data, Sequence):
        np_data = np.array(data).reshape((1, -1))
        if np_data.shape[1] not in (3, 4): # <---
            raise ValueError(f"expected sequence of length of 3 or 4, received {np_data.shape[1]}")
    return None
```
`error[index-out-of-bounds]: Index 0 is out of bounds for tuple `tuple[()]` with length 0`

A simpler version does seem to infer the shape correctly.
```python
  a = [1]
  b = np.array(a).reshape((1, -1))
  c = b.shape[1]
```

This triggers an index-out-of-bounds error (it thinks shape is an empty tuple so shape[0] also doesn't work). Mypy appears to infer the output shape from the reshape call.

Version `ty 0.0.11`

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Comment by @carljm on 2026-01-15 00:03_

Thanks for the report! I'm having trouble reproducing your results. With numpy 2.3.0 or 2.4.1, the snippet you provided works correctly for me (we infer `tuple[int, int]` for the shape). Can you clarify the version of numpy you are using, and verify you are checking exactly the code snippet provided?


---

_Label `needs-info` added by @carljm on 2026-01-15 00:03_

---

_Comment by @ntjohnson1 on 2026-01-15 00:17_

Background context:

In my PR I linked above I added a minimal ty setup for us to start checking it out. Running `uv run ty check` from the root without the index-out-of-bounds ignore triggers on https://github.com/rerun-io/rerun/blob/42f2479854d591c21eea1eccb2c47a4a9b7ae39d/rerun_py/rerun_sdk/rerun/datatypes/rgba32_ext.py#L56 however I pasted the snippet above in that same file and it also triggered.

Minimal standalone repro:
```python
#example.py
import numpy as np
from typing import Sequence

def example(data: int | Sequence[int | float]) -> None:
    if isinstance(data, Sequence):
        np_data = np.array(data).reshape((1, -1))
        if np_data.shape[1] not in (3, 4): # <---
            raise ValueError(f"expected sequence of length of 3 or 4, received {np_data.shape[1]}")
    return None
```

```bash
uv venv --python=3.11
source .venv/bin/activate
uv pip install ty==0.0.11 numpy==2.3.5
ty check example.py
```

---

_Comment by @carljm on 2026-01-15 00:36_

Thanks for the additional info! There still seems to be some mysterious additional factor in play, because when I follow your exact instructions, the issue still doesn't reproduce for me:

```
➜ mkdir issue2501 && cd issue2501

➜ uv venv --python=3.11
Using CPython 3.11.7
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

➜ source .venv/bin/activate

➜ uv pip install ty==0.0.11 numpy==2.3.5
Resolved 2 packages in 110ms
Prepared 1 package in 611ms
Installed 2 packages in 12ms
 + numpy==2.3.5
 + ty==0.0.11

➜ cat >example.py
import numpy as np
from typing import Sequence

def example(data: int | Sequence[int | float]) -> None:
    if isinstance(data, Sequence):
        np_data = np.array(data).reshape((1, -1))
        if np_data.shape[1] not in (3, 4): # <---
            raise ValueError(f"expected sequence of length of 3 or 4, received {np_data.shape[1]}")
    return None

➜ ty check .
All checks passed!
```

(ty actually doesn't care about the Python version in the virtualenv, but explicitly passing `--python-version 3.11` to ty doesn't make any difference, either.)

Is there any chance `ty` in your environment is resolving to some older ty elsewhere, rather than the 0.0.11 just installed in the venv?

---

_Comment by @ntjohnson1 on 2026-01-15 00:57_

WEIRD restarting my shell and the standalone example stopped failing.

It still fails running `uv run ty check` from root on my branch off rerun though (if I don't ignore the index-out-of-bounds rule). I'll probably have time to dig into this a bit more tomorrow to figure out what the disconnect is.

---

_Comment by @ntjohnson1 on 2026-01-16 21:26_

Ok I put together a small repo (not much more code from above but couldn't get it to trigger in a single file) that I am able to reproduce the problem in. I confirmed I saw the error on both my laptops.
https://github.com/ntjohnson1/ty-2501
clone then

```console
uv sync
uv run ty check
```

My guess is that its related to the circular dependency.

---
