```yaml
number: 1148
title: "Ignoring commented-out code with `noqa: ERA001` triggers `RUF100`"
type: issue
state: closed
author: edgarrmondragon
labels: []
assignees: []
created_at: 2022-12-08T21:53:33Z
updated_at: 2022-12-09T04:10:37Z
url: https://github.com/astral-sh/ruff/issues/1148
synced_at: 2026-01-10T12:06:18Z
```

# Ignoring commented-out code with `noqa: ERA001` triggers `RUF100`

---

_Issue opened by @edgarrmondragon on 2022-12-08 21:53_

```python
# commented.py

# print(123)  # noqa: ERA001
# print(123)
```

This is the result of running Ruff on the above:

```console
$ ruff --version
ruff 0.0.170

$ ruff --select ERA,RUF commented.py
Found 2 error(s).
commented.py:3:15: RUF100 Unused `noqa` directive for: ERA001
commented.py:4:1: ERA001 Found commented-out code
2 potentially fixable with the --fix option.
```

---

_Comment by @charliermarsh on 2022-12-08 21:56_

Ah yeah, I think the presence of the `noqa: ` causes us to treat `# print(123)  # noqa: ERA001` as _not_ code... so the `noqa` is itself suppressing the error, and then appears to have no effect.

---

_Comment by @charliermarsh on 2022-12-08 21:57_

Maybe we should only match those at the start of a comment.

---

_Closed by @charliermarsh on 2022-12-09 04:10_

---
