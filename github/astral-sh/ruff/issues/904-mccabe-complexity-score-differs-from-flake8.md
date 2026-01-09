---
number: 904
title: mccabe complexity score differs from flake8
type: issue
state: closed
author: kalekseev
labels:
  - bug
assignees: []
created_at: 2022-11-25T15:37:04Z
updated_at: 2022-11-25T18:16:04Z
url: https://github.com/astral-sh/ruff/issues/904
synced_at: 2026-01-07T13:12:14-06:00
---

# mccabe complexity score differs from flake8

---

_Issue opened by @kalekseev on 2022-11-25 15:37_

```
❯ flake8 --version
5.0.4 (flake8-bugbear: 22.10.27, flake8-no-pep420: 2.3.0, mccabe: 0.7.0, pep8-naming: 0.13.2, pycodestyle: 2.9.1, pyflakes: 2.5.0)
CPython 3.10.5 on Darwin

❯ ruff --version
ruff 0.0.138

❯ flake8 a.py

❯ ruff --select C a.py
Found 1 error(s).
a.py:1:1: C901 `fn` is too complex (11)

❯ cat a.py
def fn(a, b, c, d, e):
    if a:
        ...
    try:
        ...
    except ValueError:
        ...
    if b:
        ...
    if c:
        ...
    if d:
        ...
    else:
        try:
            if a:
                if not b:
                    ...
        except ValueError:
            ...
    ...
```

---

_Comment by @charliermarsh on 2022-11-25 17:57_

Thanks! Will take a look at this today.

---

_Label `bug` added by @charliermarsh on 2022-11-25 18:00_

---

_Comment by @charliermarsh on 2022-11-25 18:12_

I think that the actual scoring is in the same in both tools -- they bother report `11`:

```
∴ flake8 --select C a.py --max-complexity 10
a.py:1:1: C901 'fn' is too complex (11)
```

The difference is that in Flake8, just adding `--select C901` is insufficient for enabling the check. You _also_ need to set `--max-complexity` explicitly, otherwise it defaults to `-1`.

---

_Closed by @charliermarsh on 2022-11-25 18:13_

---

_Comment by @charliermarsh on 2022-11-25 18:13_

Let me know if that resolves for you! Feel free to follow-up here, just closing preemptively.

---

_Comment by @kalekseev on 2022-11-25 18:14_

Oops, my bad, sorry

---

_Comment by @charliermarsh on 2022-11-25 18:16_

No prob at all, I found that behavior confusing in Flake8 so intentionally differed in Ruff.

---
