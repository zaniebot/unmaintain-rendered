```yaml
number: 1041
title: "CLI clears `-v` logs when running in watch mode"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - cli
assignees: []
created_at: 2025-08-18T10:47:59Z
updated_at: 2026-01-09T09:50:06Z
url: https://github.com/astral-sh/ty/issues/1041
synced_at: 2026-01-12T15:54:24Z
```

# CLI clears `-v` logs when running in watch mode

---

_@MichaReiser_

### Summary

You can see the tracing logs for a split second when running `ty check -v --watch` before they get cleared. It would be nice if we could preserve the tracing logs and only clear the screen between iterations (so that the logs of the current iteration are visible)

### Version

_No response_

---

_Label `bug` added by @MichaReiser on 2025-08-18 10:48_

---

_Label `cli` added by @MichaReiser on 2025-08-18 10:48_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:59_

---

_Comment by @MichaReiser on 2026-01-09 09:50_

I fixed this at some point because it made it easier to investigate a bug but I forgot to close this issue (I didn't remember that it existed)

---

_Closed by @MichaReiser on 2026-01-09 09:50_

---
