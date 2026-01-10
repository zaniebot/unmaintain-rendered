```yaml
number: 7574
title: "Rename `pydocstyle.max-doc-length` to `max-doc-width`"
type: issue
state: closed
author: MichaReiser
labels:
  - configuration
assignees: []
created_at: 2023-09-21T11:37:14Z
updated_at: 2023-10-24T07:55:22Z
url: https://github.com/astral-sh/ruff/issues/7574
synced_at: 2026-01-10T11:09:49Z
```

# Rename `pydocstyle.max-doc-length` to `max-doc-width`

---

_Issue opened by @MichaReiser on 2023-09-21 11:37_

Rename `pydocstyle.max-doc-length` to `doc-width` to align with other text width options and clarify that the option is about the text width and not character or byte length. 

The `max-doc-length` option should act as an alias but Ruff should warn and hint users towards migrating to the new option.

---

_Label `configuration` added by @MichaReiser on 2023-09-25 09:28_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-25 09:38_

---

_Comment by @ngnpope on 2023-09-26 17:56_

Wondering whether the `max-` prefix is relevant? 🤔

If we are going for `line-width`, as the general line width value (without `max-`), would `doc-line-width` make more sense?

---

_Assigned to @MichaReiser by @MichaReiser on 2023-10-17 10:48_

---

_Closed by @MichaReiser on 2023-10-24 07:55_

---
