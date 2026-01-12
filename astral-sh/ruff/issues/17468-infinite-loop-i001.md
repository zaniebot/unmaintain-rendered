```yaml
number: 17468
title: "[Infinite loop]: I001"
type: issue
state: closed
author: normzhou
labels:
  - needs-mre
assignees: []
created_at: 2025-04-18T18:20:53Z
updated_at: 2025-05-12T08:23:29Z
url: https://github.com/astral-sh/ruff/issues/17468
synced_at: 2026-01-12T15:54:55Z
```

# [Infinite loop]: I001

---

_@normzhou_

```shell
inference/src/llm/utils.py:9:1: I001 Import block is un-sorted or un-formatted
   |
 7 |   """
 8 |
 9 | / import re
10 | | from typing import Any
11 | |
12 | | import yaml
13 | | from langchain_core.messages import AIMessage, AnyMessage
14 | | from utils.string_stats import shannon_entropy
15 | | from yaml.dumper import SafeDumper
16 | | from yaml.nodes import ScalarNode  # noqua: I001
   | |_________________________________^ I001
   |
   = help: Organize imports

Found 101 errors (100 fixed, 1 remaining).
[*] 1 fixable with the `--fix` option.
make: *** [python-fix] Error 1
(.venv) normzhou@Norms-MacBook-Pro deductive % make python-fix
Verifying python headers...
Successfully verified python headers
Running Python style checks using ruff...

error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `inference/src/llm/utils.py`, the rule codes I001, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```

---

_Comment by @ntBre on 2025-04-18 18:31_

Thanks for the report! Could you please provide a little more information? I can't reproduce this with the contents of the file visible here and selecting `I001`:

```py
import re
from typing import Any

import yaml
from langchain_core.messages import AIMessage, AnyMessage
from utils.string_stats import shannon_entropy
from yaml.dumper import SafeDumper
from yaml.nodes import ScalarNode  # noqua: I001
```

```shell
> ruff check --no-cache --select I001 --fix
All checks passed!
```

As the error message says, the contents of `utils.py` and your `pyproject.toml` could be helpful, as well as the actual command you're running in your `Makefile`.

---

_Label `needs-mre` added by @ntBre on 2025-04-18 18:31_

---

_Renamed from "[Infinite loop]" to "[Infinite loop]: I001" by @MichaReiser on 2025-04-21 10:16_

---

_Closed by @MichaReiser on 2025-05-12 08:23_

---
