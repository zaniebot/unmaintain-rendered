```yaml
number: 11653
title: Avoid installing duplicate dependencies across conflicting groups
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/d
created_at: 2025-02-20T05:31:27Z
updated_at: 2025-02-20T20:17:16Z
url: https://github.com/astral-sh/uv/pull/11653
synced_at: 2026-01-12T16:09:56Z
```

# Avoid installing duplicate dependencies across conflicting groups

---

_@charliermarsh_

## Summary

We need to compute the set of activated groups prior to evaluating the conflict markers on the groups' dependencies.

Closes https://github.com/astral-sh/uv/issues/11648.


---

_Review requested from @BurntSushi by @charliermarsh on 2025-02-20 05:31_

---

_Label `bug` added by @charliermarsh on 2025-02-20 05:31_

---

_@charliermarsh reviewed on 2025-02-20 05:32_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/installable.rs`:61 on 2025-02-20 05:32_

Is this to some degree just... impossible? I'm so confused!

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/installable.rs`:61 on 2025-02-20 14:23_

Can this somehow take advantage of the fact that dependency groups are different from extras in that they can't be enabled in dependency specifications?

---

_@BurntSushi approved on 2025-02-20 14:23_

Nice fix! Thank you!

---

_@charliermarsh reviewed on 2025-02-20 20:17_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/installable.rs`:61 on 2025-02-20 20:17_

I'm... not totally sure. I think so, but I'm not totally sure.

---

_Merged by @charliermarsh on 2025-02-20 20:17_

---

_Closed by @charliermarsh on 2025-02-20 20:17_

---

_Branch deleted on 2025-02-20 20:17_

---
