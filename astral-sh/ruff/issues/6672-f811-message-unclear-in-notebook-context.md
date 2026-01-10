```yaml
number: 6672
title: "F811: Message unclear in Notebook context"
type: issue
state: closed
author: MichaReiser
labels:
  - wish
assignees: []
created_at: 2023-08-18T08:24:41Z
updated_at: 2024-01-04T14:02:24Z
url: https://github.com/astral-sh/ruff/issues/6672
synced_at: 2026-01-10T11:09:48Z
```

# F811: Message unclear in Notebook context

---

_Issue opened by @MichaReiser on 2023-08-18 08:24_

## Input

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

## Output

```
test-notebook.ipynb:cell 5:1:8: F811 Redefinition of unused `random` from line 1
```

The message is unclear because the first import is on line 1 in cell 1. 

---

_Label `wish` added by @MichaReiser on 2023-08-18 08:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-04 02:36_

---

_Closed by @charliermarsh on 2024-01-04 14:02_

---
