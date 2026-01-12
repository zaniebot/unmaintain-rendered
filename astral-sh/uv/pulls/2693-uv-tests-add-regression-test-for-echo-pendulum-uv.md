```yaml
number: 2693
title: "uv/tests: add regression test for `echo pendulum | uv pip compile -`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - testing
assignees: []
merged: true
base: main
head: ag/pendulum-tzdata-regression
created_at: 2024-03-27T15:59:29Z
updated_at: 2024-03-27T17:04:51Z
url: https://github.com/astral-sh/uv/pull/2693
synced_at: 2026-01-12T16:05:10Z
```

# uv/tests: add regression test for `echo pendulum | uv pip compile -`

---

_@BurntSushi_

I stumbled across this when writing tests for
`--emit-marker-expressions`. Namely, I observed that in CI, the `tzdata`
dependency of `pendulum` wasn't included in the `requirements.txt`
output on Windows.

@konstin [suggested] that this was a bug, so I've created a test for it.
In particular, it looks like [`tzdata` is an unconditional dependency of
`pendulum`][tzdata-unconditional].

[suggested]: https://github.com/astral-sh/uv/pull/2651#discussion_r1539722464
[tzdata-unconditional]: https://inspector.pypi.io/project/pendulum/3.0.0/packages/67/5e/e646afbd1632bfbacdae79289d7d5879efdeeb5f5e58327bc5c698731107/pendulum-3.0.0-cp310-none-win_amd64.whl/pendulum-3.0.0.dist-info/METADATA#line.12


---

_Label `testing` added by @zanieb on 2024-03-27 16:03_

---

_Comment by @charliermarsh on 2024-03-27 16:09_

Is this related to our filters, which always remove `tzdata` on Windows...?

---

_Comment by @BurntSushi on 2024-03-27 16:23_

> Is this related to our filters, which always remove `tzdata` on Windows...?

Oh, I guess so: https://github.com/astral-sh/uv/blob/f9db0ed8f0fc50972f2a5d99f4099d6ca7d49f52/crates/uv/tests/common/mod.rs

---

_Comment by @konstin on 2024-03-27 16:32_

Sorry, i got fooled by the windows filters i actually myself -.-

---

_Review requested from @konstin by @BurntSushi on 2024-03-27 16:46_

---

_Comment by @BurntSushi on 2024-03-27 16:46_

@konstin I added a short comment noting why we do the filters, and kept the "regression" test, but with Windows filters disabled. (And indeed, the tests pass.)

---

_@konstin approved on 2024-03-27 16:48_

---

_Merged by @BurntSushi on 2024-03-27 17:04_

---

_Closed by @BurntSushi on 2024-03-27 17:04_

---

_Branch deleted on 2024-03-27 17:04_

---
