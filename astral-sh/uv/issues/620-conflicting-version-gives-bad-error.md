---
number: 620
title: Conflicting version gives bad error
type: issue
state: closed
author: konstin
labels:
  - error messages
assignees: []
created_at: 2023-12-12T16:39:04Z
updated_at: 2024-06-27T11:16:11Z
url: https://github.com/astral-sh/uv/issues/620
synced_at: 2026-01-10T01:23:04Z
---

# Conflicting version gives bad error

---

_Issue opened by @konstin on 2023-12-12 16:39_

On python 3.12

```console
$ cargo run --bin puffin -q -- pip-compile scripts/requirements/transformers-extras.in
error: Conflicting versions for `tokenizers`: `tokenizers==0.9.3` does not intersect with `tokenizers==0.9.2`
```
The problem is that
https://github.com/astral-sh/puffin/blob/2094680cdd624219dd79df7d8eac703f8f9e577e/crates/puffin-resolver/src/pubgrub/dependencies.rs#L186-L189
isn't correctly transformed not selecting the version.

---

_Comment by @charliermarsh on 2023-12-12 16:55_

> isn't correctly transformed not selecting the version

Can you explain this comment in more detail?

---

_Label `error messages` added by @charliermarsh on 2023-12-12 16:57_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-12-12 16:57_

---

_Comment by @konstin on 2023-12-12 17:45_

I haven't looked at it in enough depth, but this shouldn't be a terminal error but instead be an incompatibility, maybe return an empty set instead? Or if it's terminal, at least tell us the chain how it got there.

---

_Assigned to @zanieb by @zanieb on 2024-02-13 02:14_

---

_Comment by @zanieb on 2024-02-13 02:15_

Reproduced this today. No idea what's going on, looks very pathological.

---

_Comment by @konstin on 2024-06-27 11:16_

The code that caused this doesn't exist anymore

---

_Closed by @konstin on 2024-06-27 11:16_

---
