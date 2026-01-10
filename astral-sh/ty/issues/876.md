```yaml
number: 876
title: "Enums: advanced features"
type: issue
state: open
author: sharkdp
labels:
  - typing semantics
  - enums
assignees: []
created_at: 2025-07-23T06:51:35Z
updated_at: 2025-12-18T12:34:31Z
url: https://github.com/astral-sh/ty/issues/876
synced_at: 2026-01-10T01:53:59Z
```

# Enums: advanced features

---

_Issue opened by @sharkdp on 2025-07-23 06:51_

We have basic support for enums, but there is a long tail of things that could be improved:

- [x] Infer type for [member names](https://typing.python.org/en/latest/spec/enums.html#member-names) and [member values](https://typing.python.org/en/latest/spec/enums.html#member-values).
- [x] Special-case handling of `IntEnum`, `StrEnum`, or other custom classes that derive from both `Enum` and some other class (infer appropriate types for member values)
- [x] `auto()` values
- [ ] Handling of `enum.Flag` (enum expansion may not be performed for subclasses of `enum.Flag`)
- [ ] Custom `__new__` or `__init__` methods in enums.
- [ ] Support for `_generate_next_value_`
- [ ] Special-case handling for call to enum class to retrieve member by value (e.g. `Color("red")`)
- [ ] Function syntax to create Enums

---

_Label `typing semantics` added by @sharkdp on 2025-07-23 06:51_

---

_Comment by @Andre-Medina on 2025-09-03 01:48_

Noticed enums weren't typed quite right in the `ty` VSC extension, glad to see it being worked on.

should also consider `EnumClass.ITEM.name` being type `str`

```python
from enum import StrEnum, IntEnum

class TempTest(StrEnum):
    ITEM = "data"
    ITEM_2 = "more_data"


should_be_enum_type = TempTest.ITEM
should_be_str = TempTest.ITEM.value
should_be_str_also = TempTest.ITEM.name

class TestInt(IntEnum):

    ITEM = 1
    ITEM_2 = 2

should_be_enum_type_also = TestInt.ITEM
should_be_int = TestInt.ITEM.value
should_be_str_still = TestInt.ITEM.name
```

With the extension in its current state, `ty` confusing some type hints:
<img width="993" height="686" alt="Image" src="https://github.com/user-attachments/assets/a5962c6f-20ec-4f6c-816f-98c63ed7d786" />

---

_Comment by @sharkdp on 2025-09-03 07:15_

@Andre-Medina It looks like `StrEnum` can't be imported? It's only available on 3.11 and later. How did you configure your Python version?

Once you have fixed that, `should_be_enum_type` should indeed be of type `Literal[TempTest.ITEM]`. Accessing `.value` and `.name` with more precise types is not yet supported, but it's listed as the first item in the enumeration above.

---

_Comment by @Andre-Medina on 2025-09-03 09:11_

> It looks like StrEnum can't be imported? It's only available on 3.11 and later. How did you configure your Python version?

Unsure, probably an issue with my local env, but good to hear `.value` and `.name` will be supported :)

---

_Comment by @aidandj on 2025-09-18 20:14_

There is a backported package [backports.strenum](https://pypi.org/project/backports.strenum/) that I've been using for legacy code.

I was testing out the latest main, and there is a regression. Using that `StrEnum` package, I used to get a `str` for the revealed type on alpha20. But on the latest main (in ruff) I get `Literal[<index of item>]`

```
if sys.version_info >= (3, 11):
    from enum import StrEnum
else:
    from backports.strenum import StrEnum
from enum import Enum, auto


class SomeEnum(StrEnum):
    A = auto()
    B = auto()
    C = auto()
    D = auto()


reveal_type(SomeEnum.A.value)
```

With alpha 20, I get type string. With ruff commit: `706be0a6e7e09936511198f2ff8982915520d138` I get `Literal[1]`

---

_Comment by @thejchap on 2025-09-19 00:59_

ah - [this](https://github.com/thejchap/ruff/blob/bc89d0394ca2ee3161df37b8fa3de307d001e54f/crates/ty_python_semantic/src/types/enums.rs#L151) is being exposed now

it does look like the type could actually be `Literal['a']` instead of `builtins.str`

from the docs:
> Note Using [auto](https://docs.python.org/3/library/enum.html#enum.auto) with [StrEnum](https://docs.python.org/3/library/enum.html#enum.StrEnum) results in the lower-cased member name as the value.

https://docs.python.org/3/library/enum.html#enum.StrEnum

---

_Comment by @aidandj on 2025-09-19 01:07_

I didn't actually test it with 3.11 I'm realizing. I'll do that later.

Confirmed same behavior on 3.12 and commit `706be0a6e7e09936511198f2ff8982915520d138`

---

_Comment by @sharkdp on 2025-09-19 07:11_

Thank you for reporting this. We should fix this. That's why the *"Special-case handling of IntEnum, StrEnum"* and *"auto() values"* items are still unchecked in the description above. We could either patch this by not inferring a precise type for `.value` if we derive from `StrEnum`. Or we could implement the actual `auto` behavior for `StrEnum`, like @thejchap suggested, if it's not too hard.

---

_Comment by @thejchap on 2025-09-19 12:57_

i can take a look at this next week

---

_Label `enums` added by @AlexWaygood on 2025-09-23 19:58_

---

_Added to milestone `Stable` by @carljm on 2025-11-12 23:56_

---

_Comment by @spaceone on 2025-12-18 11:35_

I guess another example for this issue is:

```sh
$ ty check t.py 
t.py:10:22: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
Found 1 diagnostic
$ cat t.py
```
```python
import enum
from typing import Literal


class Foo(enum.IntEnum):
    foo = 1
    bar = 2


def foo(bar: Literal[Foo.foo | Foo.bar]) -> None:
    pass
```

---

_Comment by @AlexWaygood on 2025-12-18 12:03_

@spaceone that looks like a true positive to me. `Foo.foo | Foo.bar` is not an enum member. `|` unions are not allowed inside `Literal[]` slices. You probably meant to write `Literal[Foo.foo, Foo.bar]` (a comma instead of a `|`).

We could probably improve our diagnostic for that case.

---

_Comment by @spaceone on 2025-12-18 12:12_

@AlexWaygood ah thank you. Then it's a undetected case of mypy.

→https://github.com/python/mypy/issues/20438

---
