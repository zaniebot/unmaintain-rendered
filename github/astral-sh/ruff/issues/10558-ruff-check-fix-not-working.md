---
number: 10558
title: ruff check --fix not working
type: issue
state: closed
author: decoded-brain
labels:
  - question
assignees: []
created_at: 2024-03-25T08:21:05Z
updated_at: 2024-03-25T08:34:55Z
url: https://github.com/astral-sh/ruff/issues/10558
synced_at: 2026-01-07T13:12:15-06:00
---

# ruff check --fix not working

---

_Issue opened by @decoded-brain on 2024-03-25 08:21_

Ruff doesn't fix the problems. I've tried adding the ```--unsafe-fixes``` flag and it doesn't work

```bash
transactions_journal/models.py:185:7: DJ008 Model does not define `__str__` method
transactions_journal/models.py:187:17: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:188:15: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:189:12: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:190:14: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:205:21: DJ001 Avoid using `null=True` on string-based fields such as `TextField`
transactions_journal/models.py:298:12: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:299:12: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:305:14: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:312:14: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:313:18: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:314:18: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:315:15: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:316:18: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:317:31: DJ001 Avoid using `null=True` on string-based fields such as `TextField`
transactions_journal/models.py:318:15: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:319:13: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:321:15: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:322:13: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:323:18: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:324:19: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:382:12: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:383:15: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:388:20: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:389:21: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:395:26: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:397:19: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:398:21: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
transactions_journal/models.py:403:22: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
units/models.py:510:14: DJ001 Avoid using `null=True` on string-based fields such as `CharField`
vesting_bonuses/models.py:19:19: DJ001 Avoid using `null=True` on string-based fields such as `TextField`
vesting_bonuses/models.py:30:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before `__str__` method
vesting_bonuses/models.py:49:5: DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `Meta` class should come before `__str__` method
Found 562 errors.
```

* My ```ruff.toml```

```toml
# Exclude a variety of commonly ignored directories.
exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".git-rewrite",
    ".hg",
    ".ipynb_checkpoints",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pyenv",
    ".pytest_cache",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    ".vscode",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "site-packages",
    "venv",
    "apps.py",
]

[lint]
ignore = ["F401"]
select = ["T20", "G", "DJ", "COM", "PD"]
fixable = ["ALL"]
unfixable = []
```

---

_Comment by @MichaReiser on 2024-03-25 08:30_

Hy @JohnnyMagnet 

Not all lint rules have automatic fixes in Ruff. I checked `DJ001` and `DJ012` and both have no automatic fix implementation. So the reason why ruff isn't fixing anything in this case is because it doesn't know how. 

Ruff shows you when there's an automatic fix by printing:

```
Found 67 errors.
[*] 3 fixable with the `--fix` option.
```





---

_Label `question` added by @MichaReiser on 2024-03-25 08:30_

---

_Comment by @decoded-brain on 2024-03-25 08:33_

@MichaReiser thx!)

---

_Comment by @MichaReiser on 2024-03-25 08:34_

You're welcome. I'll close this issue. If you have another problem, feel free to comment on this issue or open a new one. 

---

_Closed by @MichaReiser on 2024-03-25 08:34_

---
