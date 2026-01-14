```yaml
number: 22563
title: "Rule Request: Require importing common library a certain way"
type: issue
state: open
author: DeflateAwning
labels: []
assignees: []
created_at: 2026-01-14T00:05:32Z
updated_at: 2026-01-14T00:05:32Z
url: https://github.com/astral-sh/ruff/issues/22563
synced_at: 2026-01-14T00:34:05Z
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
