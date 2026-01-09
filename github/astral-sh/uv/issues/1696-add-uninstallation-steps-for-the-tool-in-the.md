---
number: 1696
title: Add uninstallation steps for the tool in the documentation
type: issue
state: closed
author: fervand1
labels:
  - documentation
assignees: []
created_at: 2024-02-19T14:27:14Z
updated_at: 2024-09-25T00:36:17Z
url: https://github.com/astral-sh/uv/issues/1696
synced_at: 2026-01-07T13:12:16-06:00
---

# Add uninstallation steps for the tool in the documentation

---

_Issue opened by @fervand1 on 2024-02-19 14:27_

As an end user, it would be nice to also include uninstallation steps in the documentation or on pypi for windows specially if the tool is installed via the script `irm https://astral.sh/uv/install.ps1 | iex`. On pip it is relatively easy with pip uninstall.



---

_Renamed from "Add uninstallation docs in the documentation" to "Add uninstallation steps for the tool in the documentation" by @fervand1 on 2024-02-19 14:27_

---

_Label `documentation` added by @zanieb on 2024-02-19 15:36_

---

_Comment by @zanieb on 2024-02-19 15:36_

Seems reasonable!

---

_Comment by @wQvaale on 2024-04-02 11:35_

Same issue here 👋

I initially installed uv by running `curl -LsSf https://astral.sh/uv/install.sh | sh`.

I realized I was behind on versions ([v0.1.21](https://github.com/astral-sh/uv/blob/main/CHANGELOG.md#0121)) and noticed that `uv self update` was introduced in [v0.1.23](https://github.com/astral-sh/uv/blob/main/CHANGELOG.md#0123).

Since there were no explicit uninstallation instructions, and uv was installed as a standalone binary (not through Cargo, so `cargo uninstall uv` wasn't an option), I decided to manually uninstall it.

Here’s how I did it:

Clear the cache directory (optional, depending on your setup):
```bash
rm -rf $HOME/.cache/uv
```

Remove the uv binary
```bash
rm $HOME/.cargo/bin/uv
```

Then I ran the same install with curl, and now I have the `self update` capability. Life is great once again ✨ 

---

_Comment by @will-henney on 2024-09-25 00:14_

This seems to be well documented now in https://docs.astral.sh/uv/getting-started/installation/#uninstallation so you might want to close this issue

---

_Comment by @zanieb on 2024-09-25 00:36_

Thanks!

---

_Closed by @zanieb on 2024-09-25 00:36_

---
