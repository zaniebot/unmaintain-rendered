```yaml
number: 1115
title: Replace vendored RustPython code to dependency call
type: issue
state: closed
author: youknowone
labels:
  - internal
assignees: []
created_at: 2022-12-07T07:48:49Z
updated_at: 2023-01-11T01:22:18Z
url: https://github.com/astral-sh/ruff/issues/1115
synced_at: 2026-01-10T11:09:43Z
```

# Replace vendored RustPython code to dependency call

---

_Issue opened by @youknowone on 2022-12-07 07:48_

I found ruff has a vendored code from RustPython
https://github.com/charliermarsh/ruff/tree/main/src/vendored

Do you want to make me to turn them pub in RustPython and refactor to reusable out of RustPython?

---

_Comment by @charliermarsh on 2022-12-07 14:24_

@youknowone - Yeah! Would love to remove that vendored code.

---

_Label `internal` added by @charliermarsh on 2022-12-07 14:39_

---

_Comment by @charliermarsh on 2022-12-07 14:40_

\cc @olliemath in case there's other context on how the code changed when ported over (IIRC the context was well-captured in the relevant PRs though)

---

_Comment by @olliemath on 2022-12-12 09:54_

Yeah, would be good to remove the vendored code. It's mostly there since it was an issue pulling in a large crate like rustpython-vm as a dependency (even if the methods were public). None of the code we use references the VM.

With the exception of clippy fixes, the changes to the code are just removals (remove references to the VM, remove dead code, remove some of the unused parsing, ..).

---

_Comment by @charliermarsh on 2023-01-11 01:22_

Oh, this was done!

---

_Closed by @charliermarsh on 2023-01-11 01:22_

---
