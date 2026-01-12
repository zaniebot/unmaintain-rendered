```yaml
number: 4509
title: Improve reference resolution for deferred-annotations-within-classes
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/resolved
created_at: 2023-05-18T20:41:16Z
updated_at: 2023-05-27T07:27:21Z
url: https://github.com/astral-sh/ruff/pull/4509
synced_at: 2026-01-12T03:50:03Z
```

# Improve reference resolution for deferred-annotations-within-classes

---

_Pull request opened by @charliermarsh on 2023-05-18 20:41_

## Summary

This PR modifies our symbol-resolution logic to better align with Pyright and other type checkers when handling deferred annotations in nested scopes.

By way of example, one of our oldest open issues points to Ruff raising a false-positive unused import here:

```py
from __future__ import annotations
from typing import Optional
import datetime # <--- gets removed by ruff
import pydantic

class SomeClass(pydantic.BaseModel):
    datetime: Optional[datetime.datetime]
```

The problem is that `Optional[datetime.datetime]` is evaluated last, since `from __future__ import annotations` is present. When we go to evaluate it, we see that `datetime` is already assigned in the scope, and resolve to _that_.

I did some research, and it looks like Pyright (and Mypy too) defer to the module scope when resolving such annotations, and fallback to resolving locally. For example, this works fine too:

```py
from typing import Optional, TypeAlias
import datetime # <--- gets removed by ruff
import pydantic

class SomeClass(pydantic.BaseModel):
    datetime: Optional[datetime.datetime]

    # X is defined locally, not in the module scope.
    X: TypeAlias = int
    y: X
```

This PR modifies our resolution logic to match Pyright's behavior, which in turn closes two long-standing issues. Note that this is a deviation from Pyflakes, but I think it's good to be more in-line with the type checkers.

Relevant Pyright issue: https://github.com/microsoft/pyright/issues/2644.

Relevant Pyright commit: https://github.com/microsoft/pyright/commit/27bf8c1f2.

Closes #1401.

Closes #2248.


---

_Comment by @github-actions[bot] on 2023-05-18 21:23_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -2, 0 error(s))

<details><summary>airflow (+2, -2)</summary>
<p>

```diff
+ airflow/models/dagbag.py:104:40: F401 [*] `airflow.models.dag.DAG` imported but unused
- airflow/models/dagbag.py:104:40: TCH001 Move application import `airflow.models.dag.DAG` into a type-checking block
+ airflow/models/taskmixin.py:177:45: F401 [*] `airflow.models.operator.Operator` imported but unused
- airflow/models/taskmixin.py:177:45: TCH001 Move application import `airflow.models.operator.Operator` into a type-checking block
```

</p>
</details>
Rules changed: 2

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| F401 | 2 | 2 | 0 |
| TCH001 | 2 | 0 | 2 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.3±0.38ms     3.1 MB/sec    1.01     13.4±0.45ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.06ms     5.2 MB/sec    1.00      3.2±0.07ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   412.2±20.61µs     7.2 MB/sec    1.00   406.3±15.17µs     7.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.21ms     4.6 MB/sec    1.01      5.6±0.17ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.20ms     6.4 MB/sec    1.00      6.3±0.13ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1367.8±51.61µs    12.2 MB/sec    1.08  1473.1±93.21µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.1±6.43µs    19.7 MB/sec    1.02    153.8±8.16µs    19.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.06ms     9.1 MB/sec    1.02      2.9±0.09ms     8.9 MB/sec
parser/large/dataset.py                    1.00      5.1±0.17ms     8.1 MB/sec    1.01      5.1±0.21ms     8.0 MB/sec
parser/numpy/ctypeslib.py                  1.03   998.2±38.79µs    16.7 MB/sec    1.00   972.6±25.19µs    17.1 MB/sec
parser/numpy/globals.py                    1.00    100.9±3.81µs    29.2 MB/sec    1.00    101.3±3.75µs    29.1 MB/sec
parser/pydantic/types.py                   1.01      2.1±0.06ms    12.0 MB/sec    1.00      2.1±0.05ms    12.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.8±0.11ms     2.4 MB/sec    1.01     17.0±0.13ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.00      4.3±0.02ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    437.8±4.97µs     6.7 MB/sec    1.00    440.0±5.50µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.06ms     3.6 MB/sec    1.01      7.2±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.04ms     4.8 MB/sec    1.02      8.6±0.03ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1746.0±8.57µs     9.5 MB/sec    1.03  1799.6±12.87µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.1±2.47µs    15.6 MB/sec    1.01    190.6±3.50µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.01      3.9±0.03ms     6.6 MB/sec
parser/large/dataset.py                    1.00      7.0±0.03ms     5.8 MB/sec    1.32      9.2±0.04ms     4.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1326.2±9.33µs    12.6 MB/sec    1.27   1685.1±8.14µs     9.9 MB/sec
parser/numpy/globals.py                    1.00    138.5±0.90µs    21.3 MB/sec    1.20    166.0±1.48µs    17.8 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.02ms     8.6 MB/sec    1.29      3.8±0.02ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-18 21:48_

