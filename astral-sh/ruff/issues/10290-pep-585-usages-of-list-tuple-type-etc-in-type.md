```yaml
number: 10290
title: "PEP 585 usages of `list`, `tuple`, `type`, etc. in type aliases are not flagged when `requires-python` includes `< 3.9`."
type: issue
state: open
author: jagerber48
labels:
  - rule
assignees: []
created_at: 2024-03-08T03:36:25Z
updated_at: 2025-05-30T01:40:31Z
url: https://github.com/astral-sh/ruff/issues/10290
synced_at: 2026-01-10T11:09:52Z
```

# PEP 585 usages of `list`, `tuple`, `type`, etc. in type aliases are not flagged when `requires-python` includes `< 3.9`.

---

_Issue opened by @jagerber48 on 2024-03-08 03:36_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

PEP 585 allows usages like `list[int]` to be used in type annotations and type aliases. In python 3.7 and 3.8 this syntax can be used in type annotations if paired with `from __future__ import annotations`. However, as far as I can tell, it is not possible to use this syntax in **type aliases** for these versions of python.

```
MyType = list[int]
```
gives 
```
TypeError: 'type' object is not subscriptable
```
on Python 3.8 for me.

Instead I find myself needing to do
```
from typing import List

MyType = List[int]
```

I am trying to extend support for a library that was previously `>= 3.9` to `>= 3.8`.
I expected ruff to give me linting warnings about the above patterns when I set `requires-python = ">= 3.8"` in `pyproject.toml` but instead I get no such warnings.

I tried using the `keep-runtime-typing` option but that didn't cause errors to be raised. I also tried ignoring `UP006` and `UP007`, I didn't expect that to work (since I would only expect that to cause ruff not to ASK me to switch from `List`  to `list`, but not force a `list -> List` back conversion.

Is there something I am missing?

I am using ruff version 0.3.1

---

_Comment by @charliermarsh on 2024-03-08 05:57_

I don't think we currently flag those. We could, but AFAIK they're not covered by any rule right now. We only cover the inverse, e.g., you have a `List[int]` that could be a `list[int]`. It seems reasonable to support though.

---

_Label `rule` added by @charliermarsh on 2024-03-08 05:57_

---

_Comment by @MichaReiser on 2024-03-08 08:51_

That makes sense to me. To me the question is how we want to scope this rule. Should it be a dedicated rule for possible typing errors because of version incompatibility or is it a generic rule that catches any possible incompatibilities based on your python version. I'm bringing this up because there are two similar problems:

* Ruff should warn about invalid syntax that raises a runtime `SyntaxError`
* Ruff should warn about syntax that is incompatible with your target Python version

---

_Comment by @Skylion007 on 2024-03-08 15:00_

We discussed adding pydowngrade rules that did the opposite of pyupgrade, if we do implement that might be a nice rule category to add `DOWN`.

---

_Comment by @jagerber48 on 2024-03-09 14:25_

Yes exactly. I was surprised that ruff let detectable runtime  errors like this through. 

And yes exactly, a DOWN category is what I was expecting. 

I guess it’s not obvious to me how the DOWN and UP rules would work together though. I guess the DOWN rules would only be relevant pursuant to the Python versions allowed, and likewise many other rules would need version checks. But maybe version checks are already in place at least for the upgrade direction. 

---

_Comment by @charliermarsh on 2024-03-09 15:03_

> But maybe version checks are already in place at least for the upgrade direction.

Yeah, that's correct -- the `UP` direction has version checks, we'd never change `List[int]` to `list[int]` on unsupported versions.

---

_Comment by @ods on 2024-03-12 19:20_

> Yeah, that's correct -- the UP direction has version checks, we'd never change List[int] to list[int] on unsupported versions.

But it does change to incorrect variant, here is an example:
```shell
❯ ruff --version 
ruff 0.3.2

❯ ruff check --config "target-version='py38'" --select UP - <<EOF
from __future__ import annotations
from typing import List, Optional
a: List[int]
b: Optional[int]
EOF
-:3:4: UP006 Use `list` instead of `List` for type annotation
-:4:4: UP007 Use `X | Y` for type annotations
Found 2 errors.
No fixes available (2 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

Both suggested changes `list[int]` and `int | None` are invalid in Python 3.8. The error can easily pass unnoticed due to postponed evaluation though. Static checkers like mypy also don't complain, but runtime checkers like beartype do.

---

_Comment by @zanieb on 2024-03-12 19:23_

@ods  You're using `from __future__ import annotations` there and I believe this issue is specifically about type aliases which you're not using? We have a specific [`keep-runtime-typing`](https://docs.astral.sh/ruff/settings/#lint_pyupgrade_keep-runtime-typing) option for this use-case.



---

_Comment by @ods on 2024-03-12 19:28_

@zanieb Thanks, didn't know about this option!

---

_Comment by @FichteFoll on 2024-08-19 09:57_

Edit: Not actually an issue

---

Similarly, setting `target-version = "py38"` does not consider that even inside .pyi files it is not possible to use the union syntax or the stdlib types as generics when defining a `TypeAlias`, which is required afaik when targetting Python 3.8. For example:

```py
from typing import Dict, List, TypeAlias, Union

JSON: TypeAlias = Union[str, int, float, List[JSON], Dict[str, JSON]]  # raises UP006 and UP007 (and does not complain about undefined usage of "JSON")
JSON2: TypeAlias = "str | int | float | list[JSON2] | dict[str, JSON2]"  # raises PYI020
```

I can also create a new issue if desired.

Related: #2981, #3217

---

_Comment by @MichaReiser on 2024-08-19 10:10_

@FichteFoll this, as far as I understand, is intentional. See https://github.com/astral-sh/ruff/issues/12256

---

_Comment by @FichteFoll on 2024-08-19 11:13_

Thanks for the pointer. Will add a comment about my confusion there.

---

_Comment by @asglover on 2025-05-30 01:30_

Just to clarify,  Is there no "DOWN" set of rules that I can use to lint a program to make sure that it won't commit syntax errors on the minimum allowable target-version? That's what I assumed the "target-version" setting did until a close reading. So it only guarantees not to suggest breaking upgrades? 

Is there are issue tracking "DOWN" rules I could +1 on?

Could a clarifying sentence be added to the documentation for "target-version"? 

I'd be happy to make the PR for the documentation change if you're okay with adding clarification. 

I can also move this to a fresh issue if that's preferable

Thanks for making ruff! 

---

_Comment by @ntBre on 2025-05-30 01:40_

There is https://github.com/astral-sh/ruff/issues/2501 for a possible pydowngrade/DOWN rule set.

I'm not sure I'm following exactly on the `target-version` docs, so that might be good for a separate issue!

Thanks for the kind words!

---
