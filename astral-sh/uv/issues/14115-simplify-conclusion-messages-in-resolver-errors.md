```yaml
number: 14115
title: "Simplify \"conclusion\" messages in resolver errors"
type: issue
state: open
author: nedbat
labels:
  - enhancement
  - error messages
assignees: []
created_at: 2025-06-17T18:16:41Z
updated_at: 2025-06-18T10:37:47Z
url: https://github.com/astral-sh/uv/issues/14115
synced_at: 2026-01-12T16:01:43Z
```

# Simplify "conclusion" messages in resolver errors

---

_@nedbat_

### Summary

To my ear, this message seems a bit stuffy:

> Because flash-attn was not found in the package registry and you require flash-attn, we can conclude that your requirements are unsatisfiable.

I think the phrase "we can conclude that" can be dropped from message:

> Because flash-attn was not found in the package registry and you require flash-attn, your requirements are unsatisfiable.



---

_Label `enhancement` added by @nedbat on 2025-06-17 18:16_

---

_Comment by @zanieb on 2025-06-17 18:21_

I'm not sure this works in the "general" case, as these messages are derived from a complicated resolution tree structure — but I agree we should attempt to be less verbose.

---

_Label `error messages` added by @zanieb on 2025-06-17 18:21_

---

_Renamed from "Simplify "conclusion" messages" to "Simplify "conclusion" messages in resolver rrors" by @zanieb on 2025-06-17 18:21_

---

_Comment by @konstin on 2025-06-18 08:23_

This specific error message is one we commonly see. In this specific form, with a direct requirement missing, it's not actually a resolution error, but an index configuration error. We can special case it, something like:

```
error: flash-attn was not found in the package registry. Registries searched:
* https://pypi.org/simple
* https://python.example.org/simple
```

---

_Renamed from "Simplify "conclusion" messages in resolver rrors" to "Simplify "conclusion" messages in resolver errors" by @nedbat on 2025-06-18 10:32_

---

_Comment by @nedbat on 2025-06-18 10:37_

From a quick search of the repo, it looks like the phrase comes from four formatting strings in [src/pubgrub/report.rs](https://github.com/astral-sh/uv/tree/main/crates/uv-resolver/src/pubgrub/report.rs) and wouldn't be hard to change, though the format strings themselves would be harder to understand in isolation.

---
