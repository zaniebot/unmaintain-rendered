---
number: 11321
title: "Allow using `latest` with extras in `uvx`"
type: issue
state: closed
author: unexge
labels:
  - bug
assignees: []
created_at: 2025-02-07T16:58:00Z
updated_at: 2025-02-08T11:17:33Z
url: https://github.com/astral-sh/uv/issues/11321
synced_at: 2026-01-07T13:12:18-06:00
---

# Allow using `latest` with extras in `uvx`

---

_Issue opened by @unexge on 2025-02-07 16:58_

### Summary

The following fails today, though I think it should work:
```bash
$ uvx --from 'httpx[cli]@latest' httpx
  × Failed to resolve tool requirement
  ╰─▶ Distribution not found at: file:///Users/unexge/Code/uv/latest
```

The following works fine:
```
$ uvx --from 'httpx[cli]' httpx
$ uvx --from 'httpx[cli] == 0.28' httpx
```

`uvx` version is `uv-tool-uvx 0.5.29 (ca73c4754 2025-02-05)`.


---

By looking at debug logs:
```
$ RUST_LOG=debug uvx --from 'httpx[cli]@latest' httpx
DEBUG uv 0.5.29 (ca73c4754 2025-02-05)
DEBUG Ignoring non-package name `httpx[cli]` in `--from`
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/Users/unexge/.local/share/uv/python`
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/opt/homebrew/bin/python3` (first executable in the search path)
DEBUG Acquired lock for `/Users/unexge/.local/share/uv/tools`
DEBUG Checking for Python environment at `/Users/unexge/.local/share/uv/tools/httpx`
DEBUG Released lock at `/Users/unexge/.local/share/uv/tools/.lock`
DEBUG Assessing Python executable as base candidate: /opt/homebrew/opt/python@3.13/bin/python3.13
DEBUG Assessing Python executable as base candidate: /opt/homebrew/opt/python@3.13/Frameworks/Python.framework/Versions/3.13/bin/python3.13
DEBUG Caching via base interpreter: `/opt/homebrew/opt/python@3.13/bin/python3.13`
DEBUG Using request timeout of 30s
  × Failed to resolve tool requirement
  ╰─▶ Distribution not found at: file:///Users/unexge/Code/uv/latest
```

It seems like `uvx` run into this case and thinks it's a local distribution: https://github.com/astral-sh/uv/blob/main/crates/uv/src/commands/tool/mod.rs#L50

### Example

_No response_

---

_Label `enhancement` added by @unexge on 2025-02-07 16:58_

---

_Label `enhancement` removed by @zanieb on 2025-02-07 17:16_

---

_Label `bug` added by @zanieb on 2025-02-07 17:16_

---

_Comment by @zanieb on 2025-02-07 17:16_

Thanks!

---

_Comment by @unexge on 2025-02-07 17:37_

Also using `@version` fails with extras:
```bash
$ uvx --from 'httpx[cli]@0.28' httpx
error: Failed to parse: `httpx[cli]@0.28`
  Caused by: Expected path (`/Users/unexge/Code/uv/0.28`) to end in a supported file extension: `.whl`, `.tar.gz`, `.zip`, `.tar.bz2`, `.tar.lz`, `.tar.lzma`, `.tar.xz`, `.tar.zst`, `.tar`, `.tbz`, `.tgz`, `.tlz`, or `.txz`
httpx[cli]@0.28
           ^^^^
```

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-08 01:46_

---

_Referenced in [astral-sh/uv#11335](../../astral-sh/uv/pulls/11335.md) on 2025-02-08 01:51_

---

_Closed by @charliermarsh on 2025-02-08 02:07_

---

_Closed by @charliermarsh on 2025-02-08 02:07_

---

_Comment by @unexge on 2025-02-08 11:17_

That was quick! Thanks @charliermarsh!

---
