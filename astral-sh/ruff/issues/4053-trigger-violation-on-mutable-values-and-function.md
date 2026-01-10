---
number: 4053
title: Trigger violation on mutable values and function calls for non-dataclass attribute defaults
type: issue
state: closed
author: adampauls
labels:
  - question
assignees: []
created_at: 2023-04-20T23:08:03Z
updated_at: 2023-06-12T16:55:30Z
url: https://github.com/astral-sh/ruff/issues/4053
synced_at: 2026-01-10T01:22:42Z
---

# Trigger violation on mutable values and function calls for non-dataclass attribute defaults

---

_Issue opened by @adampauls on 2023-04-20 23:08_

This is a feature request. I see in [this PR](https://github.com/charliermarsh/ruff/pull/3877) that there is some recent work in flagging mutable and function call defaults for dataclass attributes. Is there a plan to enable this check for non-dataclass attributes  as well? Right now, no Python tooling I know of catches this bug:

```python
import dataclasses

class NonDataClass:
    my_list: list[int] = dataclasses.field(default_factory=list)  # oops! type checks but is nonsense

    def add_item(self, item: int) -> None:
        self.my_list.append(item)

instance = NonDataClass()
instance.add_item(1)
assert instance.my_list == [1]
```

The problem is that `dataclass.field` [lies](https://github.com/microsoft/pyright/issues/4980) about its type. 

It would be great if we could check for mutable and function call defaults in class attributes because they are just problematic for non-dataclasses as they are for dataclasses. The only difference from the existing implementation would need to be that `dataclasses.field` should only be exempted when used inside a dataclass. 

I can look into this if the work is not already planned since I think it's a simple change from what's already been done. 

Thanks for the great tool!

---

_Renamed from "Trigger violation on mutable values and function calls for non-dataclass attribut defaults" to "Trigger violation on mutable values and function calls for non-dataclass attribute defaults" by @adampauls on 2023-04-20 23:39_

---

_Comment by @charliermarsh on 2023-04-21 01:24_

Interesting! I initially assumed that the suggestion here was to flag usages of `dataclasses.field` within classes that lack the `@dataclass` decorator, but I think you're suggesting something a bit broader: disallowing mutable defaults and function calls within non-`@dataclass`-decorated classes, which would by definition include usages of `dataclasses.field`. Is that right?

---

_Label `question` added by @charliermarsh on 2023-04-21 01:24_

---

_Comment by @hmc-cs-mdrissi on 2023-04-21 01:38_

I'll note some libraries do re-use/support dataclasses field for there own decorator/class. field just serves as a convenient way to hold some annotations and there are number of dataclass like libraries. I do use internal library that does,

```python
from dataclasses import field

@configurable
class Foo:
  x = field(default_factory=list)
```

where default_factory list can be safe to use. It's own separate rule to disallow field in non dataclass sounds fine though as if you aren't using alternative dataclass like libraries it makes sense.

---

_Comment by @adampauls on 2023-04-21 02:38_

If it's easy, it would be great to allow a configurable list of decorators that allow data class.field, or maybe even any dataclass_transform should be exempted?

---

_Comment by @adampauls on 2023-04-21 02:39_

> Is that right?

Yes, exactly. 

---

_Comment by @adampauls on 2023-04-21 02:47_

Though I would settle for the more targeted warning on just dataclass.field if that's preferable!

---

_Referenced in [astral-sh/ruff#4096](../../astral-sh/ruff/pulls/4096.md) on 2023-04-25 15:58_

---

_Referenced in [astral-sh/ruff#4390](../../astral-sh/ruff/pulls/4390.md) on 2023-05-12 16:12_

---

_Comment by @charliermarsh on 2023-06-12 16:55_

Closing in favor of #4390.

---

_Closed by @charliermarsh on 2023-06-12 16:55_

---
