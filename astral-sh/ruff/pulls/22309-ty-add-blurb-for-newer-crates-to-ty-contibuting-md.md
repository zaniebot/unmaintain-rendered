```yaml
number: 22309
title: "[ty] Add blurb for newer crates to `ty/CONTIBUTING.md"
type: pull_request
state: merged
author: sinon
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: add-new-ty-creates-to-contrib
created_at: 2025-12-30T20:27:19Z
updated_at: 2025-12-31T07:58:34Z
url: https://github.com/astral-sh/ruff/pull/22309
synced_at: 2026-01-12T15:57:46Z
```

# [ty] Add blurb for newer crates to `ty/CONTIBUTING.md

---

_@sinon_

## Summary

Some of the newer `ty_` crates are not referenced in the `ty/CONTRIBUTING.md` file.

This adds a short blurb for each. Though I am not sure if `ty_combine` or `ty_static` are worth calling out 🤔 

## Test Plan

N/A


---

_Review requested from @carljm by @sinon on 2025-12-30 20:27_

---

_Review requested from @MichaReiser by @sinon on 2025-12-30 20:27_

---

_Review requested from @sharkdp by @sinon on 2025-12-30 20:27_

---

_Review requested from @dcreager by @sinon on 2025-12-30 20:27_

---

_Label `ty` added by @ntBre on 2025-12-30 23:39_

---

_Label `documentation` added by @ntBre on 2025-12-30 23:39_

---

_Review comment by @MichaReiser on `crates/ty/CONTRIBUTING.md`:106 on 2025-12-31 07:52_

```suggestion
- `ty_module_resolver`: The module resolver, which allows resolving imports to their modules.
```

---

_Review comment by @MichaReiser on `crates/ty/CONTRIBUTING.md`:105 on 2025-12-31 07:52_

```suggestion
- `ty_completion_eval`: Framework for evaluating completion suggestions returned by the ty LSP.
```

---

_Review comment by @MichaReiser on `crates/ty/CONTRIBUTING.md`:108 on 2025-12-31 07:53_

```suggestion
- `ty_combine`: Utility crate containing the `Combine` trait, which is used to combine `Options`.
```

---

_Review comment by @MichaReiser on `crates/ty/CONTRIBUTING.md`:107 on 2025-12-31 07:53_

```suggestion
- `ty_static`: Lists the known environment variables used by `ty`.
```

---

_@MichaReiser approved on 2025-12-31 07:53_

Thank you

---

_Merged by @MichaReiser on 2025-12-31 07:58_

---

_Closed by @MichaReiser on 2025-12-31 07:58_

---
