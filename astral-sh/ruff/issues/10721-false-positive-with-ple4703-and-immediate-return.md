---
number: 10721
title: False positive with PLE4703 and immediate return / break
type: issue
state: open
author: hauntsaninja
labels:
  - bug
  - preview
assignees: []
created_at: 2024-04-01T18:05:47Z
updated_at: 2024-04-08T06:36:05Z
url: https://github.com/astral-sh/ruff/issues/10721
synced_at: 2026-01-10T01:22:50Z
---

# False positive with PLE4703 and immediate return / break

---

_Issue opened by @hauntsaninja on 2024-04-01 18:05_

Not the biggest deal, but I got a false positive on this with:
```
λ cat x.py
def foo():
    s = {1, 2, 3}
    for e in s:
        if e >= 3:
            s.remove(e)
            return

λ ruff check --select PLE x.py --preview 
x.py:3:5: PLE4703 Iterated set `s` is modified within the `for` loop
  |
1 |   def foo():
2 |       s = {1, 2, 3}
3 |       for e in s:
  |  _____^
4 | |         if e >= 3:
5 | |             s.remove(e)
6 | |             return
  | |__________________^ PLE4703
  |
  = help: Iterate over a copy of `s`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

cc @boolean-light in case you're interested, and thanks for the new rule :-)

---

_Label `bug` added by @charliermarsh on 2024-04-01 18:17_

---

_Label `preview` added by @charliermarsh on 2024-04-01 18:18_

---

_Comment by @charliermarsh on 2024-04-01 18:18_

Thanks!

---

_Comment by @charliermarsh on 2024-04-01 18:20_

Struggling not to get sniped into fixing this.

---

_Comment by @MichaReiser on 2024-04-02 09:11_

Hmm that makes sense. It would be nice to use the control flow graph for this because I assume there are other situations where the loop exits early (`continue`, `break`, `raise` etc) that aren't covered and they might be more subtle. 

---

_Comment by @hikaru-kajita on 2024-04-03 06:51_

Excuse me I'm back :D Thank you, and let me have a look on this.

---

_Comment by @hikaru-kajita on 2024-04-08 02:32_

Sorry, actually I don't think I can implement this one by myself. Is there some kind of support for control flow group in Ruff already?

---

_Comment by @MichaReiser on 2024-04-08 06:36_

No, there's not. That's what I meant by my comment. We probably want to wait for a more complete control flow analysis or decided to only support a very small subset of control flow patterns for now.

---

_Referenced in [astral-sh/ruff#11695](../../astral-sh/ruff/issues/11695.md) on 2024-08-12 14:32_

---
