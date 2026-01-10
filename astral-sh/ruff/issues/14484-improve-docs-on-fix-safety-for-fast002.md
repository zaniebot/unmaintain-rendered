```yaml
number: 14484
title: Improve docs on fix safety for FAST002
type: issue
state: open
author: AlexWaygood
labels:
  - documentation
assignees: []
created_at: 2024-11-20T11:09:10Z
updated_at: 2024-11-20T11:45:05Z
url: https://github.com/astral-sh/ruff/issues/14484
synced_at: 2026-01-10T11:09:56Z
```

# Improve docs on fix safety for FAST002

---

_Issue opened by @AlexWaygood on 2024-11-20 11:09_

The fix for FAST002 is marked as always unsafe, but there isn't a `## Fix safety` section in the documentation explaining why it is marked as unsafe.

I can think of an obvious reason why the fix might be unsafe on Python 3.8 and lower: the fix adds imports of `typing_extensions` on Python 3.8 and lower, while the user's code might not have `typing_extensions` listed as a dependency. However, it's not immediately obvious to me (as a non-expert when it comes to `fastAPI`) what the exact reasons are for the fix being marked as unsafe on Python 3.9+.

It would be great to add better docs here. Cc. @TomerBin, who implemented the rule in https://github.com/astral-sh/ruff/pull/11579 and @charliermarsh, who merged it.

---

_Label `documentation` added by @AlexWaygood on 2024-11-20 11:09_

---

_Comment by @TomerBin on 2024-11-20 11:43_

At the time it was merged, we weren't handling defaults correctly. If I remember correctly, an improvement was deployed later so we need to check if it's still the case.
This is the relevant fix
https://github.com/astral-sh/ruff/pull/13133

---
