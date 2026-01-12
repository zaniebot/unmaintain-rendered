```yaml
number: 17364
title: Allow relative paths (to the cwd) in UV_PYTHON_BIN_DIR and UV_TOOL_BIN_DIR
type: issue
state: closed
author: benjyw
labels:
  - enhancement
assignees: []
created_at: 2026-01-08T19:56:56Z
updated_at: 2026-01-09T13:21:49Z
url: https://github.com/astral-sh/uv/issues/17364
synced_at: 2026-01-12T16:02:49Z
```

# Allow relative paths (to the cwd) in UV_PYTHON_BIN_DIR and UV_TOOL_BIN_DIR

---

_@benjyw_

### Summary

Currently these env vars must be absolute paths, and if not they silently fall back to the default path.

This is inconsistent with other path overrides, such as UV_PYTHON_INSTALL_DIR, which may be relpaths.

Fixing this is a matter of changing [parse_path()](https://github.com/astral-sh/uv/blob/29285db48e1917db8d93081f7008484aceab8543/crates/uv-dirs/src/lib.rs#L87) to accept relpaths. As far as I can tell there is no further blast radius, as that helper function is only used by [user_executable_directory()](https://github.com/astral-sh/uv/blob/29285db48e1917db8d93081f7008484aceab8543/crates/uv-dirs/src/lib.rs#L24)

I am happy to make this change, but wanted to check first that this is acceptable, and that there is no subtle reason why it would be a bad idea. 

Thanks!

### Example

_No response_

---

_Label `enhancement` added by @benjyw on 2026-01-08 19:56_

---

_Renamed from "Allow relative paths (to the cwd) in UV_PYTHON_BIN_DIR" to "Allow relative paths (to the cwd) in UV_PYTHON_BIN_DIR and UV_TOOL_BIN_DIR" by @benjyw on 2026-01-08 19:57_

---

_Comment by @zanieb on 2026-01-08 20:56_

Hm I think this behavior was to match `dirs_sys`, though I don't know their motivation.

---

_Comment by @zanieb on 2026-01-08 20:58_

I think it'd be fine to change, as long as we filter empty strings still.

---

_Comment by @zanieb on 2026-01-08 20:59_

Ah

> All paths set in these environment variables must be absolute. If an implementation encounters a relative path in any of these variables it should consider the path invalid and ignore it.

https://specifications.freedesktop.org/basedir/latest/

---

_Comment by @zanieb on 2026-01-08 21:00_

So changing the behavior of `parse_path` and `user_executable_directory` would be wrong.

---

_Comment by @zanieb on 2026-01-08 21:12_

I think we could change the behavior specifically for our override variables though

---

_Comment by @benjyw on 2026-01-08 22:12_

Ah yes, that makes sense. I will do this more surgically as you suggest.
Thanks for the quick response!

On Thu, Jan 8, 2026, 1:12 PM Zanie Blue ***@***.***> wrote:

> *zanieb* left a comment (astral-sh/uv#17364)
> <https://github.com/astral-sh/uv/issues/17364#issuecomment-3725822923>
>
> I think we could change the behavior specifically for our override
> variables though
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/17364#issuecomment-3725822923>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAD5F7EKPUJ2ONR6W5QEY4T4F3B35AVCNFSM6AAAAACRDLBAX2VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTOMRVHAZDEOJSGM>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @zanieb on 2026-01-08 22:18_

I put up #17367 already if you want to review that :)

---

_Comment by @benjyw on 2026-01-09 00:58_

Hah! Thanks for this, will review right now. 

For context: I'm writing a [new Python backend](https://github.com/pantsbuild/pants/discussions/20897) for the [Pants build system](https://github.com/pantsbuild/pants), and it executes uv for various effects, such as installing pythons. Pants executes processes in sandboxes, and the sandbox directory path is not yet known when setting up the process inputs, so it is not possible to locate abspaths in the sandbox up front (it can be done by wrapping the process in a shell script, but that is more complex, and means locating a shell, which we prefer not to do if we don't have to). 

---

_Closed by @zanieb on 2026-01-09 13:21_

---
