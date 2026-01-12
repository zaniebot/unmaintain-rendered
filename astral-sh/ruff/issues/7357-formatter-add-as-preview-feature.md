```yaml
number: 7357
title: "Formatter: Add as preview feature"
type: issue
state: closed
author: MichaReiser
labels:
  - cli
  - formatter
  - preview
assignees: []
created_at: 2023-09-13T17:31:49Z
updated_at: 2023-09-25T09:33:00Z
url: https://github.com/astral-sh/ruff/issues/7357
synced_at: 2026-01-12T15:54:47Z
```

# Formatter: Add as preview feature

---

_@MichaReiser_

Add the formatter as a preview-only feature instead of hiding it. 

Plus points: The `format` command documentation only shows when preview mode is enabled

> Note: We'll need to make sure that the IDE always pass the `--preview` flag when calling the formatter.

---

_Label `cli` added by @MichaReiser on 2023-09-13 17:31_

---

_Label `formatter` added by @MichaReiser on 2023-09-13 17:31_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-13 17:32_

---

_Label `preview` added by @MichaReiser on 2023-09-15 09:52_

---

_Comment by @MichaReiser on 2023-09-25 09:33_

We decided that we don't want to do this. The warning shown by the formatter is explicit enough

---

_Closed by @MichaReiser on 2023-09-25 09:33_

---
