---
number: 14314
title: "Support installation of executables of plugins with `uv tool install`"
type: issue
state: closed
author: mih
labels:
  - question
assignees: []
created_at: 2025-06-27T11:30:22Z
updated_at: 2025-08-21T14:54:45Z
url: https://github.com/astral-sh/uv/issues/14314
synced_at: 2026-01-07T13:12:18-06:00
---

# Support installation of executables of plugins with `uv tool install`

---

_Issue opened by @mih on 2025-06-27 11:30_

### Summary

I am asking for the equivalent functionality of a `pipx install --include-deps`.

This would support tools that use plugin mechanism that are based on executables (with particular names) that need to be found in the `PATH`, like `git` does (e.g. with `git-remote-rad`, `git-bug`, etc.).

At the moment, only the executables of the top-level tool package are linked to `$UV_TOOL_BIN_DIR`.

So in other words, it would be great to be able to do something like

```
uv tool install datalad --with-plugin-apps --with datalad-next
```

And have the executables provided by `datalad-next` also be linked to `$UV_TOOL_BIN_DIR`.

Thanks!

### Example

_No response_

---

_Label `enhancement` added by @mih on 2025-06-27 11:30_

---

_Comment by @konstin on 2025-08-21 12:59_

Can you use `uv tool install --with-executables-from`?

---

_Label `enhancement` removed by @konstin on 2025-08-21 12:59_

---

_Label `question` added by @konstin on 2025-08-21 12:59_

---

_Comment by @zanieb on 2025-08-21 13:06_

See https://docs.astral.sh/uv/concepts/tools/#installing-executables-from-additional-packages

---

_Comment by @mih on 2025-08-21 14:54_

Thank you! I missed that. It works just as desired.

---

_Closed by @mih on 2025-08-21 14:54_

---
