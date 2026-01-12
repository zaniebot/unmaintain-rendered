```yaml
number: 627
title: Freeze when checking project
type: issue
state: closed
author: qarmin
labels:
  - hang
assignees: []
created_at: 2025-06-10T16:05:11Z
updated_at: 2025-06-13T20:47:04Z
url: https://github.com/astral-sh/ty/issues/627
synced_at: 2026-01-12T15:54:23Z
```

# Freeze when checking project

---

_@qarmin_

### Summary

Since about a week ago, whenever I open my project, I experience a very long (possibly indefinite) freeze with the following message:

![Image](https://github.com/user-attachments/assets/93005aa5-5deb-46d8-bcb9-8337cb3c9a55)

The hotspot shows that some seemingly random functions are being executed continuously — mostly related to salsa and ty_python_semantic:
![Image](https://github.com/user-attachments/assets/78713429-327a-4f34-abc1-dcb92257d02d)

I suspect this might be related to the project's virtual environment (venv).

GDB shows a deep call stack with 1300 nested function calls - [gdb.txt](https://github.com/user-attachments/files/20676280/gdb.txt)

Unfortunately, the project is internal, so I’m not able to share it and I don't know how debug this more

### Version

ruff 161446a47a7f49b1da96e2547acdb1f963d9af10 (happens since ~1 week)

---

_Label `hang` added by @carljm on 2025-06-10 16:06_

---

_Label `needs-mre` added by @carljm on 2025-06-10 16:06_

---

_Comment by @carljm on 2025-06-10 16:09_

Thanks for the report! If you have any more time to track this down, it would be useful to a) bisect the issue to a specific commit in ruff repo where it began, and b) narrow down a minimal reproducer by eliminating code from the project (or only checking specific files in the project) and see what makes the issue go away.

---

_Comment by @qarmin on 2025-06-10 16:17_

Bisect points at - https://github.com/astral-sh/ruff/pull/18041

---

_Comment by @qarmin on 2025-06-10 16:31_

It turns out minimizing the code wasn’t as difficult as I expected

Steps to reproduce
- `uv venv; uv pip install pandas`
- Copy this to `a.py`
```
from pandas import DataFrame

class VFNorm(ABC):
    @property
    def df(self) -> DataFrame:
        return self._data

class SingleVisualFieldStatistics:
    def __init__(self, norm: VFNorm, age: float, values: list[PointValue]) -> None:
        self.model = norm.df.copy()
```
- `ty check a.py`

---

_Comment by @AlexWaygood on 2025-06-10 16:44_

Simpler repro (requires uv >= 0.7.9):

1. Save this as `foo.py` in the ruff repo root:
   ```py
   import pandas

   def f(x: pandas.DataFrame):
       y = x.copy()
   ```

2. Run `uv run --no-project --with=pandas cargo run -p ty check foo.py` from the Ruff repo root

---

_Label `needs-mre` removed by @carljm on 2025-06-10 17:03_

---

_Comment by @AlexWaygood on 2025-06-10 17:49_

It's the `DataFrame._getitem_bool_array()` method that's causing us to choke. This method is using an `isinstance()` check to narrow a parameter to an instance of `pandas.core.Series` and then accessing an attribute on the narrowed type. This is an even smaller repro:

```py
from pandas.core.series import Series

def _getitem_bool_array(self, key):
    isinstance(key, Series) and key.index.equals(self)
```

(Needs further minimizing since `Series` is another huge pandas class, but that's all I'll have time for today I think.)

---

_Comment by @AlexWaygood on 2025-06-12 11:42_

Smaller repro...

```py
from pandas.core.indexes.base import Index

def _set_values(key):
    if isinstance(key, Index):
        key = key._values
```

---

_Comment by @AlexWaygood on 2025-06-12 12:39_

I'm struggling to get much further than that.

---

_Comment by @MichaReiser on 2025-06-12 13:26_

> I'm struggling to get much further than that.

Are you saying, you're experiencing a hang too 😆 

---

_Comment by @sharkdp on 2025-06-12 13:34_

Running this with tracing shows that ty is not frozen/hanging. It "just" seems to be some kind of runtime problem. The logs show `infer_expression_types` calls inside `pandas/core/indexes/base.py`:

```
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(10a83) range=190240..190363 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(10a86) range=190737..190769 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(10955) range=154909..154965 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=154901..154906 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(10957) range=155073..155102 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1095b) range=155028..155231 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(10803) range=107725..107744 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(103fb) range=106751..106780 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(103ff) range=106669..106924 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(10940) range=153254..153296 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(10941) range=153387..153427 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=153371..153376 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(10942) range=153443..153460 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(10943) range=153443..153483 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1093e) range=153058..153096 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/indexes/base.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(c1b8) range=159141..159241 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/series.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(c186) range=124885..124935 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/series.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=124868..124882 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/series.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(c188) range=125015..125075 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/series.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=125000..125012 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/series.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(c189) range=125088..125137 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/series.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1af6) range=145701..145761 file=File(System("/home/shark/.cache/uv/archive-v0/qh6Cuh4I2HUnxuFLeH_PT/lib/python3.13/site-packages/pandas/core/frame.py"))
    in ty_python_semantic::types::infer::infer_expression_types with expression=Id(1800) range=52..60 file=File(System("/home/shark/issue_627/main.py"))
    in ty_python_semantic::types::infer::infer_definition_types with range=48..49 file=File(System("/home/shark/issue_627/main.py"))
    in ty_python_semantic::types::infer::infer_scope_types with scope=Id(1001) file=File(System("/home/shark/issue_627/main.py"))
    in ty_python_semantic::types::check_types with file=File(System("/home/shark/issue_627/main.py"))
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check
```

Checking that file in isolation confirms that it is the source of the problem. Nothing immediately stands out, but this file contains one gigantic class `Index`. As I'm reducing the size of that class, the runtime slowly becomes better. Running samply on a somewhat minified version of that file shows that we're spending a lot of time looking up implicit instance attributes in that class, which seems to match well with the bisection result

![Image](https://github.com/user-attachments/assets/c232a5b3-35d8-417e-987c-85bc441d2da7)

---

_Comment by @MichaReiser on 2025-06-12 14:04_

Does it repro both single and multi threaded? I guess the interesting question here is. Are we inferring the same attributes over and over again (some form of cache miss) or is it a very huge cycle or similar that takes forever to resolve

---

_Comment by @sharkdp on 2025-06-12 16:02_

> Does it repro both single and multi threaded?

Yes, this is just a single file. And it's not a hang, it actually finished if you give it enough time (or simplify the file).

> I guess the interesting question here is. Are we inferring the same attributes over and over again (some form of cache miss) or is it a very huge cycle or similar that takes forever to resolve

I haven't investigated, but I'm assuming it's a problem where adding more methods and/or attributes to a class leads to a nonlinear increase in type check time, maybe because each attribute access requires going through all methods. And every method that we're adding is also an attribute, so maybe there's some quadratic growth? Just a wild guess.

---

_Comment by @MichaReiser on 2025-06-12 16:24_

> I haven't investigated, but I'm assuming it's a problem where adding more methods and/or attributes to a class leads to a nonlinear increase in type check time, maybe because each attribute access requires going through all methods. And every method that we're adding is also an attribute, so maybe there's some quadratic growth? Just a wild guess.

That sounds reassuring that everything is working as expected in Salsa. Thank you

---

_Comment by @qarmin on 2025-06-12 17:04_

Another freeze(but maybe easier to reproduce) I have with sqlalchemy

Steps to reproduce
- create `pyproject.toml` with this data
```
[project]
name = "q"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
"sqlalchemy",
]
```
- `uv sync`
- `ty check .`

---

_Comment by @sharkdp on 2025-06-13 09:42_

Just noting down what I have so far.


Creating an MRE for this is really hard. The best I've came up with after a long time of automated and manual simplifications is the following:

<details>

```py
class C: ...


class Index:
    def foo(self, other: Index):
        if self.a is None or other.a is None:
            return

        if self.c != other.c:
            return

        if isinstance(self, C):
            other = Index(other.b)

        if not self.d or not other.d:
            return

    def bar(self, other):
        if not isinstance(other, Index):
            return

        if other.c != self.c:
            return

        if isinstance(self.b, C):
            return

        if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        # repeat > 20 times
```

</details>

Edit: managed to simplify it some more:

<details>

```py
from __future__ import annotations

class A: ...
class C: ...

class Index:
    def foo(self, other: Index):
        if self.a is None or other.a is None:
            return

        if self.c != other.c:
            return

    def bar(self, other: Index):
        if isinstance(self.a, A):
            return

        if other.c != self.c:
            return

        if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        # repeat > 20 times
```

</details>

The number of `if isinstance(other.c, C):` checks in the end controls the runtime. It seems to grow exponentially

`isinstance` checks | Time (s) 
-- | -- 
8 | 0.19
10 | 0.71 
12 | 3.05 
14 | 13.29 
16 | 60.16 

![Image](https://github.com/user-attachments/assets/dfbcf0d1-6dc6-415f-a10f-9ad243865548)


We apparently spend a lot of time evaluating visibility constraints. I also noticed that we repeatedly call `Type::member_lookup_with_policy` for `Index.c` (probably from `other.c`) and for `(Unknown & Index).c` (probably from accessing `c` on the narrowed `self`). It seems to me like we're not using cached values here? Like, I see the same tracing call "member_lookup_with_policy: …" over and over again. We also run into a cycle and call `member_lookup_cycle_recover` over and over (with the same arguments). To be clear, we always run these functions with slightly different calls stacks (involving `infer_expression_types` on various of these `isinstance` checks). But it still seems like we're not correctly returning the cached value maybe?

---

_Comment by @MichaReiser on 2025-06-13 10:51_

Thanks. That's useful. I'll take a look a little later to see if we miss some salsa caching.

---

_Comment by @MichaReiser on 2025-06-13 11:30_

I haven't digged very deep but I don't think this is a salsa problem. It's just that this is a very big cycle and it's two levels deep! I see many `infer_expression_types` similar to this one:

<details>

```
DEBUG infer_expression_types(Id(1493)): execute: result.revisions = QueryRevisions {
    changed_at: R2,
    durability: Durability::LOW,
    origin: Derived(
        [
            Input(
                parsed_module(Id(800)),
            ),
            Input(
                semantic_index(Id(800)),
            ),
            Input(
                Expression._node_ref(Id(1493)),
            ),
            Input(
                global_scope(Id(800)),
            ),
            Input(
                place_table(Id(c00)),
            ),
            Input(
                module_type_symbols::interned_arguments(Id(6c00)),
            ),
            Input(
                module_type_symbols(Id(6c00)),
            ),
            Input(
                ModuleNameIngredient(Id(2000)),
            ),
            Input(
                resolve_module_query(Id(2000)),
            ),
            Input(
                global_scope(Id(2400)),
            ),
            Input(
                place_table(Id(c05)),
            ),
            Input(
                place_by_id::interned_arguments(Id(340a)),
            ),
            Input(
                place_by_id(Id(340a)),
            ),
            Input(
                signature_(Id(5804)),
            ),
            Input(
                infer_expression_type(Id(1407)),
            ),
            Input(
                infer_expression_type(Id(1408)),
            ),
            Input(
                infer_expression_type(Id(1409)),
            ),
            Input(
                infer_expression_type(Id(140a)),
            ),
            Input(
                infer_expression_type(Id(140b)),
            ),
            Input(
                infer_expression_type(Id(140c)),
            ),
            Input(
                infer_expression_type(Id(140d)),
            ),
            Input(
                infer_expression_type(Id(140e)),
            ),
            Input(
                infer_expression_type(Id(140f)),
            ),
            Input(
                infer_expression_type(Id(1410)),
            ),
            Input(
                infer_expression_type(Id(1411)),
            ),
            Input(
                infer_expression_type(Id(1412)),
            ),
            Input(
                infer_expression_type(Id(1413)),
            ),
            Input(
                infer_expression_type(Id(1414)),
            ),
            Input(
                infer_expression_type(Id(1415)),
            ),
            Input(
                infer_expression_type(Id(1416)),
            ),
            Input(
                infer_expression_type(Id(1417)),
            ),
            Input(
                infer_expression_type(Id(1418)),
            ),
            Input(
                infer_expression_type(Id(1419)),
            ),
            Input(
                infer_expression_type(Id(141a)),
            ),
            Input(
                infer_expression_type(Id(141b)),
            ),
            Input(
                infer_expression_type(Id(141c)),
            ),
            Input(
                infer_expression_type(Id(141d)),
            ),
            Input(
                infer_expression_type(Id(141e)),
            ),
            Input(
                infer_expression_type(Id(141f)),
            ),
            Input(
                infer_expression_type(Id(1420)),
            ),
            Input(
                infer_expression_type(Id(1421)),
            ),
            Input(
                infer_expression_type(Id(1422)),
            ),
            Input(
                infer_expression_type(Id(1423)),
            ),
            Input(
                infer_expression_type(Id(1424)),
            ),
            Input(
                infer_expression_type(Id(1425)),
            ),
            Input(
                infer_expression_type(Id(1426)),
            ),
            Input(
                infer_expression_type(Id(1427)),
            ),
            Input(
                infer_expression_type(Id(1428)),
            ),
            Input(
                infer_expression_type(Id(1429)),
            ),
            Input(
                infer_expression_type(Id(142a)),
            ),
            Input(
                infer_expression_type(Id(142b)),
            ),
            Input(
                infer_expression_type(Id(142c)),
            ),
            Input(
                infer_expression_type(Id(142d)),
            ),
            Input(
                infer_expression_type(Id(142e)),
            ),
            Input(
                infer_expression_type(Id(142f)),
            ),
            Input(
                infer_expression_type(Id(1430)),
            ),
            Input(
                infer_expression_type(Id(1431)),
            ),
            Input(
                infer_expression_type(Id(1432)),
            ),
            Input(
                infer_expression_type(Id(1433)),
            ),
            Input(
                infer_expression_type(Id(1434)),
            ),
            Input(
                infer_expression_type(Id(1435)),
            ),
            Input(
                infer_expression_type(Id(1436)),
            ),
            Input(
                infer_expression_type(Id(1437)),
            ),
            Input(
                infer_expression_type(Id(1438)),
            ),
            Input(
                infer_expression_type(Id(1439)),
            ),
            Input(
                infer_expression_type(Id(143a)),
            ),
            Input(
                infer_expression_type(Id(143b)),
            ),
            Input(
                infer_expression_type(Id(143c)),
            ),
            Input(
                infer_expression_type(Id(143d)),
            ),
            Input(
                infer_expression_type(Id(143e)),
            ),
            Input(
                infer_expression_type(Id(143f)),
            ),
            Input(
                infer_expression_type(Id(1440)),
            ),
            Input(
                infer_expression_type(Id(1441)),
            ),
            Input(
                infer_expression_type(Id(1442)),
            ),
            Input(
                infer_expression_type(Id(1443)),
            ),
            Input(
                infer_expression_type(Id(1444)),
            ),
            Input(
                infer_expression_type(Id(1445)),
            ),
            Input(
                infer_expression_type(Id(1446)),
            ),
            Input(
                infer_expression_type(Id(1447)),
            ),
            Input(
                infer_expression_type(Id(1448)),
            ),
            Input(
                infer_expression_type(Id(1449)),
            ),
            Input(
                infer_expression_type(Id(144a)),
            ),
            Input(
                infer_expression_type(Id(144b)),
            ),
            Input(
                infer_expression_type(Id(144c)),
            ),
            Input(
                infer_expression_type(Id(144d)),
            ),
            Input(
                infer_expression_type(Id(144e)),
            ),
            Input(
                infer_expression_type(Id(144f)),
            ),
            Input(
                infer_expression_type(Id(1450)),
            ),
            Input(
                infer_expression_type(Id(1451)),
            ),
            Input(
                infer_expression_type(Id(1452)),
            ),
            Input(
                infer_expression_type(Id(1453)),
            ),
            Input(
                infer_expression_type(Id(1454)),
            ),
            Input(
                infer_expression_type(Id(1455)),
            ),
            Input(
                infer_expression_type(Id(1456)),
            ),
            Input(
                infer_expression_type(Id(1457)),
            ),
            Input(
                infer_expression_type(Id(1458)),
            ),
            Input(
                infer_expression_type(Id(1459)),
            ),
            Input(
                infer_expression_type(Id(145a)),
            ),
            Input(
                infer_expression_type(Id(145b)),
            ),
            Input(
                infer_expression_type(Id(145c)),
            ),
            Input(
                infer_expression_type(Id(145d)),
            ),
            Input(
                infer_expression_type(Id(145e)),
            ),
            Input(
                infer_expression_type(Id(145f)),
            ),
            Input(
                infer_expression_type(Id(1460)),
            ),
            Input(
                infer_expression_type(Id(1461)),
            ),
            Input(
                infer_expression_type(Id(1462)),
            ),
            Input(
                infer_expression_type(Id(1463)),
            ),
            Input(
                infer_expression_type(Id(1464)),
            ),
            Input(
                infer_expression_type(Id(1465)),
            ),
            Input(
                infer_expression_type(Id(1466)),
            ),
            Input(
                infer_expression_type(Id(1467)),
            ),
            Input(
                infer_expression_type(Id(1468)),
            ),
            Input(
                infer_expression_type(Id(1469)),
            ),
            Input(
                infer_expression_type(Id(146a)),
            ),
            Input(
                infer_expression_type(Id(146b)),
            ),
            Input(
                infer_expression_type(Id(146c)),
            ),
            Input(
                infer_expression_type(Id(146d)),
            ),
            Input(
                infer_expression_type(Id(146e)),
            ),
            Input(
                infer_expression_type(Id(146f)),
            ),
            Input(
                infer_expression_type(Id(1470)),
            ),
            Input(
                infer_expression_type(Id(1471)),
            ),
            Input(
                infer_expression_type(Id(1472)),
            ),
            Input(
                infer_expression_type(Id(1473)),
            ),
            Input(
                infer_expression_type(Id(1474)),
            ),
            Input(
                infer_expression_type(Id(1475)),
            ),
            Input(
                infer_expression_type(Id(1476)),
            ),
            Input(
                infer_expression_type(Id(1477)),
            ),
            Input(
                infer_expression_type(Id(1478)),
            ),
            Input(
                infer_expression_type(Id(1479)),
            ),
            Input(
                infer_expression_type(Id(147a)),
            ),
            Input(
                infer_expression_type(Id(147b)),
            ),
            Input(
                infer_expression_type(Id(147c)),
            ),
            Input(
                infer_expression_type(Id(147d)),
            ),
            Input(
                infer_expression_type(Id(147e)),
            ),
            Input(
                infer_expression_type(Id(147f)),
            ),
            Input(
                infer_expression_type(Id(1480)),
            ),
            Input(
                infer_expression_type(Id(1481)),
            ),
            Input(
                infer_expression_type(Id(1482)),
            ),
            Input(
                infer_expression_type(Id(1483)),
            ),
            Input(
                infer_expression_type(Id(1484)),
            ),
            Input(
                infer_expression_type(Id(1485)),
            ),
            Input(
                infer_expression_type(Id(1486)),
            ),
            Input(
                infer_expression_type(Id(1487)),
            ),
            Input(
                infer_expression_type(Id(1488)),
            ),
            Input(
                infer_expression_type(Id(1489)),
            ),
            Input(
                infer_expression_type(Id(148a)),
            ),
            Input(
                infer_expression_type(Id(148b)),
            ),
            Input(
                infer_expression_type(Id(148c)),
            ),
            Input(
                infer_expression_type(Id(148d)),
            ),
            Input(
                infer_expression_type(Id(148e)),
            ),
            Input(
                infer_expression_type(Id(148f)),
            ),
            Input(
                infer_expression_type(Id(1490)),
            ),
            Input(
                infer_expression_type(Id(1491)),
            ),
            Input(
                infer_expression_type(Id(1492)),
            ),
            Input(
                infer_definition_types(Id(1006)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1492)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1491)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1490)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(148f)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(148e)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(148d)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(148c)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(148b)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(148a)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1489)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1488)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1487)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1486)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1485)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1484)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1483)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1482)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1481)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1480)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(147f)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(147e)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(147d)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(147c)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(147b)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(147a)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1479)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1478)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1477)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1476)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1475)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1474)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1473)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1472)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1471)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1470)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(146f)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(146e)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(146d)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(146c)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(146b)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(146a)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1469)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1468)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1467)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1466)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1465)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1464)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1463)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1462)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1461)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1460)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(145f)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(145e)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(145d)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(145c)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(145b)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(145a)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1459)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1458)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1457)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1456)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1455)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1454)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1453)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1452)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1451)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1450)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(144f)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(144e)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(144d)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(144c)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(144b)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(144a)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1449)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1448)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1447)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1446)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1445)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1444)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1443)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1442)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1441)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1440)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(143f)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(143e)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(143d)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(143c)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(143b)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(143a)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1439)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1438)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1437)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1436)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1435)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1434)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1433)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1432)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1431)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1430)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(142f)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(142e)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(142d)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(142c)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(142b)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(142a)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1429)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1428)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1427)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1426)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1425)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1424)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1423)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1422)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1421)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1420)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(141f)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(141e)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(141d)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(141c)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(141b)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(141a)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1419)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1418)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1417)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1416)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1415)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1414)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1413)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1412)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1411)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1410)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(140f)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(140e)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(140d)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(140c)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(140b)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(140a)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1409)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1408)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1407)),
            ),
            Input(
                IntersectionType(Id(6809)),
            ),
            Input(
                class_member_with_policy_::interned_arguments(Id(3c11)),
            ),
            Input(
                class_member_with_policy_(Id(3c11)),
            ),
            Input(
                member_lookup_with_policy_::interned_arguments(Id(4418)),
            ),
            Input(
                member_lookup_with_policy_(Id(4418)),
            ),
            Input(
                place_by_id::interned_arguments(Id(3410)),
            ),
            Input(
                place_by_id(Id(3410)),
            ),
            Input(
                overloads_and_implementation_(Id(5404)),
            ),
            Input(
                explicit_bases_(Id(1800)),
            ),
        ],
    ),
    accumulated_inputs: AtomicInputAccumulatedValues(
        false,
    ),
    verified_final: false,
    extra: QueryRevisionsExtra(
        Some(
            QueryRevisionsExtraInner {
                accumulated: AccumulatedMap {
                    map: [],
                },
                tracked_struct_ids: [],
                cycle_heads: CycleHeads(
                    [
                        CycleHead {
                            database_key_index: infer_expression_types(Id(1408)),
                            iteration_count: IterationCount(
                                1,
                            ),
                        },
                        CycleHead {
                            database_key_index: member_lookup_with_policy_(Id(4418)),
                            iteration_count: IterationCount(
                                0,
                            ),
                        },
                    ],
                ),
                iteration: IterationCount(
                    0,
                ),
            },
        ),
    ),
}
```

</details>

You can see how the query has many dependencies. All those queries need to re-execute until the cycle converges and the nested cycle makes this even worse.

---

_Comment by @MichaReiser on 2025-06-13 11:36_

I think what would be important here is to understand why this code results in a cycle and if they're needed.

---

_Comment by @MichaReiser on 2025-06-13 11:50_

I starred it a bit more and the main thing is probably just that we have a ton of those `infer_expression_types` that all only differ by one extra narrowing constraint. But every branch has more checks than the one before. E.g. here's the diff for two expression types: https://www.diffchecker.com/3fjeVfTH/

Are the narrowing constraints purely accumulative or do we ever do a "compacting" step? E.g. aggregate the narrowing results after an if statement so that later statements don't need to repeat the entire process

Edit: These are the only `member_lookup_with_policy` calls that I see

<details>

```
INFO member_lookup_with_policy: <module 'sys'>.version_info MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: <module 'typing'>.final MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: <module 'typing'>.type_check_only MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: <module 'typing'>.final MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: <module 'typing'>.type_check_only MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Unknown.a MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: <module 'sys'>.version_info MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: <module 'typing'>.final MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Unknown.c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Unknown.__ne__ MemberLookupPolicy(NO_INSTANCE_FALLBACK)
INFO member_lookup_with_policy: <module 'typing_extensions'>.TypeAlias MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: <module 'sys'>.version_info MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: <module 'typing'>.Sequence MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: <module 'typing'>.TypeVar MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: <class 'type'>.__or__ MemberLookupPolicy(NO_INSTANCE_FALLBACK)
INFO member_lookup_with_policy: <class 'tuple[@Todo(Generic tuple specializations), ...]'>.__ror__ MemberLookupPolicy(NO_INSTANCE_FALLBACK)
INFO member_lookup_with_policy: Unknown.b MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Index.__init__ MemberLookupPolicy(NO_INSTANCE_FALLBACK)
INFO member_lookup_with_policy: <module 'typing'>.final MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Unknown.d MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Unknown | Index.d MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Index.d MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Index.__getattribute__ MemberLookupPolicy(NO_INSTANCE_FALLBACK | MRO_NO_OBJECT_FALLBACK)
INFO member_lookup_with_policy: Index.__getattr__ MemberLookupPolicy(NO_INSTANCE_FALLBACK)
INFO member_lookup_with_policy: Unknown | Index.d MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Index.d MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Unknown & Index.c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Index.c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Unknown & Index.c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Index.c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Unknown & Index.c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Index.c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Unknown & Index.c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Index.c MemberLookupPolicy(0x0)
```

</details>

The last few look like duplicates:

<details>

```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
INFO Defaulting to python-platform `darwin`
INFO Python version: Python 3.7, platform: darwin
INFO Indexed 1 file(s)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/builtins.pyi"))), module: Module { name: ModuleName("sys"), kind: Package, file: Some(File(Vendored(VendoredPathBuf("stdlib/sys/__init__.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(Sys) } }).version_info MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/sys/__init__.pyi"))), module: Module { name: ModuleName("typing"), kind: Module, file: Some(File(Vendored(VendoredPathBuf("stdlib/typing.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(Typing) } }).final MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/sys/__init__.pyi"))), module: Module { name: ModuleName("typing"), kind: Module, file: Some(File(Vendored(VendoredPathBuf("stdlib/typing.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(Typing) } }).type_check_only MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/builtins.pyi"))), module: Module { name: ModuleName("typing"), kind: Module, file: Some(File(Vendored(VendoredPathBuf("stdlib/typing.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(Typing) } }).final MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/builtins.pyi"))), module: Module { name: ModuleName("typing"), kind: Module, file: Some(File(Vendored(VendoredPathBuf("stdlib/typing.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(Typing) } }).type_check_only MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Dynamic(Unknown).a MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/_typeshed/__init__.pyi"))), module: Module { name: ModuleName("sys"), kind: Package, file: Some(File(Vendored(VendoredPathBuf("stdlib/sys/__init__.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(Sys) } }).version_info MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/_typeshed/__init__.pyi"))), module: Module { name: ModuleName("typing"), kind: Module, file: Some(File(Vendored(VendoredPathBuf("stdlib/typing.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(Typing) } }).final MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Dynamic(Unknown).c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Dynamic(Unknown).__ne__ MemberLookupPolicy(NO_INSTANCE_FALLBACK)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/builtins.pyi"))), module: Module { name: ModuleName("typing_extensions"), kind: Module, file: Some(File(Vendored(VendoredPathBuf("stdlib/typing_extensions.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(TypingExtensions) } }).TypeAlias MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/typing_extensions.pyi"))), module: Module { name: ModuleName("sys"), kind: Package, file: Some(File(Vendored(VendoredPathBuf("stdlib/sys/__init__.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(Sys) } }).version_info MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/builtins.pyi"))), module: Module { name: ModuleName("typing"), kind: Module, file: Some(File(Vendored(VendoredPathBuf("stdlib/typing.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(Typing) } }).Sequence MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/builtins.pyi"))), module: Module { name: ModuleName("typing"), kind: Module, file: Some(File(Vendored(VendoredPathBuf("stdlib/typing.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(Typing) } }).TypeVar MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: ClassLiteral(ClassLiteral { name: Name("type"), body_scope: ScopeId { [salsa id]: Id(c2c), file: File(Vendored(VendoredPathBuf("stdlib/builtins.pyi"))), file_scope_id: FileScopeId(39), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: Some(Type), dataclass_params: None, dataclass_transformer_params: None }).__or__ MemberLookupPolicy(NO_INSTANCE_FALLBACK)
INFO member_lookup_with_policy: GenericAlias(GenericAlias { origin: ClassLiteral { name: Name("tuple"), body_scope: ScopeId { [salsa id]: Id(e0e), file: File(Vendored(VendoredPathBuf("stdlib/builtins.pyi"))), file_scope_id: FileScopeId(521), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: Some(Tuple), dataclass_params: None, dataclass_transformer_params: None }, specialization: Specialization { generic_context: GenericContext { variables: {TypeVarInstance { name: Name("_T_co"), definition: Some(Definition { [salsa id]: Id(105b), file: File(Vendored(VendoredPathBuf("stdlib/builtins.pyi"))), file_scope: FileScopeId(0), place: ScopedPlaceId(84), kind: Assignment(AssignmentDefinitionKind { target_kind: Single, value: AstNodeRef(Call(ExprCall { range: 1869..1901, func: Name(ExprName { range: 1869..1876, id: Name("TypeVar"), ctx: Load }), arguments: Arguments { range: 1876..1901, args: [StringLiteral(ExprStringLiteral { range: 1877..1884, value: StringLiteralValue { inner: Single(StringLiteral { range: 1877..1884, value: "_T_co", flags: StringLiteralFlags { quote_style: Double, prefix: Empty, triple_quoted: false } }) } })], keywords: [Keyword { range: 1886..1900, arg: Some(Identifier { id: Name("covariant"), range: 1886..1895 }), value: BooleanLiteral(ExprBooleanLiteral { range: 1896..1900, value: true }) }] } })), target: AstNodeRef(Name(ExprName { range: 1861..1866, id: Name("_T_co"), ctx: Store })) }), is_reexported: true, count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::definition::Definition)> } }), bound_or_constraints: None, variance: Covariant, default_ty: None, kind: Legacy }} }, types: [Dynamic(Todo(TodoType("Generic tuple specializations")))] } }).__ror__ MemberLookupPolicy(NO_INSTANCE_FALLBACK)
INFO member_lookup_with_policy: Dynamic(Unknown).b MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> }).__init__ MemberLookupPolicy(NO_INSTANCE_FALLBACK)
INFO member_lookup_with_policy: ModuleLiteral(ModuleLiteralType { importing_file: File(Vendored(VendoredPathBuf("stdlib/types.pyi"))), module: Module { name: ModuleName("typing"), kind: Module, file: Some(File(Vendored(VendoredPathBuf("stdlib/typing.pyi")))), search_path: Some(SearchPath(StandardLibraryVendored(VendoredPathBuf("stdlib")))), known: Some(Typing) } }).final MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Dynamic(Unknown).d MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Union(UnionType { elements: [Dynamic(Unknown), NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> })] }).d MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> }).d MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> }).__getattribute__ MemberLookupPolicy(NO_INSTANCE_FALLBACK | MRO_NO_OBJECT_FALLBACK)
INFO member_lookup_with_policy: NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> }).__getattr__ MemberLookupPolicy(NO_INSTANCE_FALLBACK)
INFO member_lookup_with_policy: Union(UnionType { elements: [Dynamic(Unknown), NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> })] }).d MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> }).d MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Intersection(IntersectionType { positive: {Dynamic(Unknown), NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> })}, negative: {} }).c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: intersection: Id(6809)
INFO member_lookup_with_policy: NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> }).c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Intersection(IntersectionType { positive: {Dynamic(Unknown), NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> })}, negative: {} }).c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: intersection: Id(6809)
INFO member_lookup_with_policy: NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> }).c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Intersection(IntersectionType { positive: {Dynamic(Unknown), NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> })}, negative: {} }).c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: intersection: Id(6809)
INFO member_lookup_with_policy: NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> }).c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: Intersection(IntersectionType { positive: {Dynamic(Unknown), NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> })}, negative: {} }).c MemberLookupPolicy(0x0)
INFO member_lookup_with_policy: intersection: Id(6809)
INFO member_lookup_with_policy: NominalInstance(NominalInstanceType { class: NonGeneric(ClassLiteral { name: Name("Index"), body_scope: ScopeId { [salsa id]: Id(c02), file: File(System("/Users/micha/astral/ruff-wt/lsp-refactr/test.py")), file_scope_id: FileScopeId(2), count: Count { ghost: PhantomData<fn(ty_python_semantic::semantic_index::place::ScopeId)> } }, known: None, dataclass_params: None, dataclass_transformer_params: None }), _phantom: PhantomData<()> }).c MemberLookupPolicy(0x0)
```

</details>

And indeed, member_lookup_with_policy is executed 4 times for intersection: Id(6809). This seems expected, considering that the intersection particpates in a cycle:

<details>

```
DEBUG infer_expression_types(Id(140a)): execute: result.revisions = QueryRevisions {
    changed_at: R2,
    durability: Durability::LOW,
    origin: Derived(
        [
            Input(
                parsed_module(Id(800)),
            ),
            Input(
                semantic_index(Id(800)),
            ),
            Input(
                Expression._node_ref(Id(140a)),
            ),
            Input(
                global_scope(Id(800)),
            ),
            Input(
                place_table(Id(c00)),
            ),
            Input(
                module_type_symbols::interned_arguments(Id(6c00)),
            ),
            Input(
                module_type_symbols(Id(6c00)),
            ),
            Input(
                ModuleNameIngredient(Id(2000)),
            ),
            Input(
                resolve_module_query(Id(2000)),
            ),
            Input(
                global_scope(Id(2400)),
            ),
            Input(
                place_table(Id(c05)),
            ),
            Input(
                place_by_id::interned_arguments(Id(340a)),
            ),
            Input(
                place_by_id(Id(340a)),
            ),
            Input(
                signature_(Id(5804)),
            ),
            Input(
                infer_expression_type(Id(1407)),
            ),
            Input(
                infer_expression_type(Id(1408)),
            ),
            Input(
                infer_expression_type(Id(1409)),
            ),
            Input(
                infer_definition_types(Id(1006)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1409)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1408)),
            ),
            Input(
                all_negative_narrowing_constraints_for_expression(Id(1407)),
            ),
            Input(
                IntersectionType(Id(6809)),
            ),
            Input(
                class_member_with_policy_::interned_arguments(Id(3c11)),
            ),
            Input(
                class_member_with_policy_(Id(3c11)),
            ),
            Input(
                member_lookup_with_policy_::interned_arguments(Id(4418)),
            ),
            Input(
                member_lookup_with_policy_(Id(4418)),
            ),
            Input(
                place_by_id::interned_arguments(Id(3410)),
            ),
            Input(
                place_by_id(Id(3410)),
            ),
            Input(
                overloads_and_implementation_(Id(5404)),
            ),
            Input(
                explicit_bases_(Id(1800)),
            ),
        ],
    ),
    accumulated_inputs: AtomicInputAccumulatedValues(
        false,
    ),
    verified_final: false,
    extra: QueryRevisionsExtra(
        Some(
            QueryRevisionsExtraInner {
                accumulated: AccumulatedMap {
                    map: [],
                },
                tracked_struct_ids: [],
                cycle_heads: CycleHeads(
                    [
                        CycleHead {
                            database_key_index: infer_expression_types(Id(1408)),
                            iteration_count: IterationCount(
                                0,
                            ),
                        },
                        CycleHead {
                            database_key_index: member_lookup_with_policy_(Id(4418)),
                            iteration_count: IterationCount(
                                0,
                            ),
                        },
                    ],
                ),
                iteration: IterationCount(
                    0,
                ),
            },
        ),
    ),
}
```

</details>

I suspect that what could make this worse is that retrieving a cached value from a query participating in a query is significantely more costly than if the query doesn't participate in the cycle (in which case it's mostly an atomic load followed by a comparision). However, it isn't the reason for the exponential runtime which I think is mainly due to us evaluating the same constraints over and over again because every later statement has more constraints than the one before.

---

_Comment by @sharkdp on 2025-06-13 17:38_

> Edit: These are the only `member_lookup_with_policy` calls that I see

This is not what I see. For me second MRE that I just added (but also for the first), I see an exponential number of calls to `member_lookup_with_policy`:

```
path/to/debug/ty check mre.py 2>&1 | rg "member_lookup_with_policy: Index.c" | wc -l
```

`isinstance` checks | `member_lookup_with_policy` calls
-- | --
6 | 517
8 | 2053
10 | 8197
12 | 32773

This is actually exactly 8 × 2^N + 5 calls for N `isinstance` checks.

---

_Comment by @MichaReiser on 2025-06-13 17:40_

Uh interesting. What python version do you check against?

---

_Comment by @sharkdp on 2025-06-13 17:43_

> What python version do you check against?

Doesn't seem to matter? I get the same results with 3.9 and with 3.13. Typeshed also doesn't matter. It's the same behavior with a minimized version of typeshed.

---

_Assigned to @sharkdp by @carljm on 2025-06-13 17:57_

---

_Comment by @carljm on 2025-06-13 18:00_

Regarding the MRE in https://github.com/astral-sh/ty/issues/627#issuecomment-2969761630 -- is `from __future__ import annotations` necessary to repro?

On the one hand, this would not be surprising, since deferred lookup of annotations currently often causes us to have to infer a ton of extra reachability constraints, given our current end-of-scope semantics. If this is the problem, I would expect #210 to potentially help.

On the other hand, I do find it surprising if deferred annotation semantics are involved, because I don't see any type annotations in your MRE...

---

_Added to milestone `Beta` by @carljm on 2025-06-13 18:01_

---

_Comment by @sharkdp on 2025-06-13 18:14_

> Regarding the MRE in [#627 (comment)](https://github.com/astral-sh/ty/issues/627#issuecomment-2969761630) -- is `from __future__ import annotations` necessary to repro?

It is important in *this* MRE. A previous version of the MRE was still closer to the original, and there I could remove `from __future__ import annotations`, but it used `other = ensure_index()` and `ensure_index` returns an `Index`:

<details>

```py
class Index:

    def join(self):
        other = ensure_index()
        if isinstance(self, ABCDatetimeIndex):
            if (self.tz is None) ^ (other.tz is None):
                raise TypeError('Cannot join tz-naive with tz-aware DatetimeIndex')
        if not self._is_multi and (not other._is_multi):
            if pself is not self or pother is not other:
                return pself.join(pother, how=how, level=level, return_indexers=True, sort=sort)
        if self._is_multi or other._is_multi:
            if self.names == other.names:
                pass
            else:
                return self._join_multi(other, how=how)
        if self.dtype != other.dtype:
            return this.join(other, how=how, return_indexers=True)
        elif isinstance(self, ABCCategoricalIndex) and isinstance(other, ABCCategoricalIndex) and (not self.ordered) and (not self.categories.equals(other.categories)):
            other = Index(other._values.reorder_categories(self.categories))
        if self.is_monotonic_increasing and other.is_monotonic_increasing and self._can_use_libjoin and other._can_use_libjoin and (self.is_unique or other.is_unique):
            pass
        elif not self.is_unique or not other.is_unique:
            return self._join_non_unique(other, how=how, sort=sort)

    def equals(self, other: Any):
        if not isinstance(other, Index):
            return False
        if isinstance(self.dtype, StringDtype) and self.dtype and (other.dtype != self.dtype):
            return other.equals(self.astype(object))
        if is_object_dtype(self.dtype) and (not is_object_dtype(other.dtype)):
            return other.equals(self)
        if isinstance(other, ABCMultiIndex):
            return other.equals(self)
        if isinstance(self._values, ExtensionArray):
            return earr.equals(other._data)
        if isinstance(other.dtype, ExtensionDtype):
            return other.equals(self)

def ensure_index() -> Index:
    pass
```

</details>



> On the other hand, I do find it surprising if deferred annotation semantics are involved, because I don't see any type annotations in your MRE...

The `other` parameter is annotated with `Index`.

---

_Comment by @carljm on 2025-06-13 18:20_

> The `other` parameter is annotated with `Index`.

Yes, sorry, I did see that. What I meant to say was "annotations inside the scope where all the `isinstance` checks area." This annotation of `Index` will use end-of-scope lookup in an outer scope, but I don't see anything that would create a bunch of visibility constraints in the outer scope?

Unless we are doing the deferred name lookup on this `Index` annotation wrong, and it _is_ actually considering all the visibility constraints within the method scope?

---

_Comment by @sharkdp on 2025-06-13 18:37_

Before anyone else spends time looking into this, I *think* I know where the problem is.


> Yes, sorry, I did see that. What I meant to say was "annotations inside the scope where all the `isinstance` checks area." This annotation of `Index` will use end-of-scope lookup in an outer scope, but I don't see anything that would create a bunch of visibility constraints in the outer scope?

👍 

> Unless we are doing the deferred name lookup on this `Index` annotation wrong, and it _is_ actually considering all the visibility constraints within the method scope?

No, I don't think that's the problem. I'll try to summarize it when I understand it better.

---

_Comment by @MichaReiser on 2025-06-13 18:43_

> This is not what I see. For me second MRE that I just added (but also for the first), I see an exponential number of calls to member_lookup_with_policy:

What logs are you counting? I'm counting the `tracing::info!("member_lookup_with_policy: {}.{}", self.display(db), name);` calls. 

The part that I repeated a couple of times is this block:

```py
		if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        if isinstance(other.c, C):
            return
        if isinstance(self.b, C):
            return
```

---

_Closed by @carljm on 2025-06-13 19:50_

---

_Comment by @sharkdp on 2025-06-13 20:47_

> What logs are you counting? I'm counting the `tracing::info!("member_lookup_with_policy: {}.{}", self.display(db), name);` calls.
> 
> The part that I repeated a couple of times is this block:

Not sure what was different then. I was doing the exact same thing.

---
