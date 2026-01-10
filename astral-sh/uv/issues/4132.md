```yaml
number: 4132
title: "`==3.11.*` doesn't satisfy `Python>=3.11,<4`"
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-06-07T15:23:44Z
updated_at: 2024-06-08T01:22:58Z
url: https://github.com/astral-sh/uv/issues/4132
synced_at: 2026-01-10T05:31:37Z
```

# `==3.11.*` doesn't satisfy `Python>=3.11,<4`

---

_Issue opened by @konstin on 2024-06-07 15:23_

`requires-python = "==3.11.*"` doesn't satisfy `Python>=3.11,<4` for a project that depends on it, while `requires-python = ">=3.11, <4"` does an resolves without problem.

Example:

```toml
[project]
name = "warehouse"
version = "1.0.0"
requires-python = "==3.11.*"
# requires-python = ">=3.11, <4"
dependencies = [
  "linehaul",
]
```

```
$ cargo run -q -- lock --preview
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (>=3.11.dev0, <3.12.dev0) does not satisfy Python>=3.11,<4 and all versions of linehaul depend
      on Python>=3.11,<4, we can conclude that all versions of linehaul cannot be used.
      And because only the following versions of linehaul are available:
          linehaul==1.0.0
          linehaul==1.0.1
      and warehouse==1.0.0 depends on linehaul, we can conclude that warehouse==1.0.0 cannot be used.
      And because only warehouse==1.0.0 is available and warehouse depends on warehouse, we can conclude that the requirements are
      unsatisfiable.

      hint: The `Requires-Python` requirement (>=3.11.dev0, <3.12.dev0) defined in your `pyproject.toml` includes Python versions that
      are not supported by your dependencies (e.g., all versions of linehaul only supports >=3.11, <4). Consider using a more restrictive
      `Requires-Python` requirement (like >=3.11, <4).
```

---

_Label `preview` added by @konstin on 2024-06-07 15:23_

---

_Comment by @charliermarsh on 2024-06-07 15:55_

Oh, it's because of the `.dev0` I guess? Probably because of the tricks we use when converting specifiers around pre-releases.

---

_Label `bug` added by @charliermarsh on 2024-06-07 15:55_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-07 18:07_

---

_Comment by @charliermarsh on 2024-06-07 18:53_

We should of course fix this but like... it's... technically... correct? I think?

`==3.11.*` includes versions like `3.11.0dev0`, but `>=3.11` does not.


---

_Comment by @charliermarsh on 2024-06-07 19:15_

The easy solution is to ignore pre, post, dev, and local segments when comparing `Requires-Python`.

---

_Closed by @charliermarsh on 2024-06-08 01:22_

---
