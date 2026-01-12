```yaml
number: 619
title: Resolution non-deterministically hangs
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-12-12T16:36:58Z
updated_at: 2023-12-12T19:59:47Z
url: https://github.com/astral-sh/uv/issues/619
synced_at: 2026-01-12T15:58:24Z
```

# Resolution non-deterministically hangs

---

_@konstin_

On python 3.10:

```shell
target/profiling/puffin pip-compile scripts/requirements/transformers-extras.in
```
sometimes gets stuck at 
```
⠧ transformers[modelcreation]==4.36.0     
```
just after selecting the transformers version.

The easiest to reproduce is
```shell
hyperfine "target/profiling/puffin pip-compile scripts/requirements/transformers-extras.in"
```
I tried running it pdb but there it never got stuck.

---

_Comment by @charliermarsh on 2023-12-12 16:56_

I can look into this...

---

_Label `bug` added by @charliermarsh on 2023-12-12 16:57_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-12-12 16:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-12 18:19_

---

_Comment by @charliermarsh on 2023-12-12 18:26_

It's hanging in `get_cached_with_callback`...

---

_Comment by @charliermarsh on 2023-12-12 18:41_

Or maybe not? Actually really can't tell where it's hanging now.

---

_Closed by @charliermarsh on 2023-12-12 19:59_

---
