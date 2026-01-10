```yaml
number: 5333
title: Empty ruleset table in documenation when all rules are in the nursery section
type: issue
state: closed
author: tjkuson
labels:
  - documentation
assignees: []
created_at: 2023-06-23T14:51:27Z
updated_at: 2023-07-10T08:14:17Z
url: https://github.com/astral-sh/ruff/issues/5333
synced_at: 2026-01-10T11:09:47Z
```

# Empty ruleset table in documenation when all rules are in the nursery section

---

_Issue opened by @tjkuson on 2023-06-23 14:51_

Visit [the documentation](https://beta.ruff.rs/docs/rules/#copyright-related-rules-cpy) online (version `0.0.275`) and you will see an empty table where the rule `missing-copyright-notice` should be. I think something broke when the rule was moved to the `flake8-copyright` crate.

---

_Renamed from "Copyright documentation does not appear in latest build" to "`missing-copyright-notice` documentation does not appear in latest build" by @tjkuson on 2023-06-23 14:54_

---

_Comment by @charliermarsh on 2023-06-23 18:44_

It's probably because I moved it to the "nursery" section.

---

_Label `documentation` added by @charliermarsh on 2023-06-23 18:44_

---

_Comment by @tjkuson on 2023-06-23 21:40_

Makes sense, the empty table made it look like an error.


---

_Renamed from "`missing-copyright-notice` documentation does not appear in latest build" to "Empty ruleset table in documengari" by @tjkuson on 2023-06-23 21:42_

---

_Renamed from "Empty ruleset table in documengari" to "Empty ruleset table in documenation when all rules are in the nursery section" by @tjkuson on 2023-06-23 21:42_

---

_Comment by @charliermarsh on 2023-06-23 22:05_

I think we should add the nursery rules to the docs, and add a column to indicate that they’re nursery (like with autofix).

---

_Comment by @charliermarsh on 2023-06-23 22:05_

(Right now it looks like an error, and there’s no way to discover the nursery rules.)

---

_Comment by @tjkuson on 2023-07-10 08:14_

Closed by #5439.

---

_Closed by @tjkuson on 2023-07-10 08:14_

---
