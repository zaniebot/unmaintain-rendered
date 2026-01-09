---
number: 15902
title: Allow common numbers for PLR2004
type: issue
state: closed
author: spacemanspiff2007
labels: []
assignees: []
created_at: 2025-02-03T05:50:24Z
updated_at: 2025-02-03T09:55:10Z
url: https://github.com/astral-sh/ruff/issues/15902
synced_at: 2026-01-07T13:12:16-06:00
---

# Allow common numbers for PLR2004

---

_Issue opened by @spacemanspiff2007 on 2025-02-03 05:50_

### Description

When dealing with bytes it's common do divide by 1024 to get KiByte, MiByte, etc..
Thus this should not trigger PLR2004:

````python
b: bytes = b'asdf'
if len(b) > 1024:
    print('More than 1 KiB')
````

````
ruff.py:3:8: PLR2004 Magic value used in comparison, consider replacing `1024` with a constant variable
  |
2 | b: bytes = b'asdf'
3 | if len(b) > 1024:
  |             ^^^^ PLR2004
4 |     print('More than 1 KiB')
  |
````

---

_Closed by @MichaReiser on 2025-02-03 09:55_

---
