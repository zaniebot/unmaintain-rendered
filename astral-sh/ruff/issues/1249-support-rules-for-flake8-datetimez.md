```yaml
number: 1249
title: Support rules for flake8-datetimez
type: issue
state: closed
author: Yasu-umi
labels:
  - rule
assignees: []
created_at: 2022-12-15T17:44:28Z
updated_at: 2023-06-05T15:52:27Z
url: https://github.com/astral-sh/ruff/issues/1249
synced_at: 2026-01-10T11:09:43Z
```

# Support rules for flake8-datetimez

---

_Issue opened by @Yasu-umi on 2022-12-15 17:44_

https://github.com/pjknkda/flake8-datetimez

I started impl at [here](https://github.com/Yasu-umi/ruff/tree/feature/flake8-datetimez).
I'll create pull req in this year, or should I create draft now ? I already implemented 1 / 9 rule.

Thanks for develop incredibly fast linter !

---

_Comment by @charliermarsh on 2022-12-15 19:33_

Awesome. I’m happy to take a look whenever. We can merge before implementing the entire plugin, but it’d be good to have _most_ of the rules implemented first.

---

_Label `rule` added by @charliermarsh on 2022-12-15 19:33_

---

_Comment by @charliermarsh on 2022-12-19 05:20_

Done by @Yasu-umi!

---

_Closed by @charliermarsh on 2022-12-19 05:20_

---

_Comment by @sasanjac on 2023-03-21 14:16_

As the original flake8 plugin, this implementation also misses `.astimezone()`-calls on naive datetime objects. I implemented a fix [here](https://github.com/pjknkda/flake8-datetimez/pull/10) but the project seems abandoned, so I'm going to fix it here instead.


---

_Comment by @Kilo59 on 2023-06-02 22:29_

@sasanjac did you ever end up implementing the `.astimezone()` fix for ruff?
Is there an issue or PR I can follow your progress and potentially help out?

---

_Comment by @sasanjac on 2023-06-05 15:52_

Unfortunately, I haven't got around to it but I might tackle it later this month unless you want to have a go at it.

---
