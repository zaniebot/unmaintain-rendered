```yaml
number: 108
title: Support for E712 and others
type: issue
state: closed
author: amotl
labels: []
assignees: []
created_at: 2022-09-05T21:31:07Z
updated_at: 2022-09-07T21:08:54Z
url: https://github.com/astral-sh/ruff/issues/108
synced_at: 2026-01-12T15:54:40Z
```

# Support for E712 and others

---

_@amotl_

Dear Charlie,

thanks a stack for conceiving this excellent program. While working on modernizing an outdated code base [1,2], we started using `ruff` just recently, and are amazed by its speed. Currently, we would never look back.

As we got the chance to work on this code base, originally from Python 2, but now already ported to Python 3 and polished off with `ruff` and `black`, we thought it would be a good idea to report back what `flake8` would still observe on it.

The error codes are: E203, E711, E712, E713, E741, F841, where E712 was the most popular, followed by its neighbors E711 and E713, as well as F841.

With kind regards,
Andreas.

[1] https://github.com/isarengineering/SecPi/tree/next
[2] https://github.com/SecPi/SecPi/issues/120

---

_Renamed from "Support for E712" to "Support for E712 and others" by @amotl on 2022-09-05 21:56_

---

_Comment by @charliermarsh on 2022-09-06 00:09_

Thank you for this! Makes me so happy to hear that ruff has been useful to you. E711, E712, and E713 will be next on my list (was already considering them as they're not "stylistic" rules and they're relatively simple to implement).

F841 is actually supported... but it's not enabled by default, as it may miss edge cases in its current implementation. You can run it with `ruff --select F841`, or by adding it to your `pyproject.toml` file.


---

_Comment by @charliermarsh on 2022-09-06 14:24_

711, 712, 713, and 714 are now implemented in #110 and #111. Release is building now.

---

_Closed by @charliermarsh on 2022-09-06 14:24_

---

_Comment by @amotl on 2022-09-07 21:08_

Dear Charlie,

wow, that was quick. Thank you very much.

I wanted to take the chance to report back about the outcome after upgrading to the most recent version of `ruff`, when going back to the commit before https://github.com/isarengineering/SecPi/commit/e4befdf6d7, where the code base was polished off a bit more with `flake8`. I hope the report will be helpful to you.

1. E711, E712, and E713 are detected well. Excellent.

2. Differences on E713 at one place with two occurances on the same line:

   #### ruff
   ```
   ./secpi/notifier/spark.py:15:57: E713 Test for membership should be `not in`
   ./secpi/notifier/spark.py:15:33: E713 Test for membership should be `not in`
   ```

   #### flake8
   ```
   ./secpi/notifier/spark.py:15:12: E713 test for membership should be 'not in'
   ```

   I definitively like that `ruff` is reporting each occurrance. As a second thing, you may want to have another look at the line/column reporting deviances, and, maybe, at the output ordering of `ruff` in this case. No strong opinions though, as this is definitively a nit.

Keep up the spirit and with kind regards,
Andreas.


---
