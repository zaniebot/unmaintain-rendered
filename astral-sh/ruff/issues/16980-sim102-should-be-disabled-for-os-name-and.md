```yaml
number: 16980
title: SIM102 should be disabled for os.name and platform.system() checks (in addition to sys.platform)
type: issue
state: open
author: real-or-random
labels:
  - rule
assignees: []
created_at: 2025-03-26T12:36:42Z
updated_at: 2025-05-06T06:56:46Z
url: https://github.com/astral-sh/ruff/issues/16980
synced_at: 2026-01-10T11:09:58Z
```

# SIM102 should be disabled for os.name and platform.system() checks (in addition to sys.platform)

---

_Issue opened by @real-or-random on 2025-03-26 12:36_

### Summary

SIM102 (collapsible-if) has an exception for version and os checks. This currently matches `sys.platform`.

```python
if sys.platform == "linux":  # no SIM102
    if foo:
```

Analogously, it should also be disabled `os.name` and `platform.system()`:

```python
if os.name == "posix": # SIM102
    if foo:
```

See https://stackoverflow.com/a/58071295 to learn about the various methods. (And if you've looked at this post, you're also in a good position to have an opinion on the otherwise unrelated https://github.com/astral-sh/ruff/issues/5622.)

---

_Label `rule` added by @MichaReiser on 2025-03-26 13:37_

---

_Comment by @MichaReiser on 2025-03-27 19:58_

Can you tell me a bit more why you think SIM102 should be disabled. Is it because some type checkers don't support narrowing if it's an if-else expression?

---

_Comment by @real-or-random on 2025-03-31 07:05_

> Can you tell me a bit more why you think SIM102 should be disabled. Is it because some type checkers don't support narrowing if it's an if-else expression?

Okay, TIL, and now that you ask, I'm not so sure anymore.

Until now, my point was merely readability: I think branching on a Python version or on an OS is a different thing than branching on business logic, so believe it's good to have it in a separate if: Then your brain can easily focus on the part that concerns a particular OS, and skip the other parts. But this is my personal view, and I'm not sure how objective it is. My thinking was simply that if it has been decided that checks for `sys.platform` and `sys.version_info` should be treated differently for readability, then the same should apply to `os.name` and `platform.system()`.

But I was not aware that type checkers may be a reason. This goes back to [PEP 484](https://peps.python.org/pep-0484/#version-and-platform-checking), which says "Type checkers are expected to understand simple version and platform checks, e.g.: (...) `if sys.version_info[0] >= 3:` (...) `if sys.platform == 'win32':`". The [current docs](https://typing.python.org/en/latest/spec/directives.html#version-and-platform-checking) are similar, but use `sys.version_info >= (3,12)` as an example. 

After some research, I found [this issue about the set of conditions being underspecified](https://github.com/python/typing/issues/1732)... It's a good summary of what is supported by different typecheckers. For example, pyright supports `os.name` but mypy doesn't. None of them support platform.system()`.

My feeling is that a linter should be conservative here. If someone uses `os.name` to make pyright happy, this seems fine. But given that this is still very much under debate, I'm not sure if it's the right time to change SIM102...

In either case, I think the comments here should be updated:
https://github.com/astral-sh/ruff/blob/c6efa93cf7a630332901367a1badbdbadc46280e/crates/ruff_linter/src/rules/flake8_simplify/rules/collapsible_if.rs#L100-L108
 If I'm not mistaken, this file is only about SIM102, so this is not about ternaries. Moreover, `is_sys_version_block` also checks for `sys.platform`, so the name is misleading.

---

_Comment by @polyvertex on 2025-05-06 06:54_

I am not sure about the case given by OP, however there is at least one specific case where it might be okay to disable SIM102:

```python
if __debug__:  # noqa: SIM102
    if foo:
        ...
```

Although this may be considered bad practice which is another topic, the rationale behind this is to allow the interpreter to fully strip the code block when running with `python -O`.

This is at least what CPython does from what I can recall, as long as `__debug__` is the only test in the `if` statement.

---
