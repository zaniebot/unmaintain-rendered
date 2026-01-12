```yaml
number: 2342
title: F722 and F821 errors reported using typing annotations.
type: issue
state: closed
author: KelSolaar
labels: []
assignees: []
created_at: 2023-01-30T08:43:01Z
updated_at: 2024-02-15T16:02:49Z
url: https://github.com/astral-sh/ruff/issues/2342
synced_at: 2026-01-12T15:54:42Z
```

# F722 and F821 errors reported using typing annotations.

---

_@KelSolaar_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

Hello,

Using `ruff 0.0.237`, I get similar `F722` and `F821` errors as those reported in #548. I'm opening a new ticket as the previous one is old and closed.

Given the following:

```python
from __future__ import annotations

import numpy as np

from colour.adaptation import CHROMATIC_ADAPTATION_TRANSFORMS
from colour.algebra import matrix_dot, vector_dot, sdiv, sdiv_mode
from colour.hints import ArrayLike, Literal, NDArrayFloat, Union
from colour.utilities import (
    from_range_1,
    row_as_diagonal,
    to_domain_1,
    validate_method,
)

def matrix_chromatic_adaptation_VonKries(
    XYZ_w: ArrayLike,
    XYZ_wr: ArrayLike,
    transform: Union[
        Literal[
            "Bianco 2010",
            "Bianco PC 2010",
            "Bradford",
            "CAT02 Brill 2008",
            "CAT02",
            "CAT16",
            "CMCCAT2000",
            "CMCCAT97",
            "Fairchild",
            "Sharp",
            "Von Kries",
            "XYZ Scaling",
        ],
        str,
    ] = "CAT02",
) -> NDArrayFloat:
    ...
```

```
colour/adaptation/vonkries.py:49:13: F722 Syntax error in forward annotation: `Bianco 2010`
colour/adaptation/vonkries.py:50:13: F722 Syntax error in forward annotation: `Bianco PC 2010`
colour/adaptation/vonkries.py:51:13: F821 Undefined name `Bradford`
colour/adaptation/vonkries.py:52:13: F722 Syntax error in forward annotation: `CAT02 Brill 2008`
colour/adaptation/vonkries.py:53:13: F821 Undefined name `CAT02`
colour/adaptation/vonkries.py:54:13: F821 Undefined name `CAT16`
colour/adaptation/vonkries.py:55:13: F821 Undefined name `CMCCAT2000`
colour/adaptation/vonkries.py:56:13: F821 Undefined name `CMCCAT97`
colour/adaptation/vonkries.py:57:13: F821 Undefined name `Fairchild`
colour/adaptation/vonkries.py:58:13: F821 Undefined name `Sharp`
colour/adaptation/vonkries.py:59:13: F722 Syntax error in forward annotation: `Von Kries`
colour/adaptation/vonkries.py:60:13: F722 Syntax error in forward annotation: `XYZ Scaling`
```

I only have 1413 of them! ;)

Cheers,

Thomas

---

_Comment by @charliermarsh on 2023-01-30 12:17_

So, I think what you need to do here is tell Ruff that the imports from `colour.hints` should be treated as aliases for the `typing` module, like so:

```toml
[tool.ruff]
typing-modules = ["colour.hints"]
```

Can you give that a try? Will reopen if you're still seeing this of course!


---

_Closed by @charliermarsh on 2023-01-30 12:17_

---

_Comment by @KelSolaar on 2023-01-30 18:23_

That was it, thank you!

---

_Comment by @crypdick on 2024-02-15 16:02_

For future visitors, the solution above will throw a warning `warning: The top-level linter settings are deprecated in favour of their counterparts in the lint section. Please update the following options in pyproject.toml: - 'typing-modules' -> 'lint.typing-modules'` . The updated code is:

```
[tool.ruff.lint]
typing-modules = ["colour.hints"]
```

---
