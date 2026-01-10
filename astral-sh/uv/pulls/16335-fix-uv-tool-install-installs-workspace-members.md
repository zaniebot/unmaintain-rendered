```yaml
number: 16335
title: "Fix: uv tool install installs workspace members non‑editable by default; --editable to opt‑in"
type: pull_request
state: open
author: krishmas
labels:
  - enhancement
  - breaking
assignees: []
base: main
head: krish/tool3
created_at: 2025-10-17T05:10:51Z
updated_at: 2025-11-26T23:21:20Z
url: https://github.com/astral-sh/uv/pull/16335
synced_at: 2026-01-10T05:58:11Z
```

# Fix: uv tool install installs workspace members non‑editable by default; --editable to opt‑in

---

_Pull request opened by @krishmas on 2025-10-17 05:10_

## Summary

Resolves #16306

- Default non‑editable for workspace deps in tool installs
    - Tools no longer get editable .pth links to workspace members; installs are self‑contained.
    - Passing --editable makes both the target package and workspace members editable.
- Share editable conversion across commands
    - Move editable conversion into apply_editable_mode and reuse from both sync and tool install

## Test Plan

- Tool install integration tests
    - Default non‑editable workspace member (`tool_install::tool_install_no_editable_workspace_member`):
    - Explicit --editable (`tool_install::tool_install_workspace_editable`):
 - Locally used built uv with the Python project mentioned in #16306 and observed that changes to workspace dependency source code no longer changed tool behavior unless `--editable` was used 


---

_Marked ready for review by @krishmas on 2025-10-17 07:53_

---

_Converted to draft by @krishmas on 2025-10-17 07:53_

---

_Marked ready for review by @krishmas on 2025-10-17 08:24_

---

_Review requested from @zanieb by @konstin on 2025-10-20 09:35_

---

_Label `enhancement` added by @konstin on 2025-10-20 09:35_

---

_Label `breaking` added by @konstin on 2025-10-20 09:35_

---

_Comment by @krishmas on 2025-10-23 20:59_

Hi @zanieb , would it be possible to get a review here? 🙏

---

_Comment by @zanieb on 2025-10-23 21:02_

I spent a bit of time exploring an alternative (mostly guiding Claude) and was considering something more of the flavor here https://github.com/astral-sh/uv/compare/zb/tool-workspace

I'm not sure which is best yet, I'll try to find time to look closer but my time is elsewhere right now.

---

_Comment by @krishmas on 2025-11-10 22:07_

Hi @zanieb , just wanted to ping again - anything I can do to help out here?

---

_Comment by @krishmas on 2025-11-20 19:00_

@zanieb just bumping this again - lmk your thoughts. Happy to stop pinging if this is still something you want to think about more before merging this in.

---

_Comment by @zanieb on 2025-11-20 20:17_

I haven't had a chance to look further. I'm pretty sure the approach in https://github.com/astral-sh/uv/pull/16335#issuecomment-3439165062 makes more sense, but maybe @konstin can confirm he agrees before you pursue that?

---

_Comment by @konstin on 2025-11-24 13:52_

The approach in https://github.com/astral-sh/uv/compare/zb/tool-workspace makes more sense to me: We should decide before doing the resolution if the default for workspace packages should be editable or non-editable, instead of changing this after the resolution is done.

There's also the question whether we want to expose inheriting the workspace editable state to users (like `path = "foo", editable = "workspace"`), but that's a second or third step.

---

_Comment by @krishmas on 2025-11-26 23:21_

Great thanks @zanieb @konstin ! I'll try to take a stab when I get some time

---
