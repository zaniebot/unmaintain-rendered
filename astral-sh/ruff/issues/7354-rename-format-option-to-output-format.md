```yaml
number: 7354
title: "Rename `format` option to `output-format`"
type: issue
state: closed
author: zanieb
labels:
  - breaking
  - configuration
  - cli
  - formatter
assignees: []
created_at: 2023-09-13T16:32:43Z
updated_at: 2023-09-20T13:19:00Z
url: https://github.com/astral-sh/ruff/issues/7354
synced_at: 2026-01-10T11:09:49Z
```

# Rename `format` option to `output-format`

---

_Issue opened by @zanieb on 2023-09-13 16:32_

The `format` option could be confused with our formatter. It should be renamed to `output-format` in both the configuration file and CLI e.g.

`--format <kind>` -> `--output-format <kind>`
`format = <kind>` -> `output-format = <kind>`

The previous name should undergo a deprecation period during which we display a warning on use.

---

_Label `configuration` added by @zanieb on 2023-09-13 16:32_

---

_Label `cli` added by @zanieb on 2023-09-13 16:32_

---

_Label `breaking` added by @MichaReiser on 2023-09-13 17:58_

---

_Comment by @zanieb on 2023-09-14 18:09_

I think we'll want to look into building tooling for rename deprecations in our configuration macro.

---

_Label `formatter` added by @MichaReiser on 2023-09-15 09:55_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-15 09:55_

---

_Closed by @MichaReiser on 2023-09-20 13:19_

---
