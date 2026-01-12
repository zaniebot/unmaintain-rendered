```yaml
number: 8984
title: Allow apostrophe in venv name
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/venv-apostrophe
created_at: 2024-11-10T10:43:50Z
updated_at: 2024-11-15T09:52:11Z
url: https://github.com/astral-sh/uv/pull/8984
synced_at: 2026-01-12T16:08:35Z
```

# Allow apostrophe in venv name

---

_@konstin_

Escape an apostrophe in the venv path name.

Fixes #8947

---

_Label `bug` added by @konstin on 2024-11-10 10:43_

---

_@pantheraleo-7 reviewed on 2024-11-10 17:15_

---

_Review comment by @pantheraleo-7 on `crates/uv-virtualenv/src/virtualenv.rs`:310 on 2024-11-10 17:15_

```suggestion
                    // We can use implicit string concatenations, by putting the single quote into double
```

---

_Comment by @samypr100 on 2024-11-10 18:44_

CC @paveldikov for visiblity on the relocatable side

---

_@paveldikov reviewed on 2024-11-10 22:12_

---

_Review comment by @paveldikov on `crates/uv-virtualenv/src/virtualenv.rs`:312 on 2024-11-10 22:12_

I think this is implemented somewhere in the codebase already... IIRC it was cited as a `shlex.quote()` analogue?

---

_@samypr100 reviewed on 2024-11-11 04:18_

---

_Review comment by @samypr100 on `crates/uv-virtualenv/src/virtualenv.rs`:312 on 2024-11-11 04:18_

Yea, it's in another crate

https://github.com/astral-sh/uv/blob/489d42a0125b0770bd6ee14f3068191beff0a8dd/crates/uv/src/commands/venv.rs#L420-L432

---

_@konstin reviewed on 2024-11-12 12:55_

---

_Review comment by @konstin on `crates/uv-virtualenv/src/virtualenv.rs`:312 on 2024-11-12 12:55_

Thank you! Split out to #9055

---

_@charliermarsh approved on 2024-11-15 04:05_

---

_Merged by @konstin on 2024-11-15 09:52_

---

_Closed by @konstin on 2024-11-15 09:52_

---

_Branch deleted on 2024-11-15 09:52_

---
