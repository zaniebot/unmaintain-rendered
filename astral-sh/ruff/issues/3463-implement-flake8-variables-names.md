```yaml
number: 3463
title: Implement flake8-variables-names
type: issue
state: open
author: Cielquan
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-03-12T17:21:36Z
updated_at: 2025-10-08T04:46:08Z
url: https://github.com/astral-sh/ruff/issues/3463
synced_at: 2026-01-12T15:54:43Z
```

# Implement flake8-variables-names

---

_@Cielquan_

# [flake8-variables-names](https://pypi.org/project/flake8-variables-names/)

Forbids ambiguous variable names.

### Error Codes
* [ ] `VNE001` single letter variable names like 'XXX' are not allowed
* [ ] `VNE002` variable name 'XXX' should be clarified
* [x] `VNE003` variable names that shadow builtins are not allowed

VNE003 is already implemented via A001.



---

_Label `plugin` added by @charliermarsh on 2023-03-12 17:47_

---

_Comment by @latonis on 2023-03-17 02:39_

I can take a look at this one :eyes: 

---

_Comment by @latonis on 2023-03-17 13:52_

`VNE001` is marginally covered by Pycodestyle's E741, but it only looks for
 `name == "l" || name == "I" || name == "O"` which could be confused with other letters, so this would need to cover all all single letter use cases for `VNE001`. 

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---

_Comment by @v-goncharenko on 2025-10-08 01:36_

Such tool would be very useful, could you please solve this?

---
