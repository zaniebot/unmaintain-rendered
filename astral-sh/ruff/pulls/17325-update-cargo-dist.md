```yaml
number: 17325
title: update cargo-dist
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: gankra/update-dist2
created_at: 2025-04-09T21:26:27Z
updated_at: 2025-04-10T13:43:15Z
url: https://github.com/astral-sh/ruff/pull/17325
synced_at: 2026-01-12T15:56:01Z
```

# update cargo-dist

---

_@Gankra_

Putting this up to confirm that it does what it should:

* undirty the release.yml by including action-commits in the config
* add persist-credentials=false hardening

You can choose to not merge this right away if a prerelease is conceptually spooky to you, otherwise it should work fine.

---

_@Gankra reviewed on 2025-04-09 21:27_

---

_Review comment by @Gankra on `Cargo.toml`:340 on 2025-04-09 21:27_

this is future-proofing in case you decide to enable `github-attestations = true` :)

---

_Comment by @github-actions[bot] on 2025-04-09 21:36_

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

_@MichaReiser approved on 2025-04-10 06:30_

I don't mind using a pre-release of cargo dist. I know who I need to ping if something goes wrong ;)



---

_Merged by @Gankra on 2025-04-10 13:43_

---

_Closed by @Gankra on 2025-04-10 13:43_

---

_Branch deleted on 2025-04-10 13:43_

---
