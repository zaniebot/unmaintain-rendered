```yaml
number: 3681
title: "Evaluate arbitrary markers to `false`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/arbitrary
created_at: 2024-05-20T23:38:30Z
updated_at: 2024-05-21T11:45:45Z
url: https://github.com/astral-sh/uv/pull/3681
synced_at: 2026-01-12T16:05:47Z
```

# Evaluate arbitrary markers to `false`

---

_@charliermarsh_

## Summary

See: https://github.com/astral-sh/uv/pull/3679#issuecomment-2121387428.

Closes: https://github.com/astral-sh/uv/issues/3675 (although I think we have another improvement to make there -- will file separately).


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-05-20 23:38_

---

_@charliermarsh reviewed on 2024-05-20 23:39_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/marker.rs`:39 on 2024-05-20 23:39_

@ibraheemdev -- is this wrong? I was trying to understand the comment here: https://github.com/astral-sh/uv/pull/3679#issuecomment-2121392361

---

_@zanieb reviewed on 2024-05-20 23:46_

---

_Review comment by @zanieb on `crates/uv-resolver/src/marker.rs`:39 on 2024-05-20 23:46_

This sounds right to me fwiw

---

_@zanieb reviewed on 2024-05-20 23:48_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker.rs`:1348 on 2024-05-20 23:48_

If we always evaluate these to false that means that we will skip any requirement that has unknown marker expressions right? It seems like true would _ignore_ the bad marker which makes more sense?

---

_@zanieb reviewed on 2024-05-20 23:49_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker.rs`:1348 on 2024-05-20 23:49_

"true and warn that it's meaningless" seems like good behavior?

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker.rs`:1348 on 2024-05-21 00:05_

I believe this was our behavior until very recently (return `false` here) and is pip's behavior.

---

_@charliermarsh reviewed on 2024-05-21 00:05_

---

_@ibraheemdev reviewed on 2024-05-21 00:05_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/marker.rs`:39 on 2024-05-21 00:05_

Yes, that is correct. However, I wonder if maybe we should be pessimistic regarding forking on meaningless expressions, cc @BurntSushi.

Also if we do make this change, I expected a test to be failing. I think we need to check if `expr2` is `Arbitrary` as well, I can make that change in a later PR.

---

_@charliermarsh reviewed on 2024-05-21 00:08_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker.rs`:1348 on 2024-05-21 00:08_

To be explicit: I think we should omit requirements that have invalid markers, not include them.

---

_@ibraheemdev approved on 2024-05-21 00:10_

I agree with reverting back to our original behavior here, and fine with leaving arbitrary expression forking as an open question.

---

_@charliermarsh reviewed on 2024-05-21 00:31_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/marker.rs`:39 on 2024-05-21 00:31_

Okay, think I fixed this.

---

_Label `bug` added by @charliermarsh on 2024-05-21 00:31_

---

_Merged by @charliermarsh on 2024-05-21 01:01_

---

_Closed by @charliermarsh on 2024-05-21 01:01_

---

_Branch deleted on 2024-05-21 01:01_

---

_@BurntSushi reviewed on 2024-05-21 11:45_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/marker.rs`:39 on 2024-05-21 11:45_

I'm not fully sure. My instinct is also that we probably ought to be pessimistic on forking here. If we say two arbitrary expressions are disjoint, then we will fork. But logically speaking, if the arbitrary expressions are always false, then, well, I guess that makes sense?

I'm happy to let practice dictate what we do here. (So this is fine for now.)

---
