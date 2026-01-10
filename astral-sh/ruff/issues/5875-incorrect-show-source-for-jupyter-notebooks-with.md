```yaml
number: 5875
title: "Incorrect `--show-source` for Jupyter Notebooks with continuous import blocks"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - core
assignees: []
created_at: 2023-07-19T02:13:49Z
updated_at: 2023-07-19T03:59:36Z
url: https://github.com/astral-sh/ruff/issues/5875
synced_at: 2026-01-10T11:09:48Z
```

# Incorrect `--show-source` for Jupyter Notebooks with continuous import blocks

---

_Issue opened by @dhruvmanila on 2023-07-19 02:13_

Take the [test case](https://github.com/astral-sh/ruff/blob/main/crates/ruff/resources/test/fixtures/jupyter/isort.ipynb) as an example where there are two cells with unsorted imports.

When run using the following command:
```console
cargo run --bin ruff -- check --select=I001 --no-cache --isolated crates/ruff/resources/test/fixtures/jupyter/isort.ipynb --show-source
```

The output is a single diagnostic:
```
crates/ruff/resources/test/fixtures/jupyter/isort.ipynb:cell 1:1:1: I001 [*] Import block is un-sorted or un-formatted
  |
1 | / from pathlib import Path
2 | | import random
3 | | import math
4 | | from typing import Any
5 | | import collections
6 | | # Newline should be added here
  | |_^ I001
7 |   def foo():
8 |       pass
  |
  = help: Organize imports

Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

While there are 2 import blocks (separated by a cell boundary). The fix produces the correct number which is 2:
```console
$ cargo run --bin ruff -- check --select=I001 --no-cache --isolated crates/ruff/resources/test/fixtures/jupyter/isort.ipynb --fix       
    Finished dev [unoptimized + debuginfo] target(s) in 0.38s
     Running `target/debug/ruff check --select=I001 --no-cache --isolated crates/ruff/resources/test/fixtures/jupyter/isort.ipynb --fix`
Found 2 errors (2 fixed, 0 remaining).
```

---

_Label `bug` added by @dhruvmanila on 2023-07-19 02:13_

---

_Label `core` added by @dhruvmanila on 2023-07-19 02:13_

---

_Comment by @dhruvmanila on 2023-07-19 02:21_

Oh, I think I found the problem. Should be an easy fix actually :)

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-07-19 02:21_

---

_Closed by @dhruvmanila on 2023-07-19 03:59_

---
