---
number: 7794
title: "ISC001: Opportunity for more autofixes"
type: issue
state: open
author: JelleZijlstra
labels:
  - fixes
assignees: []
created_at: 2023-10-04T00:08:10Z
updated_at: 2024-01-04T03:57:32Z
url: https://github.com/astral-sh/ruff/issues/7794
synced_at: 2026-01-10T01:22:47Z
---

# ISC001: Opportunity for more autofixes

---

_Issue opened by @JelleZijlstra on 2023-10-04 00:08_

Rule ISC001 merges adjacent implicitly concatenated strings. It can't always be autofixed, but I noticed a few occasions where Ruff didn't autofix issues that I would have expected it to fix for me:

```
$ cat isc001.py 
"""hi""" "bye"
"hi" f"{something}"
$ python -m ruff check --select=ISC001 isc001.py --fix
a/gating_defs/isc001.py:1:1: ISC001 Implicitly concatenated string literals on one line
a/gating_defs/isc001.py:2:1: ISC001 Implicitly concatenated string literals on one line
Found 2 errors.
$ python -m ruff --version
ruff 0.0.292
```

In the first case, I assume it doesn't autofix because it doesn't know what quote style to use. But I think it's fine to assume that if the user used both triple and normal quotes, the triple quotes are "bigger" and should be in the output.

For the f-string case, there is a risk of issues if the non-fstring part contains braces. However, it should be safe to combine both strings into a single f-string if the non-f-string doesn't contain `{` or `}`. (You could also consider merging strings that do contain those characters and escape the braces, but that might be more controversial.)

---

_Label `autofix` added by @dhruvmanila on 2023-10-05 03:45_

---

_Label `needs-decision` added by @dhruvmanila on 2023-10-05 03:45_

---

_Label `needs-decision` removed by @charliermarsh on 2024-01-04 03:57_

---
