```yaml
number: 5017
title: "`uvx` should warn or error when an executable outside of `--from` is used"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - cli
  - preview
assignees: []
created_at: 2024-07-12T16:25:37Z
updated_at: 2024-07-15T19:27:11Z
url: https://github.com/astral-sh/uv/issues/5017
synced_at: 2026-01-12T15:58:53Z
```

# `uvx` should warn or error when an executable outside of `--from` is used

---

_@zanieb_

As mentioned at https://github.com/astral-sh/uv/pull/4997#discussion_r1675091857

e.g. in this invocation:

```
uvx --from fastapi fastapi
```

`fastapi` is actually provided by a `fastapi` dependency `fastapi-cli`. If a user later runs `uv tool install fastapi` it will fail saying no executables are available. We should try to have a smooth transition from `uv tool run` to `uv tool install`. If the invoked executable does not belong to the `--from` package, I think we should _at least_ warn and maybe hard-error unless some opt-in is given e.g. `--include-from fastapi-cli`.

See https://github.com/astral-sh/uv/issues/4994 for more discussion on the opt-in flags.

---

_Label `cli` added by @zanieb on 2024-07-12 16:26_

---

_Label `preview` added by @zanieb on 2024-07-12 16:26_

---

_Label `help wanted` added by @charliermarsh on 2024-07-13 16:35_

---

_Closed by @zanieb on 2024-07-15 19:27_

---

_Closed by @zanieb on 2024-07-15 19:27_

---
