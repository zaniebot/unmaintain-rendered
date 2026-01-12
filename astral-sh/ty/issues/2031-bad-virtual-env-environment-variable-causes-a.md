```yaml
number: 2031
title: Bad VIRTUAL_ENV environment variable causes a panic
type: issue
state: closed
author: joshzcold
labels:
  - configuration
assignees: []
created_at: 2025-12-17T19:50:21Z
updated_at: 2025-12-18T14:47:04Z
url: https://github.com/astral-sh/ty/issues/2031
synced_at: 2026-01-12T15:54:26Z
```

# Bad VIRTUAL_ENV environment variable causes a panic

---

_@joshzcold_

### Summary

If `VIRTUAL_ENV` has a value that doesn't resolve properly `ty` will panic and exit.

```
ERROR panicked at crates/ty_server/src/session.rs:519:30:\nDefault configuration to be valid: Failed to discover local Python environment\n\nCaused by:\n    0: Invalid `VIRTUAL_ENV` environment variable `/home/joshua/git/neovim/python.nvim/examples/python_projects/uv/.venv`: does not point to a directory on disk\n    1: No such file or directory (os error 2)\n   0: <unknown>\n   1: <unknown>\n   2: <unknown>\n   3: <unknown>\n   4: <unknown>\n   5: <unknown>\n   6: <unknown>\n   7: <unknown>\n   8: <unknown>\n   9: <unknown>\n  10: <unknown>\n  11: start_thread\n             at ./nptl/pthread_create.c:447:8\n  12: clone3\n             at ./misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:78:0\n\npanicked at crates/ty_server/src/session.rs:519:30:\nDefault configuration to be valid: Failed to discover local Python environment\n\nCaused by:\n    0: Invalid `VIRTUAL_ENV` environment variable `/home/joshua/git/neovim/python.nvim/examples/python_projects/uv/.venv`: does not point to a directory on disk\n    1: No such file or directory (os error 2)\n   0: <unknown>\n   1: <unknown>\n   2: <unknown>"
```

**Expected Behavior**: 
`ty` should see this issue in `VIRTUAL_ENV`, log a warning and fail back to the next available python environment (i.e system)

This would add flexability in code editors that need to add/delete/change their venv and still have `ty` be attached/detached without issue.

### Version

ty 0.0.2

---

_Comment by @zanieb on 2025-12-17 19:59_

Thanks for the report!

---

_Assigned to @zanieb by @zanieb on 2025-12-17 20:01_

---

_Comment by @zanieb on 2025-12-17 20:48_

There are two concerns here

1. Generally, what should we do when `VIRTUAL_ENV` refers to a path that does not exist?
2. We should not panic when the default settings are not valid. See https://github.com/astral-sh/ty/issues/859

---

_Comment by @zanieb on 2025-12-17 20:57_

Can you share how you were running ty?

---

_Label `configuration` added by @carljm on 2025-12-17 21:15_

---

_Comment by @MichaReiser on 2025-12-17 21:58_

I think all that is needed here is to have VIRTUAL_ENV set to an incorrect path

I'm not sure if it's even worth erroring if the VIRTUAL_ENV path is incorrect. Just logging a warning is probably fine

---

_Comment by @AlexWaygood on 2025-12-17 22:02_

> I'm not sure if it's even worth erroring if the VIRTUAL_ENV path is incorrect. Just logging a warning is probably fine

I think I'd much prefer a hard exit when invoked on the CLI. I don't see a use case for setting an invalid `VIRTUAL_ENV` from the CLI and I'd want to fix that immediately as a user, since the type-checking results could make very little sense in that situation.

The LSP case seems different; I think logging a warning and continuing could make sense there

---

_Comment by @zanieb on 2025-12-17 23:10_

> I think I'd much prefer a hard exit when invoked on the CLI. I don't see a use case for setting an invalid VIRTUAL_ENV from the CLI and I'd want to fix that immediately as a user, since the type-checking results could make very little sense in that situation.

I'm not sure if I agree. `uv pip install` doesn't hard exit here — and from experience there it's unfortunately common for people to have invalid `VIRTUAL_ENV` variables.

---

_Comment by @joshzcold on 2025-12-17 23:17_

> Can you share how you were running ty?

unique use case here. Running `ty` as an lsp server in neovim.

I maintain a python dev neovim plugin.
https://github.com/joshzcold/python.nvim/tree/main

Ran into this issue where I started with a good `VIRTUAL_ENV` path. My plugin allows the user to delete their venv (for whatever reason, maybe they were stuck).
My plugin did not clear out `VIRTUAL_ENV` (probably a good thing for me to fix) and after restarting the lsp server I ran into this issue.

In my mind an lsp server should avoid crashing if possible. I do think the cli might be a seperate ballpark.

In my plugin the user would re-install the venv, restart the lsp server and continue as normal. but a crashed lsp I think would interrupt the user without them knowing why (without going to logs)

---

_Comment by @zanieb on 2025-12-17 23:38_

@AlexWaygood moving that discussion to https://github.com/astral-sh/ty/issues/2044

Thanks for the details @joshzcold — we should definitely not have the server fail and can track that here. https://github.com/astral-sh/ruff/pull/22040 will resolve that.

---

_Closed by @Gankra on 2025-12-18 14:47_

---
