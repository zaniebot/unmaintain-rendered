---
number: 1989
title: F401 false positives when match statements are in the file 
type: issue
state: closed
author: kendangclubhouse
labels: []
assignees: []
created_at: 2023-01-19T04:33:02Z
updated_at: 2023-02-22T00:05:07Z
url: https://github.com/astral-sh/ruff/issues/1989
synced_at: 2026-01-07T13:12:14-06:00
---

# F401 false positives when match statements are in the file 

---

_Issue opened by @kendangclubhouse on 2023-01-19 04:33_

Im seeing that ruff is not finding F401's in files that have match statements. 

Code snippet

```
import datetime

x = 5
match x:
    case _:
        print("hello")
```


You should expect that `import datetime` should cause a F401 but when runing `ruff /path/to/file --select F401` nothing is returned.  When I remove the match statement, ruff will return the F401 issue.

```
❯ ruff ~ruff_bug.py --select F401
ruff_bug.py:1:8: F401 `datetime` imported but unused
  |
1 | import datetime
  |        ^^^^^^^^ F401
  |
  = help: Remove unused import: `datetime
```

Im running `ruff 0.0.225` 


---

_Comment by @charliermarsh on 2023-01-19 04:49_

I believe this is due to our lack of support for `match` statements. It's the last big language feature that we don't yet support (#282). I _assume_ that if you `--select E999`, you'll see a syntax error? That's _probably_ blocking the `F401` from being picked up.

---

_Closed by @charliermarsh on 2023-01-19 12:40_

---

_Comment by @kendangclubhouse on 2023-01-19 18:10_

 I find it odd that I cannot ignore the E999. Does E999 have to block all other errors in the file? 

---

_Comment by @charliermarsh on 2023-01-19 18:15_

The issue is that if we can't parse the code, then we can't get a valid AST, and so we can't enforce a large portion of the rule set. It's a limitation of the parser right now: (1) it can't parse `match` statements, and (2) it can't recover from errors gracefully. (Even if it could, you would still end up with false positives or negative -- for example, if an import was used only in a `match` statement, we'd have no way of knowing.)

---

_Comment by @kendangclubhouse on 2023-01-19 18:25_

I see, that make a lot of sense. Thank you so much for explaining. 🙂

---

_Comment by @charliermarsh on 2023-01-20 05:47_

No prob! I know it’s not a satisfying answer, but hopefully it makes some sense. The real fix here is to get match statements working so that it’s a non-issue!

---

_Comment by @charliermarsh on 2023-02-22 00:05_

Fixed as of v0.0.250.

---
