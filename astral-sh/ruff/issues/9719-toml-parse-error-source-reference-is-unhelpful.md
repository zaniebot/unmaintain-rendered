---
number: 9719
title: TOML parse error source reference is unhelpful
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2024-01-30T20:47:34Z
updated_at: 2025-01-11T16:38:23Z
url: https://github.com/astral-sh/ruff/issues/9719
synced_at: 2026-01-10T01:22:49Z
---

# TOML parse error source reference is unhelpful

---

_Issue opened by @zanieb on 2024-01-30 20:47_

e.g. in https://github.com/astral-sh/ruff/issues/9718 the provided source context is one line early and in a simple example

```toml
[tool.ruff]
err = "foo"
```

```
❯ ruff check
ruff failed
  Cause: Failed to parse /Users/mz/eng/src/astral-sh/ruff/pyproject.toml
  Cause: TOML parse error at line 1, column 1
  |
1 | [tool.ruff]
  | ^^^^^^^^^^^
unknown field `err`
```

---

_Label `bug` added by @zanieb on 2024-01-30 20:47_

---

_Label `configuration` added by @zanieb on 2024-01-30 20:47_

---

_Label `configuration` removed by @zanieb on 2024-01-30 20:47_

---

_Comment by @charliermarsh on 2024-01-31 22:17_

Off-by-one error? Couldn't be us!

---

_Comment by @charliermarsh on 2024-01-31 22:27_

This actually might be intended, like Serde is saying there's an unknown field _in that struct_?

---

_Comment by @zanieb on 2024-01-31 22:36_

Hm true if I do

```toml
[tool.ruff]
preview = true
err = "what?"
```

we get the same thing

```
❯ ruff check .
ruff failed
  Cause: Failed to parse /Users/mz/eng/src/astral-sh/ruff/pyproject.toml
  Cause: TOML parse error at line 1, column 1
  |
1 | [tool.ruff]
  | ^^^^^^^^^^^
unknown field `err`
```

But what about the linked issue where it highlights `select`??

---

_Comment by @zanieb on 2024-01-31 22:38_

Ah they're using a `ruff.toml`.. it highlights the first line there

```toml
preview = true
err = "what"
```

```
❯ ruff check . --config ruff.toml
ruff failed
  Cause: Failed to parse /Users/mz/eng/src/astral-sh/ruff/ruff.toml
  Cause: TOML parse error at line 1, column 1
  |
1 | preview = true
  | ^^^^^^^^^^^^^^
unknown field `err`
```

---

_Renamed from "TOML parse error source reference is off by one line" to "TOML parse error source reference is unhelpful" by @zanieb on 2024-01-31 22:38_

---

_Comment by @charliermarsh on 2024-02-01 15:59_

Some chance that we have no control over this, not sure.

---

_Referenced in [astral-sh/ruff#11865](../../astral-sh/ruff/issues/11865.md) on 2024-06-14 00:32_

---

_Comment by @charliermarsh on 2024-11-26 03:16_

I suspect this is from using `flatten`. In uv, we have a fully expanded version (`OptionsWire`) that we then translate to `Options`.

---

_Referenced in [astral-sh/ruff#14640](../../astral-sh/ruff/issues/14640.md) on 2024-11-27 18:14_

---

_Comment by @MichaReiser on 2025-01-10 19:27_

Ruff only uses one flatten but it's a struct with a fair amount of options, but they change rarely. It might be worth to copy them if it helps improve the error messages

---

_Comment by @charliermarsh on 2025-01-10 19:28_

I'm in favor but I know it's annoying.

---

_Comment by @MichaReiser on 2025-01-10 19:29_

Yeah, it's like 400 lines of code :D 

https://github.com/astral-sh/ruff/blob/1ef0f615f173e5899fe0e3cfd08e015ff5565e4b/crates/ruff_workspace/src/options.rs#L535-L935

---

_Comment by @MichaReiser on 2025-01-10 19:30_

Could we hack something with [`include`](https://doc.rust-lang.org/std/macro.include.html)?

---

_Referenced in [astral-sh/ruff#15414](../../astral-sh/ruff/pulls/15414.md) on 2025-01-11 06:05_

---

_Closed by @dhruvmanila on 2025-01-11 16:38_

---

_Closed by @dhruvmanila on 2025-01-11 16:38_

---
