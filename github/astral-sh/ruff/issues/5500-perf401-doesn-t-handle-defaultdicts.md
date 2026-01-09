---
number: 5500
title: "PERF401 doesn't handle defaultdicts"
type: issue
state: closed
author: hynek
labels: []
assignees: []
created_at: 2023-07-04T06:27:15Z
updated_at: 2023-07-04T18:21:07Z
url: https://github.com/astral-sh/ruff/issues/5500
synced_at: 2026-01-07T13:12:15-06:00
---

# PERF401 doesn't handle defaultdicts

---

_Issue opened by @hynek on 2023-07-04 06:27_

Given this code:

```python
from collections import defaultdict

d = defaultdict(list)

for i in [1, 2, 3]:
    d[i].append(i**2)
```

Gives me this:

```python
$ pipx run ruff --version
ruff 0.0.276
$ pipx run ruff --isolated --select PERF401 t2.py
t2.py:6:5: PERF401 Use a list comprehension to create a transformed list
Found 1 error.
```

Maybe I'm dense, but I don't think I can rewrite that as a list comprehension, non the less because it's not a list, but a (default)dict. 😅

Interesting fact: the error doesn't trigger if there's no computation in `append()`.

---

_Comment by @jerr0328 on 2023-07-04 10:09_

Came here for similar reasons, though I'm getting PERF402, seems like #5494 is similar.

---

_Referenced in [WeblateOrg/weblate#9500](../../WeblateOrg/weblate/pulls/9500.md) on 2023-07-04 10:56_

---

_Comment by @charliermarsh on 2023-07-04 12:30_

I believe this is a dupe of #5494 - we need to avoid triggering when the loop variable is used on the left-hand side (or perhaps just avoid whenever the left-hand side is more complicated than a name).

---

_Closed by @charliermarsh on 2023-07-04 12:30_

---

_Comment by @nijel on 2023-07-04 16:19_

I have no clue about the implementation,  but there are different checks being triggered in the issues PERF402 and PERF401.

---

_Comment by @charliermarsh on 2023-07-04 16:20_

Yup acknowledged — it’s the same underlying problem and fix.

---

_Referenced in [astral-sh/ruff#5508](../../astral-sh/ruff/pulls/5508.md) on 2023-07-04 18:12_

---

_Closed by @charliermarsh on 2023-07-04 18:21_

---
