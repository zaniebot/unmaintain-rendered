```yaml
number: 10281
title: "`ruff format` moves `# noqa` comment to wrong location"
type: issue
state: closed
author: jakob-keller
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-03-07T21:13:31Z
updated_at: 2024-03-08T14:51:04Z
url: https://github.com/astral-sh/ruff/issues/10281
synced_at: 2026-01-12T15:54:50Z
```

# `ruff format` moves `# noqa` comment to wrong location

---

_@jakob-keller_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I would like to switch my codebase to `ruff format`, but there is one annoying issue that breaks my use case: Some `# noqa` comments are moved resulting in issues when calling `ruff check`.

Hers's a minimal code snippet to demonstrate what happens:

```python
# test.py (original)

import typing


def foo(
    __bar: str,
    /,
    **specifiers: typing.Any,  # noqa: ANN401
) -> int:
    return len(specifiers)
```

```
$ ruff --version
ruff 0.3.1

$ ruff check --isolated --select ANN401 test.py

$ ruff format --isolated test.py
1 file reformatted
```

```python
# test.py (reformatted)

import typing


def foo(
    __bar: str,
    /,  # noqa: ANN401
    **specifiers: typing.Any,
) -> int:
    return len(specifiers)
```

```
$ ruff check --isolated --select ANN401 test.py
test.py:7:19: ANN401 Dynamically typed expressions (typing.Any) are disallowed in `**specifiers`
Found 1 error.
```

I assume this issue is an edge case that has to do with the keyword-only `/` syntax being used. Are there any workarounds available?

---

_Label `suppression` added by @charliermarsh on 2024-03-07 22:01_

---

_Label `formatter` added by @charliermarsh on 2024-03-07 22:01_

---

_Label `bug` added by @charliermarsh on 2024-03-07 22:01_

---

_Comment by @charliermarsh on 2024-03-07 22:02_

Oh hmm, this looks like a bug, thanks. Will look into it. The only workaround I could think of is using `# fmt: skip` on the header for now:

```python
import typing


def foo(
    __bar: str,
    /,
    **specifiers: typing.Any,  # noqa: ANN401
) -> int:  # fmt: skip
    return len(specifiers)
```

---

_Comment by @charliermarsh on 2024-03-07 22:11_

This is why we need a programming language that doesn't allow comments :joy:

---

_Label `suppression` removed by @MichaReiser on 2024-03-08 07:40_

---

_Comment by @MichaReiser on 2024-03-08 07:42_

> This is why we need a programming language that doesn't allow comments 😂

Or an `AST` that represents `/` ;) 

FYI: This is not specific to noqa. We incorrectly associate any end-of-line comment `**kwargs` comment following `/` 

https://play.ruff.rs/cb2a370c-ae53-48a5-a35f-810b9c537ae2

I suspect that the error is somewhere in https://github.com/astral-sh/ruff/blob/184241f99affa895c5e8c1a2e144587cfaa2339a/crates/ruff_python_formatter/src/comments/placement.rs#L702

---

_Closed by @MichaReiser on 2024-03-08 14:45_

---

_Comment by @jakob-keller on 2024-03-08 14:51_

Thanks @MichaReiser and everyone else at https://github.com/astral-sh! You rock 🚀

---
