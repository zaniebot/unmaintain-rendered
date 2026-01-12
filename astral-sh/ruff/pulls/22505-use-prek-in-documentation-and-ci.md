```yaml
number: 22505
title: Use prek in documentation and CI
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/prek
created_at: 2026-01-11T18:30:58Z
updated_at: 2026-01-11T19:17:13Z
url: https://github.com/astral-sh/ruff/pull/22505
synced_at: 2026-01-12T02:26:21Z
```

# Use prek in documentation and CI

---

_Pull request opened by @charliermarsh on 2026-01-11 18:30_

## Summary

AFAIK, many of us on the team are using prek now. It seems appropriate to modify the docs to formalize it.


---

_Label `documentation` added by @charliermarsh on 2026-01-11 18:31_

---

_Renamed from "Use prek in docuemntation" to "Use prek in documentation" by @AlexWaygood on 2026-01-11 18:35_

---

_@AlexWaygood reviewed on 2026-01-11 18:35_

---

_Review comment by @AlexWaygood on `.devcontainer/post-create.sh`:8 on 2026-01-11 18:35_

what's pip

---

_Marked ready for review by @charliermarsh on 2026-01-11 18:35_

---

_Review requested from @carljm by @charliermarsh on 2026-01-11 18:35_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-11 18:35_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-11 18:35_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-11 18:35_

---

_@AlexWaygood approved on 2026-01-11 18:41_

You could also update these comments:

https://github.com/astral-sh/ruff/blob/2c68057c4b0a666ffd73324013fad2ce3c2fa547/.pre-commit-config.yaml#L122-L123

---

_@charliermarsh reviewed on 2026-01-11 18:42_

---

_Review comment by @charliermarsh on `.devcontainer/post-create.sh`:8 on 2026-01-11 18:42_

Lol

---

_@charliermarsh reviewed on 2026-01-11 18:42_

---

_Review comment by @charliermarsh on `.devcontainer/post-create.sh`:8 on 2026-01-11 18:42_

(I don't dare touch `.devcontainer` beyond this.)

---

_@AlexWaygood reviewed on 2026-01-11 18:48_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:797 on 2026-01-11 18:48_

nit: while this is what I usually run locally, I think it's probably clearer in committed code if we use the full name of the argument here?

```suggestion
          SKIP=cargo-fmt uvx prek run --all-files --show-diff-on-failure --color always --hook-stage manual | \
```

---

_Renamed from "Use prek in documentation" to "Use prek in documentation and CI" by @AlexWaygood on 2026-01-11 18:51_

---

_Comment by @astral-sh-bot[bot] on 2026-01-11 19:04_


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

_Merged by @charliermarsh on 2026-01-11 19:17_

---

_Closed by @charliermarsh on 2026-01-11 19:17_

---

_Branch deleted on 2026-01-11 19:17_

---
