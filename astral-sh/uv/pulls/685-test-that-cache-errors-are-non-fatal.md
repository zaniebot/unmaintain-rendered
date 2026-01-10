```yaml
number: 685
title: Test that cache errors are non-fatal
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/non-fatal-cache-errors
created_at: 2023-12-18T14:05:12Z
updated_at: 2023-12-19T12:02:50Z
url: https://github.com/astral-sh/uv/pull/685
synced_at: 2026-01-10T15:44:44Z
```

# Test that cache errors are non-fatal

---

_Pull request opened by @konstin on 2023-12-18 14:05_

The test creates a cache from multiple sources and injects faults (once using invalid data and once by making the files unreadable on the fs level), then resolves again.

I didn't test git because it has its own locking and correctness logic.

The main drawback is that this test is slow (2.5s for me), we could `#[ignore]` it.

---

_Review requested from @BurntSushi by @konstin on 2023-12-18 14:05_

---

_Review requested from @charliermarsh by @konstin on 2023-12-18 14:05_

---

_Review comment by @BurntSushi on `crates/puffin-cli/tests/pip_compile.rs`:2683 on 2023-12-18 14:43_

Do you mean write-only? Although, the mode below is `000`. (I'm also not sure what setting both `write(true)` and `mode(0o000)` do. I would partially expect the latter to override the former, but the std docs actually don't seem to say anything about the interaction between the two options.)

---

_@BurntSushi approved on 2023-12-18 14:44_

Nice! Do we know what makes the test take so long?

---

_@charliermarsh reviewed on 2023-12-18 14:49_

Perhaps we run this selectively either via a feature (feels like overkill) or by ignoring it, yet.

---

_Comment by @konstin on 2023-12-19 11:59_

> Nice! Do we know what makes the test take so long?

Mostly network i'd expect, the test run without a cache

I marked the test as `#[ignore]`,  i think of it as a script you can run if you want to.

---

_Merged by @konstin on 2023-12-19 12:02_

---

_Closed by @konstin on 2023-12-19 12:02_

---

_Branch deleted on 2023-12-19 12:02_

---
