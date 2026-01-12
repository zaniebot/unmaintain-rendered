```yaml
number: 12607
title: "`uv build --no-build` fails"
type: issue
state: open
author: zanieb
labels:
  - needs-decision
assignees: []
created_at: 2025-04-01T20:30:22Z
updated_at: 2025-10-21T02:08:30Z
url: https://github.com/astral-sh/uv/issues/12607
synced_at: 2026-01-12T16:01:07Z
```

# `uv build --no-build` fails

---

_@zanieb_

```
❯ uv build --no-build
Building source distribution...
  × Failed to build `/Users/zb/workspace/uv/example`
  ╰─▶ Building source distributions is disabled
````

Shouldn't we allow this as it's a local source tree?

 _Originally posted in [#11682](https://github.com/astral-sh/uv/issues/11682#issuecomment-2768520419)_

---

_Label `needs-decision` added by @zanieb on 2025-04-01 20:30_

---

_Assigned to @konstin by @zanieb on 2025-04-01 20:30_

---

_Comment by @konstin on 2025-04-08 14:37_

We currently deny local source tree builds in other places, e.g., `uv sync --no-build`, so it's consistent that `UV_NO_BUILD=0` blocks `uv build`, too. We would need to differentiate our no-build flags by whether they include or exclude local source trees.

---

_Comment by @zanieb on 2025-04-08 17:21_

Oh, thanks for clarifying. I think that's weird tbh.

```
❯ uv init --package example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ uv sync --no-build
Using CPython 3.13.2
Creating virtual environment at: .venv
Resolved 1 package in 0.97ms
error: Distribution `example==0.1.0 @ editable+.` can't be installed because it is marked as `--no-build` but has no binary distribution
❯ uv pip install --no-build .
Resolved 1 package in 0.81ms
error: Failed to prepare distributions
  Caused by: Building source distributions is disabled, but attempted to build `example`
```

As a note, the workaround is

```
❯ uv sync --no-build --no-binary-package example
```

---

_Unassigned @konstin by @konstin on 2025-08-26 09:42_

---

_Comment by @CHC383 on 2025-10-21 02:08_

Differentiate no-build for local source trees would be helpful. In my case I would like to enable no-build for my dependencies but not my current project/local source tree while running `uv sync`

---
