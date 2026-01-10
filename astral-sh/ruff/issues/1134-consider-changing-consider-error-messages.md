---
number: 1134
title: "Consider changing \"consider\" error messages"
type: issue
state: closed
author: smackesey
labels: []
assignees: []
created_at: 2022-12-08T00:19:13Z
updated_at: 2022-12-08T01:10:38Z
url: https://github.com/astral-sh/ruff/issues/1134
synced_at: 2026-01-10T01:22:38Z
---

# Consider changing "consider" error messages

---

_Issue opened by @smackesey on 2022-12-08 00:19_

Something about this doesn't feel right:

```
ruff --fix --extend-ignore=F401 .
Found 2 error(s).
python_modules/automation-internal/automation_internal/cloud/check/old_images.py:79:9: PLR1722 Consider using `sys.exit()`
python_modules/automation-internal/automation_internal/infra/terragrunt.py:318:5: PLR1722 Consider using `sys.exit()`
2 potentially fixable with the --fix option.
```

Why am I only supposed to "consider" fixing these but not all the other issues Ruff flags? Ruff exits with error code 1 if there are errors-- most users will use ruff in a context where they try to get 0, for which you have to fix or ignore these (same as for other errors). So IMO it should just say: `use sys.exit()`.

---

_Comment by @charliermarsh on 2022-12-08 00:22_

Hah, I think we took this from [Pylint](https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/consider-using-sys-exit.html), but yeah, agree.

---

_Referenced in [astral-sh/ruff#1135](../../astral-sh/ruff/pulls/1135.md) on 2022-12-08 01:10_

---

_Closed by @charliermarsh on 2022-12-08 01:10_

---
