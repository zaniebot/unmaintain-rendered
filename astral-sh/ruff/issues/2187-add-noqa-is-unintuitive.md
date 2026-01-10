```yaml
number: 2187
title: "`--add-noqa` is unintuitive"
type: issue
state: closed
author: not-my-profile
labels:
  - cli
assignees: []
created_at: 2023-01-26T05:47:48Z
updated_at: 2023-02-03T04:43:07Z
url: https://github.com/astral-sh/ruff/issues/2187
synced_at: 2026-01-10T11:09:45Z
```

# `--add-noqa` is unintuitive

---

_Issue opened by @not-my-profile on 2023-01-26 05:47_

I would expect `--add-noqa` to only add the directive to violations that ruff cannot fix but it in fact adds it everywhere.

Even worse the following just ignores the `--fix` option and does not fix anything but adds `noqa` everywhere:

```
ruff foo.py --add-noqa --fix
```

I think at least the former should be clearly explained in the option description in `--help` and the latter should result in an error until that is supported.

---

_Comment by @charliermarsh on 2023-01-26 14:34_

I think a reasonable thing to do for now would be to warn users that `--fix` is incompatible with `--add-noqa` (and recommend that users run `ruff --fix .` and then `ruff --add-noqa .`).

I view `--add-noqa` as something that's run once, or perhaps a few times, as an upgrade tool. In that light, it might be fine as a separate subcommand.


---

_Label `cli` added by @charliermarsh on 2023-01-26 14:48_

---

_Closed by @charliermarsh on 2023-02-03 04:43_

---
