---
number: 5630
title: "Consistent usage of \"pre-release\" vs. \"prerelease\""
type: issue
state: closed
author: ibraheemdev
labels:
  - documentation
assignees: []
created_at: 2024-07-30T19:11:49Z
updated_at: 2024-08-01T20:56:30Z
url: https://github.com/astral-sh/uv/issues/5630
synced_at: 2026-01-07T13:12:17-06:00
---

# Consistent usage of "pre-release" vs. "prerelease"

---

_Issue opened by @ibraheemdev on 2024-07-30 19:11_

Similar to https://github.com/astral-sh/uv/pull/5629. Also `pre_release` vs. `prerelease` and `PreRelease` vs. `Prerelease` in code.

---

_Renamed from "Consistent use of "pre-release" vs. "prerelease:" to "Consistent usage of "pre-release" vs. "prerelease"" by @ibraheemdev on 2024-07-30 19:11_

---

_Comment by @charliermarsh on 2024-07-30 19:12_

I think it should be "prerelease" (single word) to match PEP 440.

---

_Comment by @zanieb on 2024-07-30 19:34_

Sounds good to me. 

---

_Label `documentation` added by @zanieb on 2024-07-30 19:34_

---

_Comment by @charliermarsh on 2024-08-01 17:10_

Oh no, PEP 440 uses "prerelease" once but "pre-release" many times. Rust also seems to use "pre-release": https://doc.rust-lang.org/cargo/reference/resolver.html#pre-releases. And SemVer uses pre-release too: https://semver.org/.

So I'm going back on my suggestion: we should use "pre-release", and thus `PreRelease` in code.

---

_Comment by @charliermarsh on 2024-08-01 17:13_

Cargo uses "prerelease" in the code and error messages though.

---

_Comment by @charliermarsh on 2024-08-01 17:14_

My preference is "pre-release" but then `Prerelease` in code, even though it's not fully consistent.

---

_Comment by @zanieb on 2024-08-01 17:14_

I want the flag `--prerelease` and not `--pre-release` fwiw. I think it's okay to have the style differ in prose vs the interface.

---

_Comment by @charliermarsh on 2024-08-01 17:15_

Ok, let's do that then.

---

_Comment by @charliermarsh on 2024-08-01 17:16_

I will fix it up now.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-01 17:22_

---

_Referenced in [astral-sh/uv#5697](../../astral-sh/uv/pulls/5697.md) on 2024-08-01 17:25_

---

_Closed by @charliermarsh on 2024-08-01 20:56_

---
