```yaml
number: 4166
title: Add markers to edges rather than distributions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/marker-deps-i
created_at: 2024-06-08T18:08:07Z
updated_at: 2024-06-10T12:40:57Z
url: https://github.com/astral-sh/uv/pull/4166
synced_at: 2026-01-12T16:06:04Z
```

# Add markers to edges rather than distributions

---

_@charliermarsh_

## Summary

We've debated this a bit but the thing that tipped me over the edge is https://github.com/astral-sh/uv/issues/4157. As-is, there's no way to represent "a package should be installed, but the extra should only be installed conditionally based on the markers", because the markers sit on the _distribution_. By placing the markers on the edge, we can now represent scenarios that weren't previously representable.

Closes https://github.com/astral-sh/uv/issues/4137.
Closes https://github.com/astral-sh/uv/issues/4125.
Closes https://github.com/astral-sh/uv/issues/4157.


---

_Comment by @charliermarsh on 2024-06-08 18:17_

I think it's possible that we could continue to store markers on distributions _if_ we changed the schema such that extras were once again stored as their own "distributions" (i.e., their own nodes in the graph), and then changed the routine in `graph.rs` to treat a `None` marker as "universal" (right now, `None` markers are discarded, effectively).

---

_Label `bug` added by @charliermarsh on 2024-06-08 18:17_

---

_Label `preview` added by @charliermarsh on 2024-06-08 18:17_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-08 18:38_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution/graph.rs`:294 on 2024-06-10 12:33_

Yeah, I think I buy this.

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:1132 on 2024-06-10 12:36_

Beautiful.

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:982 on 2024-06-10 12:37_

Out of curiosity, why the change in Python version here?

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:2641 on 2024-06-10 12:38_

Awesome

---

_@BurntSushi approved on 2024-06-10 12:39_

This is a really great PR. I love it.

> I think it's possible that we could continue to store markers on distributions if we changed the schema such that extras were once again stored as their own "distributions" (i.e., their own nodes in the graph), and then changed the routine in graph.rs to treat a None marker as "universal" (right now, None markers are discarded, effectively).

At least as of right now, I think I like the approach you've taken in this PR.

---

_@charliermarsh reviewed on 2024-06-10 12:40_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:982 on 2024-06-10 12:40_

I think it was because we typically test against 3.12 and 3.8. (This isn't required, it's just the common versions we use in these tests.) And so if I left it as-is, I would've had to install on 3.7 to hit the "exclusion" case. So it was just a bit easier to raise to 3.10, so that 3.12 is above and 3.8 is below.

---

_Merged by @charliermarsh on 2024-06-10 12:40_

---

_Closed by @charliermarsh on 2024-06-10 12:40_

---

_Branch deleted on 2024-06-10 12:40_

---

_Comment by @charliermarsh on 2024-06-10 12:40_

Thanks @BurntSushi!

---
