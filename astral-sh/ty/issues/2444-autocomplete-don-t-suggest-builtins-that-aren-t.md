```yaml
number: 2444
title: "Autocomplete: don't suggest builtins that aren't supported in current python version"
type: issue
state: closed
author: AndBoyS
labels:
  - server
  - completions
assignees: []
created_at: 2026-01-11T12:03:11Z
updated_at: 2026-01-11T13:02:57Z
url: https://github.com/astral-sh/ty/issues/2444
synced_at: 2026-01-12T02:26:12Z
```

# Autocomplete: don't suggest builtins that aren't supported in current python version

---

_Issue opened by @AndBoyS on 2026-01-11 12:03_

### Summary

For example, in 3.10 ty suggests importing assert_never from typing module first, but there is not typing.assert_never in 3.10

### Version

0.0.11

---

_Comment by @MichaReiser on 2026-01-11 12:42_

Thank you. This is https://github.com/astral-sh/ty/issues/1754

---

_Closed by @MichaReiser on 2026-01-11 12:42_

---

_Label `server` added by @AlexWaygood on 2026-01-11 13:02_

---

_Label `completions` added by @AlexWaygood on 2026-01-11 13:02_

---
