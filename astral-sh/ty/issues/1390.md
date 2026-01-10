```yaml
number: 1390
title: "[panic] converting pandas dataframe to polars"
type: issue
state: closed
author: nicwest
labels:
  - fatal
assignees: []
created_at: 2025-10-17T18:04:07Z
updated_at: 2025-10-18T13:02:57Z
url: https://github.com/astral-sh/ty/issues/1390
synced_at: 2026-01-10T02:06:25Z
```

# [panic] converting pandas dataframe to polars

---

_Issue opened by @nicwest on 2025-10-17 18:04_

I'm not super familar with pandas or polars but just encountered this panic in some code I was looking at.

```python
import pandas
import polars

polars.from_pandas(pandas.DataFrame())

```

```text
pandas==2.2.3
polars==1.23.0
```

```
◤ (.venv) $ (DEV-11671/ty) ty check foo.py
Checking ------------------------------------------------------------ 1/1 files
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/fetch.rs:264:21 when checking `/home/nic/foo.py`: `dependency graph cycle when querying TypeVarInstance < 'db >::lazy_constraints_(Id(a00f)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_file_impl(Id(c00)),
    infer_scope_types(Id(1000)),
    FunctionType < 'db >::signature_(Id(4403)),
    infer_deferred_types(Id(1524)),
    ClassLiteral < 'db >::is_typed_dict_(Id(5433)),
    ClassLiteral < 'db >::try_mro_(Id(843a)),
    ClassLiteral < 'db >::explicit_bases_(Id(5438)),
    infer_deferred_types(Id(10476)),
    ClassLiteral < 'db >::explicit_bases_(Id(543a)),
    infer_deferred_types(Id(1042e)),
    ClassLiteral < 'db >::explicit_bases_(Id(5447)),
    infer_deferred_types(Id(1042d)),
    TypeVarInstance < 'db >::lazy_constraints_(Id(a00f)),
    infer_deferred_types(Id(10403)),
    ClassLiteral < 'db >::is_typed_dict_(Id(544c)),
    ClassLiteral < 'db >::try_mro_(Id(844d)),
    ClassLiteral < 'db >::explicit_bases_(Id(544d)),
    infer_deferred_types(Id(10477)),
    Type < 'db >::is_redundant_with_(Id(b4d8)),
    ClassLiteral < 'db >::try_mro_(Id(8450)),
    ClassLiteral < 'db >::try_mro_(Id(8451)),
    Type < 'db >::member_lookup_with_policy_(Id(286c)),
    place_by_id(Id(306f)),
    infer_definition_types(Id(e775)),
    Type < 'db >::member_lookup_with_policy_(Id(286d)),
    place_by_id(Id(3070)),
    infer_definition_types(Id(1070c)),
    parsed_module(Id(e76)),
    source_text(Id(e76)),
    infer_definition_types(Id(e46e)),
    Type < 'db >::member_lookup_with_policy_(Id(2868)),
    place_by_id(Id(306c)),
    infer_definition_types(Id(106b7)),
    parsed_module(Id(e73)),
    source_text(Id(e73)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.23
info: Args: ["ty", "check", "foo.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_deferred_types(Id(10477))
             at crates/ty_python_semantic/src/types/infer.rs:145
             cycle heads: ClassLiteral < 'db >::is_typed_dict_(Id(544c)) -> iteration = 0
   1: ClassLiteral < 'db >::explicit_bases_(Id(544d))
             at crates/ty_python_semantic/src/types/class.rs:1415
   2: ClassLiteral < 'db >::try_mro_(Id(844d))
             at crates/ty_python_semantic/src/types/class.rs:1415
   3: ClassLiteral < 'db >::is_typed_dict_(Id(544c))
             at crates/ty_python_semantic/src/types/class.rs:1415
   4: infer_deferred_types(Id(10403))
             at crates/ty_python_semantic/src/types/infer.rs:145
             cycle heads: ClassLiteral < 'db >::is_typed_dict_(Id(5433)) -> iteration = 0
   5: TypeVarInstance < 'db >::lazy_constraints_(Id(a00f))
             at crates/ty_python_semantic/src/types.rs:8172
   6: infer_deferred_types(Id(1042d))
             at crates/ty_python_semantic/src/types/infer.rs:145
   7: ClassLiteral < 'db >::explicit_bases_(Id(5447))
             at crates/ty_python_semantic/src/types/class.rs:1415
   8: infer_deferred_types(Id(1042e))
             at crates/ty_python_semantic/src/types/infer.rs:145
   9: ClassLiteral < 'db >::explicit_bases_(Id(543a))
             at crates/ty_python_semantic/src/types/class.rs:1415
  10: infer_deferred_types(Id(10476))
             at crates/ty_python_semantic/src/types/infer.rs:145
  11: ClassLiteral < 'db >::explicit_bases_(Id(5438))
             at crates/ty_python_semantic/src/types/class.rs:1415
  12: ClassLiteral < 'db >::try_mro_(Id(843a))
             at crates/ty_python_semantic/src/types/class.rs:1415
  13: ClassLiteral < 'db >::is_typed_dict_(Id(5433))
             at crates/ty_python_semantic/src/types/class.rs:1415
  14: infer_deferred_types(Id(1524))
             at crates/ty_python_semantic/src/types/infer.rs:145
  15: FunctionType < 'db >::signature_(Id(4403))
             at crates/ty_python_semantic/src/types/function.rs:697
  16: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:70
  17: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

---

_Comment by @nicwest on 2025-10-17 18:21_

I've now read the https://github.com/astral-sh/ty/issues/445 and it sounds like this a known issue due to a self referential type https://github.com/astral-sh/ty/issues/256

another example I've just encountered.

```python
import pandas

pandas.Timedelta(days=10) < pandas.Timedelta(days=20)
```

turned up an immediately self referential type:

```python
#.venv/lib/python3.12/site-packages/pandas-stubs/_libs/tslibs/timedeltas.py
class Timedelta(timedelta):
    min: ClassVar[Timedelta]  # pyright: ignore[reportIncompatibleVariableOverride]
    max: ClassVar[Timedelta]  # pyright: ignore[reportIncompatibleVariableOverride]
    resolution: ClassVar[  # pyright: ignore[reportIncompatibleVariableOverride]
```
so I'm inclined to believe this is all related.

thanks team, closing this issue as it already appears to be on the radar

---

_Closed by @nicwest on 2025-10-17 18:21_

---

_Comment by @AlexWaygood on 2025-10-17 18:30_

Would you be able to confirm which version of ty you're using? I actually can't reproduce these panics with the latest alpha release :-)

It would also be helpful if you could run `uv pip list` so we could see exactly which packages are installed in your virtual environment (it looks like you have `pandas-stubs` installed?)

---

_Comment by @AlexWaygood on 2025-10-17 18:32_

> I've now read the [#445](https://github.com/astral-sh/ty/issues/445) and it sounds like this a known issue due to a self referential type [#256](https://github.com/astral-sh/ty/issues/256)

We still have some issues to do with self-referential types, but _most_ of them should be fixed now, and the error message here is also a little different to most of those panics. So I'm somewhat curious what the problem here is, actually!

---

_Comment by @nicwest on 2025-10-17 18:52_

> Would you be able to confirm which version of ty you're using?

`info: Version: 0.0.1-alpha.23`

I isolated this out of the large code base into it's own thing:

```
◤ ~ cd things
◤ ~/things mkdir ty-1390
◤ ~/things cd ty-1390/
◤ ~/t/ty-1390 python3 --version
Python 3.12.3
◤ ~/t/ty-1390 python3 -m venv .venv
◤ ~/t/ty-1390 source .venv/bin/activate.fish
◤ (.venv) ~/t/ty-1390 pip install pandas==2.2.3
....
Successfully installed numpy-2.3.4 pandas-2.2.3 python-dateutil-2.9.0.post0 pytz-2025.2 six-1.17.0 tzdata-2025.2
◤ (.venv) ~/t/ty-1390 pip install ty==0.0.1-alpha.23
...
Successfully installed ty-0.0.1a23
◤ (.venv) ~/t/ty-1390 vim foo.py
◤ (.venv) ~/t/ty-1390 cat foo.py
import pandas

pandas.Timedelta(days=10) < pandas.Timedelta(days=20)
◤ (.venv) ~/t/ty-1390 ty check foo.py
Checking ------------------------------------------------------------ 1/1 files
All checks passed!
◤ (.venv) ~/t/ty-1390 python foo.py
```

you are correct! after installing the same version of `pandas-stubs` as the source repo it broke!

```
◤ (.venv) ~/t/ty-1390 pip install pandas-stubs==2.2.3.241126
Successfully installed pandas-stubs-2.2.3.241126 types-pytz-2025.2.0.20250809
◤ (.venv) ~/t/ty-1390 ty check foo.py
Checking ------------------------------------------------------------ 1/1 files
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/fetch.rs:264:21 when checking `/home/nic/things/ty-1390/foo.py`: `dependency graph cycle when querying TypeVarInstance < 'db >::lazy_constraints_(Id(9413)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_file_impl(Id(c00)),
    infer_scope_types(Id(1000)),
    FunctionType < 'db >::signature_(Id(4807)),
    FunctionType < 'db >::signature_(Id(4808)),
    infer_deferred_types(Id(1608)),
    ClassLiteral < 'db >::is_typed_dict_(Id(342b)),
    ClassLiteral < 'db >::try_mro_(Id(5447)),
    ClassLiteral < 'db >::explicit_bases_(Id(3440)),
    infer_deferred_types(Id(e40c)),
    TypeVarInstance < 'db >::lazy_constraints_(Id(9413)),
    infer_deferred_types(Id(e398)),
    ClassLiteral < 'db >::is_typed_dict_(Id(344a)),
    ClassLiteral < 'db >::try_mro_(Id(545c)),
    ClassLiteral < 'db >::explicit_bases_(Id(344b)),
    infer_deferred_types(Id(e40b)),
    ClassLiteral < 'db >::explicit_bases_(Id(344c)),
    infer_deferred_types(Id(e3c3)),
    ClassLiteral < 'db >::explicit_bases_(Id(3457)),
    infer_deferred_types(Id(e3c2)),
    Type < 'db >::is_redundant_with_(Id(8cf4)),
    ClassLiteral < 'db >::try_mro_(Id(5462)),
    ClassLiteral < 'db >::try_mro_(Id(5463)),
    ClassLiteral < 'db >::try_mro_(Id(545e)),
    infer_definition_types(Id(e34e)),
    Type < 'db >::member_lookup_with_policy_(Id(2866)),
    place_by_id(Id(3072)),
    infer_definition_types(Id(c3bb)),
    Type < 'db >::member_lookup_with_policy_(Id(2868)),
    source_text(Id(cb1)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.23
info: Args: ["ty", "check", "foo.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_deferred_types(Id(e3c2))
             at crates/ty_python_semantic/src/types/infer.rs:145
   1: ClassLiteral < 'db >::explicit_bases_(Id(3457))
             at crates/ty_python_semantic/src/types/class.rs:1415
   2: infer_deferred_types(Id(e3c3))
             at crates/ty_python_semantic/src/types/infer.rs:145
   3: ClassLiteral < 'db >::explicit_bases_(Id(344c))
             at crates/ty_python_semantic/src/types/class.rs:1415
   4: infer_deferred_types(Id(e40b))
             at crates/ty_python_semantic/src/types/infer.rs:145
   5: ClassLiteral < 'db >::explicit_bases_(Id(344b))
             at crates/ty_python_semantic/src/types/class.rs:1415
   6: ClassLiteral < 'db >::try_mro_(Id(545c))
             at crates/ty_python_semantic/src/types/class.rs:1415
   7: ClassLiteral < 'db >::is_typed_dict_(Id(344a))
             at crates/ty_python_semantic/src/types/class.rs:1415
   8: infer_deferred_types(Id(e398))
             at crates/ty_python_semantic/src/types/infer.rs:145
   9: TypeVarInstance < 'db >::lazy_constraints_(Id(9413))
             at crates/ty_python_semantic/src/types.rs:8172
  10: infer_deferred_types(Id(e40c))
             at crates/ty_python_semantic/src/types/infer.rs:145
             cycle heads: ClassLiteral < 'db >::is_typed_dict_(Id(342b)) -> iteration = 0
  11: ClassLiteral < 'db >::explicit_bases_(Id(3440))
             at crates/ty_python_semantic/src/types/class.rs:1415
  12: ClassLiteral < 'db >::try_mro_(Id(5447))
             at crates/ty_python_semantic/src/types/class.rs:1415
  13: ClassLiteral < 'db >::is_typed_dict_(Id(342b))
             at crates/ty_python_semantic/src/types/class.rs:1415
  14: infer_deferred_types(Id(1608))
             at crates/ty_python_semantic/src/types/infer.rs:145
  15: FunctionType < 'db >::signature_(Id(4808))
             at crates/ty_python_semantic/src/types/function.rs:697
             cycle heads: FunctionType < 'db >::signature_(Id(4808)) -> iteration = 0
  16: FunctionType < 'db >::signature_(Id(4807))
             at crates/ty_python_semantic/src/types/function.rs:697
  17: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:70
  18: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
◤ (.venv) ~/t/ty-1390 pip freeze
numpy==2.3.4
pandas==2.2.3
pandas-stubs==2.2.3.241126
python-dateutil==2.9.0.post0
pytz==2025.2
six==1.17.0
ty==0.0.1a23
types-pytz==2025.2.0.20250809
tzdata==2025.2
```

---

_Comment by @AlexWaygood on 2025-10-17 18:58_

> you are correct! after installing the same version of `pandas-stubs` as the source repo it broke!

Aha! And now I can reproduce, great!

---

_Comment by @nicwest on 2025-10-17 19:00_

here's the polars thing:

```
◤ (.venv) ~/t/ty-1390 pip install polars==1.23.0
...
Successfully installed polars-1.23.0
◤ (.venv) ~/t/ty-1390 vim bar.py
◤ (.venv) ~/t/ty-1390 cat bar.py
import pandas
import polars

polars.from_pandas(pandas.DataFrame())
◤ (.venv) ~/t/ty-1390 ty check bar.py
Checking ------------------------------------------------------------ 1/1 files
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/ef9f932/src/function/fetch.rs:264:21 when checking `/home/nic/things/ty-1390/bar.py`: `dependency graph cycle when querying TypeVarInstance < 'db >::lazy_constraints_(Id(a011)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    check_file_impl(Id(c00)),
    infer_scope_types(Id(1000)),
    FunctionType < 'db >::signature_(Id(4403)),
    infer_deferred_types(Id(1524)),
    ClassLiteral < 'db >::is_typed_dict_(Id(5435)),
    ClassLiteral < 'db >::try_mro_(Id(843a)),
    ClassLiteral < 'db >::explicit_bases_(Id(543a)),
    infer_deferred_types(Id(10cac)),
    ClassLiteral < 'db >::explicit_bases_(Id(543c)),
    infer_deferred_types(Id(10c64)),
    ClassLiteral < 'db >::explicit_bases_(Id(544a)),
    infer_deferred_types(Id(10c63)),
    TypeVarInstance < 'db >::lazy_constraints_(Id(a011)),
    infer_deferred_types(Id(10c39)),
    ClassLiteral < 'db >::is_typed_dict_(Id(544f)),
    ClassLiteral < 'db >::try_mro_(Id(844c)),
    ClassLiteral < 'db >::explicit_bases_(Id(5450)),
    infer_deferred_types(Id(10cad)),
    Type < 'db >::is_redundant_with_(Id(b4d8)),
    ClassLiteral < 'db >::try_mro_(Id(844f)),
    ClassLiteral < 'db >::try_mro_(Id(8450)),
    Type < 'db >::member_lookup_with_policy_(Id(286e)),
    place_by_id(Id(3073)),
    infer_definition_types(Id(f3ab)),
    Type < 'db >::member_lookup_with_policy_(Id(286f)),
    place_by_id(Id(3074)),
    infer_definition_types(Id(10f50)),
    parsed_module(Id(cfe)),
    source_text(Id(cfe)),
    infer_definition_types(Id(f0a4)),
    Type < 'db >::member_lookup_with_policy_(Id(286a)),
    place_by_id(Id(3070)),
    infer_definition_types(Id(10efb)),
    parsed_module(Id(cfb)),
    source_text(Id(cfb)),
    Type < 'db >::member_lookup_with_policy_(Id(285e)),
    place_by_id(Id(3067)),
    infer_definition_types(Id(10d80)),
    TupleType < 'db >::to_class_type_(Id(5c0a)),
]`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.23
info: Args: ["ty", "check", "bar.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_deferred_types(Id(10cad))
             at crates/ty_python_semantic/src/types/infer.rs:145
             cycle heads: ClassLiteral < 'db >::is_typed_dict_(Id(544f)) -> iteration = 0
   1: ClassLiteral < 'db >::explicit_bases_(Id(5450))
             at crates/ty_python_semantic/src/types/class.rs:1415
   2: ClassLiteral < 'db >::try_mro_(Id(844c))
             at crates/ty_python_semantic/src/types/class.rs:1415
   3: ClassLiteral < 'db >::is_typed_dict_(Id(544f))
             at crates/ty_python_semantic/src/types/class.rs:1415
   4: infer_deferred_types(Id(10c39))
             at crates/ty_python_semantic/src/types/infer.rs:145
             cycle heads: ClassLiteral < 'db >::is_typed_dict_(Id(5435)) -> iteration = 0
   5: TypeVarInstance < 'db >::lazy_constraints_(Id(a011))
             at crates/ty_python_semantic/src/types.rs:8172
   6: infer_deferred_types(Id(10c63))
             at crates/ty_python_semantic/src/types/infer.rs:145
   7: ClassLiteral < 'db >::explicit_bases_(Id(544a))
             at crates/ty_python_semantic/src/types/class.rs:1415
   8: infer_deferred_types(Id(10c64))
             at crates/ty_python_semantic/src/types/infer.rs:145
   9: ClassLiteral < 'db >::explicit_bases_(Id(543c))
             at crates/ty_python_semantic/src/types/class.rs:1415
  10: infer_deferred_types(Id(10cac))
             at crates/ty_python_semantic/src/types/infer.rs:145
  11: ClassLiteral < 'db >::explicit_bases_(Id(543a))
             at crates/ty_python_semantic/src/types/class.rs:1415
  12: ClassLiteral < 'db >::try_mro_(Id(843a))
             at crates/ty_python_semantic/src/types/class.rs:1415
  13: ClassLiteral < 'db >::is_typed_dict_(Id(5435))
             at crates/ty_python_semantic/src/types/class.rs:1415
  14: infer_deferred_types(Id(1524))
             at crates/ty_python_semantic/src/types/infer.rs:145
  15: FunctionType < 'db >::signature_(Id(4403))
             at crates/ty_python_semantic/src/types/function.rs:697
  16: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:70
  17: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:535


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

---

_Reopened by @AlexWaygood on 2025-10-17 19:05_

---

_Comment by @AlexWaygood on 2025-10-17 19:06_

The good news is that this one-line change fixes the panic:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 53c4f427aa..3ae2adaa75 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -8312,7 +8312,7 @@ impl<'db> TypeVarInstance<'db> {
         Some(TypeVarBoundOrConstraints::UpperBound(ty))
     }
 
-    #[salsa::tracked(heap_size=ruff_memory_usage::heap_size)]
+    #[salsa::tracked(cycle_fn=lazy_bound_cycle_recover, cycle_initial=lazy_bound_cycle_initial, heap_size=ruff_memory_usage::heap_size)]
     fn lazy_constraints(self, db: &'db dyn Db) -> Option<TypeVarBoundOrConstraints<'db>> {
         let definition = self.definition(db)?;
         let module = parsed_module(db, definition.file(db)).load(db);
```

now we just need a minimized example to use as a test...

---

_Label `fatal` added by @AlexWaygood on 2025-10-17 19:06_

---

_Added to milestone `Beta` by @AlexWaygood on 2025-10-18 11:27_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-10-18 11:27_

---

_Comment by @AlexWaygood on 2025-10-18 12:31_

huzzah, here's a self-contained repro (still not exactly minimal, though):

```py
from typing import Generic, TypeVar, Any

class DatetimeIndex(DatetimeIndexProperties): ...

_DTTimestampTimedeltaReturnType = TypeVar(
    "_DTTimestampTimedeltaReturnType",
    Any,
    DatetimeIndex,
)

class _DatetimeRoundingMethods(Generic[_DTTimestampTimedeltaReturnType]): ...

_DTNormalizeReturnType = TypeVar("_DTNormalizeReturnType", Any, DatetimeIndex)

class _DatetimeNoTZProperties(
    Generic[_DTTimestampTimedeltaReturnType, _DTNormalizeReturnType],
): ...

class DatetimeIndexProperties(
    _DatetimeNoTZProperties[DatetimeIndex, DatetimeIndex]
): ...

class TimedeltaIndexProperties(_DatetimeRoundingMethods[Any]): ...


class TimedeltaIndex(TimedeltaIndexProperties): ...

class Timedelta:
    def __lt__(self, other: TimedeltaIndex): ...

Timedelta(days=10) < Timedelta(days=20)
```

It only reproduces if you save it as a `.pyi` file.

---

_Comment by @AlexWaygood on 2025-10-18 12:40_

I think this is about as minimal as I can get it.

```py
from typing import Generic, TypeVar

class A(B): ...
class G: ...

T = TypeVar("T", G, A)

class C(Generic[T]): ...
class B(C[A]): ...
class D(C[G]): ...

def func(x: D): ...

func(G())
```

---

_Comment by @AlexWaygood on 2025-10-18 13:02_

Thanks for the report @nicwest! (And thank you very much for reading through #445 as well, even if it led you astray in this instance!)

---

_Closed by @AlexWaygood on 2025-10-18 13:02_

---
