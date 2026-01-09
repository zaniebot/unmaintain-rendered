---
number: 6175
title: "[Proposal] Rule to keep a collection alpha-sorted."
type: issue
state: closed
author: tylerlaprade
labels:
  - needs-decision
assignees: []
created_at: 2023-07-29T17:37:33Z
updated_at: 2023-08-15T03:05:45Z
url: https://github.com/astral-sh/ruff/issues/6175
synced_at: 2026-01-07T13:12:15-06:00
---

# [Proposal] Rule to keep a collection alpha-sorted.

---

_Issue opened by @tylerlaprade on 2023-07-29 17:37_

Sometimes we have large dictionaries, enums, or even lists that contain relevant config info, but the order of the elements is not relevant. For example, we have a map from each of our Django models to the relevant basenames. We need to include every model, and it's easier to verify that a model is already included if you know where to look. This can be accomplished by alpa-sorting the keys. My suggestion is for there to be a special comment I can put at the top of the collection which will cause the entries to always be alpha-sorted. Maybe `# autosort` so that other tools can adopt the convention if they so choose?

---

_Label `needs-decision` added by @charliermarsh on 2023-08-14 21:55_

---

_Comment by @charliermarsh on 2023-08-14 21:55_

I think this can be collapsed with https://github.com/astral-sh/ruff/issues/1198 which proposes that we support sorting collections based on isort's rules (though that's not fully decided yet).

---

_Closed by @charliermarsh on 2023-08-14 21:55_

---

_Comment by @tylerlaprade on 2023-08-15 02:51_

Would that apply to all collections? This would potentially break other lists for which the order changes the behavior. 

---

_Comment by @charliermarsh on 2023-08-15 03:05_

No, I believe there's a pragma comment to mark such collections (see the linked issue for more).

---

_Referenced in [astral-sh/ruff#10085](../../astral-sh/ruff/issues/10085.md) on 2024-02-26 11:45_

---
