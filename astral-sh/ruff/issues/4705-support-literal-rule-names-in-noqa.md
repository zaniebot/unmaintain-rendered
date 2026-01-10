```yaml
number: 4705
title: "support literal rule names in `noqa`"
type: issue
state: closed
author: t184256
labels: []
assignees: []
created_at: 2023-05-29T12:37:19Z
updated_at: 2023-05-29T16:48:32Z
url: https://github.com/astral-sh/ruff/issues/4705
synced_at: 2026-01-10T11:09:47Z
```

# support literal rule names in `noqa`

---

_Issue opened by @t184256 on 2023-05-29 12:37_

Rules, conveniently, have names.
I want to be able to suppress errors by rule names and not codes.

With ruff 0.0.270:

`a = 3; not a is None  # noqa: E714` -> finds `E702` \
`a = 3; not a is None  # noqa: not-is-test` -> suppresses both errors

Expectations:

`a = 3; not a is None  # noqa: E714` -> finds `E702` \
`a = 3; not a is None  # noqa: not-is-test` -> finds `E702`

---

_Comment by @JonathanPlasse on 2023-05-29 16:46_

- Duplicate of #1773.

---

_Comment by @t184256 on 2023-05-29 16:47_

Indeed, sorry for the noise.

---

_Closed by @t184256 on 2023-05-29 16:47_

---

_Comment by @charliermarsh on 2023-05-29 16:48_

All good, we plan to support this eventually.

---
