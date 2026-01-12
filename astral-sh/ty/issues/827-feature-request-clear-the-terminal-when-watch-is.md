```yaml
number: 827
title: "Feature request: clear the terminal when --watch is used."
type: issue
state: closed
author: borissmidt
labels:
  - cli
assignees: []
created_at: 2025-07-15T13:44:15Z
updated_at: 2025-08-04T11:45:39Z
url: https://github.com/astral-sh/ty/issues/827
synced_at: 2026-01-12T15:54:24Z
```

# Feature request: clear the terminal when --watch is used.

---

_@borissmidt_

Because `ty --watch` keeps on appending to the current terminal it can be a bit confusing to read its output.
Its also hard to find the start of the output of the latest run.

With `watchexec`, which i assume ty is using behind the scenes, you can set `--clear` which clears the terminal.
Then all you see in the terminal is the latest data of a run making it easier to see which issues are still present.

---

_Comment by @MichaReiser on 2025-07-15 14:01_

That makes sense

> With watchexec, which i assume ty is using behind the scenes, you can set --clear which clears the terminal.

ty uses custom watcher because it allows us to do more fine grained updates.

---

_Label `cli` added by @MichaReiser on 2025-07-15 14:01_

---

_Added to milestone `GA` by @MichaReiser on 2025-07-15 14:01_

---

_Closed by @MichaReiser on 2025-08-04 11:45_

---
