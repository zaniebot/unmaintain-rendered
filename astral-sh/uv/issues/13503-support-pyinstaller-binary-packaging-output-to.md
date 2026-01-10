---
number: 13503
title: Support PyInstaller binary packaging output to dist/ directory
type: issue
state: closed
author: xaviergurram
labels:
  - enhancement
assignees: []
created_at: 2025-05-17T05:10:04Z
updated_at: 2025-05-17T11:15:59Z
url: https://github.com/astral-sh/uv/issues/13503
synced_at: 2026-01-10T01:25:34Z
---

# Support PyInstaller binary packaging output to dist/ directory

---

_Issue opened by @xaviergurram on 2025-05-17 05:10_

### Summary

It would be beneficial if uv could support creating standalone binaries using PyInstaller, outputting the result to the standard dist/ directory alongside wheels and source distributions.

Currently, developers who want to distribute their apps as binaries must manage PyInstaller separately. Adding support in uv would:

Provide a unified experience for packaging Python apps (wheels + binaries).

Streamline workflows, especially in CI/CD.

Follow a consistent build artifact structure (dist/).

Improve developer ergonomics and reduce the need for extra configuration scripts.

Optional Features
Optional PyInstaller config integration (.spec file).
Support for cross-platform builds (long-term).
Optional Rust-native equivalent for performance and smaller binaries (exploratory).

### Example

A new command or flag, e.g.:

uv build --binary

or

uv pyinstaller main.py --onefile

Automatically places the .exe or binary into dist/.

---

_Label `enhancement` added by @xaviergurram on 2025-05-17 05:10_

---

_Comment by @FishAlchemist on 2025-05-17 10:42_

Given that not everyone chooses ``PyInstaller`` for packaging into binary files, I don't think making it part of the uv command is a good decision.

There's a discussion going on about packaging into binary files.
* https://github.com/astral-sh/uv/issues/2799
* https://github.com/astral-sh/uv/issues/5802

---

_Comment by @charliermarsh on 2025-05-17 11:15_

👍 I think we'd prefer to track this as a first-class feature elsewhere.

---

_Closed by @charliermarsh on 2025-05-17 11:15_

---
