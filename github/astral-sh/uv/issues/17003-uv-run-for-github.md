---
number: 17003
title: Uv run for GitHub
type: issue
state: open
author: antoinebrijesh13
labels:
  - enhancement
assignees: []
created_at: 2025-12-05T18:11:33Z
updated_at: 2025-12-26T16:00:59Z
url: https://github.com/astral-sh/uv/issues/17003
synced_at: 2026-01-07T13:12:19-06:00
---

# Uv run for GitHub

---

_Issue opened by @antoinebrijesh13 on 2025-12-05 18:11_

### Summary

uv run currently works great for local projects, but running something directly from a GitHub repo requires manual cloning, syncing, and cleanup. Adding support for Git URLs—similar to Cargo’s --git feature—would let users instantly run remote Python projects in a clean, temporary environment. This avoids filesystem pollution and dramatically simplifies trying out demos, tools, and PR branches.

### Example

Before (Current Workflow)

git clone https://github.com/user/repo
cd repo
uv sync
uv run example.py
cd ..
rm -rf repo

A multi-step process with leftover folders.

After (Proposed Workflow)

uv run https://github.com/user/repo -- example.py

Auto-clone into a temporary directory

Auto-create & sync a virtual environment

Run the command

Auto-clean everything afterward


Zero manual steps. Zero local clutter. Fully Cargo-like convenience.

---

_Label `enhancement` added by @antoinebrijesh13 on 2025-12-05 18:11_

---

_Comment by @zanieb on 2025-12-05 19:36_

We do allow running Python projects from GitHub, but not invoking arbitrary scripts unless they're standalone PEP 723 scripts. Do you have a real example you want to use?

---

_Comment by @woodruffw on 2025-12-26 16:00_

Triage: Adding some examples of what @zanieb is talking about, we already support these:

```
# runs directly from a package
uvx git+https://...

# runs a specific script
uv run https://raw.github.com/...
```

`uv run` also supports Gists (https://github.com/astral-sh/uv/pull/15058), and I suspect `--script` would work for inline script metadata.

---

_Comment by @woodruffw on 2025-12-26 16:00_

Triage: potential dupe: https://github.com/astral-sh/uv/issues/12268

---
