```yaml
number: 312
title: "Failed to resolve property on `dagster.Config`"
type: issue
state: closed
author: ion-elgreco
labels:
  - bug
  - type properties
assignees: []
created_at: 2025-05-10T17:08:33Z
updated_at: 2025-05-15T15:14:57Z
url: https://github.com/astral-sh/ty/issues/312
synced_at: 2026-01-12T15:54:22Z
```

# Failed to resolve property on `dagster.Config`

---

_@ion-elgreco_

### Summary

```python
from datetime import datetime
import dagster as dg

class Foo(dg.ConfigurableResource):
    @property
    def current_date(self) -> datetime:
        return datetime.now()

def random_function(date: datetime) -> datetime:
    return date

foo = Foo()
random_function(date=foo.current_date)

```
Results in

```
error[missing-argument]: No argument provided for required parameter `self` of function `__init__`
  --> test.py:16:7
   |
16 | foo = Foo()
   |       ^^^^^
17 | random_function(date=foo.current_date)
   |
info: `missing-argument` is enabled by default

error[invalid-argument-type]: Argument to function `random_function` is incorrect
  --> test.py:17:17
   |
16 | foo = Foo()
17 | random_function(date=foo.current_date)
   |                 ^^^^^^^^^^^^^^^^^^^^^ Expected `datetime`, found `property`
   |
info: Function defined here
  --> test.py:12:5
   |
12 | def random_function(date: datetime) -> datetime:
   |     ^^^^^^^^^^^^^^^ -------------- Parameter declared here
13 |     return date
   |
info: `invalid-argument-type` is enabled by default

Found 2 diagnostics
```
If we remove the inherited class, it will pass

### Version

ty 0.0.0-alpha.8 (0474b40e1 2025-05-09)

---

_Label `bug` added by @MichaReiser on 2025-05-12 06:46_

---

_Label `attribute access` added by @MichaReiser on 2025-05-12 06:46_

---

_Comment by @sharkdp on 2025-05-12 10:06_

