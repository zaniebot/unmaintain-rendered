```yaml
number: 18740
title: ASYNC115 fix deletes comments
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-18T01:47:17Z
updated_at: 2025-06-19T11:01:35Z
url: https://github.com/astral-sh/ruff/issues/18740
synced_at: 2026-01-12T15:54:56Z
```

# ASYNC115 fix deletes comments

---

_@MeGaGiGaGon_

### Summary

Applying the ASYNC115 fix can delete comments inside the function args, so should be marked unsafe in that case [playground](https://play.ruff.rs/2899be0f-6552-4f6e-8b6d-5fb2c92bcad1)
```
PS ~\Desktop\New_folder>Set-Content .\issue.py @"
>> import trio
>>
>>
>> async def func():
>>     await ( # 1
>>     trio # 2
>>     # 3
>>     .sleep( # 4
>>     # 5
>>     0 # 6
>>     # 7
>>     ) # 8
>>     ) # 9
>> "@
PS ~\Desktop\New_folder>uvx ruff check --select ASYNC --fix issue.py
Found 1 error (1 fixed, 0 remaining).
PS ~\Desktop\New_folder>Get-Content issue.py
import trio


async def func():
    await ( # 1
    trio.lowlevel.checkpoint() # 8
    ) # 9
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Label `bug` added by @ntBre on 2025-06-18 03:06_

---

_Label `fixes` added by @ntBre on 2025-06-18 03:06_

---

_Label `help wanted` added by @ntBre on 2025-06-18 03:06_

---

_Closed by @MichaReiser on 2025-06-19 11:01_

---
