---
number: 12638
title: Complain more loudly about unknown Dependency Object Specifiers
type: issue
state: closed
author: Gankra
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2025-04-02T20:59:53Z
updated_at: 2025-04-10T20:56:48Z
url: https://github.com/astral-sh/uv/issues/12638
synced_at: 2026-01-10T01:25:22Z
---

# Complain more loudly about unknown Dependency Object Specifiers

---

_Issue opened by @Gankra on 2025-04-02 20:59_

### Summary

This currently does not produce an error or default-visible warning. (It results in a `tracing::warn!` which is hidden by default):

```toml
[dependency-groups]
foo = ["pyparsing"]
bar = [{set-phasers-to = "stun"}]
```

The `set-phasers-to` get parsed as an unknown Dependency Object Specifier (`DependencyGroupSpecifier::Object`), which is a future-compat concept [from PEP-735 dependency-groups](https://peps.python.org/pep-0735/#validation-and-compatibility):

> Tools SHOULD error when evaluating or processing unrecognized data in Dependency Groups.
> 
> Tools SHOULD NOT eagerly validate the list contents of all Dependency Groups.
> 
> This means that in the presence of the (above example), most tools will allow the foo group to be used, and will only error when the bar group is used.

"When the group is used" is a fuzzy concept because we generate lockfiles that are supposed to validate/cover all possible invocations. As such we should probably error when we encounter these, or at least unconditionally warn to stderr!

### Platform

all

### Version

uv 0.6.11

### Python version

_No response_

---

_Label `bug` added by @Gankra on 2025-04-02 20:59_

---

_Label `help wanted` added by @Gankra on 2025-04-02 20:59_

---

_Assigned to @Gankra by @Gankra on 2025-04-02 21:00_

---

_Label `good first issue` added by @Gankra on 2025-04-02 21:24_

---

_Comment by @kiran-4444 on 2025-04-04 18:17_

I'd love to work on this.

---

_Comment by @zanieb on 2025-04-04 18:37_

Feel free to put up a pull request.

---

_Comment by @kiran-4444 on 2025-04-04 18:38_

Thanks!  Can you assign this to me so that I won't lose track of it?

---

_Assigned to @kiran-4444 by @zanieb on 2025-04-04 18:40_

---

_Comment by @kiran-4444 on 2025-04-09 18:51_

Umm, how do I reproduce this locally using `cargo run` from the uv clone directory? Do I have to create a new workspace with a pyproject.toml in it? 

Sorry if this seems too basic, this is my first time working with this locally (but I have used uv, though, mostly as pip replacement

---

_Comment by @zanieb on 2025-04-09 18:58_

I would `uv init example && cd example`, then edit `pyproject.toml`, then `cargo run -- uv sync` or similar.

---

_Referenced in [astral-sh/uv#12811](../../astral-sh/uv/pulls/12811.md) on 2025-04-10 16:27_

---

_Closed by @Gankra on 2025-04-10 20:56_

---
