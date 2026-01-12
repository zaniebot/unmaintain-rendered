```yaml
number: 10171
title: "Remove the `ruff` to `ruff check` alias"
type: issue
state: closed
author: MichaReiser
labels:
  - breaking
assignees: []
created_at: 2024-02-29T14:00:14Z
updated_at: 2024-10-08T13:45:13Z
url: https://github.com/astral-sh/ruff/issues/10171
synced_at: 2026-01-12T15:54:50Z
```

# Remove the `ruff` to `ruff check` alias

---

_@MichaReiser_

We deprecated the `ruff` to `ruff check`, `ruff --explain` to `ruff rule`, `ruff --generate-shell-completion` to `ruff generate-shell-completion` and the `ruff --clean` to `ruff clean` aliases in 0.3. Ruff 0.5 changed the commands to error.  We should remove the aliases in an upcoming minor release.

https://github.com/astral-sh/ruff/pull/10169
https://github.com/astral-sh/ruff/pull/12011 

Note that the ruff-lsp used the `ruff` to `ruff check` alias https://github.com/astral-sh/ruff-lsp/pull/392


---

_Label `breaking` added by @MichaReiser on 2024-02-29 14:00_

---

_Added to milestone `v0.4` by @MichaReiser on 2024-02-29 14:00_

---

_Removed from milestone `v0.4.0` by @dhruvmanila on 2024-04-18 19:49_

---

_Added to milestone `v0.5.0` by @dhruvmanila on 2024-04-18 19:49_

---

_Removed from milestone `v0.5.0` by @MichaReiser on 2024-06-25 07:04_

---

_Added to milestone `v0.6` by @MichaReiser on 2024-06-25 07:04_

---

_Comment by @MichaReiser on 2024-06-26 08:17_

https://github.com/astral-sh/ruff/pull/12011 made using any of the above commands a hard error, but we keep printing an error. We should remove the commands entirely in a future release (0.6? or 0.7?)

---

_Comment by @charliermarsh on 2024-06-26 20:59_

So the current state is that these have a custom error, and the task here is to remove the custom errors?

---

_Comment by @MichaReiser on 2024-06-27 06:20_

> So the current state is that these have a custom error, and the task here is to remove the custom errors?

Exactly

---

_Removed from milestone `v0.6` by @MichaReiser on 2024-08-12 13:59_

---

_Added to milestone `v0.7` by @MichaReiser on 2024-08-12 13:59_

---

_Closed by @AlexWaygood on 2024-10-08 13:45_

---
