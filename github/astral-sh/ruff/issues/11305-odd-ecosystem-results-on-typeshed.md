---
number: 11305
title: Odd ecosystem results on typeshed
type: issue
state: open
author: AlexWaygood
labels:
  - help wanted
  - ci
assignees: []
created_at: 2024-05-06T09:15:42Z
updated_at: 2024-08-13T17:32:08Z
url: https://github.com/astral-sh/ruff/issues/11305
synced_at: 2026-01-07T13:12:15-06:00
---

# Odd ecosystem results on typeshed

---

_Issue opened by @AlexWaygood on 2024-05-06 09:15_

Seems like there's something up with the ecosystem check when it runs on typeshed currently. We've had issues on several PRs where the ecosystem check has produced surprising results that don't make logical sense and can't be reproduced locally:
- https://github.com/astral-sh/ruff/pull/11128#issuecomment-2075020838
- https://github.com/astral-sh/ruff/pull/11269#issuecomment-2093776126
- https://github.com/astral-sh/ruff/pull/10779#issuecomment-2038209241

No idea what's up here

---

_Label `ci` added by @AlexWaygood on 2024-05-06 09:15_

---

_Referenced in [astral-sh/ruff#11269](../../astral-sh/ruff/pulls/11269.md) on 2024-05-06 09:16_

---

_Comment by @MichaReiser on 2024-05-06 09:24_

Would you mind listing some of the "odd" examples and explaining why they're odd?

---

_Comment by @AlexWaygood on 2024-05-06 09:26_

> Would you mind listing some of the "odd" examples and explaining why they're odd?

In all three comments I linked to above, the ecosystem check either surfaced new lint errors that could not be reproduced locally when running ruff with the PR branch, and/or failed to report errors going away even though locally running ruff with the PR branch led to existing lint errors going away

---

_Referenced in [astral-sh/ruff#12860](../../astral-sh/ruff/pulls/12860.md) on 2024-08-13 14:15_

---

_Label `help wanted` added by @AlexWaygood on 2024-08-13 17:32_

---

_Referenced in [astral-sh/ruff#14273](../../astral-sh/ruff/pulls/14273.md) on 2024-11-13 11:28_

---
