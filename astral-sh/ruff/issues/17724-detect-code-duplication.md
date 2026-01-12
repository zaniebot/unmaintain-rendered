```yaml
number: 17724
title: detect code duplication
type: issue
state: open
author: ctcjab
labels:
  - rule
  - wish
assignees: []
created_at: 2025-04-29T21:04:15Z
updated_at: 2025-09-10T14:22:45Z
url: https://github.com/astral-sh/ruff/issues/17724
synced_at: 2026-01-12T15:54:56Z
```

# detect code duplication

---

_@ctcjab_

### Summary

Detecting excessive use of copy/paste would be a valuable starting point in and of itself. Detecting less basic code duplication ("refactoring required") cases would be even cooler.

Prior art:
* https://pylint.readthedocs.io/en/stable/user_guide/messages/refactor/duplicate-code.html
  * https://github.com/pylint-dev/pylint/blob/main/pylint/checkers/symilar.py

---

_Label `rule` added by @ntBre on 2025-04-29 21:32_

---

_Comment by @MichaReiser on 2025-04-30 06:21_

Related to https://github.com/astral-sh/ruff/issues/10522

---

_Label `wish` added by @MichaReiser on 2025-04-30 06:22_

---

_Comment by @Spenhouet on 2025-09-10 14:22_

Not having tested any of these projects but still sharing for reference:
- https://github.com/blingenf/copydetect
- https://github.com/platisd/duplicate-code-detection-tool
- https://github.com/fyrestone/pycode_similar
- https://github.com/saltudelft/CD4Py
- https://github.com/otzhora/potator
- https://github.com/Talk2TheHand/CodeSimilarityChecker
- https://github.com/MalloryAnn/code-clone-detection-tool
- https://github.com/swharden/duplicate-code-finder

Does anyone have experience with any of these or any other? Looking for recommendations.

---
