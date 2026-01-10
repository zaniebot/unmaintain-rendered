```yaml
number: 3762
title: Support an allowlist of function calls in argument defaults for B008
type: issue
state: closed
author: rouge8
labels:
  - good first issue
  - rule
  - accepted
assignees: []
created_at: 2023-03-27T20:00:42Z
updated_at: 2023-08-23T15:52:01Z
url: https://github.com/astral-sh/ruff/issues/3762
synced_at: 2026-01-10T11:09:46Z
```

# Support an allowlist of function calls in argument defaults for B008

---

_Issue opened by @rouge8 on 2023-03-27 20:00_

It would be great if B008 supported an allowlist of function calls, similar to `extend-immutable-calls` for B006. We do a lot of this in our code which as far as I know has no subtle bugs:

```python
def f(x=Decimal("1000")):
    pass
```

Or maybe it's worth hardcoding `Decimal` as an exception?

---

_Comment by @charliermarsh on 2023-03-27 20:12_

Yeah perhaps we add that to the list of `IMMUTABLE_FUNCS` in `crates/ruff/src/rules/flake8_bugbear/rules/function_call_argument_default.rs`.

---

_Label `rule` added by @charliermarsh on 2023-03-27 20:12_

---

_Comment by @JonathanPlasse on 2023-03-27 20:16_

It could be a good first issue.

---

_Label `good first issue` added by @charliermarsh on 2023-03-27 20:18_

---

_Comment by @rouge8 on 2023-03-27 20:34_

> Yeah perhaps we add that to the list of `IMMUTABLE_FUNCS` in `crates/ruff/src/rules/flake8_bugbear/rules/function_call_argument_default.rs`.

I think it's probably worth doing that and adding an allowlist. Looking at our codebase, we have some custom functions we'd want to allow while still benefiting from flagging this in general.

---

_Comment by @rouge8 on 2023-03-27 20:55_

Okay, I opened https://github.com/charliermarsh/ruff/pull/3764 adding a few more immutable functions. I can work on another PR adding a setting if you're open to that!

---

_Comment by @rouge8 on 2023-03-28 16:18_

Oh wait, reading the code, it looks like B008 might already support `extend-immutable-calls`?

---

_Comment by @charliermarsh on 2023-03-28 16:20_

Oh wait, you're right...

---

_Comment by @rouge8 on 2023-03-28 16:20_

Hmm and I don't see it used in https://github.com/charliermarsh/ruff/blob/main/crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs ? Is the doc saying it's used for B006 wrong and it's actually used for B008?

---

_Comment by @charliermarsh on 2023-03-28 16:22_

Yeah something's a bit off between these.

---

_Comment by @aberres on 2023-03-29 10:23_

@rouge8 `pathlib.Path` is another candidate. See https://peps.python.org/pep-0428/#immutability

---

_Comment by @rouge8 on 2023-03-29 14:07_

> @rouge8 `pathlib.Path` is another candidate. See https://peps.python.org/pep-0428/#immutability

Good catch! Added it in https://github.com/charliermarsh/ruff/pull/3794


---

_Comment by @JonathanPlasse on 2023-03-29 19:04_

Why not `(Pure)(Posix|Windows)Path` too?

---

_Label `accepted` added by @charliermarsh on 2023-07-10 01:29_

---

_Comment by @Apakottur on 2023-07-19 16:43_

So at this point should it be possible to use `extend-immutable-calls` for B008?
I have the following code:
```python
# test.py
from typing import TypeAlias

MyTypeAlias: TypeAlias = str

def func(
    x: str = str("default"),
    y: str = MyTypeAlias("default"),
):
    return x + y
```
```toml
# ruff.toml
[flake8-bugbear]
extend-immutable-calls = ["MyTypeAlias"]
```

And running `ruff check --config ruff.toml --select B008 test.py` still gives me:
```shell
test.py:9:14: B008 Do not perform function call `MyTypeAlias` in argument defaults
Found 1 error.
```

---

_Comment by @charliermarsh on 2023-07-19 17:05_

It should work for B008 if you use a fully-qualified path to the symbol, rather than the string name. For example:

```toml
[flake8-bugbear]
extend-immutable-calls = ["path.to.module.MyTypeAlias"]
```

Unfortunately, that won't work for symbols defined in the file in which they're used -- they have to be imported from another file. It's just a limitation which we'll resolve eventually (#5486).

---

_Comment by @charliermarsh on 2023-07-19 17:09_

Unrelatedly, looking back at this issue, I see a few touch-ups we can do:

- B008 could ignore arguments with immutable annotations.
- B006 could consider `extend_immutable_calls` when determining whether an annotation is immutable.

---

_Comment by @zanieb on 2023-08-23 15:52_

`B008` already supports this so I'm going to close this issue.

I've also added the improvements that Charlie suggested in #6781 and #6784

---

_Closed by @zanieb on 2023-08-23 15:52_

---
