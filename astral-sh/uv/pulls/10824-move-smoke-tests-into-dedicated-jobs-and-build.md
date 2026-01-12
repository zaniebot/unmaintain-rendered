```yaml
number: 10824
title: "Move smoke tests into dedicated jobs and build `uvx` explicitly"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/uvx-test
created_at: 2025-01-21T20:40:15Z
updated_at: 2025-01-22T16:44:57Z
url: https://github.com/astral-sh/uv/pull/10824
synced_at: 2026-01-12T16:09:31Z
```

# Move smoke tests into dedicated jobs and build `uvx` explicitly

---

_@zanieb_

In the interest of expanding these tests and debugging weird behaviors, I've moved the smoke tests out of the `cargo test` job and into dedicated `smoke test` jobs. We explicitly build `uvx` in the `build binary` jobs instead of relying on the implicit build for the test run.

I also added a `uvx` test case to the smoke tests: `uvx ruff --version`

---

_Label `testing` added by @zanieb on 2025-01-21 20:40_

---

_Marked ready for review by @zanieb on 2025-01-21 21:16_

---

_Comment by @zanieb on 2025-01-21 21:18_

Why is the compilation not re-used when running `cargo test` after `cargo build`?

---

_Comment by @zanieb on 2025-01-21 21:19_

This may be an unacceptable performance degradation in the test CI jobs — in which case I'll move the smoke testing out into separate tests (which may be best anyway).

---

_Renamed from "Explicitly build `uvx` in CI" to "Move smoke tests into dedicated jobs and build `uvx` explicitly" by @zanieb on 2025-01-21 22:01_

---

_Review requested from @Gankra by @zanieb on 2025-01-21 22:04_

---

_Merged by @zanieb on 2025-01-21 22:46_

---

_Closed by @zanieb on 2025-01-21 22:46_

---

_Branch deleted on 2025-01-21 22:46_

---

_Comment by @zanieb on 2025-01-21 22:59_

One downside of this change: these don't block CI / auto-merge anymore

---

_@Gankra reviewed on 2025-01-22 14:02_

Hmm might have been able to use a matrix here but nbd

---

_Comment by @zanieb on 2025-01-22 15:27_

I think maybe I could use a matrix if I used `bash` on Windows but I need some PowerShell-specific invocations? I might move some of the testing out into a script, I could consider it then.

---

_Comment by @Gankra on 2025-01-22 16:44_

I was picturing having the power-shell specific part wrapped in an `if matrix.platform == "windows":` or something but yeah that's the hardest part.

---
