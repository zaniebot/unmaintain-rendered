---
number: 7806
title: Remove rule A003
type: issue
state: closed
author: joaoe
labels:
  - bug
assignees: []
created_at: 2023-10-04T10:18:46Z
updated_at: 2024-01-11T17:59:42Z
url: https://github.com/astral-sh/ruff/issues/7806
synced_at: 2026-01-10T01:22:47Z
---

# Remove rule A003

---

_Issue opened by @joaoe on 2023-10-04 10:18_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi.

Rule A003 (inherited from the flake8-builtins project) makes no sense.
Class scopes do not leak symbols to the global scope. They are their own isolated scopes.

E.g. this will trigger the warning
```
class X(logging.Formatter):
    def format(self, msg):
        ...
```

The warning is implemented here
https://github.com/astral-sh/ruff/blob/a1509dfc7c2b16f3762545fc802d49f5f03726e2/crates/ruff_linter/src/rules/flake8_builtins/rules/builtin_attribute_shadowing.rs

Issue on master branch.

Alternatively, rename it to A903 so it is an opinionated warning.

PS: there was a bug open in the original project but the owner is not taking any initiative https://github.com/gforcada/flake8-builtins/issues/75

---

_Comment by @charliermarsh on 2023-10-04 13:44_

Yeah, it's kind of a confusing rule. It's not _entirely_ useless -- it does make some sense when you consider cases like:

```python
class C:
    @staticmethod
    def list() -> None:
        pass

    @staticmethod
    def repeat(value: int, times: int) -> list[int]:
        return [value] * times
```

In the latter definition, `list` in `list[int]` resolves to the `def list` in the class scope. We've actually seen issues before asking us to catch this exact error. Is this clarifying at all?

I don't know that renaming to A903 is that helpful, since it'd still be enabled via `--select A`. (The 900-level thing isn't respected in Ruff.) So you'd be in the same position of needing to add it to your `ignore`.

We could consider only flagging A003 the actual _usage_, i.e., when you do `list[int]` above and it resolves to the class symbol, since that's very likely a mistake?


---

_Label `needs-decision` added by @charliermarsh on 2023-10-04 13:44_

---

_Comment by @joaoe on 2023-10-05 22:03_

> We could consider only flagging A003 the actual usage, i.e., when you do list[int] above and it resolves to the class symbol, since that's very likely a mistake?

That sounds like a good idea ! 

But that example is not the best, since type hints can be just strings with a future import annotations.

> The 900-level thing isn't respected in Ruff

DIdn't know that.

---

_Comment by @wyuenho on 2023-10-10 19:37_

@charliermarsh That is a very niche case that's probably better dealt with by having A003 exclusively detect the case where a function or **static** method name is shadowing a built-in. 

The problem now is the code path that detects A001 will warn about A003 if it's a class attribute assignment, A003 will warn again when it is a class attribute assignment, and A003 makes no distinction between instance/class methods which you have to access via a selector, and static methods, which do not have this requirement.

P.S. I think there should be a rule that errors whenever a static method is introduced but I digress...

---

_Comment by @charliermarsh on 2023-10-10 20:17_

@wyuenho - I don't believe that `list` has to be static above -- Pyright gives you an `error: Expected type expression but received "(self: Self@C) -> None" (reportGeneralTypeIssues)` error even when `list` is an instance method, and that _does_ make sense to me given Python's scoping rules: bindings introduced in the class scope can be accessed when referenced from directly within the class scope.

---

_Comment by @wyuenho on 2023-10-11 02:18_

That makes sense, and it's a case of https://github.com/gforcada/flake8-builtins/issues/75#issuecomment-1176549868.

Is there an easy way to only warn when there's a reference in the class scope call site? If not then I guess it's worth documenting some of the cases here and suggest to the user to ignore A003 as there are too many false negatives, and type checkers such as mypy and pyright can already catch some of them.

---

_Comment by @charliermarsh on 2023-10-11 03:48_

I think we can feasibly change this rule to only flag cases in which you shadow a builtin, and then it gets referenced (like the `list` case above). That should eliminate the vast, vast majority of false positives.

---

_Label `needs-decision` removed by @charliermarsh on 2023-10-11 03:48_

---

_Label `bug` added by @charliermarsh on 2023-10-11 03:48_

---

_Referenced in [astral-sh/ruff#9462](../../astral-sh/ruff/pulls/9462.md) on 2024-01-11 02:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-11 02:34_

---

_Comment by @charliermarsh on 2024-01-11 02:35_

PR up that should make this rule _far_ more useful by only flagging actual shadowed reads (like in the example above) rather than the method definitions (which will almost always be fine): https://github.com/astral-sh/ruff/pull/9462

---

_Closed by @charliermarsh on 2024-01-11 17:59_

---

_Closed by @charliermarsh on 2024-01-11 17:59_

---

_Referenced in [astral-sh/ruff#10433](../../astral-sh/ruff/issues/10433.md) on 2024-03-17 10:48_

---
