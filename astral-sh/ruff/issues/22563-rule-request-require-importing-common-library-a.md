```yaml
number: 22563
title: "Rule Request: Require importing common library a certain way"
type: issue
state: open
author: DeflateAwning
labels:
  - question
assignees: []
created_at: 2026-01-14T00:05:32Z
updated_at: 2026-01-14T08:57:39Z
url: https://github.com/astral-sh/ruff/issues/22563
synced_at: 2026-01-14T09:34:58Z
```

# Rule Request: Require importing common library a certain way

---

_@DeflateAwning_

### Summary

Have seen a codebase that uses imports like this:

```
from dagster import AssetExecutionContext, AssetIn, Config, asset, get_dagster_logger
```

I propose the following libraries should be imported with the following aliases:

```python
import polars as pl
import pandas as pd
import numpy as np
import dagster as dg
```

Any other ways of importing these libraries (including with the `from` syntax) is non-standard. 

Not sure if this should be a single rule or many rules. There are likely many more of these libraries with sufficient backing that they should be imported as such! 

---

_Comment by @MichaReiser on 2026-01-14 08:07_

Does https://docs.astral.sh/ruff/rules/unconventional-import-alias/ work for you?

---

_Label `question` added by @MichaReiser on 2026-01-14 08:07_

---

_Comment by @DeflateAwning on 2026-01-14 08:55_

Neat! That's very close to what I was looking for.

Do you know what libraries it currently applies to?

---

_Comment by @MichaReiser on 2026-01-14 08:57_

See https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_aliases

---
