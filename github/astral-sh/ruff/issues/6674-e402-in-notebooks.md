---
number: 6674
title: E402 in Notebooks
type: issue
state: closed
author: MichaReiser
labels:
  - bug
assignees: []
created_at: 2023-08-18T11:32:02Z
updated_at: 2023-08-18T13:21:00Z
url: https://github.com/astral-sh/ruff/issues/6674
synced_at: 2026-01-07T13:12:15-06:00
---

# E402 in Notebooks

---

_Issue opened by @MichaReiser on 2023-08-18 11:32_

```ipynb
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import random"
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
    "    pass\n"
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
    "if foo == 2:\n",
    "    raise ValueError(f\"Invalid foo: {foo == 1}\")"
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

Ruff reports that the `import random` in the cell 5 is not at the top of the file

```py
import random

random.randint(10, 20)
```

```
/home/micha/astral/test/test-notebook.ipynb:cell 5:1:1: E402 Module level import not at top of file
  |
1 | import random
  | ^^^^^^^^^^^^^ E402
2 | 
3 | random.randint(10, 20)
  |
```

However, Ruff doesn't report the error if you remove the `print("Test")` in cell 4. 

 ## Expected outcome

Ruff to either report `E402` for all cells other than the first cell or Ruff to only report `E402` if the import isn't at the top of the cell.


---

_Label `bug` added by @MichaReiser on 2023-08-18 11:32_

---

_Comment by @MichaReiser on 2023-08-18 11:35_

Oh interesting, this isn't specific to notebooks:

```py
try:
    print()
except ValueError:
    pass

print("Test")

import random
```

reports `E401` [Playground](https://play.ruff.rs/d5a8f5a4-49a7-424a-90d6-442850a6f41a)

```py
try:
    print()
except ValueError:
    pass


import random
```

does not [Playground](https://play.ruff.rs/6726c5df-dde4-40ba-9bc9-027be36e9ff9)

I assume this is intentional to support "optional" imports? @charliermarsh 

---

_Comment by @charliermarsh on 2023-08-18 13:19_

Yeah I believe this is intentional. You’re also allowed to use if statements as long as the body only contains imports.

---

_Closed by @MichaReiser on 2023-08-18 13:21_

---
