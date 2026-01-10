```yaml
number: 374
title: Support custom builtins
type: issue
state: closed
author: kkpattern
labels:
  - configuration
assignees: []
created_at: 2025-05-14T02:32:45Z
updated_at: 2025-12-23T13:48:16Z
url: https://github.com/astral-sh/ty/issues/374
synced_at: 2026-01-10T01:56:40Z
```

# Support custom builtins

---

_Issue opened by @kkpattern on 2025-05-14 02:32_

Thanks for this awesome tool!

Some python projects have custom builtins defined. For example, we may add a `tr` function to builtins for localization:

```python
# defined somewhere else.
import builtins

def _tr(source: str) -> str:
    "localize a string."
    return source

builtins.__dict__["tr"] = _tr

# in other python scripts.
tr(1)
```

Pyright [support](https://github.com/microsoft/pyright/blob/main/docs/builtins.md) custom builtins with a `__builtins__.pyi` file:

```python
def tr(source: str) -> str: ...
```

```bash
❯ pyright main.py
/.../main.py
  /.../main.py:12:4 - error: Argument of type "Literal[1]" cannot be assigned to parameter "source" of type "str" in function "tr"
    "Literal[1]" is not assignable to "str" (reportArgumentType)
1 error, 0 warnings, 0 informations
```

Customizing builtins may not be a good practice. But considering ruff also [support](https://docs.astral.sh/ruff/settings/#builtins) additional builtins, there should be many Python projects doing this. So it would be nice if `ty` support custom builtins.

---

_Label `wish` added by @MichaReiser on 2025-05-14 05:47_

---

_Comment by @MichaReiser on 2025-05-14 05:48_

Thanks for opening this issue. This makes sense. 

I don't have a great recommendation for you. The only thing that comes to mind is that you could define a custom typeshed and include your custom builtins file in it but that's a lot of work and something like `__builtins__` sounds like a nice alternative.

---

_Label `configuration` added by @AlexWaygood on 2025-05-14 10:47_

---

_Comment by @Julian-J-S on 2025-05-16 07:26_

yes, please 🤓 

Databricks notebooks are another widely used example where this is "required".
Databricks injects lots of essential variables into every notebook like
- `spark`: The `SparkSession` every data operation is build upon
- `dbutils`: Databricks utility functions like notebook parameters/widgets
- `display`: Displaying notebook inside Databrticks with interactive  tables and plots
- much more...

These are all top-level variables available by default.

It is commen (and supported by pyright) to create a `typings/__builtins__.pyi` file like @kkpattern  mentioned.
Example file:
```python
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

_Comment by @Julian-J-S on 2025-10-11 17:09_

Is there any update or plan on this?

Would still be very useful to have 🤓😊

---

_Label `wish` removed by @carljm on 2025-10-30 16:49_

---

_Comment by @carljm on 2025-10-30 16:49_

I don't think we will likely do this for our upcoming beta release, but given the (somewhat surprising, to me) number of upvotes on the issue, maybe we should consider doing it before our GA release.

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:23_

---

_Comment by @Wizzerinus on 2025-12-17 09:09_

I'm thinking to attempt this since it is one of our team's current blockers in adopting ty over basedpyright and probably doesn't involve complicated type system tinkering (having to figure out both workarounds for python typing and the intricacies of how ty works under the hood at the same time is probably too much). If it's reasonable to pick this up of course :)

---

_Comment by @andrewgross on 2025-12-19 18:45_

Adding my +1 for this. I have a similar use case to @Julian-J-S where I have notebooks (also from Databricks) where I have pre-run scripts for Jupyter Kernels:

```
"jupyter.runStartupCommands": [
        "import somelib.helpers.development",
    ],
```

This defines things like `spark`, `dbutils` etc. With `basedpyright` I was using the `typings/__builtins.pyi__` to avoid flagging things defined in my pre-run code. I am not sure if there is a good way to hook into the actual jupyter kernel for these sorts of definitions, but I think just having a way to define the typings would be helpful

---

_Comment by @Wizzerinus on 2025-12-19 20:55_

okie, I think I should ask about the design here before continuing.

the possible designs are:
a) find `__builtins__.pyi` in the root of the project
done: currently in PR (except I need to fix the fact that this file can also be found in a venv package)
b) find `__builtins__.pyi` anywhere? I only know that Pyright supports the first option, but idk if it looks for the file anywhere.
c) have a configuration option where to find the builtins file, default to not look for that file.
d) Some other option.

Pyright uses `__builtins__.pyi` found somewhere (I'm not sure where).
As far as I know, Mypy, Pyrefly, and Pycharm do not support custom builtins (Pycharm supports custom names, but they will be untyped).

I don't think I should really discuss the design here a lot since I think either version accomplishes the same thing from my point of view, and only 1 typechecker supports this currently so there's no convention, so it's more about what would be the most consistent with what ty is currently doing.

---

_Comment by @MichaReiser on 2025-12-19 20:58_

I haven't tried but the documentation suggests that Pyrefly supports some version of this https://pyrefly.org/en/docs/migrating-from-pyright/#extending-builtins

---

_Comment by @Wizzerinus on 2025-12-19 21:08_

That seems like a new mechanic I didn't see last time I scoured the docs, so that's cool. Thank you for finding it!

I'm thinking that we could probably have `__builtins__.pyi` in project root detected by default, and allow overriding the path to the stub in the config. That would mean overriding stubPath would require putting `typings.__builtins__` instead of just `typings` in the config, but also means we're not cornercasing a name that doesn't really mean anything (which alleviates a concern I've heard earlier).

---

_Closed by @MichaReiser on 2025-12-23 13:48_

---
