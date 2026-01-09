---
number: 19394
title: N812 and ICN001 conflict
type: issue
state: open
author: guillaume-alliander
labels:
  - rule
assignees: []
created_at: 2025-07-17T08:33:56Z
updated_at: 2025-09-30T07:14:50Z
url: https://github.com/astral-sh/ruff/issues/19394
synced_at: 2026-01-07T13:12:16-06:00
---

# N812 and ICN001 conflict

---

_Issue opened by @guillaume-alliander on 2025-07-17 08:33_

### Summary

The ruff command I will use everywhere (rb.py being the file with the test code) is `uvx ruff --isolated  check rb.py --select N812,ICN001  --config 'lint.flake8-import-conventions.extend-aliases={"lightning"="L"}'`

```python
import lightning as l
```
Ruff says:  ICN001 `lightning` should be imported as `L`

```python
import lightning as L
```
Ruff says:   N812 Lowercase `lightning` imported as non-lowercase `L`

See [playground](https://play.ruff.rs/90e4fb40-61b5-4797-af01-3027a30f7dfe).

### Version

ruff 0.12.3 (5bc81f26c 2025-07-11)

---

_Comment by @ntBre on 2025-07-17 13:52_

I knew this sounded familiar! I think this is a more general version of #18310. I think you should be able to work around this by also configuring [ignore-names](https://docs.astral.sh/ruff/settings/#lint_pep8-naming_ignore-names), but there are some other ideas about a possible fix in the other thread.

---

_Label `question` added by @ntBre on 2025-07-17 13:52_

---

_Comment by @guillaume-alliander on 2025-07-18 07:42_

It is very similar indeed. The solution suggested on https://github.com/astral-sh/ruff/issues/18310#issuecomment-2923073287 looks like it would solve my particular issue as well :

> Maybe the most reasonable way to do this is to just say that anything defined in import-conventions.aliases automatically passes N812. If a name is the standard, as defined by import-conventions.aliases, then rules should just accept that that's an allowed name.

Although I would go one step further: anything explicitly defined in in import-conventions.aliases / import-convention.extend-aliases should pass all N rules.

In the meantime, thanks for the workaround :) 

---

_Label `question` removed by @MichaReiser on 2025-09-30 07:14_

---

_Label `rule` added by @MichaReiser on 2025-09-30 07:14_

---
