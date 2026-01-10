---
number: 1070
title: Does not know about pydantic_settings.BaseSettings from env vars
type: issue
state: open
author: jankatins
labels:
  - library
assignees: []
created_at: 2025-08-20T22:24:29Z
updated_at: 2025-12-23T09:20:26Z
url: https://github.com/astral-sh/ty/issues/1070
synced_at: 2026-01-10T01:52:52Z
---

# Does not know about pydantic_settings.BaseSettings from env vars

---

_Issue opened by @jankatins on 2025-08-20 22:24_

### Summary

This is working python code:

```
import os

from pydantic_settings import BaseSettings

os.environ["EXAMPLE_SETTING"] = "example_value"


class Settings(BaseSettings):
    EXAMPLE_SETTING: str


s = Settings()
print(s.EXAMPLE_SETTING)
```

Mypy has a plugin which makes it recognise that the arguments are filled from env vars. How would I accomplish something similar with ty?

```shell
λ  python test.py      
example_value

# Without the pydantic.mypy plugin
λ  mypy test.py        
test.py:12: error: Missing named argument "EXAMPLE_SETTING" for "Settings"  [call-arg]
Found 1 error in 1 file (checked 1 source file)

# With the pydantic.mypy plugin
λ  mypy test.py
Success: no issues found in 1 source file

 λ  uvx ty check test.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                                                                                                                     error[missing-argument]: No argument provided for required parameter `EXAMPLE_SETTING`                                                                                                                                                                                                                              
  --> test.py:12:5
   |
12 | s = Settings()
   |     ^^^^^^^^^^
13 | print(s.EXAMPLE_SETTING)
   |
info: rule `missing-argument` is enabled by default

Found 1 diagnostic
```

### Version

ty 0.0.1-alpha.19

---

_Label `wish` added by @carljm on 2025-08-20 23:00_

---

_Label `library` added by @carljm on 2025-08-20 23:00_

---

_Comment by @carljm on 2025-08-20 23:10_

I'm not totally sure, but I _think_ the root cause of this issue is that Pydantic's ModelMetaclass behaves differently from dataclasses, in that it won't synthesize a new `__init__` method for a subclass if a base class defines a custom `__init__` method. `BaseSettings` defines `__init__` with no required arguments, but then type checkers assume that `Settings` will synthesize a new `__init__` with the `EXAMPLE_SETTING` field -- but Pydantic doesn't actually do that.

I think it will be best for pydantic-settings library to find a way to make this work within the standard type system. One option for that would be to use a distinct constructor like `Settings.from_env()` instead of relying on `__init__`. Another option could be to add a new keyword argument to `dataclass_transform` that would specify this alternate behavior where a custom `__init__` is not overridden with a synthesized one on a subclass.


---

_Comment by @zanieb on 2025-08-21 01:08_

cc @hramezani

---

_Comment by @Viicos on 2025-08-21 08:09_

> I'm not totally sure, but I _think_ the root cause of this issue is that Pydantic's ModelMetaclass behaves differently from dataclasses, in that it won't synthesize a new `__init__` method for a subclass if a base class defines a custom `__init__` method. `BaseSettings` defines `__init__` with no required arguments, but then type checkers assume that `Settings` will synthesize a new `__init__` with the `EXAMPLE_SETTING` field -- but Pydantic doesn't actually do that.

More generally, the `BaseSettings` has a number of dynamic behavior that can't reasonably be supported by standards compliant type checkers (without any plugin). Pydantic settings has a concept of _sources_ (env variables, configuration files), that are loaded on the fly during validation. On top of that, sources can be configured at validation time with extra keyword arguments to `__init__()` (e.g. `_env_file`).

---

