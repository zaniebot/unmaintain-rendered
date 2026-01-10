```yaml
number: 6057
title: "False positive `A003` when overriding `logging.Filter.filter`"
type: issue
state: closed
author: scur-iolus
labels: []
assignees: []
created_at: 2023-07-25T08:42:58Z
updated_at: 2023-07-25T15:54:36Z
url: https://github.com/astral-sh/ruff/issues/6057
synced_at: 2026-01-10T11:09:48Z
```

# False positive `A003` when overriding `logging.Filter.filter`

---

_Issue opened by @scur-iolus on 2023-07-25 08:42_

It's probably a quite common case to use a custom `logging.Filter`, either to add context to log records or to decide if a given `Record` is to be logged (cf. [Python documentation](https://docs.python.org/3/library/logging.html#logging.Filter.filter)).

However, `Ruff` currently considers it breaks the `A003` rule:
```
Class attribute `filter` is shadowing a Python builtinRuff [A003](https://beta.ruff.rs/docs/rules/builtin-attribute-shadowing)
```
This issue seems to be somewhat related to #5956 and I'm aware that it's probably low-priority.

Here's a minimal reproducible example of the problem:
```
from logging import Filter, LogRecord

class CustomFilter(Filter):
    def filter(self, record: LogRecord) -> bool:  # noqa: A003 is required here
        return super().filter(self, record)
```
I'm using `ruff 0.0.280`.


---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-25 14:02_

---

_Comment by @charliermarsh on 2023-07-25 15:29_

I will special-case some stuff in the standard library for now.

---

_Closed by @charliermarsh on 2023-07-25 15:54_

---
