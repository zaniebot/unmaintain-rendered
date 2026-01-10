```yaml
number: 11076
title: "Add support for `uvx python`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/uvx-python
created_at: 2025-01-29T18:45:24Z
updated_at: 2025-01-30T22:04:18Z
url: https://github.com/astral-sh/uv/pull/11076
synced_at: 2026-01-10T11:10:34Z
```

# Add support for `uvx python`

---

_Pull request opened by @zanieb on 2025-01-29 18:45_

Supersedes https://github.com/astral-sh/uv/pull/7491
Closes https://github.com/astral-sh/uv/issues/7430

Thanks @mikeleppane for starting this implementation. I took a bit of a different approach and it was easier to start over fresh, but I used some of the test cases there.

---

_Label `enhancement` added by @zanieb on 2025-01-29 18:45_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:452 on 2025-01-29 18:48_

This is mostly just indented in the if else

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:569 on 2025-01-29 18:49_

This is new, to prevent confusing errors about not finding `python` in the registry.

---

_@zanieb reviewed on 2025-01-29 18:49_

---

_Comment by @zanieb on 2025-01-29 18:50_

As follow-ups, we can add support for

- `uvx python<version>`, e.g., `uvx python3.12`
- `uvx python@latest` (we have no concept of selecting the latest Python version right now)

---

_Review comment by @inoa-jboliveira on `docs/reference/cli.md`:3040 on 2025-01-29 19:58_

```suggestion
`uvx` can be used to invoke Python, e.g., with `uvx python` or `uvx python@<version>`. A Python interpreter will be started in an isolated virtual environment.
```

---

_@inoa-jboliveira reviewed on 2025-01-29 20:23_

IMO, this is much better than what we have right now, which is `uv run -p 3.12 -- python`, not just because it is shorter but because it is on a clean env. uv run also fails if you are in a directory with a pyproject that does not support the version you want to run. 

I would still love a short to type shim similar to how `py` works on Windows. 

---

_@charliermarsh reviewed on 2025-01-30 01:05_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:225 on 2025-01-30 01:05_

I would probably make the things within ticks green, for consistency.

---

_@charliermarsh reviewed on 2025-01-30 01:06_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:489 on 2025-01-30 01:06_

This is like, `uvx --python 3.7 python@3.7`?

---

_@charliermarsh reviewed on 2025-01-30 01:08_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:188 on 2025-01-30 01:08_

Do we need to do anything to ensure that `python` resolves to the "correct" Python? Is there any risk that the user does, like, `uvx --python 3.7 python` and we end up picking a Python other than the discovered interpreter?

---

_@zanieb reviewed on 2025-01-30 01:45_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:489 on 2025-01-30 01:45_

Yeah. We could check for compatibility or merge them or whatever because `uvx --python pypy python@3.7` is "fine" but I don't do anything complicated here.

---

_@zanieb reviewed on 2025-01-30 01:45_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:225 on 2025-01-30 01:45_

👍 good call


---

_@zanieb reviewed on 2025-01-30 01:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:188 on 2025-01-30 01:46_

I did not consider that; I will add a guard.

---

_Merged by @zanieb on 2025-01-30 17:53_

---

_Closed by @zanieb on 2025-01-30 17:53_

---

_Branch deleted on 2025-01-30 17:54_

---

_@zanieb reviewed on 2025-01-30 22:04_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:188 on 2025-01-30 22:04_

Opted not to do this as it complicates the implementation and I'm pretty sure there can't be a problem in-practice. We do the same thing for actual tool executables.

---
