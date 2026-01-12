```yaml
number: 3830
title: "False positive: SIM222"
type: issue
state: closed
author: AA-Turner
labels:
  - bug
assignees: []
created_at: 2023-03-31T16:26:42Z
updated_at: 2023-03-31T18:50:36Z
url: https://github.com/astral-sh/ruff/issues/3830
synced_at: 2026-01-12T15:54:44Z
```

# False positive: SIM222

---

_@AA-Turner_

Ruff 0.0.260

Reproducer:

```pwsh
(sphinx) PS I:\Development\sphinx> ruff -V                                                 
ruff 0.0.260
(sphinx) PS I:\Development\sphinx> type bug_sim222.py
a = 'foo'

if a == 'bar' and False:  # NoQA: SIM223
   pass

if 1 == 2 and False:  # NoQA: SIM223
   pass
(sphinx) PS I:\Development\sphinx> ruff --isolated --show-source --select SIM bug_sim222.py
bug_sim222.py:3:4: SIM222 [*] Use `True` instead of `... or True`
  |
3 | if a == 'bar' and False:  # NoQA: SIM223
  |    ^^^^^^^^^^^^^^^^^^^^ SIM222
  |
  = help: Replace with `True`

bug_sim222.py:6:4: SIM222 [*] Use `True` instead of `... or True`
  |
6 | if 1 == 2 and False:  # NoQA: SIM223
  |    ^^^^^^^^^^^^^^^^ SIM222
  |
  = help: Replace with `True`

Found 2 errors.
[*] 2 potentially fixable with the --fix option.
(sphinx) PS I:\Development\sphinx> 
```

Note that the `# NoQA: SIM223` is intentional, this is from a 'TODO' part of a method.

In reducing the error case I wondered if the logic required a variable at all, and hence the second reprodcuer -- clearly this is statically false!

I don't have a guess as to what is causing this, sorry!

Thanks,
Adam


---

_Comment by @charliermarsh on 2023-03-31 16:28_

Oops!

---

_Label `bug` added by @charliermarsh on 2023-03-31 16:28_

---

_Comment by @charliermarsh on 2023-03-31 16:28_

\cc @JonathanPlasse - Not that you're obligated to fix, but in the event that the fix is obvious to you :)

---

_Comment by @JonathanPlasse on 2023-03-31 17:21_

Oops! Indeed! :sweat_smile: 
I am much obliged to fix this.

---

_Closed by @charliermarsh on 2023-03-31 18:50_

---
