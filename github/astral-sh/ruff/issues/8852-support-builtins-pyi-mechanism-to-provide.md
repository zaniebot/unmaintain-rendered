---
number: 8852
title: "Support `__builtins__.pyi` mechanism to provide additional builtins"
type: issue
state: open
author: hassec
labels:
  - configuration
assignees: []
created_at: 2023-11-27T14:21:45Z
updated_at: 2024-10-07T08:11:30Z
url: https://github.com/astral-sh/ruff/issues/8852
synced_at: 2026-01-07T13:12:15-06:00
---

# Support `__builtins__.pyi` mechanism to provide additional builtins

---

_Issue opened by @hassec on 2023-11-27 14:21_

Sometimes one has to work in a python environment that exposes additional builtins. 
In pyright on can provide such information via a `__builtins__.pyi` file, [see their docs](https://github.com/microsoft/pyright/blob/main/docs/builtins.md) 

I was wondering what you would think about enabling ruff be able to read such a file too and then e.g. not throw an undefined name error for these special builtins?

---

_Comment by @zanieb on 2023-11-27 15:30_

This seems reasonable but I'm unsure of the performance cost and feasibility with our current setup cc @charliermarsh 

---

_Comment by @charliermarsh on 2023-11-27 16:57_

We do support defining addition builtins via the [`builtins`](https://docs.astral.sh/ruff/settings/#builtins) setting which is also consistent with Flake8. Does that help here?

---

_Comment by @hassec on 2023-11-27 17:32_

Thanks @charliermarsh, I currently use that option, but it requires that I keep that list in sync with the `__builtins__.pyi` and if there is a long list of additional builtins, it clutters the `pyproject.toml` file quite a bit.

For the specific project I'm working on, the `__builtins__.pyi` is something that is generated, being able to not worry about having the two settings in sync would be nice. 

---

_Label `configuration` added by @charliermarsh on 2023-11-28 22:00_

---

_Comment by @cpbotha on 2024-10-04 07:42_

Just FYI, below is an example `__builtins__.pyi` supplied by the Databricks VSCode extension to facilitate local development of Databricks. Note that the built-in values are also typed (with types being imported) so that one gets autocomplete in the IDE.

I am able to re-add the globals to the ruff `builtins` and it seems reasonably happy. It would of course be much more convenient if ruff could somehow extract and apply the list of globals from the same file.

```python
# Typings for Pylance in Visual Studio Code
# see https://github.com/microsoft/pyright/blob/main/docs/builtins.md
from databricks.sdk.runtime import *

from databricks.sdk.runtime import *
from pyspark.sql.session import SparkSession
from pyspark.sql.functions import udf as U
from pyspark.sql.context import SQLContext

udf = U
spark: SparkSession
sc = spark.sparkContext
sqlContext: SQLContext
sql = sqlContext.sql
table = sqlContext.table
getArgument = dbutils.widgets.getArgument

def displayHTML(html): ...

def display(input=None, *args, **kwargs): ...
```

---

_Comment by @MichaReiser on 2024-10-07 08:11_

Our work on red-knot (type checker) builds the foundation to support this in the future.

---
