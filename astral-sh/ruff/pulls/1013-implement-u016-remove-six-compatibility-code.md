```yaml
number: 1013
title: "Implement U016: Remove six compatibility code"
type: pull_request
state: merged
author: martinlehoux
labels: []
assignees: []
merged: true
base: main
head: u016-remove-six-compat
created_at: 2022-12-03T14:18:55Z
updated_at: 2023-02-28T18:07:40Z
url: https://github.com/astral-sh/ruff/pull/1013
synced_at: 2026-01-12T15:55:05Z
```

# Implement U016: Remove six compatibility code

---

_@martinlehoux_

- #827 
- There are still a few things to fix, but I've been holding onto this one for too long

---

_Comment by @martinlehoux on 2022-12-03 14:19_

@charliermarsh FYI

---

_Review comment by @charliermarsh on `resources/test/fixtures/pyupgrade/U016.py`:80 on 2022-12-03 15:23_

We should probably correct this one to `next(iter(dct.items()))` (right now it's `next(dct.items())`) before merging.

---

_@charliermarsh reviewed on 2022-12-03 15:23_

---

_Review comment by @charliermarsh on `resources/test/fixtures/pyupgrade/U016.py`:77 on 2022-12-03 15:23_

If you want to punt on the class conversions before merging, that's fine.

---

_@charliermarsh reviewed on 2022-12-03 15:23_

---

_@charliermarsh reviewed on 2022-12-03 15:32_

---

_Review comment by @charliermarsh on `resources/test/fixtures/pyupgrade/U016.py`:80 on 2022-12-03 15:32_

Are you up for that @martinlehoux?

---

_@charliermarsh reviewed on 2022-12-03 15:33_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/remove_six_compat.rs`:26 on 2022-12-03 15:33_

@martinlehoux - Were these intentionally returning single-element tuples?

---

_@martinlehoux reviewed on 2022-12-05 07:50_

---

_Review comment by @martinlehoux on `src/pyupgrade/plugins/remove_six_compat.rs`:26 on 2022-12-05 07:50_

No I guess you're right!

---

_Review comment by @martinlehoux on `resources/test/fixtures/pyupgrade/U016.py`:77 on 2022-12-05 07:50_

I'll have a try today, I'll let you know if I can address this

---

_@martinlehoux reviewed on 2022-12-05 07:50_

---

_@martinlehoux reviewed on 2022-12-05 07:52_

---

_Review comment by @martinlehoux on `resources/test/fixtures/pyupgrade/U016.py`:80 on 2022-12-05 07:52_

Do you have any advice on how I could do that? 

I guess going through the LST is done from higher level to lower level, so if I rewrite `next(six.iteritems(dct))` to `next(iter(dct.items()))`, then there won't be a "lower level" issue to fix (what is getting fixed today)

---

_Review comment by @martinlehoux on `src/pyupgrade/plugins/remove_six_compat.rs`:26 on 2022-12-05 07:58_

Okay I dodn't see you made this change when I answered. I must say I don't have any background on `six` usage, but I took the examples from pyupgrade as test, so Iguess yes, it was intentional to have single element tuples.

---

_@martinlehoux reviewed on 2022-12-05 07:58_

---

_@martinlehoux reviewed on 2022-12-05 07:59_

---

_Review comment by @martinlehoux on `src/pyupgrade/plugins/remove_six_compat.rs`:26 on 2022-12-05 07:59_

```
>>> import six
>>> six.class_types
(<class 'type'>,)
```

---

_Review comment by @martinlehoux on `resources/test/fixtures/pyupgrade/U016.py`:80 on 2022-12-14 18:39_

seems to work

---

_@martinlehoux reviewed on 2022-12-14 18:39_

---

_@martinlehoux reviewed on 2022-12-14 18:40_

---

_Review comment by @martinlehoux on `resources/test/fixtures/pyupgrade/U016.py`:77 on 2022-12-14 18:40_

Sorry I didn't take the time, let's merge this one before and then I'll get on classes

---

_Comment by @martinlehoux on 2022-12-14 18:48_

@charliermarsh Looks good to me now

---

_Merged by @charliermarsh on 2022-12-15 19:16_

---

_Closed by @charliermarsh on 2022-12-15 19:16_

---

_Branch deleted on 2023-02-28 18:07_

---
