```yaml
number: 1040
title: "virtual environment discovery doesn't work when using `--project`"
type: issue
state: closed
author: MichaReiser
labels:
  - cli
assignees: []
created_at: 2025-08-18T10:44:42Z
updated_at: 2025-08-18T10:49:30Z
url: https://github.com/astral-sh/ty/issues/1040
synced_at: 2026-01-12T15:54:24Z
```

# virtual environment discovery doesn't work when using `--project`

---

_@MichaReiser_

### Summary

It's open up to debate if this is the intended behavior but I found it surprising. 

`--project <dir>` runs ty in `<dir>` but without changing the current directory. However, I noticed that ty isn't picking up the virtual environment in `<dir>`, instead, it searches for a virtual environment in `<cwd>`. I'd have to double check what uv does when using `--project` but I would expect `--project` to behave as if I'd ran `ty` in `<dir>`.

### Version

_No response_

---

_Label `cli` added by @MichaReiser on 2025-08-18 10:44_

---

_Comment by @MichaReiser on 2025-08-18 10:49_

Never mind, RustRover activated a virtual environment in my shell.

---

_Closed by @MichaReiser on 2025-08-18 10:49_

---
