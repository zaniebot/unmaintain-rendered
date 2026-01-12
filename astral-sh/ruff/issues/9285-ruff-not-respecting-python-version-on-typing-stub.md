```yaml
number: 9285
title: ruff not respecting python version on typing stub files?
type: issue
state: closed
author: ktbarrett
labels:
  - question
assignees: []
created_at: 2023-12-26T18:59:28Z
updated_at: 2024-01-01T01:14:56Z
url: https://github.com/astral-sh/ruff/issues/9285
synced_at: 2026-01-12T15:54:49Z
```

# ruff not respecting python version on typing stub files?

---

_@ktbarrett_

I used mypy's stubgen to generate a `.pyi` file for a C extension module. Some functions return a tuple. It looks a bit like this.

```python
from typing import Tuple

def get_sim_time() -> Tuple[int, int]: ...
```

My project's `target-version = "py37"`, but I'm getting a UP006 error for not preferring the use of PEP585 annotations of `tuple`. This PEP wasn't accepted until Python 3.9. 

---

_Comment by @ktbarrett on 2023-12-26 19:05_

Nvm, this was fixed in a newer version.

---

_Closed by @ktbarrett on 2023-12-26 19:05_

---

_Comment by @ktbarrett on 2023-12-26 19:42_

Never never mind. Tools be gaslighting me. This is still an issue. ruff version 0.1.9.

---

_Reopened by @ktbarrett on 2023-12-26 19:42_

---

_Comment by @zanieb on 2023-12-26 19:48_

This looks intentional

https://github.com/astral-sh/ruff/blob/b0ae1199e8ccab439db91ec6aa78fa51f2f591bb/crates/ruff_linter/src/checkers/ast/analyze/expression.rs#L290-L298

From the documentation

> This rule is enabled when targeting Python 3.9 or later (see:
> [`target-version`]). By default, it's _also_ enabled for earlier Python
> versions if `from __future__ import annotations` is present, as
> `__future__` annotations are not evaluated at runtime. If your code relies
> on runtime type annotations (either directly or via a library like
> Pydantic), you can disable this behavior for Python versions prior to 3.9
> by setting [`pyupgrade.keep-runtime-typing`] to `true`.

I believe stub files are always treated as using future annotation semantics?

---

_Comment by @zanieb on 2023-12-26 19:50_

Seems corroborated by https://github.com/astral-sh/ruff/issues/5649

---

_Label `question` added by @zanieb on 2023-12-26 19:50_

---

_Comment by @ktbarrett on 2023-12-26 20:05_

Does this imply that all tools that might consume a stub file are required to support the future syntax, regardless of the version of Python they may be running on/assuming?

---

_Comment by @ktbarrett on 2023-12-26 20:11_

