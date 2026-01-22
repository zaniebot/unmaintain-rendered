```yaml
number: 19576
title: "SIM114: Safe fix removes comments"
type: issue
state: open
author: Nikratio
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-07-27T16:39:52Z
updated_at: 2026-01-22T01:28:47Z
url: https://github.com/astral-sh/ruff/issues/19576
synced_at: 2026-01-22T02:08:53Z
```

# SIM114: Safe fix removes comments

---

_@Nikratio_

### Summary

Example:

```
ruff check src/s3ql/backends/b2/b2_backend.py 
src/s3ql/backends/b2/b2_backend.py:362:9: SIM114 [*] Combine `if` branches using logical `or` operator
    |
360 |               return True
361 |
362 | /         elif isinstance(exc, HTTPError) and (
363 | |             (500 <= exc.status <= 599)
364 | |             or exc.status == 408  # server errors
365 | |             or exc.status == 429  # request timeout
366 | |         ):  # too many requests
367 | |             return True
368 | |
369 | |         # Consider all SSL errors as temporary. There are a lot of bug
370 | |         # reports from people where various SSL errors cause a crash
371 | |         # but are actually just temporary. On the other hand, we have
372 | |         # no information if this ever revealed a problem where retrying
373 | |         # was not the right choice.
374 | |         elif isinstance(exc, ssl.SSLError):
375 | |             return True
    | |_______________________^ SIM114
376 |
377 |           return False
    |
    = help: Combine `if` branches
[...]
Found 7 errors.
[*] 1 fixable with the `--fix` option (2 hidden fixes can be enabled with the `--unsafe-fixes` option).
```


I noticed that there's at least one older report about a similar problem with another rule. Perhaps there's a need for some generic safeguard that prevents removal of comments for *any* rule?

### Version

ruff 0.12.5

---

_Label `bug` added by @MichaReiser on 2025-07-28 09:33_

---

_Label `fixes` added by @MichaReiser on 2025-07-28 09:33_

---

_Label `help wanted` added by @MichaReiser on 2025-07-28 09:33_

---

_Comment by @mikeleppane on 2025-08-05 09:26_

I'm working on this...👨‍💻

---

_Comment by @phongddo on 2025-12-12 12:44_

Hey @ntBre 👋 I'm having the same issue. I can take it over if you're fine with that, perhaps continue from where Mikko left off.

---

_Comment by @ntBre on 2025-12-12 14:25_

Sounds good, thank you!

---

_Assigned to @phongddo by @ntBre on 2025-12-12 14:25_

---

_Label `bug` removed by @MichaReiser on 2025-12-26 08:33_

---

_Label `bug` added by @MichaReiser on 2025-12-26 08:33_

---

_Comment by @eureka928 on 2026-01-15 23:24_

@MichaReiser is it good to ake this issue as well?

---

_Comment by @amyreese on 2026-01-16 02:27_

@leonace924 Phong has a draft PR #22079 for this already.

@phongddo are you still working on your PR?  

---

_Comment by @phongddo on 2026-01-16 10:23_

Hey @amyreese @leonace924 thanks for tagging, feel free to take it 🙏 I'm currently busy with other things so I won't be actively working on it until next month.

---

_Assigned to @eureka928 by @amyreese on 2026-01-16 19:07_

---

_Comment by @eureka928 on 2026-01-22 01:28_

@ntBre is this available? I am afraid of someone else put a PR at the same time

---
