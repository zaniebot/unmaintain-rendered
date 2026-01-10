```yaml
number: 10431
title: "Split `--quiet` mode into \"quiet\" and \"silent\" modes"
type: issue
state: closed
author: zanieb
labels:
  - tracing
  - cli
assignees: []
created_at: 2025-01-09T14:47:17Z
updated_at: 2025-03-26T20:46:18Z
url: https://github.com/astral-sh/uv/issues/10431
synced_at: 2026-01-10T03:41:46Z
```

# Split `--quiet` mode into "quiet" and "silent" modes

---

_Issue opened by @zanieb on 2025-01-09 14:47_

e.g., `-q` is quiet, only prints important information and `-qq` is silent, doesn't print anything

---

_Label `tracing` added by @zanieb on 2025-01-09 14:47_

---

_Label `cli` added by @zanieb on 2025-01-09 14:47_

---

_Comment by @notatallshaw on 2025-01-09 17:34_

Is `uv build` in scope for this or should I make another issue? A very minor pet peeve is my only options for it appear to be "very noisey" and "completely silent".

---

_Comment by @zanieb on 2025-01-09 18:02_

@notatallshaw I think that'd be reasonable. The `--no-build-logs` option might be helpful in the meantime.

---

_Comment by @Gankra on 2025-02-28 14:29_

Point of clarification: are we indeed proposing `-qq` is silent even on stdout, even for commands whose entire purpose is to produce output? (e.g. if you hypothetically ran `uv python list -qq`, it essentially does nothing (except maybe has non-zero status)?)



---

_Comment by @zanieb on 2025-02-28 15:18_

I think so, though I don't feel strongly. "Silent is silent" seems easier to maintain.

---

_Closed by @Gankra on 2025-03-26 20:46_

---

_Closed by @Gankra on 2025-03-26 20:46_

---
