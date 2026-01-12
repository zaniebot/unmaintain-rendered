```yaml
number: 3967
title: Avoid re-adding solutions to forked state
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/glob
created_at: 2024-06-02T21:14:09Z
updated_at: 2024-06-02T21:58:43Z
url: https://github.com/astral-sh/uv/pull/3967
synced_at: 2026-01-12T16:05:57Z
```

# Avoid re-adding solutions to forked state

---

_@charliermarsh_

## Summary

Running a resolution that required forking was failing due to breaking an invariant in PubGrub. It looks like we were adding the same incompatibility multiple times, or something like that. The issue appears to be that when forking, we modify the current state, then clone it as the "next state", then push to the "forked states" -- but that means we're cloning the _modified_ state.

This PR changes the order of operations such that we clone, then modify. It shouldn't introduce any additional clones though.


---

_Label `bug` added by @charliermarsh on 2024-06-02 21:14_

---

_Label `preview` added by @charliermarsh on 2024-06-02 21:14_

---

_Marked ready for review by @charliermarsh on 2024-06-02 21:14_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-02 21:15_

---

_@BurntSushi approved on 2024-06-02 21:41_

I bet I flubbed this when I fixed the perf regression in my initial PR. Namely, I had previously been cloning forked states eagerly. But this resulted in cloning the state for every call to `get_dependencies`, even when there was no forking.

---

_Merged by @charliermarsh on 2024-06-02 21:58_

---

_Closed by @charliermarsh on 2024-06-02 21:58_

---

_Branch deleted on 2024-06-02 21:58_

---

_Comment by @charliermarsh on 2024-06-02 21:58_

No prob, you would’ve noticed immediately next week when you wrote the universal tests anyway!

---
