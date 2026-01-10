---
number: 8774
title: "Equivalent command to `check-wheel-contents`"
type: issue
state: closed
author: kdeldycke
labels:
  - build-backend
assignees: []
created_at: 2024-11-03T12:07:10Z
updated_at: 2025-06-27T09:02:53Z
url: https://github.com/astral-sh/uv/issues/8774
synced_at: 2026-01-10T01:24:32Z
---

# Equivalent command to `check-wheel-contents`

---

_Issue opened by @kdeldycke on 2024-11-03 12:07_

In the same spirit as `twine check` (see #8641), there is a nice utility called [`check-wheel-contents`](https://github.com/jwodder/check-wheel-contents).

Do we still need this tool now that `uv publish` exists? I mean, is [`check-wheel-contents` checking for stuff](https://github.com/jwodder/check-wheel-contents) that `uv publish` does not?

---

_Comment by @konstin on 2024-11-03 17:37_

I'm not opposed to including these checks in uv, but ideally, these checks would already be performed by the build backed: We should error when building the wheel, ideally locally, instead of only failing during a publish run (on CI).

---

_Referenced in [astral-sh/uv#7839](../../astral-sh/uv/issues/7839.md) on 2024-11-03 18:09_

---

_Referenced in [astral-sh/uv#8779](../../astral-sh/uv/issues/8779.md) on 2024-11-03 18:30_

---

_Comment by @kdeldycke on 2025-01-19 22:09_

I'm now OK if there's no equivalent feature proposed by `uv`, as I trust `uv` to embed these kind of checks naturally during the build process.

---

_Referenced in [astral-sh/uv#11032](../../astral-sh/uv/pulls/11032.md) on 2025-01-28 18:41_

---

_Label `build-backend` added by @konstin on 2025-06-26 14:43_

---

_Comment by @konstin on 2025-06-26 19:43_

I went through the list of lints, and almost all of them are prevented for the uv build backend by its design. The only exception is W002, which is something that shows up on a git diff immediately.

---

_Comment by @kdeldycke on 2025-06-27 07:13_

> I went through the list of lints, and almost all of them are prevented for the uv build backend by its design.

Thanks for the thorough review! With time that's more or less the conclusion I reached out: `check-wheel-contents` was a stop-gap solution from where wheels production was messy. Now with `uv` most things are right by design.

I no longer rely on `check-wheel-contents` and trust `uv`. I propose to close this issue now that @konstin reviewed the list of lints.

---

_Closed by @konstin on 2025-06-27 09:02_

---
