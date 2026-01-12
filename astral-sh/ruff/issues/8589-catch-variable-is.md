```yaml
number: 8589
title: "Catch `variable is []`"
type: issue
state: closed
author: francorbacho
labels:
  - good first issue
  - rule
assignees: []
created_at: 2023-11-09T19:08:26Z
updated_at: 2023-11-11T15:15:31Z
url: https://github.com/astral-sh/ruff/issues/8589
synced_at: 2026-01-12T15:54:48Z
```

# Catch `variable is []`

---

_@francorbacho_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Make [F632](https://docs.astral.sh/ruff/rules/is-literal/) catch the following trap:

```python
variable = []
if variable is []:
  print("never runs")
```

Tested in Arch Linux:

```bash
$ pacman -Q ruff 
ruff 0.1.5-1
```

---

_Label `rule` added by @charliermarsh on 2023-11-09 19:09_

---

_Comment by @charliermarsh on 2023-11-09 19:09_

It seems reasonable to me.

---

_Label `good first issue` added by @zanieb on 2023-11-09 20:16_

---

_Comment by @francorbacho on 2023-11-10 10:28_

Although maybe it should be a different rule if we want to also test for `variable is [0, 1, 2]`.

---

_Comment by @charliermarsh on 2023-11-10 14:41_

Seems ok to fold it into the same rule for me, since that will also always be false, right?

---

_Comment by @jesse1412 on 2023-11-10 17:50_

Could this be expanded to cover any of the mutable initialisers? E.g set, dict, etc?

---

_Comment by @charliermarsh on 2023-11-11 15:15_

I believe this was closed by #8607 :) Thanks @jesse1412!

---

_Closed by @charliermarsh on 2023-11-11 15:15_

---
