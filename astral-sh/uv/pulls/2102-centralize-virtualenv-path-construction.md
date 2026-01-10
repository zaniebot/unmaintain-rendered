```yaml
number: 2102
title: Centralize virtualenv path construction
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/venv
created_at: 2024-03-01T05:29:58Z
updated_at: 2024-03-01T15:52:49Z
url: https://github.com/astral-sh/uv/pull/2102
synced_at: 2026-01-10T14:54:43Z
```

# Centralize virtualenv path construction

---

_Pull request opened by @charliermarsh on 2024-03-01 05:29_

## Summary

Right now, we have virtualenv construction encoded in a few different places. Namely, it happens in both `gourgeist` and `virtualenv_layout.rs` -- _and_ `interpreter.rs` also encodes some knowledge about how they work, by way of reconstructing the `SysconfigPaths`.

Instead, `gourgeist` now returns the complete layout, enumerating all the directories it created. So, rather than returning a root directory, and re-creating all those paths in `uv-interpreter`, we pass the data directly back to it.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-03-01 05:30_

---

_Review requested from @konstin by @charliermarsh on 2024-03-01 05:30_

---

_Label `internal` added by @charliermarsh on 2024-03-01 05:30_

---

_@charliermarsh reviewed on 2024-03-01 05:30_

---

_Review comment by @charliermarsh on `crates/gourgeist/src/bare.rs`:168 on 2024-03-01 05:30_

I assume this is right? (This is the only behavior change in the PR -- I tried to do the above TODO.)

---

_@charliermarsh reviewed on 2024-03-01 05:31_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/python_environment.rs`:163 on 2024-03-01 05:31_

This check happens at the call sites.

---

_@charliermarsh reviewed on 2024-03-01 05:32_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/python_environment.rs`:164 on 2024-03-01 05:32_

We still encode a small amount of venv knowledge here, but I think it's ok. This method is actually _different_ than anything that happens elsewhere: we're trying to find the Python in an existing venv, which we may or may not have created.

---

_Marked ready for review by @charliermarsh on 2024-03-01 05:36_

---

_@charliermarsh reviewed on 2024-03-01 05:37_

---

_Review comment by @charliermarsh on `crates/gourgeist/src/bare.rs`:320 on 2024-03-01 05:37_

Eventually, we should be creating these paths by using templatized paths from the base interpreter and filling them in. This isn't correct on all platforms -- but this isn't a new problem, no behavior changes. And this centralized data flow will make this problem easier to solve later.

---

_@charliermarsh reviewed on 2024-03-01 05:37_

---

_Review comment by @charliermarsh on `crates/gourgeist/src/bare.rs`:98 on 2024-03-01 05:37_

These are just renamed to match `sysconfig`.

---

_@charliermarsh reviewed on 2024-03-01 05:41_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/sysconfig.rs`:18 on 2024-03-01 05:41_

(This is moved out of `interpreter.rs`.)

---

_Review comment by @konstin on `crates/gourgeist/src/bare.rs`:88 on 2024-03-01 09:01_

Just saw that the comment never got removed

```suggestion
```

---

_Review comment by @konstin on `crates/uv-interpreter/src/python_environment.rs`:173 on 2024-03-01 10:04_

Does this fix https://github.com/astral-sh/uv/issues/1251?

---

_@konstin approved on 2024-03-01 10:04_

---

_Review comment by @BurntSushi on `crates/gourgeist/src/bare.rs`:160 on 2024-03-01 12:01_

I'm wondering whether using conditional compilation for logic like this is the right thing to do. For example, are we sure that all `uv` binaries compiled for Windows will 100% of the time use the Windows oriented path below? Or is it possible for the Unix path (WSL?) to be used in a Windows context?

---

_Review comment by @BurntSushi on `crates/gourgeist/src/bare.rs`:160 on 2024-03-01 12:01_

Same deal for use of conditional compilation elsewhere.

---

_@BurntSushi approved on 2024-03-01 12:03_

Yesssssss! This makes a lot of sense. Thank you!

---

_@charliermarsh reviewed on 2024-03-01 14:46_

---

_Review comment by @charliermarsh on `crates/gourgeist/src/bare.rs`:160 on 2024-03-01 14:46_

Yeah probably not, but I think it's ~the same as it is today. I have an idea for how to fix this properly (#2095).

---

_@konstin reviewed on 2024-03-01 15:12_

---

_Review comment by @konstin on `crates/gourgeist/src/bare.rs`:160 on 2024-03-01 15:12_

Unfortunately they don't (https://github.com/astral-sh/uv/issues/1251), but #2095 should solve either case

---

_Merged by @charliermarsh on 2024-03-01 15:52_

---

_Closed by @charliermarsh on 2024-03-01 15:52_

---

_Branch deleted on 2024-03-01 15:52_

---
