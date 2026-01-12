```yaml
number: 3579
title: Add flake8-copyright support
type: issue
state: closed
author: Ryang20718
labels:
  - plugin
assignees: []
created_at: 2023-03-17T16:33:37Z
updated_at: 2023-06-11T02:18:00Z
url: https://github.com/astral-sh/ruff/issues/3579
synced_at: 2026-01-12T15:54:43Z
```

# Add flake8-copyright support

---

_@Ryang20718_

Hi, would it be possible to add flake8-copyright to ruff please?

---

_Label `plugin` added by @charliermarsh on 2023-03-17 17:14_

---

_Comment by @akx on 2023-03-28 12:27_

I wonder if this should be a more generic "require a header comment" rule?

---

_Comment by @Paul-AUB on 2023-04-26 09:22_

Would be great ! 

Some additional information :
- repo : https://github.com/savoirfairelinux/flake8-copyright
- pypi : https://pypi.org/project/flake8-copyright/
- latest version released in 2023 (`0.2.4`), so still supported
- only code seems to be `C801` (cf [here](https://github.com/savoirfairelinux/flake8-copyright/blob/master/flake8_copyright.py#L27)), so could be `C80` in `ruff` without conflicting with `mccbe` which is `C90`

---

_Closed by @charliermarsh on 2023-06-11 02:18_

---
