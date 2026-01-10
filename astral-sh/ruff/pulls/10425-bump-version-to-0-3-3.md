```yaml
number: 10425
title: Bump version to 0.3.3
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/033
created_at: 2024-03-15T17:02:43Z
updated_at: 2024-03-15T17:53:08Z
url: https://github.com/astral-sh/ruff/pull/10425
synced_at: 2026-01-10T22:47:02Z
```

# Bump version to 0.3.3

---

_Pull request opened by @zanieb on 2024-03-15 17:02_

_No description provided._

---

_Label `internal` added by @zanieb on 2024-03-15 17:02_

---

_Review requested from @AlexWaygood by @zanieb on 2024-03-15 17:07_

---

_Review requested from @charliermarsh by @zanieb on 2024-03-15 17:07_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:23 on 2024-03-15 17:13_

This seems _huge_ -- we could maybe put it first for this release? The second bullet also seems unnecessary here, since it's just a minor tweak to a feature that's new in this release.

```suggestion
### Server

- Added `ruff server` - a new built-in LSP for Ruff, written in Rust. It currently requires `--preview`. ([#10158](https://github.com/astral-sh/ruff/pull/10158))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:31 on 2024-03-15 17:14_

```suggestion
- \[`PIE970`\] Allow trailing ellipsis in `typing.TYPE_CHECKING` ([#10413](https://github.com/astral-sh/ruff/pull/10413))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:33 on 2024-03-15 17:14_

```suggestion
- \[`F811`\] Avoid removing shadowed imports that point to different symbols ([#10387](https://github.com/astral-sh/ruff/pull/10387))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:39 on 2024-03-15 17:16_

```suggestion
- \[`C413`\] Wrap expressions in parentheses when negating ([#10346](https://github.com/astral-sh/ruff/pull/10346))
```

---

_@AlexWaygood approved on 2024-03-15 17:16_

---

_@zanieb reviewed on 2024-03-15 17:20_

---

_Review comment by @zanieb on `CHANGELOG.md`:23 on 2024-03-15 17:20_

Yeah I'd actually prefer if @snowsignal authored a brief introduction here

---

_Comment by @github-actions[bot] on 2024-03-15 17:27_

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

_Review comment by @snowsignal on `CHANGELOG.md`:23 on 2024-03-15 17:31_

I'd prefer if we announced this in a release next week, just so that we'd have setup documentation and a pre-release VSCode extension to point to.

---

_@snowsignal reviewed on 2024-03-15 17:31_

---

_Merged by @zanieb on 2024-03-15 17:51_

---

_Closed by @zanieb on 2024-03-15 17:51_

---

_Branch deleted on 2024-03-15 17:51_

---
