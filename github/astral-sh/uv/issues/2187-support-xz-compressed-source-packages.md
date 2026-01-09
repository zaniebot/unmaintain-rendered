---
number: 2187
title: "Support `xz` compressed source packages"
type: issue
state: closed
author: lengau
labels:
  - help wanted
  - compatibility
assignees: []
created_at: 2024-03-04T23:56:49Z
updated_at: 2024-07-28T18:37:50Z
url: https://github.com/astral-sh/uv/issues/2187
synced_at: 2026-01-07T13:12:17-06:00
---

# Support `xz` compressed source packages

---

_Issue opened by @lengau on 2024-03-04 23:56_

Example failure:

```
$ uv pip install 'python-apt @ https://launchpad.net/ubuntu/+archive/primary/+sourcefiles/python-apt/2.7.6/python-apt_2.7.6.tar.xz'
error: Failed to download and build: python-apt @ https://launchpad.net/ubuntu/+archive/primary/+sourcefiles/python-apt/2.7.6/python-apt_2.7.6.tar.xz
  Caused by: Failed to extract source distribution
  Caused by: Unsupported archive type: python-apt_2.7.6.tar.xz
```

This is probably low priority, I'm sure.

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-03-05 00:19_

Interesting, does `pip` support it?

---

_Comment by @lengau on 2024-03-05 00:30_

It does - I haven't looked at how `pip` extracts source distributions in tarballs, but I'm guessing it does so through the `tarfile` module, which supports `gz`, `bz2` and `xz` compressed tarballs, so I would guess full feature parity here would be to support all three.

---

_Comment by @charliermarsh on 2024-03-05 00:32_

Sweet, thank you!

---

_Label `compatibility` added by @charliermarsh on 2024-03-05 00:32_

---

_Label `help wanted` added by @zanieb on 2024-07-01 21:57_

---

_Referenced in [astral-sh/uv#5513](../../astral-sh/uv/pulls/5513.md) on 2024-07-28 12:58_

---

_Closed by @charliermarsh on 2024-07-28 18:37_

---

_Referenced in [astral-sh/uv#16911](../../astral-sh/uv/issues/16911.md) on 2025-12-01 17:35_

---
