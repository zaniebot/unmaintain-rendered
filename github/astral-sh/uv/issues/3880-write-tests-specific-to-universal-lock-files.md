---
number: 3880
title: write tests specific to universal lock files
type: issue
state: closed
author: BurntSushi
labels:
  - internal
assignees: []
created_at: 2024-05-28T13:37:41Z
updated_at: 2024-06-10T12:41:46Z
url: https://github.com/astral-sh/uv/issues/3880
synced_at: 2026-01-07T13:12:17-06:00
---

# write tests specific to universal lock files

---

_Issue opened by @BurntSushi on 2024-05-28 13:37_

After #3831 lands, I should spend some time coming up with a bunch of test scenarios that specifically invoke the resolver without a marker environment. Pretty much all extant tests exercise a code path that invokes the resolver _with_ a marker environment. So we should consider both adding new tests and exploring a way to leverage our existing test suite in a "universal" context.

Ref #3347 

---

_Label `internal` added by @BurntSushi on 2024-05-28 13:37_

---

_Referenced in [astral-sh/uv#3347](../../astral-sh/uv/issues/3347.md) on 2024-05-28 13:37_

---

_Comment by @BurntSushi on 2024-06-10 12:41_

I think this was closed by https://github.com/astral-sh/uv/pull/4019.

---

_Closed by @BurntSushi on 2024-06-10 12:41_

---
