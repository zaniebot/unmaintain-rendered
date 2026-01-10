```yaml
number: 18902
title: "EXE003 and `uvx`or `uv tool run`"
type: issue
state: closed
author: jretz
labels:
  - rule
assignees: []
created_at: 2025-06-23T19:06:20Z
updated_at: 2025-06-30T13:38:19Z
url: https://github.com/astral-sh/ruff/issues/18902
synced_at: 2026-01-10T11:09:59Z
```

# EXE003 and `uvx`or `uv tool run`

---

_Issue opened by @jretz on 2025-06-23 19:06_

### Summary

Just as #13021 added support to EXE003 for `uv run` in shebang lines, it would be nice to add `uvx` and/or `uv tool run`. This is useful for tools like marimo:

- `#!/usr/bin/env -S uvx marimo edit --sandbox`

  or

- `#!/usr/bin/env -S uv tool run marimo edit --sandbox`

For those unfamiliar with marimo: https://docs.astral.sh/uv/guides/integration/marimo/

### Version

_No response_

---

_Label `rule` added by @ntBre on 2025-06-23 19:10_

---

_Label `needs-decision` added by @ntBre on 2025-06-23 19:10_

---

_Comment by @MichaReiser on 2025-06-26 06:59_

This makes sense to me

---

_Label `needs-decision` removed by @MichaReiser on 2025-06-26 06:59_

---

_Comment by @CodeMan62 on 2025-06-26 18:01_

I have took refrence of #16849 and made a PR 

---

_Closed by @ntBre on 2025-06-30 13:38_

---
