---
number: 9368
title: Fuzzer fails to build memchr
type: issue
state: closed
author: zanieb
labels:
  - ci
assignees: []
created_at: 2024-01-02T21:34:36Z
updated_at: 2024-01-26T14:46:55Z
url: https://github.com/astral-sh/ruff/issues/9368
synced_at: 2026-01-10T01:22:49Z
---

# Fuzzer fails to build memchr

---

_Issue opened by @zanieb on 2024-01-02 21:34_

See https://github.com/astral-sh/ruff/actions/runs/7390478731/job/20105487965

---

_Label `ci` added by @zanieb on 2024-01-02 21:34_

---

_Referenced in [astral-sh/ruff#9369](../../astral-sh/ruff/pulls/9369.md) on 2024-01-02 21:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-02 22:23_

---

_Comment by @charliermarsh on 2024-01-02 22:57_

@addisoncrump -- Any chance it's obvious to you why this started failing?

---

_Comment by @addisoncrump on 2024-01-02 23:27_

The CI tells the truth :dove: : https://github.com/astral-sh/ruff/actions/runs/7390478731/job/20105487965#step:6:39

It seems that musl is being set somehow.

---

_Comment by @addisoncrump on 2024-01-03 00:05_

Notably, it seems that the bin provided from binstall is a musl binary: https://github.com/astral-sh/ruff/actions/runs/7390478731/job/20105487965#step:5:39
This did not use to be the case: https://github.com/astral-sh/ruff/actions/runs/7389857960/job/20103579755#step:5:39

At a guess, this is some weird "rust binary expects consistency" issue. I'll try to repro locally.

---

_Comment by @addisoncrump on 2024-01-03 00:13_

Yup, just verified locally: building with [the version provided by cargo-fuzz upstream](https://github.com/rust-fuzz/cargo-fuzz/releases/tag/0.11.3) causes a build failure as it expects musl.

---

_Referenced in [rust-fuzz/cargo-fuzz#355](../../rust-fuzz/cargo-fuzz/issues/355.md) on 2024-01-03 00:15_

---

_Comment by @charliermarsh on 2024-01-03 00:26_

Nice, thank you. I was able to get CI passing by pinning to the previous minor: https://github.com/astral-sh/ruff/pull/9372

---

_Comment by @addisoncrump on 2024-01-03 00:36_

Resolved at record pace :rocket: I'll communicate upstream to try to resolve this there so we can remove the pinning. Don't close this issue yet?

---

_Referenced in [astral-sh/ruff#9372](../../astral-sh/ruff/pulls/9372.md) on 2024-01-03 00:45_

---

_Comment by @MichaReiser on 2024-01-26 07:28_

> Resolved at record pace 🚀 I'll communicate upstream to try to resolve this there so we can remove the pinning. Don't close this issue yet?

@addisoncrump Let us know when we're good to close the issue. 

---

_Comment by @addisoncrump on 2024-01-26 14:37_

You can close it this side, there's now a tracking issue upstream: https://github.com/taiki-e/install-action/pull/352

---

_Comment by @charliermarsh on 2024-01-26 14:46_

Thanks!

---

_Closed by @charliermarsh on 2024-01-26 14:46_

---

_Referenced in [astral-sh/ruff#14478](../../astral-sh/ruff/pulls/14478.md) on 2024-11-20 05:04_

---
