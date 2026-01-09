---
number: 15416
title: "UP045 rule don't respect target-version after upgrage to 0.9.1 from 0.8.5"
type: issue
state: closed
author: 57an
labels:
  - question
assignees: []
created_at: 2025-01-11T07:54:54Z
updated_at: 2025-01-11T10:12:13Z
url: https://github.com/astral-sh/ruff/issues/15416
synced_at: 2026-01-07T13:12:16-06:00
---

# UP045 rule don't respect target-version after upgrage to 0.9.1 from 0.8.5

---

_Issue opened by @57an on 2025-01-11 07:54_

I've upgraded from 0.8.5 to 0.9.1.
When check python 3.8 code with pyproject.toml field tool.ruff.target-version = 'py38' now shows invalid errors from UP045:

```
C:\Python38\Scripts\ruff.exe check D:\py\core --fix --preview
source\analyzer\backend\analysis_card\card.py:120:10: UP045 Use `X | None` for type annotations
    |
118 |     async def calculate(
119 |         self, storage, progress_bar: AnalysisProgressBar, level=AnalysisPullCalculationLevel.condition
120 |     ) -> Optional[CardData]:
    |          ^^^^^^^^^^^^^^^^^^ UP045
121 |         if self._status == AnalysisCardStatus.calculations_ended:
122 |             return
    |
    = help: Convert to `X | None`
```

same behaviour on project.requires-python = '==3.8'

---

_Comment by @MichaReiser on 2025-01-11 08:57_

Thanks for reporting this. 

That's interesting. Both rules (`UP007` and `UP045` use the exact same check). Any chance that your file contains a `from __future__ import annotations`? 


https://github.com/astral-sh/ruff/blob/8c620b9b4b69e228347ca6e882c46f4c37ebff43/crates/ruff_linter/src/checkers/ast/analyze/expression.rs#L51-L59

---

_Label `question` added by @MichaReiser on 2025-01-11 08:58_

---

_Comment by @57an on 2025-01-11 10:02_

It looks like you are right, I do use __annotations__ in such places. 
I studied changelog and now see that in ruff 0.9.0 you split UP007 with UP045. UP007 is disabled in my project, so UP045 I've just disabled too. 

Thanks!



---

_Closed by @57an on 2025-01-11 10:02_

---
