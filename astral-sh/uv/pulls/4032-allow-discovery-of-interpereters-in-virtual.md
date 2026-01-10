```yaml
number: 4032
title: Allow discovery of interpereters in virtual environments activated by a shim
type: pull_request
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
base: main
head: zb/discover-default
created_at: 2024-06-05T00:12:35Z
updated_at: 2025-02-04T15:55:56Z
url: https://github.com/astral-sh/uv/pull/4032
synced_at: 2026-01-10T11:10:33Z
```

# Allow discovery of interpereters in virtual environments activated by a shim

---

_Pull request opened by @zanieb on 2024-06-05 00:12_

Closes https://github.com/astral-sh/uv/issues/4009
Closes https://github.com/astral-sh/uv/issues/2109


---

_Label `enhancement` added by @zanieb on 2024-06-05 00:12_

---

_Comment by @zanieb on 2024-06-05 00:12_

Considering if this is the correct approach still. We should also test with the `pyenv-virtualenv` shim.

---

_Comment by @zanieb on 2024-06-05 00:17_

I find this implementation awkward as I only want this to be used if it's a virtual environment interpreter but we can't easily enforce that in `python_executables`. I'll probably poke at a better implementation.

---

_@zanieb reviewed on 2024-06-05 00:31_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:403 on 2024-06-05 00:31_

No changes to the match statement, just indented

---

_Marked ready for review by @zanieb on 2024-06-06 12:42_

---

_Comment by @zanieb on 2024-06-06 12:42_

I'd appreciate input on a better approach here.

---

_Review requested from @charliermarsh by @zanieb on 2024-06-06 12:43_

---

_Comment by @charliermarsh on 2024-06-08 18:19_

Just confirming: still relevant to review? I know there was some back-and-forth on the issue but didn't read it deeply.

---

_Comment by @zanieb on 2024-06-08 18:21_

@charliermarsh yeah this is still necessary if we want to support detecting virtual environments that are only activated by an executable shim. I'm not in love with the implementation though.

---

_@charliermarsh reviewed on 2024-06-08 20:14_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:232 on 2024-06-08 20:14_

How do we know this is a shim?

---

_@charliermarsh reviewed on 2024-06-08 20:15_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:396 on 2024-06-08 20:15_

What is a shim environment that isn't a virtual environment? What would appear here to fail this filter?

---

_@zanieb reviewed on 2024-06-08 21:35_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:232 on 2024-06-08 21:35_

We don't. We're going to naively say the first executable in the path _might_ be a shim then verify it during the interpreter query portion.

---

_@zanieb reviewed on 2024-06-08 21:36_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:396 on 2024-06-08 21:36_

Since we're just taking the first executable and calling it a "shim environment" in `python_executables` this filtering is where we check if the executable is _actually_ a shim which invokes inside a virtual environment.

---

_@zanieb reviewed on 2024-06-08 21:37_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:396 on 2024-06-08 21:37_

(this and the other review comment are my concern with this implementation, it's misleading but we do it to avoid querying the interpreter during `python_executables`)

---

_Review requested from @konstin by @zanieb on 2024-06-10 14:31_

---

_@konstin approved on 2024-06-11 07:53_

---

_Closed by @zanieb on 2025-02-04 15:55_

---
