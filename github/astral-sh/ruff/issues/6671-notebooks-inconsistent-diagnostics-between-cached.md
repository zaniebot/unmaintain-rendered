---
number: 6671
title: "Notebooks: Inconsistent diagnostics between cached/uncached runs"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
assignees: []
created_at: 2023-08-18T08:14:09Z
updated_at: 2023-09-12T12:59:05Z
url: https://github.com/astral-sh/ruff/issues/6671
synced_at: 2026-01-07T13:12:15-06:00
---

# Notebooks: Inconsistent diagnostics between cached/uncached runs

---

_Issue opened by @MichaReiser on 2023-08-18 08:14_

## Input

Notebook:

```ipynb
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import random\n",
    "import math"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Some explanation\n",
    "\n",
    "See below"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "try:\n",
    "    print()\n",
    "except ValueError:\n",
    "    pass"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import random\n",
    "import pprint\n",
    "\n",
    "random.randint(10, 20)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "foo = 1\n",
    "if foo is 2:\n",
    "    raise ValueError(f\"Invalid foo: {foo is 1}\")"
   ]
  }
 ],
 "metadata": {
  "language_info": {
   "name": "python"
  },
  "orig_nbformat": 4
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
```

## Actual Behavior

**No cache**

```bash
 cargo run --bin ruff -- check ../test/test-notebook.ipynb --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check ../test/test-notebook.ipynb --no-cache`
/home/micha/astral/test/test-notebook.ipynb:cell 1:2:8: F401 [*] `math` imported but unused
/home/micha/astral/test/test-notebook.ipynb:cell 5:1:8: F811 Redefinition of unused `random` from line 1
/home/micha/astral/test/test-notebook.ipynb:cell 5:2:8: F401 [*] `pprint` imported but unused
/home/micha/astral/test/test-notebook.ipynb:cell 7:2:4: F632 [*] Use `==` to compare constant literals
/home/micha/astral/test/test-notebook.ipynb:cell 7:3:38: F632 [*] Use `==` to compare constant literals
Found 5 errors.
[*] 4 potentially fixable with the --fix option.
```

**Cached**

```bash
cargo run --bin ruff -- check ../test/test-notebook.ipynb 
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check ../test/test-notebook.ipynb`
warning: Detected debug build without --no-cache.
/home/micha/astral/test/test-notebook.ipynb:2:8: F401 [*] `math` imported but unused
/home/micha/astral/test/test-notebook.ipynb:7:8: F811 Redefinition of unused `random` from line 1
/home/micha/astral/test/test-notebook.ipynb:8:8: F401 [*] `pprint` imported but unused
/home/micha/astral/test/test-notebook.ipynb:13:4: F632 [*] Use `==` to compare constant literals
/home/micha/astral/test/test-notebook.ipynb:14:38: F632 [*] Use `==` to compare constant literals
Found 5 errors.
[*] 4 potentially fixable with the --fix option.
```
Notice how the uncached uses `cell: ` positions whereas the cached run uses row/colum only.

## Expected Behavior

The output between cached and uncached runs is identical.

---

_Label `bug` added by @MichaReiser on 2023-08-18 08:14_

---

_Assigned to @dhruvmanila by @MichaReiser on 2023-08-18 08:14_

---

_Renamed from "Notebooks: Cached diagnostics use row/col instead of cell numbers" to "Notebooks: Inconsistent diagnostics between cached/uncached runs" by @MichaReiser on 2023-08-18 08:15_

---

_Referenced in [astral-sh/ruff#6863](../../astral-sh/ruff/pulls/6863.md) on 2023-08-25 06:15_

---

_Closed by @dhruvmanila on 2023-09-12 12:59_

---
