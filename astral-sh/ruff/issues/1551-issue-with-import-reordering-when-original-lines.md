---
number: 1551
title: issue with import reordering when original lines have noqa comments. 
type: issue
state: closed
author: ghuls
labels:
  - bug
  - isort
assignees: []
created_at: 2023-01-02T10:53:36Z
updated_at: 2023-01-02T21:24:42Z
url: https://github.com/astral-sh/ruff/issues/1551
synced_at: 2026-01-10T01:22:39Z
---

# issue with import reordering when original lines have noqa comments. 

---

_Issue opened by @ghuls on 2023-01-02 10:53_

ruff combines import lines with and without noqa comments

File after `isort`:
```python
from aaa import used_function_a
from bbb import unused_function_b  # noqa: F401
from bbb import first_used_function_b
from ccc import used_function_c
from ddd import used_function_d

used_function_a()
first_used_function_b()
used_function_c()
used_function_d()
```

After `ruff --fix --select I001`
```python
from aaa import used_function_a
from bbb import unused_function_b, unused_function_b  # noqa: F401
from ccc import used_function_c
from ddd import used_function_d

used_function_a()
first_used_function_b()
used_function_c()
used_function_d()
```



---

_Referenced in [pola-rs/polars#5916](../../pola-rs/polars/pulls/5916.md) on 2023-01-02 13:20_

---

_Comment by @charliermarsh on 2023-01-02 17:25_

Hmm yeah. `isort` can do stuff like this too -- for example, this:

```py
from module import (
    # noqa: F401
    foo,
    bar
)
```

Becomes:

```py
from module import bar, foo  # noqa: F401
```

I think `isort` generally puts any `from` import with an inline comment on its own line, which doesn't seem ideal to me either.

Probably the "right" fix here is to output:

```py
from aaa import used_function_a
from bbb import (
    first_used_function_b,
    unused_function_b,  # noqa: F401
)
from ccc import used_function_c
from ddd import used_function_d
```

What do you think?

---

_Label `isort` added by @charliermarsh on 2023-01-02 17:25_

---

_Comment by @ghuls on 2023-01-02 19:27_

I am happy with the latter.

---

_Comment by @charliermarsh on 2023-01-02 19:45_

This might be a regression because I thought we _did_ do this. Regardless, will fix.

---

_Comment by @ghuls on 2023-01-02 20:26_

You do it, but I think only if you have 3 from imports for the same module. It took a bit of time to make this minimal case to reproduce it because of this. 

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-02 21:23_

---

_Label `bug` added by @charliermarsh on 2023-01-02 21:23_

---

_Referenced in [astral-sh/ruff#1562](../../astral-sh/ruff/pulls/1562.md) on 2023-01-02 21:24_

---

_Closed by @charliermarsh on 2023-01-02 21:24_

---
