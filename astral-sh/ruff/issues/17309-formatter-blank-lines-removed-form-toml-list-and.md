---
number: 17309
title: Formatter - Blank lines removed form toml list, and could not tur it off
type: issue
state: closed
author: GeorgeFischhof
labels:
  - question
assignees: []
created_at: 2025-04-09T10:43:36Z
updated_at: 2025-04-09T13:51:30Z
url: https://github.com/astral-sh/ruff/issues/17309
synced_at: 2026-01-10T01:22:58Z
---

# Formatter - Blank lines removed form toml list, and could not tur it off

---

_Issue opened by @GeorgeFischhof on 2025-04-09 10:43_

### Summary

Hi,

in my pyproject.toml I have pytest config, added markers into a list.
The list contained blank lines to separate the markers into groups. Pycharm set to use Ruff to format


relevant part of toml file:

```
[tool.pytest.ini_options]
# some ini options

addopts = "some_options"
markers = [
    # try out the test
    "george",

    # test execution type
    "api",
    "ui",

    # speed and test type related
    "slow: will run only with --runslow parameter",
]
```

ruff formatter configuration contains only line-length and exclude for hidden folders
I tried `# fmt: off` , `# fmt: skip` on many places but it did not help. 

Ruff removed the blank lines from my marks list. (Black did not do this, I did not find this behavior in ruff - black deviations part)



versions:
ruff: ruff 0.11.4 (95d6ed40c 2025-04-04)
pycharm: PyCharm 2024.3.5 (Community Edition)  Build #PC-243.26053.29, built on March 17, 2025
ruff pycharm plugin: 0.0.47
Windows 11.0


BR,
George

### Version

ruff 0.11.4 (95d6ed40c 2025-04-04)

---

_Comment by @MichaReiser on 2025-04-09 10:53_

Ruff doesn't format toml files. This must be some other formatter (the pycharm default)?

---

_Label `question` added by @MichaReiser on 2025-04-09 10:53_

---

_Comment by @GeorgeFischhof on 2025-04-09 13:51_

Ok, thanks: I disabled PyCharm's formatting for *.toml and it solved :)


---

_Closed by @GeorgeFischhof on 2025-04-09 13:51_

---
