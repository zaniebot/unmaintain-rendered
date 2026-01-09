---
number: 5586
title: Missing license text for crates/uv-python/fetch-download-metadata.py
type: issue
state: closed
author: musicinmybrain
labels:
  - internal
assignees: []
created_at: 2024-07-30T04:30:27Z
updated_at: 2024-07-30T12:26:00Z
url: https://github.com/astral-sh/uv/issues/5586
synced_at: 2026-01-07T13:12:17-06:00
---

# Missing license text for crates/uv-python/fetch-download-metadata.py

---

_Issue opened by @musicinmybrain on 2024-07-30 04:30_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Similarly to https://github.com/astral-sh/uv/issues/5584, `crates/uv-python/fetch-download-metadata.py` is derived from MIT-licensed code but does not reproduce the original license text.

https://github.com/astral-sh/uv/blob/c0d3da8b6ae134036b3ed024762b08c4fb9f967b/crates/uv-python/fetch-download-metadata.py#L18-L19

It looks like this is just a utility script for generating `download-metadata.json` and is not compiled into the `uv` executable, so one possibility would be to just add the contents of https://github.com/astral-sh/rye/raw/f9822267a7f00332d15be8551f89a212e7bc9017/LICENSE as a comment in the script itself, rather than, say, adding a `LICENSE.rye` file to the crate. (For files that *do* end up somewhere inside the `uv` executable, a separate license text file is much easier for distribution packagers who are trying to make sure all necessary license texts are included in compiled binary packages.)

---

_Referenced in [astral-sh/uv#5587](../../astral-sh/uv/pulls/5587.md) on 2024-07-30 04:56_

---

_Closed by @charliermarsh on 2024-07-30 12:15_

---

_Label `internal` added by @charliermarsh on 2024-07-30 12:26_

---
