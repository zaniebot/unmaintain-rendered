---
number: 5556
title: SPECIFICATION.md in async_zip fork is not redistributable
type: issue
state: closed
author: musicinmybrain
labels: []
assignees: []
created_at: 2024-07-29T11:48:39Z
updated_at: 2024-07-29T16:05:54Z
url: https://github.com/astral-sh/uv/issues/5556
synced_at: 2026-01-07T13:12:17-06:00
---

# SPECIFICATION.md in async_zip fork is not redistributable

---

_Issue opened by @musicinmybrain on 2024-07-29 11:48_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

The fork of the [`async_zip` crate](https://crates.io/crates/async_zip) that `uv` uses contains a file, `SPECIFICATION.md`, with a proprietary license that does not allow redistribution. This is potentially an issue on its own, and it is also a challenge for downstream packaging because an archive of a snapshot of the git repository cannot be included in a source package until the file is filtered out.

This was reported upstream as https://github.com/Majored/rs-async-zip/issues/146 and fixed in https://github.com/Majored/rs-async-zip/commit/2d841d600c6d509cb4ecc611001ec339876ca6c9. (There hasn’t been a release since then, so the file is still present in the published `async_zip` crates, version 0.0.17 and earlier.)

Please consider fixing this by pulling https://github.com/Majored/rs-async-zip/commit/2d841d600c6d509cb4ecc611001ec339876ca6c9 into the fork https://github.com/charliermarsh/rs-async-zip and then updating the git hash in `uv`’s `Cargo.toml`:

https://github.com/astral-sh/uv/blob/78be9a6a6b2d6d315769c8ac6a0c6ee11602321b/Cargo.toml#L65

---

_Comment by @musicinmybrain on 2024-07-29 11:48_

Mentioning @charliermarsh as the owner of the fork.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-29 12:41_

---

_Comment by @charliermarsh on 2024-07-29 12:41_

Thanks, will take a look at this today.

---

_Referenced in [astral-sh/uv#5560](../../astral-sh/uv/pulls/5560.md) on 2024-07-29 14:14_

---

_Closed by @charliermarsh on 2024-07-29 14:42_

---

_Comment by @musicinmybrain on 2024-07-29 16:05_

Thanks! https://github.com/astral-sh/uv/pull/5560 looks good.

---

_Referenced in [astral-sh/uv#5588](../../astral-sh/uv/issues/5588.md) on 2024-07-30 05:05_

---
