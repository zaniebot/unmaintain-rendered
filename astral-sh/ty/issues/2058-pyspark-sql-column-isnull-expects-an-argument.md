```yaml
number: 2058
title: pyspark.sql.Column.isNull expects an argument
type: issue
state: closed
author: jdferreira
labels: []
assignees: []
created_at: 2025-12-18T10:13:30Z
updated_at: 2025-12-18T18:19:11Z
url: https://github.com/astral-sh/ty/issues/2058
synced_at: 2026-01-12T15:54:26Z
```

# pyspark.sql.Column.isNull expects an argument

---

_@jdferreira_

### Summary

With this file:

```py
from pyspark.sql import DataFrame
from pyspark.sql import functions as F


def fn(df: DataFrame) -> DataFrame:
    return df.withColumn("foo", F.col("bar").isNull())
```

Checking results in 1 found diagnostic:

```
$ ty check src/x.py
error[missing-argument]: No argument provided for required parameter 1
 --> src/x.py:6:33
  |
5 | def fn(df: DataFrame) -> DataFrame:
6 |     return df.withColumn("foo", F.col("bar").isNull())
  |                                 ^^^^^^^^^^^^^^^^^^^^^
  |
info: Union variant `(Column, /) -> Column` is incompatible with this call site
info: Attempted to call union type `Unknown | ((Column, /) -> Column)`
info: rule `missing-argument` is enabled by default
```

The correct output should be empty. `mypy` correctly handles this.

The issue exists because `pyspark` does some magical things to define functions. I've attached a [`script.py`](https://github.com/user-attachments/files/24232914/script.py) file that contains a minimal working example of this issue (which doesn't require `pyspark`, at all).

I don't know for sure that this is an issue with `ty`, since there are some unconventional definitions happening in `pyspark`, but my intuition tells me that the fact that the code works at runtime means `ty` should be able to handle this.

### Version

ty 0.0.2

---

_Comment by @sharkdp on 2025-12-18 10:32_

Thank you for reporting this.

Have you seen https://github.com/astral-sh/ty/issues/1802#issuecomment-3625969605 (in particular that last comment)?

---

_Comment by @jdferreira on 2025-12-18 12:39_

I swear I tried to look for this. It seems the error reported is precisely the same I have. Sorry for the extra issue 🙈. Unfortunately, I'm stuck at a particular `pyspark` version at work for now, so I guess I'll keep seeing this diagnostic.

Before we close this, however, I'd like to understand if the diagnostics provided for my script above should be considered correct or incorrect, though, specifically because `ty` raises a concern that does not translate to a runtime issue.

---

_Comment by @sharkdp on 2025-12-18 12:49_

For older pyspark versions, and for the MRE in your script.py, please see the discussion in https://github.com/astral-sh/ty/issues/491. This is a weak point in Python's type system (not specific to ty, in general): should a `Callable` attribute (`is_null_magic`) be considered a method descriptor or not?

For your example here, you would like it to be (the callable takes a "`self`" parameter of type `Column`), but there are some other cases where this is not intended. And there is no way to indicate this in the type of a `Callable`.

---

_Comment by @jdferreira on 2025-12-18 18:19_

Thanks for the explanation. Closing as duplicate of #1802.

---

_Closed by @jdferreira on 2025-12-18 18:19_

---
