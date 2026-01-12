```yaml
number: 2425
title: "Support `flake8-class-attributes-order`"
type: issue
state: open
author: edgarrmondragon
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-01-31T23:02:30Z
updated_at: 2024-10-07T21:24:53Z
url: https://github.com/astral-sh/ruff/issues/2425
synced_at: 2026-01-12T15:54:42Z
```

# Support `flake8-class-attributes-order`

---

_@edgarrmondragon_

Hello, 
Is there a plan to support this flake8 extension?
https://pypi.org/project/flake8-class-attributes-order/

_Originally posted by @WorkHardes in https://github.com/charliermarsh/ruff/issues/106#issuecomment-1347928865_

- [ ] `CCE001`: Wrong class attributes order (`XXX should be after YYY`)
- [ ] `CCE002`: Class level expression detected


---

_Renamed from "Hello," to "Support `flake8-class-attributes-order`" by @edgarrmondragon on 2023-01-31 23:02_

---

_Label `plugin` added by @charliermarsh on 2023-01-31 23:06_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---

_Comment by @sigma67 on 2023-11-06 14:45_

Has there been a decision to include this yet? Seems quite a few people would use it

---

_Comment by @lexygon on 2024-01-30 14:41_

Any decisions on this? It would be an awesome addition

---

_Comment by @zanieb on 2024-03-11 17:06_

This one's hard to implement and can generally be unsafe. We don't have plans to implement it soon. Now that we unsafe fix support it _might_ be more reasonable.

cc @AlexWaygood this kind of thing may be worth considering in categorization.

Related https://github.com/astral-sh/ruff/issues/3946



---

_Comment by @sigma67 on 2024-03-11 17:23_

I think auto-fixing isn't even the feature request here - although certainly a nice bonus.

Just having the detection capability would be great.

Does your argument only apply to the fixing part or do you also consider the detection hard to implement?

---

_Comment by @yury-fedotov on 2024-05-31 19:46_

+1 to this, would be awesome to have it! I think almost any Python project defines many classes, hence having a consistent order of attributes is critical.

---

_Comment by @mukul-mpac on 2024-10-07 21:24_

+1, even having the capability to detect inconsistencies without having an auto-fix as such would be great!

---
