```yaml
number: 12811
title: "Warn on invalid `# noqa` rule codes"
type: pull_request
state: closed
author: charliermarsh
labels:
  - cli
  - suppression
assignees: []
base: main
head: charlie/warn
created_at: 2024-08-11T23:24:16Z
updated_at: 2025-03-07T10:30:50Z
url: https://github.com/astral-sh/ruff/pull/12811
synced_at: 2026-01-12T15:55:42Z
```

# Warn on invalid `# noqa` rule codes

---

_@charliermarsh_

## Summary

Today, we already warn if you do `# ruff: noqa: F401F841`... But not inline (e.g., `import os  # noqa: F401F841`).

This PR extends the warnings to the latter case.

It's not "breaking", but I'd suggest we ship it in a minor.

Fixes https://github.com/astral-sh/ruff/issues/15682


---

_Label `cli` added by @charliermarsh on 2024-08-11 23:24_

---

_Label `suppression` added by @charliermarsh on 2024-08-11 23:24_

---

_Renamed from "Warn on invalid # noqa rule codes" to "Warn on invalid `# noqa` rule codes" by @charliermarsh on 2024-08-11 23:27_

---

_@dhruvmanila approved on 2024-08-12 04:57_

> It's not "breaking", but I'd suggest we ship it in a minor.

Seems reasonable to me.

I think it might be useful for https://github.com/astral-sh/ruff/issues/12831 to be fixed as well.

---

_Added to milestone `v0.6` by @MichaReiser on 2024-08-12 07:14_

---

_@MichaReiser approved on 2024-08-12 07:16_

---

_Comment by @MichaReiser on 2024-08-12 09:27_

There's now a `ruff-0.6` feature branch into which you can merge this change.

---

_Comment by @MichaReiser on 2024-08-13 15:57_

@charliermarsh do you plan on merging this for the 0.6 release?

---

_Comment by @charliermarsh on 2024-08-13 18:36_

@MichaReiser - No, I should address Dhruv’s concern first but don’t have time immediately.

---

_Removed from milestone `v0.6` by @MichaReiser on 2024-08-16 10:01_

---

_Added to milestone `v0.7` by @MichaReiser on 2024-08-16 10:01_

---

_Removed from milestone `v0.7` by @AlexWaygood on 2024-10-17 15:18_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-10-17 15:18_

---

_Removed from milestone `v0.8` by @AlexWaygood on 2024-11-18 11:38_

---

_Added to milestone `v0.9` by @AlexWaygood on 2024-11-18 11:38_

---

_Removed from milestone `v0.9` by @MichaReiser on 2025-01-08 13:35_

---

_Added to milestone `v.0.10` by @MichaReiser on 2025-01-08 13:35_

---

_Comment by @MichaReiser on 2025-03-07 10:30_

Closing in favor of https://github.com/astral-sh/ruff/pull/16483

---

_Closed by @MichaReiser on 2025-03-07 10:30_

---