I see [this](https://typing.readthedocs.io/en/latest/source/stubs.html#syntax).

> Type stubs are syntactically valid Python 3.7 files with a .pyi suffix. The Python syntax used for type stubs is independent from the Python versions supported by the implementation, and from the Python version the type checker runs under (if any). Therefore, type stub authors should use the latest available syntax features in stubs (up to Python 3.7), even if the implementation supports older, pre-3.7 Python versions. Type checker authors are encouraged to support syntax features from post-3.7 Python versions, although type stub authors should not use such features if they wish to maintain compatibility with all type checkers.
> For example, Python 3.7 added the async keyword (see PEP 492 [[3]](https://typing.readthedocs.io/en/latest/source/stubs.html#pep492)). Stub authors should use it to mark coroutines, even if the implementation still uses the @coroutine decorator. On the other hand, type stubs should not use the positional-only syntax from PEP 570 [[7]](https://typing.readthedocs.io/en/latest/source/stubs.html#pep570), introduced in Python 3.8, although type checker authors are encouraged to support it.
> Stubs are treated as if from __future__ import annotations is enabled. In particular, built-in generics, pipe union syntax (X | Y), and forward references can be used.

Specifically

> On the other hand, type stubs should not use the positional-only syntax from PEP 570 [[7]](https://typing.readthedocs.io/en/latest/source/stubs.html#pep570), introduced in Python 3.8, although type checker authors are encouraged to support it.

This seems to imply that enforcing 3.9 syntax here is *not* correct unless the user says they are only support 3.9 and up.

Mypy seems to corroborate this assumption seeing as it was the tool that spit out the above stub. 

---

_Comment by @zanieb on 2023-12-26 20:17_

That's a good question!

> The Python syntax used for type stubs is independent from the Python versions supported by the implementation, and from the Python version the type checker runs under (if any).
> ...
> Stubs are treated as if from future import annotations is enabled. In particular, built-in generics, pipe union syntax (X | Y), and forward references can be used.

These seem to support our behavior.

> On the other hand, type stubs should not use the positional-only syntax from PEP 570 [[7]](https://typing.readthedocs.io/en/latest/source/stubs.html#pep570), introduced in Python 3.8, although type checker authors are encouraged to support it.

This is different. This is a new _syntax_ for declaring parameters, i.e. the introduction of `/`, not a different type annotation. This would break the assumption:

> Type stubs are syntactically valid Python 3.7 files with a .pyi suffix.

---

_Comment by @ktbarrett on 2023-12-26 22:47_

So why would mypy output `typing.Tuple` rather than `tuple`? This is the most recent mypy stubgen installed into a Python 3.11 environment. And even if that goes against what was propounded above, why wouldn't implementation trump our interpretation?

---

_Comment by @charliermarsh on 2023-12-26 23:52_

To clarify, are you seeing any problem using tuple rather than typing.Tuple? Or you’re wondering why Mypy would output typing.Tuple, despite it accepting either?

(As an aside, many of the quoted sections about seem to be about syntax, like the async keyword. This isn’t a question of syntax — it’s a question of semantics. And the Mypy docs make clear that you _can_ use standard-library generics in type stubs.)

---

_Comment by @ktbarrett on 2023-12-27 02:37_

Well I guess this is an issue I should file with mypy to see if they have a reason for the madness. Otherwise I guess I'll just check it in and hope it doesn't break anyone.

---

_Comment by @AlexWaygood on 2023-12-31 12:07_

I think ruff's behaviour is correct here.

If mypy, pyright, or any other type checker is rejecting PEP-585 syntax in a stub file when you're running that type checker with `--python-version=3.8` or lower, that's absolutely a bug with that type checker that you should file with them. We long ago agreed in the typing community that stub authors should be able to use newer typing idioms even if targeting older Python versions — since stubs are never executed at runtime, whether or not `tuple` is actually _subscriptable_ at runtime is irrelevant in a stub file. (But note the caveat @zanieb mentioned in https://github.com/astral-sh/ruff/issues/9285#issuecomment-1869753453 w.r.t. new idioms that required changes to Python's grammar itself vs. those that didn't.)

We've enforced the use of PEP-585 idioms across all of typeshed for a long time now, and stubs in typeshed are tested with mypy, pyright and pytype in CI, across a matrix of all non-EOL Python versions. (While we don't test in CI with Pyre, Pyre also vendors typeshed's stubs.) If a type checker had issues with using this syntax in stub files when targeting older Python versions, I think we would have heard about it by now 🙂

> Does this imply that all tools that might consume a stub file are required to support the future syntax, regardless of the version of Python they may be running on/assuming?

Yes, I think so

---

_Comment by @AlexWaygood on 2023-12-31 12:26_

> So why would mypy output `typing.Tuple` rather than `tuple`? This is the most recent mypy stubgen installed into a Python 3.11 environment.

That's just a missing stubgen feature: stubgen just hasn't been updated to output PEP-585 idioms yet. It doesn't reveal anything deep about how mypy understands the type.

I agree it would be great if stubgen automatically output stubs that conformed with typeshed's style guide; it would mean we'd have to rely less on autofixes in our typeshed CI :) it just needs someone to find the time to make a stubgen PR to implement it

---

_Comment by @charliermarsh on 2023-12-31 12:38_

👍 I agree with @AlexWaygood (and thank you for chiming in). I'm gonna close this as "working as intended".

---

_Closed by @charliermarsh on 2023-12-31 12:38_

---
