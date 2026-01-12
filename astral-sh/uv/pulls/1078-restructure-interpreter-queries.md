```yaml
number: 1078
title: Restructure interpreter queries
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/int
created_at: 2024-01-24T17:30:30Z
updated_at: 2024-01-24T19:26:03Z
url: https://github.com/astral-sh/uv/pull/1078
synced_at: 2026-01-12T16:04:24Z
```

# Restructure interpreter queries

---

_@charliermarsh_

This illustrates what I was thinking for the interpreter lookup. I think the only change in behavior here is that if a version is provided, and `python3` or `python.exe` satisfy the version, we now choose _that_ interpreter instead of looking for `python3.7` or similar.


---

_Comment by @zanieb on 2024-01-24 18:54_

Hm.... I _think_ this is better? The precedence is less clear when reading the code, but it does avoid extra calls to detect the virtual environment and such.

---

_Marked ready for review by @charliermarsh on 2024-01-24 19:09_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-24 19:09_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:133 on 2024-01-24 19:12_

This looks wrong actually, now we'll look at `python3.x` for the version without a matching patch without checking if it's satisfied by the base interpreter without the patch.

---

_@zanieb reviewed on 2024-01-24 19:12_

---

_@zanieb reviewed on 2024-01-24 19:14_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/interpreter.rs`:145 on 2024-01-24 19:14_

A major downside to this approach is that now this interface _cannot_ be public because it doesn't actually check all the relevant locations.

If we need an interface to find a specific Python version in the future (seems very likely) we'll need to go back to something that looks a lot what we had before. This makes me less certain that we should merge this.

---

_Comment by @charliermarsh on 2024-01-24 19:16_

Okay, I defer to you

---

_Closed by @charliermarsh on 2024-01-24 19:16_

---

_@charliermarsh reviewed on 2024-01-24 19:26_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/interpreter.rs`:145 on 2024-01-24 19:26_

Good call, we probably need it for `venv`.

---

_@charliermarsh reviewed on 2024-01-24 19:26_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/interpreter.rs`:133 on 2024-01-24 19:26_

👍 I think this would be solved by re-checking the default interpreter against the patch here.

---
