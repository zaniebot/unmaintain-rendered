```yaml
number: 218
title: Suggest fixes to users
type: issue
state: closed
author: cnpryer
labels:
  - fixes
  - cli
assignees: []
created_at: 2022-09-17T03:23:07Z
updated_at: 2023-01-12T03:31:29Z
url: https://github.com/astral-sh/ruff/issues/218
synced_at: 2026-01-12T15:54:40Z
```

# Suggest fixes to users

---

_@cnpryer_

Hi! Love this project. Looking forward to helping out in whatever way I can.

If not already implemented, are there plans to offer suggestions for those auto-fixable changes?

One thing I enjoy about `clippy` is using it as a learning instrument. New suggested changes help me understand what might be idiomatic, what may not. As a new Python user I'd love to have that same experience.

---

_Label `enhancement` added by @charliermarsh on 2022-09-17 03:47_

---

_Comment by @charliermarsh on 2022-09-17 18:21_

That's a nice idea! Right now, we have a `--fix` flag that will automatically fix a small subset of issues. It'd be cool to suggest those fixes directly to the user (as in clippy) when `--fix` is disabled.

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:20_

---

_Label `autofix` added by @charliermarsh on 2022-12-31 18:20_

---

_Label `cli` added by @charliermarsh on 2022-12-31 18:20_

---

_Comment by @charliermarsh on 2023-01-12 03:31_

I would say we now do this. It's not exactly the same as Clippy, but we do show you the fix message with `--show-source`.

---

_Closed by @charliermarsh on 2023-01-12 03:31_

---
