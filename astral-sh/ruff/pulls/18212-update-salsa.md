```yaml
number: 18212
title: Update salsa
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/salsa-update
created_at: 2025-05-20T06:40:32Z
updated_at: 2025-05-24T16:18:49Z
url: https://github.com/astral-sh/ruff/pull/18212
synced_at: 2026-01-12T15:56:14Z
```

# Update salsa

---

_@MichaReiser_

## Summary

Update salsa.

The biggest change is that salsa no longer automatically derives `PartialOrd` and `Ord` for salsa-structs. Instead, an explicit `#[derive(PartialOrd, Ord)]` is required
to opt in to the ID based ordering.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-05-20 06:40_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-20 06:40_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-20 06:40_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-20 06:40_

---

_Label `internal` added by @MichaReiser on 2025-05-20 06:40_

---

_Comment by @github-actions[bot] on 2025-05-20 06:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-20 06:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@sharkdp approved on 2025-05-20 07:10_

Thank you! (also for making that change in salsa)

---

_Merged by @MichaReiser on 2025-05-20 07:19_

---

_Closed by @MichaReiser on 2025-05-20 07:19_

---

_Branch deleted on 2025-05-20 07:19_

---

_Comment by @ognevny on 2025-05-24 07:54_

this change caused a regression with `cargo fetch --locked` command. build ran in GitHub Actions machine

```
Updating crates.io index
      Updating git repository `[https://github.com/salsa-rs/salsa.git`](https://github.com/salsa-rs/salsa.git%60)
  error: failed to get `salsa` as a dependency of package `ruff_db v0.0.0 (D:\_\B\src\ruff\crates\ruff_db)`
  
  Caused by:
    failed to load source for dependency `salsa`
  
  Caused by:
    Unable to update https://github.com/salsa-rs/salsa.git?rev=4818b15f3b7516555d39f5a41cb75970448bee4c#4818b15f
  
  Caused by:
    failed to clone into: C:\Users\runneradmin\.cargo\git\db\salsa-e6f3bb7c2a062968
  
  Caused by:
    revision 4818b15f3b7516555d39f5a41cb75970448bee4c not found
  
  Caused by:
    network failure seems to have happened
    if a proxy or similar is necessary `net.git-fetch-with-cli` may help here
    https://doc.rust-lang.org/cargo/reference/config.html#netgit-fetch-with-cli
  
  Caused by:
    the SSL certificate is invalid; class=Ssl (16)
```

---

_Comment by @MichaReiser on 2025-05-24 15:42_

This looks like a network issue rather than a problem with the pr itself 

---

_Comment by @ognevny on 2025-05-24 16:04_

> This looks like a network issue rather than a problem with the pr itself 

I reproduced it on my Windows machine too. also retries didn't help

---

_Comment by @MichaReiser on 2025-05-24 16:08_

Any chance you're behind a proxy? Or have otherwise limited network access? Can you open the url in your browser?

---

_Comment by @ognevny on 2025-05-24 16:11_

> Any chance you're behind a proxy? Or have otherwise limited network access? Can you open the url in your browser?

yes, I can. there shouldn't be a proxy on Windows machine...

anyway I'll open a PR now which updates salsa to stable version

---

_Comment by @MichaReiser on 2025-05-24 16:18_

We prefer to keep the git dependency as we're still very actively developing salsa

---
