```yaml
number: 1091
title: "OOM when trying to process Tauon's codebase"
type: issue
state: closed
author: C0rn3j
labels:
  - performance
assignees: []
created_at: 2025-08-23T20:07:04Z
updated_at: 2025-08-28T06:43:02Z
url: https://github.com/astral-sh/ty/issues/1091
synced_at: 2026-01-12T15:54:24Z
```

# OOM when trying to process Tauon's codebase

---

_@C0rn3j_

### Summary

https://github.com/Taiko2k/Tauon

`ty check src/tauon` eats RAM endlessly, I killed it after 10GB consumed.

### Version

ty 0.0.1-alpha.19 (e9cb838b3 2025-08-19)

---

_Label `memory` added by @AlexWaygood on 2025-08-23 20:07_

---

_Label `memory` removed by @sharkdp on 2025-08-25 08:34_

---

_Label `performance` added by @sharkdp on 2025-08-25 08:34_

---

_Comment by @sharkdp on 2025-08-25 08:45_

Thank you for reporting this.

The problem seems to come from [a single file](https://github.com/Taiko2k/Tauon/blob/master/src/tauon/t_modules/t_main.py) specifically (which is 50k lines large).

Interestingly, this seems to be a performance problem during semantic index building, with a lot of time spent in `ReachabilityConstraintsBuilder::add_and_constraint`:

<img width="962" height="177" alt="Image" src="https://github.com/user-attachments/assets/14648473-35b6-488d-921a-7b683d7153f1" />

<img width="1918" height="1152" alt="Image" src="https://github.com/user-attachments/assets/4552fd7a-34b6-4597-af86-01ff30bcb1c1" />

---

_Comment by @C0rn3j on 2025-08-25 09:38_

https://github.com/microsoft/pyright/discussions/10838

I presume this is due to the rather massive 9Kloc main function, which Pyright also chugs on, but Pyright has a complexity calculation and refuses to work to prevent the OOM on such functions.

---

_Assigned to @sharkdp by @sharkdp on 2025-08-26 15:29_

---

_Closed by @sharkdp on 2025-08-27 18:42_

---

_Comment by @sharkdp on 2025-08-27 18:46_

With the changes in https://github.com/astral-sh/ruff/pull/20098, I can now type-check the project in 9s on my laptop, where almost all of the time is still spent on `t_main.py`. So while definitely not great, we can now run to completion, and peak memory usage is around 2 GB. I am certain that we could do better here, but I don't think it's an immediate priority at the moment, given that it's rather unusual to have more than a thousand `if` statements in a single function. Feel free to reopen/comment if you think that this should be looked at more.

Splitting that function into several parts would almost certainly lead to much better type check times.

---

_Comment by @C0rn3j on 2025-08-28 06:43_

That is positively awesome, thanks a lot for this find&fix!

There's [this branch](https://github.com/Taiko2k/Tauon/pull/1801) where I split the functions up a bit, which I'll be trying to get in to master, but it is awesome to know it can work as-is too, albeit not in the most resource-friendly fashion.

---
