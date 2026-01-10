```yaml
number: 22452
title: "False positive for F821 in function with `lambda: var` arg when `del var` is present"
type: issue
state: open
author: DeflateAwning
labels:
  - rule
assignees: []
created_at: 2026-01-08T04:20:48Z
updated_at: 2026-01-08T13:36:48Z
url: https://github.com/astral-sh/ruff/issues/22452
synced_at: 2026-01-10T11:10:00Z
```

# False positive for F821 in function with `lambda: var` arg when `del var` is present

---

_Issue opened by @DeflateAwning on 2026-01-08 04:20_

### Summary

False positive for F821 ("Undefined name `var` - [F821](https://docs.astral.sh/ruff/rules/undefined-name)") in function with `lambda: var` argument when `del var` is present.

Minimum example:
```python
def main() -> None:
    def consume(fn) -> None:
        # Pretend this stores the callable for later use
        fn()

    x = 1

    consume(lambda: x)  # Ruff flags this lambda as "F821: Undefined name `x`"

    del x  # The warning is silenced if x is deleted here.
```

Here's a less-minimal example that shows where in our codebase this issue came up:

```python
import polars as pl

records_df: pl.DataFrame = db.read_query_into_polars("""
    SELECT
        entity_key,
        group_id,
        external_ref,
        internal_id,
        event_id,
        source_id
    FROM app_data.entities
""")
logger.info(f"entities: {shape_str(records_df)}")

with log_row_count_change(lambda: records_df, "remove null entity_key"):
    records_df = records_df.filter(pl.col("entity_key").is_not_null())

base_df = base_df.join(records_df, on=["entity_key"], how="inner", validate="1:m")

del records_df
```

### Version

ruff 0.14.5

---

_Comment by @MichaReiser on 2026-01-08 08:13_

Thanks. There are multiple similar issues open related to `F821` and deleting variables (after their use). The closest of those is https://github.com/astral-sh/ruff/issues/9858

What I'm surprised by is that no type checker reports those uses, including ty. @AlexWaygood is this because we special case `lambda`'s and assume they're immediately executed? 

---

_Label `rule` added by @MichaReiser on 2026-01-08 08:13_

---

_Comment by @AlexWaygood on 2026-01-08 13:36_

> What I'm surprised by is that no type checker reports those uses, including ty. [@AlexWaygood](https://github.com/AlexWaygood) is this because we special case `lambda`'s and assume they're immediately executed?

It wouldn't surprise me if other type checkers apply that kind of special-casing (we already have similar special-casing in place for generator expressions, which are similar in that they are also lazy scopes that are nearly always immediately executed). But don't think we do any such special-casing right now. `lambda` scopes are [correctly recognised as lazy](https://github.com/astral-sh/ruff/blob/eea9ad83528a7f492662f6427cdbb6fc2f655bb5/crates/ty_python_semantic/src/semantic_index/scope.rs#L218-L226). And the false negative also exists for non-lambda functions -- ty also doesn't emit an error for this:

```py
def main() -> None:
    def consume(fn) -> None:
        # Pretend this stores the callable for later use
        fn()

    x = 1

    def closure():
        print(x)

    del x
```

I suspect we have some missing logic around `del` statements that means that ty doesn't notice in this situation that the variable has been deleted prior to the end of the scope here.

---
