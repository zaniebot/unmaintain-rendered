---
number: 12750
title: "Incorrect error: Camelcase `SimpleITK` imported as lowercase `sitk` Ruff N813"
type: issue
state: closed
author: MattTheCuber
labels:
  - configuration
assignees: []
created_at: 2024-08-08T12:30:04Z
updated_at: 2024-08-09T15:18:27Z
url: https://github.com/astral-sh/ruff/issues/12750
synced_at: 2026-01-07T13:12:15-06:00
---

# Incorrect error: Camelcase `SimpleITK` imported as lowercase `sitk` Ruff N813

---

_Issue opened by @MattTheCuber on 2024-08-08 12:30_

Example code:
```python
import SimpleITK as sitk
```

Enable the Ruff rule `N813` shows an error for this line. It is correct, however this is the standard and recommended import method according to the [official docs](https://simpleitk.org/), similar to numpy and pandas. I found no issues mentioning SimpleITK.

---

_Comment by @charliermarsh on 2024-08-09 01:25_

Thanks for filing. I'm hesitant to add library-specific exceptions here. I think my advice would be to add `SimpleITK` to [`ignore-names`](https://docs.astral.sh/ruff/settings/#lint_pep8-naming_ignore-names):

```toml
[tool.ruff.lint.pep8-naming]
ignore-names = ["SimpleITK"]
```


---

_Label `configuration` added by @charliermarsh on 2024-08-09 01:26_

---

_Closed by @charliermarsh on 2024-08-09 01:26_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-09 01:26_

---

_Comment by @MattTheCuber on 2024-08-09 15:16_

> Thanks for filing. I'm hesitant to add library-specific exceptions here. I think my advice would be to add `SimpleITK` to [`ignore-names`](https://docs.astral.sh/ruff/settings/#lint_flake8-self_ignore-names):
> 
> ```toml
> [tool.ruff.lint.flake8-self]
> ignore-names = ["SimpleITK"]
> ```

This didn't quite work, but I found this worked:

```toml
[tool.ruff.lint.pep8-naming]
ignore-names = ["SimpleITK"]
```

---

_Comment by @charliermarsh on 2024-08-09 15:18_

Gah sorry, I linked to the wrong setting.

---

_Comment by @charliermarsh on 2024-08-09 15:18_

Edited my comment to reflect the correct version.

---
