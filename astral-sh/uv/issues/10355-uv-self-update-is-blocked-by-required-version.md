```yaml
number: 10355
title: "`uv self update` is blocked by `required-version`"
type: issue
state: closed
author: tpgillam
labels:
  - bug
assignees: []
created_at: 2025-01-07T11:47:56Z
updated_at: 2025-01-07T18:17:44Z
url: https://github.com/astral-sh/uv/issues/10355
synced_at: 2026-01-10T04:36:21Z
```

# `uv self update` is blocked by `required-version`

---

_Issue opened by @tpgillam on 2025-01-07 11:47_

Initial state:
```console
> uv --version
uv 0.5.14
> cat pyproject.toml | grep required-version
required-version = ">=0.5.15"
> uv sync
error: Required version `>=0.5.15` does not match the running version `0.5.14`
```

All fine so far, but:
```console
> uv self update
error: Required version `>=0.5.15` does not match the running version `0.5.14`
```
so the obvious way to fix the uv-is-too-old problem doesn't work :) Naturally this is easily worked around by cd-ing to a different directory, but it seems likely to me that blocking `uv self` wasn't intended!

I've also tried the same thing after upgrading to uv 0.5.15, and changing `required-version` to >=0.5.16, just to check whether the issue had been fixed already.

(Aside: it might also be useful if the error message said "Required **uv** version...". I can imagine a user who didn't add the uv version bound constraint potentially being confused about what is actually binding.)

---

_Label `bug` added by @konstin on 2025-01-07 12:42_

---

_Comment by @charliermarsh on 2025-01-07 13:44_

I think this makes sense to fix, thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-07 13:44_

---

_Closed by @charliermarsh on 2025-01-07 18:17_

---
