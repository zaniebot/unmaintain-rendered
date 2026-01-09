---
number: 21892
title: "docs: clarify jupyter inclusion for editor features"
type: issue
state: open
author: KotlinIsland
labels:
  - documentation
assignees: []
created_at: 2025-12-10T11:04:55Z
updated_at: 2025-12-10T15:50:28Z
url: https://github.com/astral-sh/ruff/issues/21892
synced_at: 2026-01-07T13:12:16-06:00
---

# docs: clarify jupyter inclusion for editor features

---

_Issue opened by @KotlinIsland on 2025-12-10 11:04_

### Summary

these two docs contradict each other

### https://docs.astral.sh/ruff/editors/features/#jupyter-notebook
> [...] similar to the Ruff's CLI, the native language server requires user to explicitly include the Jupyter Notebook files in the set of files to lint and format. Refer to the [Jupyter Notebook discovery](https://docs.astral.sh/ruff/configuration/#jupyter-notebook-discovery) section on how to do this.

> the Ruff's


### https://docs.astral.sh/ruff/configuration/#jupyter-notebook-discovery
> Ruff has built-in support for linting and formatting [Jupyter Notebooks](https://jupyter.org/), which are linted and formatted by default on version 0.6.0 and higher.

i'm guessing that the information in the editor section is outdated

### Version

_No response_

---

_Comment by @MichaReiser on 2025-12-10 15:50_

Thanks

---

_Label `documentation` added by @MichaReiser on 2025-12-10 15:50_

---
