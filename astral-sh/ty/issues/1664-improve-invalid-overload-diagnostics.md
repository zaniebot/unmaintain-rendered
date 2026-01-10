```yaml
number: 1664
title: "Improve `invalid-overload` diagnostics"
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-11-28T12:36:05Z
updated_at: 2025-11-28T12:41:17Z
url: https://github.com/astral-sh/ty/issues/1664
synced_at: 2026-01-10T01:58:59Z
```

# Improve `invalid-overload` diagnostics

---

_Issue opened by @AlexWaygood on 2025-11-28 12:36_

> I find the diagnostic range here confusing. I read the message like 4 times and was confused why ty would think that the definition of `f` on line 248 isn't the implementation when it clearly is. I think we should change the diagnostic range to underline the `@final` on line 243 because that's where the error really is and is also the code you have to change (move the `final` to line 247)

_Originally posted by @MichaReiser in https://github.com/astral-sh/ruff/pull/21646#discussion_r2570714029_
            

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-28 12:36_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-28 12:41_

---
