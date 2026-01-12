```yaml
number: 18741
title: B004 fix deletes comments
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-18T01:54:36Z
updated_at: 2025-06-19T10:46:55Z
url: https://github.com/astral-sh/ruff/issues/18741
synced_at: 2026-01-12T15:54:56Z
```

# B004 fix deletes comments

---

_@MeGaGiGaGon_

### Summary

B004 deletes comments inside the `hasattr` call, so should be marked unsafe in that case [playground](https://play.ruff.rs/a55ba38f-091a-46ed-9274-ad6da6b7ec1f)
```
PS ~\Desktop\New_folder>Set-Content .\issue.py @"
>> hasattr(
>> # 1
>> obj, # 2
>> # 3
>> "__call__" # 4
>> # 5
>> )
>> "@
PS ~\Desktop\New_folder>uvx ruff check --select B --fix issue.py
Found 1 error (1 fixed, 0 remaining).
PS ~\Desktop\New_folder>Get-Content issue.py
callable(obj)
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Label `bug` added by @ntBre on 2025-06-18 03:05_

---

_Label `fixes` added by @ntBre on 2025-06-18 03:05_

---

_Label `help wanted` added by @ntBre on 2025-06-18 03:05_

---

_Closed by @MichaReiser on 2025-06-19 10:46_

---
