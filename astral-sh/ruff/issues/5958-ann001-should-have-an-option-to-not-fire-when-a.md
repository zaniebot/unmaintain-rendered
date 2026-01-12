```yaml
number: 5958
title: "`ANN001` should have an option to not fire when a non-`None` default argument is provided"
type: issue
state: open
author: tylerlaprade
labels:
  - needs-decision
assignees: []
created_at: 2023-07-21T21:59:45Z
updated_at: 2023-07-27T01:06:31Z
url: https://github.com/astral-sh/ruff/issues/5958
synced_at: 2026-01-12T15:54:45Z
```

# `ANN001` should have an option to not fire when a non-`None` default argument is provided

---

_@tylerlaprade_

This was already raised in #794 but closed for unclear reasons. It seemed to have positive support in that thread. When I do something like
```
def make_test_foo(
    bar: Bar | None = None,
    create_bar=False,
    baz=None,
    create_baz=True,
    qux: Qux | None = None,
    create_qux=True,
):
```
in my test fixtures, I want my attention to be called to the missing type annotation on `baz`, not unnecessary warnings on `create_bar`, `create_baz`, and `create_qux`, because Pyright is already safely assuming them to be `bool` types.

This could be implemented as either breaking up `ANN001` into two separate rules, or as an additional config option as mentioned in #794.

I personally think the former option results in a cleaner settings file, but it would obviously be a somewhat breaking change unless the new rule was added as `ANN0011` so that people would have to opt out of it (I don't believe any such codes currently exist, though, so it would be a deviation from the norm). The best option is therefore likely to be a new config flag, although I am happy to see this implemented in any form possible!

---

_Renamed from "`ANN001` should have an option to not fire when default argument is provided" to "`ANN001` should have an option to not fire when a non-`None` default argument is provided" by @tylerlaprade on 2023-07-21 22:00_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-23 03:07_

---

_Comment by @charliermarsh on 2023-07-23 03:16_

I suspect it was closed because Mypy infers those types as `Any` (e.g., for `create_baz`).

---

_Comment by @tylerlaprade on 2023-07-23 06:09_

Ah, I see. Given that Pyright (and, I assume, Pytype and Pyre, although I haven't used them) behave(s) differently, I'd like to keep the discussion open.

---

_Comment by @tylerlaprade on 2023-07-26 20:53_

This is by far my #1 most desired feature at the moment because, without it, I don't feel comfortable forcing this rule upon my teammates. Is there any information I can provide to help reach a decision?

---

_Comment by @zanieb on 2023-07-26 21:41_

@tylerlaprade Is there an issue on mypy and/or pyright discussing this difference in behavior?

---

_Comment by @zanieb on 2023-07-26 21:42_

Also, in your use-case would this apply to other types than `bool`?

---

_Comment by @tylerlaprade on 2023-07-26 21:46_

>Also, in your use-case would this apply to other types than `bool`?

Ideally, it would apply to all non-`None` types. Pyright assumes a type for all of them.
>Is there an issue on mypy and/or pyright discussing this difference in behavior?

I can go take a look, but I'm not sure why it would be relevant here. It's definitely not an unintentional feature of Pyright.

---

_Comment by @zanieb on 2023-07-26 22:53_

Per the discussion at https://github.com/python/mypy/issues/3090#issuecomment-290691282 PEP-484 says the inferred type should be `Any` (https://peps.python.org/pep-0484/#the-any-type)

It'd be great to get that amended on the language standard and have some consensus around inference. It seems like that's probably a ways off but perhaps we can start a discussion about that in the relevant forum. In the meantime, this does feel reasonable to implement although I'm not sure of the best way how.

---

_Comment by @tylerlaprade on 2023-07-27 01:06_

I don't know Rust (yet), otherwise I'd be happy to attempt it myself. There are other existing type-aware rules, right? Is there anything they do that can be reused?

---
