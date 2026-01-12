```yaml
number: 804
title: Implement autofix for E713 and E714
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: autofix-E713-E714
created_at: 2022-11-18T14:53:58Z
updated_at: 2022-11-18T17:16:12Z
url: https://github.com/astral-sh/ruff/pull/804
synced_at: 2026-01-12T05:48:45Z
```

# Implement autofix for E713 and E714

---

_Pull request opened by @harupy on 2022-11-18 14:53_

#722 

---

_@harupy reviewed on 2022-11-18 14:56_

---

_Review comment by @harupy on `src/pycodestyle/plugins.rs`:222 on 2022-11-18 14:56_

Should we fix `not (x in y in z)` (= `not (x in y and y in z)`) to `x not in z or y not in z`?

---

_Review comment by @charliermarsh on `src/pycodestyle/plugins.rs`:222 on 2022-11-18 15:01_

I think it's fine to omit those. They're less in the spirit of the rule.

---

_@charliermarsh reviewed on 2022-11-18 15:01_

---

_@charliermarsh reviewed on 2022-11-18 15:04_

---

_Review comment by @charliermarsh on `src/pycodestyle/plugins.rs`:222 on 2022-11-18 15:04_

It looks like Flake8 doesn't even flag those as errors.

---

_@harupy reviewed on 2022-11-18 15:10_

---

_Review comment by @harupy on `src/pycodestyle/plugins.rs`:222 on 2022-11-18 15:10_

Wow interesting.

```
> cat resources/test/fixtures/a.py
if not (a in b in c):
    pass
> flake8 resources/test/fixtures/a.py --select E713
```

---

_@harupy reviewed on 2022-11-18 15:12_

---

_Review comment by @harupy on `src/pycodestyle/plugins.rs`:222 on 2022-11-18 15:12_

```
> cat resources/test/fixtures/a.py
if not a in b in c:
    pass
> flake8 resources/test/fixtures/a.py --select E713
resources/test/fixtures/E713.py:1:4: E713 test for membership should be 'not in'
```

---

_@charliermarsh reviewed on 2022-11-18 15:12_

---

_Review comment by @charliermarsh on `src/pycodestyle/plugins.rs`:222 on 2022-11-18 15:12_

Haha :)

---

_Merged by @charliermarsh on 2022-11-18 17:16_

---

_Closed by @charliermarsh on 2022-11-18 17:16_

---
