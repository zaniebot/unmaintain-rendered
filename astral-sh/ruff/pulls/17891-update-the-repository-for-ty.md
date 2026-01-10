```yaml
number: 17891
title: update the repository for ty
type: pull_request
state: merged
author: Gankra
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: gankra/urlfix
created_at: 2025-05-06T14:51:13Z
updated_at: 2025-05-06T16:03:40Z
url: https://github.com/astral-sh/ruff/pull/17891
synced_at: 2026-01-10T18:57:03Z
```

# update the repository for ty

---

_Pull request opened by @Gankra on 2025-05-06 14:51_

This metadata is used by cargo-dist for artifact URLs (in curl-sh expressions)


---

_Review requested from @carljm by @Gankra on 2025-05-06 14:51_

---

_Review requested from @MichaReiser by @Gankra on 2025-05-06 14:51_

---

_Review requested from @AlexWaygood by @Gankra on 2025-05-06 14:51_

---

_Review requested from @sharkdp by @Gankra on 2025-05-06 14:51_

---

_Review requested from @dcreager by @Gankra on 2025-05-06 14:51_

---

_Comment by @Gankra on 2025-05-06 14:53_

```
cargo dist plan -ojson | rg 'install_hint'
      "install_hint": "powershell -ExecutionPolicy Bypass -c \"irm https://github.com/astral-sh/ty/releases/download/v0.0.0-alpha.5/ty-installer.ps1 | iex\"",
      "install_hint": "curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/ty/releases/download/v0.0.0-alpha.5/ty-installer.sh | sh",
```

---

_Label `ty` added by @AlexWaygood on 2025-05-06 14:59_

---

_Label `internal` added by @Gankra on 2025-05-06 15:02_

---

_Comment by @github-actions[bot] on 2025-05-06 15:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty/Cargo.toml`:7 on 2025-05-06 15:19_

We can do this as a separate PR but we could also override homepage and documentation with `"https://github.com/astral-sh/ty/"`

---

_@MichaReiser approved on 2025-05-06 15:19_

Thanks

---

_Review comment by @Gankra on `crates/ty/Cargo.toml`:7 on 2025-05-06 15:25_

Yeah I wasn't sure if there were Even Better URLs planned.

---

_@Gankra reviewed on 2025-05-06 15:25_

---

_Merged by @Gankra on 2025-05-06 16:03_

---

_Closed by @Gankra on 2025-05-06 16:03_

---

_Branch deleted on 2025-05-06 16:03_

---