I think the only possible way to support such use cases would be via plugins/special casing, but this has drawbacks and a lot of challenges that can really increase the complexity of type checkers (Some related discussion on Pyrefly: https://github.com/facebook/pyrefly/discussions/854#discussioncomment-14063403).

---

_Comment by @carljm on 2025-08-21 14:44_

I'm not sure why static type checkers would need to know or care about all the implementation details of sources etc: they just need to understand the correct `__init__` signature, and then what attributes exist on the resulting object, with what types. To me that seems like a much smaller problem, that should be tractable to fit into the standard type system, with either some adjustments to the API of `BaseSettings`, or some addition to the type system. But it's quite possible I'm missing something.

(Edited to add: it may partly depend on expectations for how much static type checkers should validate. But I think the goal here should not be "static type checkers fully understand all the config sources and can validate and catch every possible runtime error", it should rather be "static type checkers understand just enough of the API to avoid false positive errors, there are runtime errors that they simply aren't going to catch.")

---

_Comment by @Viicos on 2025-10-21 10:53_

> I'm not sure why static type checkers would need to know or care about all the implementation details of sources etc: they just need to understand the correct `__init__` signature, and then what attributes exist on the resulting object, with what types.

The issue here is that `pydantic-settings` "injects" values from different sources (env vars, config files, etc), so that instantiating a settings class _without_ any arguments still works, even though there are required fields:

```python
class Settings(BaseSettings):
    EXAMPLE_SETTING: str

Settings()  # Fine at runtime, `EXAMPLE_SETTING` is read from env.
```

So yes static type checkers understand `__init__()` as being `(self, EXAMPLE_SETTING: str) -> None` as per the spec, but Pydantic is "deviating" from the spec, in a number of places (type coercion, etc).

---

_Comment by @carljm on 2025-10-30 15:33_

Yes, makes sense. On further thought, I do think it is quite difficult (or impossible) for `BaseSettings` to get the automatic behavior it wants from type-checkers via dataclass-transform (understanding the possible constructor arguments and their types, for arbitrary custom user subclasses), while making arbitrary tweaks to that behavior (making all parameters optional, maybe some type-coercion things too?)

I think improving this in ty will require either further additions to the dataclass-transform spec, or dedicated builtin support for pydantic.

---

_Comment by @Viicos on 2025-10-30 15:47_

Indeed, there seem to be two paths we could take:
- have special-casing for specific libraries in static type checkers (what Pyrefly is aiming for). I recently exchanged with the Pyrefly team about this, there are benefits but also drawbacks [^1].
- on our side, trying as much as possible to be compliant to the typing spec, and possibly propose changes to the spec if necessary (for instance, I believe it would be beneficial to have a way to specify the type coercion logic with `@dataclass_transform`, instead of incorporating such logic in type checkers).

[^1]: Special casing incorporated in type checkers puts the burden on type checkers maintainers, is subject to (breaking) changes in the special-cased library, and going the other way with plugins isn't perfect either (one plugin is necessary for each type checker, designing a plugin API is hard, challenges for static languages such as Rust).

---

_Label `wish` removed by @carljm on 2025-11-15 01:55_

---

_Comment by @K-Yo on 2025-12-19 09:16_

For informations, this topic has been discussed for pyright validation of pydantic settings here: https://github.com/pydantic/pydantic-settings/issues/201

---

_Added to milestone `Stable` by @carljm on 2025-12-22 23:54_

---

_Comment by @Cjkjvfnby on 2025-12-23 09:20_

I'd say this class does not have `__init__` defined, because it's generated at runtime.  Such classes are common to Python, and treating them as a dataclass (expecting that fields in the class are part of the `__init__`) is not always correct. 

What about giving a special setting, like per-class-override:  that will allow us to set specific rules for a specific class, and its descendants.  

```toml
[[tool.ty.overrides]]
class = "BaseSettings"

[tool.ty.overrides.rules]
missing-argument = "ignored"
```

A similar approach is used for coverage, where you could exclude specific patterns from report:

```toml
[tool.coverage.report]
exclude_also = [
    "if TYPE_CHECKING:",
    ]
```

---
