```yaml
number: 9104
title: Use the latest poetry ref in the ecosystem formatter check script
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: upgrade-poetry-in-ecosystem-check
created_at: 2023-12-12T06:19:45Z
updated_at: 2023-12-12T06:35:49Z
url: https://github.com/astral-sh/ruff/pull/9104
synced_at: 2026-01-12T15:55:27Z
```

# Use the latest poetry ref in the ecosystem formatter check script

---

_@MichaReiser_

## Summary

Update the poetry revision in the `formatter_ecosystem_checks` script and remove the second peotry entry (that ended up overriding the first entry). 

Updating poetry allows us to compare compatibility with a more recent black `--preview` style.

## Test Plan

I ran the `formatter_ecosystem_checks` script. The compatibility for Poetry is now 0.99888
<!-- How was it tested? -->


---

_Comment by @MichaReiser on 2023-12-12 06:19_

Current dependencies on/for this PR:
* main
  * **PR #9104** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9104?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9104?utm_source=stack-comment).

---

_@zanieb approved on 2023-12-12 06:31_

---

_Merged by @MichaReiser on 2023-12-12 06:32_

---

_Closed by @MichaReiser on 2023-12-12 06:32_

---

_Branch deleted on 2023-12-12 06:32_

---

_Label `internal` added by @MichaReiser on 2023-12-12 06:33_

---

_Comment by @github-actions[bot] on 2023-12-12 06:35_

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
