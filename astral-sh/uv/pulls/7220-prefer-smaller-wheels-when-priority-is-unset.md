```yaml
number: 7220
title: Prefer smaller wheels when priority is unset
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
  - external
assignees: []
base: main
head: charlie/small
created_at: 2024-09-09T15:19:59Z
updated_at: 2024-11-06T22:10:31Z
url: https://github.com/astral-sh/uv/pull/7220
synced_at: 2026-01-10T11:59:59Z
```

# Prefer smaller wheels when priority is unset

---

_Pull request opened by @charliermarsh on 2024-09-09 15:19_

## Summary

Closes https://github.com/astral-sh/uv/issues/7216.


---

_Review requested from @konstin by @charliermarsh on 2024-09-09 15:20_

---

_Label `performance` added by @charliermarsh on 2024-09-09 15:20_

---

_Comment by @konstin on 2024-09-09 16:29_

Files an issue upstream for the tensorflow problem: https://github.com/tensorflow/tensorflow/issues/75415

Imo this is blocking since it breaks tensorflow.

---

_Label `upstream` added by @konstin on 2024-09-09 16:30_

---

_Comment by @charliermarsh on 2024-09-09 16:33_

Is the new resolution any more wrong / right than the old?

---

_Comment by @konstin on 2024-09-09 18:04_

The windows package is a stub, just forwarding to `tensorflow-intel`, while the unix package is a real tensorflow package that users want to install and whose deps users want to have, so i consider the unix one more correct.

---

_Comment by @charliermarsh on 2024-09-09 18:19_

That makes sense, thanks @konstin. Do you think the heuristic is a lost cause? Or should we special-case TensorFlow in some way? Something else?

---

_Comment by @konstin on 2024-09-09 18:51_

Either way we choose, i believe tensorflow should consolidate their metadata, since coherent metadata is a core assumption in universal resolvers such as poetry. What we do with the PR depends on how much problems the current heuristic causes for our users: If the current behaviour causes trouble, add a workaround for tensorflow and merge. If it doesn't, i'd favor not adding package-specific workarounds and wait until tensorflow has fixed their metadata and that propagated through the ecosystem (and then doing this change).

---

_Comment by @charliermarsh on 2024-09-09 18:52_

I don't think it's necessarily causing problems. It can wait (at least for a response). But it will continue to be wrong for older versions no matter what.

---

_Closed by @charliermarsh on 2024-11-06 22:10_

---
