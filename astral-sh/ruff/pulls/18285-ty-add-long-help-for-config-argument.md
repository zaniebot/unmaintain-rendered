```yaml
number: 18285
title: "[ty] Add long help for `--config` argument"
type: pull_request
state: merged
author: j178
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: cli-doc
created_at: 2025-05-24T11:20:20Z
updated_at: 2025-05-25T11:09:02Z
url: https://github.com/astral-sh/ruff/pull/18285
synced_at: 2026-01-12T15:56:16Z
```

# [ty] Add long help for `--config` argument

---

_@j178_

## Summary

Closes https://github.com/astral-sh/ty/issues/499

Help for the `--config` argument is given in a builder style. I decided to just use the docstring from the `ConfigsArg` struct. 

As for the trucated help for possible values (e.g. `--output-format full`), I couldn’t figure out how to get the long help from clap.

---

_Review requested from @carljm by @j178 on 2025-05-24 11:20_

---

_Review requested from @MichaReiser by @j178 on 2025-05-24 11:20_

---

_Review requested from @AlexWaygood by @j178 on 2025-05-24 11:20_

---

_Review requested from @sharkdp by @j178 on 2025-05-24 11:20_

---

_Review requested from @dcreager by @j178 on 2025-05-24 11:20_

---

_Label `ty` added by @AlexWaygood on 2025-05-24 11:21_

---

_Renamed from "Add long help for `--config` argument" to "[ty] Add long help for `--config` argument" by @AlexWaygood on 2025-05-24 11:22_

---

_Comment by @github-actions[bot] on 2025-05-24 11:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-05-25 11:08_

Thanks

---

_Comment by @MichaReiser on 2025-05-25 11:08_

> As for the trucated help for possible values (e.g. --output-format full), I couldn’t figure out how to get the long help from clap.

That's fine. I think we should fix the rendering separately

---

_Label `documentation` added by @MichaReiser on 2025-05-25 11:08_

---

_Merged by @MichaReiser on 2025-05-25 11:09_

---

_Closed by @MichaReiser on 2025-05-25 11:09_

---
