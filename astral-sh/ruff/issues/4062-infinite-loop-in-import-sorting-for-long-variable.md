```yaml
number: 4062
title: "Infinite loop in import sorting for long variable names with force-single-line (ruff >= 0.0.256)"
type: issue
state: closed
author: pstjohn
labels:
  - bug
  - isort
assignees: []
created_at: 2023-04-21T20:15:19Z
updated_at: 2023-04-24T04:39:53Z
url: https://github.com/astral-sh/ruff/issues/4062
synced_at: 2026-01-12T15:54:44Z
```

# Infinite loop in import sorting for long variable names with force-single-line (ruff >= 0.0.256)

---

_@pstjohn_

The following code snippet causes `ruff` to hang for ruff >= 0.0.256.

pyproject.toml
```toml
[tool.ruff]
select = ["I"]
fix = true
line-length = 88

[tool.ruff.isort]
force-single-line = true
```

test.py
```python
from mypackage.subpackage2 import (  # long comment that seems to be a problem
    a_long_variable_name_that_causes_problems,
    item2,
)
```

Doing some bisection, it works in 0.0.255 and hangs in 0.0.256 onwards.
Probably related to #3530? But it happens regardless of the `combine-as-imports` setting

In the newer versions, it seems to get stuck in an infinite loop
```
[2023-04-21][20:08:42][ruff_cli::commands::run][DEBUG] Identified files to lint in: 521.625µs
[2023-04-21][20:08:42][ruff_cli::diagnostics][DEBUG] Checking: /Users/pstjohn/Misc/ruff_issue/test.py
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:08:42][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
```

The older versions run through twice
```
[2023-04-21][20:15:05][ruff_cli::commands::run][DEBUG] Identified files to lint in: 381.25µs
[2023-04-21][20:15:05][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:15:05][ruff::rules::isort::categorize][DEBUG] Categorized 'mypackage' as ThirdParty (NoMatch)
[2023-04-21][20:15:05][ruff_cli::commands::run][DEBUG] Checked 1 files in: 1.910292ms
Found 1 error (1 fixed, 0 remaining).
```

resulting in this formatted import
```python
from mypackage.subpackage2 import (  # long comment that seems to be a problem
    a_long_variable_name_that_causes_problems,
)
from mypackage.subpackage2 import item2
```


---

_Renamed from "Ruff hangs when sorting imports with long variable names / comments" to "Infinite loop in for long variable names with force-single-line in ruff >= 0.0.256" by @pstjohn on 2023-04-22 02:19_

---

_Renamed from "Infinite loop in for long variable names with force-single-line in ruff >= 0.0.256" to "Infinite loop in import sorting for long variable names with force-single-line in ruff >= 0.0.256" by @pstjohn on 2023-04-22 02:19_

---

_Renamed from "Infinite loop in import sorting for long variable names with force-single-line in ruff >= 0.0.256" to "Infinite loop in import sorting for long variable names with force-single-line (ruff >= 0.0.256)" by @pstjohn on 2023-04-22 02:19_

---

_Label `bug` added by @charliermarsh on 2023-04-22 05:48_

---

_Label `isort` added by @charliermarsh on 2023-04-22 05:48_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-04-23 05:14_

---

_Comment by @charliermarsh on 2023-04-23 05:15_

Thanks for the clear issue, taking a look :)

---

_Comment by @charliermarsh on 2023-04-23 05:46_

I've identified the issue (introduced in https://github.com/charliermarsh/ruff/pull/3521), which is that we're duplicating the comment with an exponential blowup. Figuring out a proper fix, if I can't fix it properly I'll revert that change prior to the next release.

---

_Comment by @pstjohn on 2023-04-23 20:01_

You're welcome, thanks for the quick reply (and great package)!

---

_Closed by @charliermarsh on 2023-04-24 04:39_

---