---

_Review requested from @konstin by @charliermarsh on 2023-05-18 21:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/context.rs`:123 on 2023-05-19 07:05_

```suggestion
            if let Some(binding_id) = self.scopes.global().get(symbol) {
```

---

_@MichaReiser approved on 2023-05-19 07:08_

---

_@konstin approved on 2023-05-19 09:56_

---

_Merged by @charliermarsh on 2023-05-20 02:54_

---

_Closed by @charliermarsh on 2023-05-20 02:54_

---

_Branch deleted on 2023-05-20 02:54_

---

_Comment by @sshishov on 2023-05-26 22:29_

Hello @charliermarsh ,

I do not know if this issue caused the new false-positive error in such code:

```python
from __future__ import annotations
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from .. import some_module


def some_function(param: str) -> some_module.SupportedAdapters:
    from .. import some_module  # To fight with circular import, like this is util function  <-- this import is reported unused F401

    return cast(
            some_module.SupportedAdapters,
            import_string(f"some_path_here.adapters{param}")(value="init_value"),
        )
```

Or maybe the adapter pattern should use something else?

If everything is valid on `ruff` side, you can just add the comment here and ignore the issue. I do not want to create new unconfirmed issue, actually...

---

_Comment by @charliermarsh on 2023-05-26 22:36_

Hmm, running `ruff check foo.py --select F --isolated` on that file does not yield an unused import error for me. Are you able to produce a reproducible file + command?

---

_Comment by @sshishov on 2023-05-27 07:27_

Thanks @charliermarsh , this is the small example for reproduction (please note that this `TYPE_CHECKING` is required, you can try to delete it and you will get the circular import error).

NOTE: all these files are located inside the same package

**adapter.py**
```python
from __future__ import annotations

from utils import get_name


class Adapter:
    def __repr__(self) -> str:
        return f"{get_name()}()"
```

**main.py**
```python
from utils import get_adapter


if __name__ == "__main__":
    adapter = get_adapter()
    print(repr(adapter))
```

**utils.py**
```python
from __future__ import annotations

from typing import cast, TYPE_CHECKING

from django.utils.module_loading import import_string

if TYPE_CHECKING:
    from adapter import Adapter


def get_adapter() -> Adapter:
    from adapter import Adapter  # this import is marked unused, but deleting it will cause NameError

    return cast(Adapter, import_string("adapter.Adapter")())


def get_name() -> str:
    return "MyAdapter"
```

Run it out with/without problematic string:
```shell
╰─ python main.py
MyAdapter()

╰─ python main.py
Traceback (most recent call last):
  File "/home/user/ruff_example/main.py", line 4, in <module>
    adapter = get_adapter()
              ^^^^^^^^^^^^^
  File "/home/user/ruff_example/utils.py", line 14, in get_adapter
    return cast(Adapter, import_string("adapter.Adapter")())
                ^^^^^^^
NameError: name 'Adapter' is not defined
```

---
