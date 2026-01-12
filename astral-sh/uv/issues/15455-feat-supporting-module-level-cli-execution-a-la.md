```yaml
number: 15455
title: "FEAT: supporting module-level CLI execution (à la `python -m`) directly through `uvx` ?"
type: issue
state: closed
author: neutrinoceros
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2025-08-22T14:59:11Z
updated_at: 2025-08-22T16:32:15Z
url: https://github.com/astral-sh/uv/issues/15455
synced_at: 2026-01-12T16:02:11Z
```

# FEAT: supporting module-level CLI execution (à la `python -m`) directly through `uvx` ?

---

_@neutrinoceros_

### Summary

I have a CLI tool that's designed to run as a module (`python -m runtime_introspect`), and I'd like to run it through the `uv tool` interface.

Currently, this command works
```
uv run -w runtime_introspect python -m runtime_introspect
```
but it's a little verbose.

meanwhile
```
uvx runtime_introspect
```
breaks because the package (intentionally) doesn't define an entry point.

For the same reason, it's impossible to run as
```
uvx --from runtime_introspect
```
because there's no named entry point to run.
Some packages, have a number of such modules, and no corresponding entry points (I'm thinking primarily of `rich`)

Would it make sense to add options to run such modules as
```
uvx -m rich.inspect
```

maybe even 
```
uvx --main runtime_introspect
```

as a shorthand for
```
uvx -m runtime_introspect.__main__
```
?
Similar arguments `-m` and `--main` could be added to `uv run` too (I think).

The one argument against it I could come up with is that such tools couldn't possibly be installed via `uv tool install`, which perhaps breaks the internal consistency too much to be acceptable. In any case, please let me know how reasonable this proposal sounds.

### Example

_No response_

---

_Label `enhancement` added by @neutrinoceros on 2025-08-22 14:59_

---

_Comment by @zanieb on 2025-08-22 16:15_

Thanks for the details!

This is a duplicate of https://github.com/astral-sh/uv/issues/7552

---

_Label `duplicate` added by @zanieb on 2025-08-22 16:15_

---

_Comment by @neutrinoceros on 2025-08-22 16:31_

Oh great, sorry I didn't find it before posting !


---

_Closed by @neutrinoceros on 2025-08-22 16:32_

---
