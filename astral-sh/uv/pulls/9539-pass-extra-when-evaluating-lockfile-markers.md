```yaml
number: 9539
title: Pass extra when evaluating lockfile markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/r
created_at: 2024-11-30T14:13:30Z
updated_at: 2024-12-03T12:50:20Z
url: https://github.com/astral-sh/uv/pull/9539
synced_at: 2026-01-12T16:08:51Z
```

# Pass extra when evaluating lockfile markers

---

_@charliermarsh_

## Summary

When we serialize and deserialize the lockfile, we remove the conflict markers. So in the linked case, the edges for the `tqdm` entries are like:

```
complexified_marker: UniversalMarker {
    pep508_marker: python_full_version >= '3.9.0',
    conflict_marker: true,
},
```

However... when we evaluate in-memory, the conflict markers are still there...

```
complexified_marker: UniversalMarker {
    pep508_marker: true,
    conflict_marker: extra == 't1' and extra != 't2',
},
```

So if `uv run` creates the lockfile, we evaluate this as `false`.

We should make this consistent, and I expect @BurntSushi is aware. But for now, it's reasonable / correct to pass the extra when evaluating at this specific point, since we know the dependency was enabled by the marker.

Closes https://github.com/astral-sh/uv/issues/9533#issuecomment-2508908591.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-30 14:13_

---

_Label `bug` added by @charliermarsh on 2024-11-30 14:13_

---

_Marked ready for review by @charliermarsh on 2024-11-30 14:13_

---

_Merged by @charliermarsh on 2024-11-30 14:22_

---

_Closed by @charliermarsh on 2024-11-30 14:22_

---

_Branch deleted on 2024-11-30 14:22_

---

_Comment by @BurntSushi on 2024-12-03 12:50_

Yeah I think this makes sense. In #9370, the call site here is indeed changed. But it passes all activated extras.

---
