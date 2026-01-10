```yaml
number: 1501
title: "Auto-remove unused `type: ignore`"
type: issue
state: open
author: janosh
labels:
  - suppression
assignees: []
created_at: 2025-11-08T03:10:46Z
updated_at: 2025-11-23T14:09:44Z
url: https://github.com/astral-sh/ty/issues/1501
synced_at: 2026-01-10T01:58:59Z
```

# Auto-remove unused `type: ignore`

---

_Issue opened by @janosh on 2025-11-08 03:10_

going one step beyond #278 (which would be much appreciated!), would be great if there was a flag to enable automatically remove any unused `# type: ignore` comments

---

_Label `suppression` added by @MichaReiser on 2025-11-08 13:59_

---

_Label `wish` added by @MichaReiser on 2025-11-08 13:59_

---

_Added to milestone `GA` by @MichaReiser on 2025-11-10 17:49_

---

_Label `wish` removed by @MichaReiser on 2025-11-10 17:49_

---

_Comment by @MichaReiser on 2025-11-23 14:09_

https://github.com/astral-sh/ruff/pull/21582 implements the fix for the LSP. The biggest outstanding question here is whether we want a `ty fix` command or `ty check --fix`. 

This should otherwise be straightforward to implement by copying over Ruff's fix-loop. 

---
