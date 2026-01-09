---
number: 17821
title: "`lint.typing-extensions` not respected"
type: issue
state: closed
author: Bibo-Joshi
labels:
  - bug
  - configuration
assignees: []
created_at: 2025-05-03T18:37:49Z
updated_at: 2025-05-04T09:31:35Z
url: https://github.com/astral-sh/ruff/issues/17821
synced_at: 2026-01-07T13:12:16-06:00
---

# `lint.typing-extensions` not respected

---

_Issue opened by @Bibo-Joshi on 2025-05-03 18:37_

### Summary

Follow up for #9761. The issue was closed through https://github.com/astral-sh/ruff/pull/17611 which introduced a new setting `lint.typing-extensions .
Unfortunately I can't get ruff to respect the setting on my machine and also not in pre-commit(.ci)

MWE:

```python
from typing import TypeVar

T = TypeVar('T', bound='Foo')

class Foo:
    def return_self(self: T) -> T:
        return self
```

```shell
$ ruff version
ruff 0.11.8 (75effb8ed 2025-05-01)
$ ruff check --isolated --config "lint.typing-extensions = false" --config "target-version= 'py39'" --select PYI019 foo.py
foo.py:6:20: PYI019 [*] Use `Self` instead of custom TypeVar `T`
  |
5 | class Foo:
6 |     def return_self(self: T) -> T:
  |                    ^^^^^^^^^^^^^^ PYI019
7 |         return self
  |
  = help: Replace TypeVar `T` with `Self`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Using `typing.Self` should not be recommend as it's not available in py3.9 and since I told ruff not to use `typing_extensions`.
What confuses me is that it does work as expected in the playground: https://play.ruff.rs/eca7e265-3332-4fa2-8993-3db06c4a5786, i.e. here no issue is reported.

What am I doing wrong that makes ruff ignore the setting?

For reference: I'm trying to update ruff on our project in https://github.com/python-telegram-bot/python-telegram-bot/pull/4748, which also includes the failing pre-commit.ci run.

### Version

ruff 0.11.8 (75effb8ed 2025-05-01)

---

_Referenced in [python-telegram-bot/python-telegram-bot#4748](../../python-telegram-bot/python-telegram-bot/pulls/4748.md) on 2025-05-03 18:38_

---

_Comment by @AlexWaygood on 2025-05-03 19:13_

cc. @ntBre 

---

_Label `configuration` added by @AlexWaygood on 2025-05-03 19:13_

---

_Assigned to @ntBre by @ntBre on 2025-05-03 19:18_

---

_Comment by @ntBre on 2025-05-03 19:37_

I missed a step combining configurations [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_workspace/src/configuration.rs#L1175). I'll put up a PR shortly, thanks for the report!

---

_Label `bug` added by @AlexWaygood on 2025-05-03 19:39_

---

_Referenced in [astral-sh/ruff#17823](../../astral-sh/ruff/pulls/17823.md) on 2025-05-03 19:49_

---

_Closed by @ntBre on 2025-05-03 22:19_

---

_Closed by @ntBre on 2025-05-03 22:19_

---

_Comment by @Bibo-Joshi on 2025-05-04 09:31_

that was awesome fast, many thanks! I've build ruff locally on https://github.com/astral-sh/ruff/commit/fe4051b2e6bf74a33f67f60fdbd775a6568e96a3 and also double checked that running `ruff check .` on our code base gives the expected results with the config in `pyproject.toml` 👍 
Looking forward to the next release :)

---
