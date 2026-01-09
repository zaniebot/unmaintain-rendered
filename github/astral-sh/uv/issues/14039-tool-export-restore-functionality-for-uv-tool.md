---
number: 14039
title: "Tool export/restore functionality for `uv tool`"
type: issue
state: closed
author: apcamargo
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2025-06-14T18:31:20Z
updated_at: 2025-06-18T16:34:14Z
url: https://github.com/astral-sh/uv/issues/14039
synced_at: 2026-01-07T13:12:18-06:00
---

# Tool export/restore functionality for `uv tool`

---

_Issue opened by @apcamargo on 2025-06-14 18:31_

### Summary

Currently, `uv` does not provide a straightforward way to export or restore tools installed via `uv tool`. For users who have many tools installed, or who rely on complex installations (e.g., tools with multiple optional dependencies, which is common in data science workflows), there is no simple mechanism to back up these tools' environments and restore them across different machines.

This feature exists in Homebrew via the [`brew bundle`](https://docs.brew.sh/Brew-Bundle-and-Brewfile) command, which allows users to generate a Brewfile that lists all installed applications. This file can then be used to reproduce the setup on another machine.

It would be very helpful if `uv` offered a comparable feature: the ability to export the list of installed tools and their configurations into a file, and restore them from that file on another system. This would significantly simplify environment setup and synchronization across multiple devices.

### Example

```sh
uv tool dump > toolfile.txt
uv tool restore toolfile.txt
```

---

_Label `enhancement` added by @apcamargo on 2025-06-14 18:31_

---

_Comment by @konstin on 2025-06-18 16:18_

The difficult part here is defining a (toml) export format that we can commit to supporting over many versions. It may be easier to fold this into https://github.com/astral-sh/uv/issues/5903, where each tool would be defined in `pyproject.toml` and tied to that `pyproject.toml`.

---

_Comment by @zanieb on 2025-06-18 16:27_

This is a duplicate of https://github.com/astral-sh/uv/issues/12533

I actually think we're pretty far along on a TOML format, because we already write TOML tool receipts to our storage — we'd just need to do the work to make a similar format user-facing.

---

_Label `duplicate` added by @zanieb on 2025-06-18 16:27_

---

_Closed by @konstin on 2025-06-18 16:34_

---
