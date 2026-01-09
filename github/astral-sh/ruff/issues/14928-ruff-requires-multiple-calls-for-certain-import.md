---
number: 14928
title: Ruff requires multiple calls for certain import configurations
type: issue
state: closed
author: leep-frog
labels:
  - needs-mre
assignees: []
created_at: 2024-12-12T04:01:02Z
updated_at: 2024-12-12T07:00:46Z
url: https://github.com/astral-sh/ruff/issues/14928
synced_at: 2026-01-07T13:12:16-06:00
---

# Ruff requires multiple calls for certain import configurations

---

_Issue opened by @leep-frog on 2024-12-12 04:01_

I noticed that I need to call ruff multiple times for the following configuration/code:

Configuration:
```
      lint: {
        select: [
          'I001',
          'I002',
        ],
        isort: {
          'required-imports': [
            "from typing import Dict"
          ],
          'lines-after-imports': 2,
          'combine-as-imports': true,
        },
```

Code:

```python
from typing import Any

def func() -> Any:
  pass
```

After first pass of ruff:

```python
from typing import Dict
from typing import Any


def func() -> Any:
    pass
```

After second pass of ruff:

```python
from typing import Any, Dict


def func() -> Any:
    pass
```

Not sure if there is a config flag to add that would allow this to work in one go.  If not, seems like a bug.

---

_Comment by @dylwil3 on 2024-12-12 04:16_

Hmm... I'm not able to reproduce this. Does it look like I'm doing everything you're doing?

```console
❯ uv run ruff --version
ruff 0.8.2

❯ cat ruff.toml
[lint]
select = ["I001","I002"]

[lint.isort]
required-imports = ["from typing import Dict"]
lines-after-imports = 2
combine-as-imports = true

❯ cat ex.py
from typing import Any

def func() -> Any:
  pass%                                     
❯ uv run ruff check --fix ex.py
Found 3 errors (3 fixed, 0 remaining).

❯ cat ex.py
from typing import Any, Dict


def func() -> Any:
  pass%  
```

(For what it's worth, under the hood `ruff check --fix` _is_ running ruff multiple times and applying fixes. So maybe you're invoking Ruff in a different setting where only one iteration is being applied?)

---

_Label `needs-mre` added by @dylwil3 on 2024-12-12 04:17_

---

_Comment by @leep-frog on 2024-12-12 04:37_

Ah I see. I'm using the [ruff-wasm-nodejs](https://www.npmjs.com/package/@astral-sh/ruff-wasm-nodejs) npm package. 

Any chance that can be updated to do the same? 

---

_Comment by @MichaReiser on 2024-12-12 07:00_

I consider this out of scope for the JS API. It only provides an API to get all diagnostics for the initial source text. It doesn't even provide an official API for applying fixes. Instead, that's left to the caller. 

You have to keep calling `check` until it returns no more diagnostics with fixes. You also have to be careful, fixes can overlap and some fixes shouldn't be applied when other fixes are present (e.g. one fix might try to remove an import when another edit adds it or even requires it). 

Overall, the JS api has no support for fixes.

---

_Closed by @MichaReiser on 2024-12-12 07:00_

---
