---
number: 14424
title: "Warn when `uv tool install package@latest` does not use the actual latest version"
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-07-02T15:44:31Z
updated_at: 2025-07-07T15:34:58Z
url: https://github.com/astral-sh/uv/issues/14424
synced_at: 2026-01-10T01:25:44Z
---

# Warn when `uv tool install package@latest` does not use the actual latest version

---

_Issue opened by @zanieb on 2025-07-02 15:44_

e.g., in https://github.com/astral-sh/uv/issues/14415#issuecomment-3028278275 it took me explicitly pinning the latest version to discover that we weren't allowing it for some reason.

---

_Label `enhancement` added by @zanieb on 2025-07-02 15:44_

---

_Comment by @stdedos on 2025-07-02 16:01_

So ... `uv tool install azure-cli` still won't install latest (for valid _reasons_ https://github.com/astral-sh/uv/issues/14415#issuecomment-3028278275).

"Will it warn in `package`, though?" Or only `package@latest`?

---

_Comment by @zanieb on 2025-07-02 16:04_

Yeah. We might want to warn in general, but at a minimum `@latest` should definitely help guide someone to a solution.

---

_Comment by @powercoconola on 2025-07-07 15:32_

When `package` is initially uninstalled, does `uv tool install package` imply `uv tool install package@latest`? Is there any functional difference?

---

_Comment by @zanieb on 2025-07-07 15:34_

The latter will invalidate our cache, which is otherwise effectively a ~10m retention of the latest version.

---
