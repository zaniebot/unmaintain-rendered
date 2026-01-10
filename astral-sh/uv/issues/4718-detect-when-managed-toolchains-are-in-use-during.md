```yaml
number: 4718
title: Detect when managed toolchains are in use during uninstall
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2024-07-02T02:23:40Z
updated_at: 2025-10-06T09:29:32Z
url: https://github.com/astral-sh/uv/issues/4718
synced_at: 2026-01-10T03:23:52Z
```

# Detect when managed toolchains are in use during uninstall

---

_Issue opened by @zanieb on 2024-07-02 02:23_

We should track our uses of managed toolchains (e.g. when used in `uv venv`, `uv tool install` and `uv sync`) then ensure that the toolchain is no longer in use before removing it during uninstall or require opt-in to force removal.

---

_Label `needs-design` added by @zanieb on 2024-07-02 02:23_

---

_Label `enhancement` added by @zanieb on 2024-07-02 02:23_

---

_Comment by @aretrace on 2025-01-07 04:23_

Could tools invoke a separate interpreter copy (e.g., `cpython-3.12.8-macos-aarch64-none-<TOOL_NAME>`)?
An additional option for interpreter selection during tool installation would make sense  under this scenario.

---

_Comment by @zanieb on 2025-01-07 17:26_

Yeah we could have each tool get a dedicated interpreter, but I don't think that's ideal unless we can use hard-links?

I would rather:

- Not allow uninstall unless you change the tools (or use a force flag)
- Make it easy to change tools to another Python version
- Link tools to a "stable" directory of minor Python interpreter versions so patch upgrades do not affect them

---

_Comment by @aretrace on 2025-01-07 23:16_

👆 yes

---

_Assigned to @jtfmumm by @jtfmumm on 2025-03-15 18:46_

---

_Unassigned @jtfmumm by @konstin on 2025-10-06 09:29_

---
