---
number: 10282
title: "`Parameter` range is incorrect for `*args` and `**kwargs`"
type: issue
state: closed
author: GtrMo
labels:
  - internal
assignees: []
created_at: 2024-03-07T22:58:34Z
updated_at: 2024-03-08T23:57:50Z
url: https://github.com/astral-sh/ruff/issues/10282
synced_at: 2026-01-10T01:22:49Z
---

# `Parameter` range is incorrect for `*args` and `**kwargs`

---

_Issue opened by @GtrMo on 2024-03-07 22:58_

I'm working on a fix for the `flake8-unused-arguments` (ARG) rules that would delete them.

While working on this, I noticed that the `*args` and `**kwargs` `Parameter.range` does not include the `*`. 
I think that `Parameter.range` should include `*` or `**`, leaving `Parameter.name.range` for the actual name range if needed.

```python
def f(*args, **kwargs): pass
#      ~~~~    ~~~~~~    <-- actual range
#     ^^^^^  ^^^^^^^^    <-- expected range
```

<details>
<summary>See the currently generated AST</summary>

https://github.com/astral-sh/ruff/blob/7b4a73d421c130430883bb944a285f096e3aa9a5/crates/ruff_python_parser/src/snapshots/ruff_python_parser__function__tests__function_pos_and_kw_only_args_with_defaults_and_varargs_and_kwargs.snap#L58-L67

Here I would expect the `Parameter` range to be 1 longer than the `Identifier` range.

https://github.com/astral-sh/ruff/blob/7b4a73d421c130430883bb944a285f096e3aa9a5/crates/ruff_python_parser/src/snapshots/ruff_python_parser__function__tests__function_pos_and_kw_only_args_with_defaults_and_varargs_and_kwargs.snap#L124-L134

And here I would expect the `Parameter` range to be 2 longer than the `Identifier` range.
</details>

If this is indeed considered a bug, I have a PR ready to fix it.





---

_Comment by @charliermarsh on 2024-03-07 23:18_

I generally agree, since `x: int = 1` is the parameter range for a non-variadic parameter, it makes sense that the `*` and `**` would be included. CPython doesn't seem to include it in the range, but our AST structure is a bit different...

---

_Label `internal` added by @charliermarsh on 2024-03-07 23:18_

---

_Referenced in [astral-sh/ruff#10283](../../astral-sh/ruff/pulls/10283.md) on 2024-03-07 23:57_

---

_Closed by @charliermarsh on 2024-03-08 23:57_

---

_Closed by @charliermarsh on 2024-03-08 23:57_

---
