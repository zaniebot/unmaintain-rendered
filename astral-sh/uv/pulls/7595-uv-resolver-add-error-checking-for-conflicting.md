```yaml
number: 7595
title: "uv-resolver: add error checking for conflicting distributions"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/test-version-duplicates
created_at: 2024-09-20T17:53:31Z
updated_at: 2024-09-24T14:55:27Z
url: https://github.com/astral-sh/uv/pull/7595
synced_at: 2026-01-12T16:07:54Z
```

# uv-resolver: add error checking for conflicting distributions

---

_@BurntSushi_

This PR adds some additional sanity checking on resolution graphs to
ensure we can never install different versions of the same package into
the same environment.

I used code similar to this to provoke bugs in the resolver before the
release, but it never made it into `main`. Here, we add the error
checking to the creation of `ResolutionGraph`, since this is where it's
most convenient to access the "full" markers of each distribution.

We only report an error when `debug_assertions` are enabled to avoid
rendering `uv` *completely* unusuable if a bug were to occur in a
production binary. For example, maybe a conflict is detected in a marker
environment that isn't actually used. While not ideal, `uv` is still
usable for any other marker environment.

Closes #5598


---

_Review requested from @charliermarsh by @BurntSushi on 2024-09-23 14:25_

---

_Review requested from @konstin by @BurntSushi on 2024-09-23 14:25_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:1516 on 2024-09-23 16:23_

commented out code

---

_Review comment by @konstin on `crates/uv-resolver/src/resolution/graph.rs`:265 on 2024-09-23 16:28_

Are there cases when this is not a bug? Otherwise we should attach the bug reporting URL to the warning.

---

_Review comment by @konstin on `crates/uv-resolver/src/resolution/graph.rs`:718 on 2024-09-23 16:33_

nit: This can be `node_weights()`

---

_Review comment by @konstin on `crates/uv-resolver/src/resolution/graph.rs`:756 on 2024-09-23 16:36_

typo

---

_@konstin approved on 2024-09-23 16:37_

This is a great addition

---

_@BurntSushi reviewed on 2024-09-23 17:08_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution/graph.rs`:265 on 2024-09-23 17:08_

I don't believe so. I've added `https://github.com/astral-sh/uv/issues/new` to the warning message. Good call. :)

---

_Merged by @BurntSushi on 2024-09-24 14:55_

---

_Closed by @BurntSushi on 2024-09-24 14:55_

---

_Branch deleted on 2024-09-24 14:55_

---
