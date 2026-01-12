```yaml
number: 686
title: "Add `--statistics` for an overview (like ruff)"
type: issue
state: open
author: Julian-J-S
labels:
  - cli
  - wish
assignees: []
created_at: 2025-06-20T10:04:18Z
updated_at: 2025-12-18T10:50:23Z
url: https://github.com/astral-sh/ty/issues/686
synced_at: 2026-01-12T15:54:23Z
```

# Add `--statistics` for an overview (like ruff)

---

_@Julian-J-S_

ruff has: `ruff check --statistics` which can be very useful.

I would love to see the same for ty.

Integration ruff / ty into a big repo and getting 10k+ "errors" is hard to manage.
Getting some idea of how many different problems and counts would be very helpful :)

---

_Label `cli` added by @MichaReiser on 2025-06-20 10:18_

---

_Label `wish` added by @MichaReiser on 2025-06-20 10:18_

---

_Comment by @sharkdp on 2025-06-20 13:21_

Not exactly what you want, but something that you can use as a workaround in the meantime:

The [`ecosystem-analyzer`](https://github.com/astral-sh/ecosystem-analyzer/) program can parse ty's "concise" diagnostic output. You can use the `parse-diagnostics` subcommand to create structured diagnostic information, and then use `generate-report` to create a single HTML page overview of all diagnostics. This includes statistics (somewhat hidden in the filter-dropdowns), and allows you to browse different categories of errors easily:

```bash
alias ecosystem-analyzer='uvx --from "git+https://github.com/astral-sh/ecosystem-analyzer" ecosystem-analyzer'

uvx ty check --output-format=concise | ecosystem-analyzer parse-diagnostics
ecosystem-analyzer generate-report parsed-diagnostics.json 
```

If you supply `--project-location` and `--commit` to `parse-diagnostics`, you'll also get clickable GitHub links to the source code locations.

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 09:16_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Comment by @spaceone on 2025-12-18 10:50_

I would also love to see it.

---
