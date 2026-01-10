---
number: 15781
title: "Expect `uv publish` to generate a `blake2b-256` hash value and send it to the server"
type: issue
state: closed
author: wenxue202012
labels:
  - enhancement
assignees: []
created_at: 2025-09-11T02:14:35Z
updated_at: 2025-09-11T20:17:08Z
url: https://github.com/astral-sh/uv/issues/15781
synced_at: 2026-01-10T01:25:59Z
---

# Expect `uv publish` to generate a `blake2b-256` hash value and send it to the server

---

_Issue opened by @wenxue202012 on 2025-09-11 02:14_

### Summary

Expect uv publish to generate a `blake2b-256` hash value and send it to the server

### Example

https://github.com/astral-sh/uv/blob/main/crates/uv-publish/src/lib.rs#L739

Adding the passing of the `blake2_256_digest` parameter value.



---

_Label `enhancement` added by @wenxue202012 on 2025-09-11 02:14_

---

_Renamed from "Expect uv publish to generate a blake2b-256 hash value and send it to the server" to "Expect `uv publish` to generate a `blake2b-256` hash value and send it to the server" by @wenxue202012 on 2025-09-11 02:15_

---

_Comment by @zanieb on 2025-09-11 02:27_

Can you share why you expect, need, or want this?

---

_Comment by @wenxue202012 on 2025-09-11 06:59_

The CNB artifact repository （https://cnb.cool） I'm using checks on the server side whether the blake2b hash is empty, and the relative path used for pip install is based on the blake2b hash.

​​Like Poetry、Hatch and Twine, all have blake2b hash values.​​
@zanieb 




---

_Comment by @woodruffw on 2025-09-11 12:50_

Hi @wenxue202012, thanks for opening an issue.

I don't see any harm to us supporting blake2 hashes in the upload payload, but I'll note: PyPI itself doesn't require this field (or even SHA256), so your third-party index should probably not *require* them. Would you be able to reach out to them first and see whether they can fix their consistency with PyPI?

---

_Assigned to @woodruffw by @woodruffw on 2025-09-11 16:33_

---

_Referenced in [astral-sh/uv#15794](../../astral-sh/uv/pulls/15794.md) on 2025-09-11 16:36_

---

_Closed by @woodruffw on 2025-09-11 20:17_

---
