---
number: 3246
title: PTH-rules should name pathlib
type: issue
state: closed
author: MartinThoma
labels:
  - good first issue
assignees: []
created_at: 2023-02-27T06:38:11Z
updated_at: 2023-03-04T04:33:14Z
url: https://github.com/astral-sh/ruff/issues/3246
synced_at: 2026-01-07T13:12:14-06:00
---

# PTH-rules should name pathlib

---

_Issue opened by @MartinThoma on 2023-02-27 06:38_

Currently, all [PTH rules](https://beta.ruff.rs/docs/rules/#flake8-use-pathlib-pth) are phrased rather confusingly.

## Code

```python
import os

os.unlink("foo.bar")
```

## What ruff does

```
$ ruff --select=PTH foo.py --isolated
foo.py:3:1: PTH108 `os.unlink` should be replaced by `.unlink()`
Found 1 error.
```

## What ruff should do

Telling the user to move `os.unlink` to `.unlink()` is just not helpful. Something like this would be better:

```
$ ruff --select=PTH foo.py --isolated
foo.py:3:1: PTH108 `os.unlink(name)` should be replaced by `Path(name).unlink()`
Found 1 error.
```

I think this might even be auto-fixable.

## Environment

```
$ python --version
Python 3.11.1

$ ruff --version
ruff 0.0.252
```


---

_Comment by @charliermarsh on 2023-02-27 15:17_

Agreed.

---

_Label `good first issue` added by @charliermarsh on 2023-02-27 15:49_

---

_Comment by @evanrittenhouse on 2023-02-28 02:18_

I can try to take this one! Do you have a recommendation for where I should start?

---

_Comment by @charliermarsh on 2023-02-28 02:33_

Great, thanks! It should be almost entirely in `crates/ruff/src/rules/flake8_use_pathlib/violations.rs`.

---

_Comment by @charliermarsh on 2023-02-28 02:33_

E.g., we could change:

```rust
format!("`os.mkdir` should be replaced by `.mkdir()`")
```

...to:

```rust
format!("`os.mkdir(...)` should be replaced by `Path(...).mkdir()`")
```

---

_Comment by @charliermarsh on 2023-02-28 02:34_

(I think it's ok to literally use "..." as a placeholder rather than trying to replicate the input value.)

---

_Referenced in [astral-sh/ruff#3333](../../astral-sh/ruff/pulls/3333.md) on 2023-03-04 03:09_

---

_Closed by @charliermarsh on 2023-03-04 04:33_

---