Thank you for reporting this. Something seems to be wrong with attribute access on subclasses of `dagster.Config`. It might be related to the custom metaclass on `dg.Config` (see [here](https://github.com/dagster-io/dagster/blob/be9a732f930f18ae63d724d9ba98f7f51d40f8c6/python_modules/dagster/dagster/_config/pythonic_config/config.py#L154)):

```py
import dagster as dg

class Foo(dg.Config):
    x: int = 1

reveal_type(Foo.x)  # Unknown
```

---

_Renamed from "Inherited class causes property to not be resolved" to "Broken attribute access when subclassing `dagster.Config`" by @sharkdp on 2025-05-12 10:24_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-14 13:36_

---

_Comment by @sharkdp on 2025-05-14 14:26_

I did some further analysis here. The problem seems to be that we do not resolve the *second* `ModelMetaclass` import from pydantic [here](https://github.com/dagster-io/dagster/blob/0322c21c0594fffdac8157773fb54eb2a4ebc08b/python_modules/dagster/dagster/_config/pythonic_config/typing_utils.py#L9-L14) (which seems correct? I don't see `ModelMetaclass` [here](https://github.com/pydantic/pydantic/blob/main/pydantic/main.py)):
```py 
try:
    # Pydantic 1.x
    from pydantic._internal._model_construction import ModelMetaclass
except ImportError:
    # Pydantic 2.x
    from pydantic.main import ModelMetaclass
```

This leads us to infer `<class 'ModelMetaclass'> | Unknown` for the base of `BaseConfigMeta` [here](https://github.com/dagster-io/dagster/blob/0322c21c0594fffdac8157773fb54eb2a4ebc08b/python_modules/dagster/dagster/_config/pythonic_config/typing_utils.py#L63).

```py
@dataclass_transform(kw_only_default=True, field_specifiers=(Field,))
class BaseConfigMeta(ModelMetaclass):  # type: ignore
```

Since we don't support unions as bases, we construct an MRO for `BaseConfigMeta` that includes `Unknown`. This means that `dagter.Config` [here](https://github.com/dagster-io/dagster/blob/be9a732f930f18ae63d724d9ba98f7f51d40f8c6/python_modules/dagster/dagster/_config/pythonic_config/config.py#L154) ends up having a metaclass with a base that is `Unknown`:

```py
class Config(MakeConfigCacheable, metaclass=BaseConfigMeta):
```

When looking up attributes on class objects (`Foo`/`dg.Config`), we look them up on the `type(Foo)` first, i.e. on the `BaseConfigMeta` metaclass. Since it has an unknown base, *every* attribute is present on that metaclass (with type `Unknown`):

```py
reveal_type(dg.Config.whatever)  # Unknown
```

… which then leads to the observed behavior in the original post here.

---

It seems that we can do better here, even if that import is actually problematic. When we see `<class 'ModelMetaclass'> | Unknown` as a base, we should probably at least attempt to include `<class 'ModelMetaclass'>` in the MRO? I'll put up a draft with an experiment...

---

_Label `attribute access` removed by @sharkdp on 2025-05-14 14:27_

---

_Label `mro` added by @sharkdp on 2025-05-14 14:28_

---

_Renamed from "Broken attribute access when subclassing `dagster.Config`" to "Invalid metaclass in MRO for `dagster.Config`" by @sharkdp on 2025-05-14 14:44_

---

_Comment by @ion-elgreco on 2025-05-14 14:46_

@sharkdp it's actually here: https://github.com/pydantic/pydantic/blob/main/pydantic/_internal/_model_construction.py

---

_Comment by @sharkdp on 2025-05-14 15:00_

> [@sharkdp](https://github.com/sharkdp) it's actually here: https://github.com/pydantic/pydantic/blob/main/pydantic/_internal/_model_construction.py

That's the 1.x import, but the 2.x one (`from pydantic.main import ModelMetaclass`)?

---

_Comment by @ion-elgreco on 2025-05-14 15:08_

> > [@sharkdp](https://github.com/sharkdp) it's actually here: https://github.com/pydantic/pydantic/blob/main/pydantic/_internal/_model_construction.py
> 
> That's the 1.x import, but the 2.x one (`from pydantic.main import ModelMetaclass`)?

Their code description is incorrect in dagster, the 1.x import path is actually still the same for 2.x

---

_Comment by @carljm on 2025-05-14 18:17_

> When looking up attributes on class objects (`Foo`/`dg.Config`), we look them up on the `type(Foo)` first, i.e. on the `BaseConfigMeta` metaclass. Since it has an unknown base, _every_ attribute is present on that metaclass (with type `Unknown`):

I think the other interesting question is why this results in false positive type errors in the original example here. In general, assuming `Unknown` is supposed to silence errors (since we don't know the type), so it seems like there is probably something to fix there (in addition to seeing if we can do better in inferring a non-Unknown metaclass here.)

The other potential improvement here is around `try: import foo; except ImportError: ...` pattern, where in principle we could consider it a "statically known branch" and avoid the `| Unknown` type in this case. Though I would probably consider this the lowest priority improvement raised by this issue.



---

_Label `mro` removed by @sharkdp on 2025-05-15 13:59_

---

_Label `type properties` added by @sharkdp on 2025-05-15 13:59_

---

_Comment by @sharkdp on 2025-05-15 14:08_

> I think the other interesting question is why this results in false positive type errors in the original example here. In general, assuming `Unknown` is supposed to silence errors (since we don't know the type), so it seems like there is probably something to fix there (in addition to seeing if we can do better in inferring a non-Unknown metaclass here.)

Thank you for asking the right questions! This was another extremely subtle point. We currently fail to recognize that subclass-of-type (`type[…`]) of a class with a non-fully-static metaclass is assignable to `type`. This is why the call to the `property.__get__` method failed (which requires the `owner` argument to be of type `type`). Once [that is fixed](https://github.com/astral-sh/ruff/pull/18121), no false diagnostics are left in the original example here. And we actually infer the correct types (not `Unknown`) for that property.

---

_Renamed from "Invalid metaclass in MRO for `dagster.Config`" to "Failed to resolve property on `dagster.Config`" by @sharkdp on 2025-05-15 14:11_

---

_Closed by @sharkdp on 2025-05-15 15:13_

---

_Comment by @sharkdp on 2025-05-15 15:14_

> The other potential improvement here is around `try: import foo; except ImportError: ...` pattern, where in principle we could consider it a "statically known branch" and avoid the `| Unknown` type in this case. Though I would probably consider this the lowest priority improvement raised by this issue.

Given that we handle the given example correct now, I think we can probably defer this idea until it comes up again.

---
