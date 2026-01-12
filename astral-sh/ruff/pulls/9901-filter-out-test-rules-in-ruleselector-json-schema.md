```yaml
number: 9901
title: "Filter out test rules in `RuleSelector` JSON schema"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: filter-out-test-rules-in-json-schema
created_at: 2024-02-08T20:35:06Z
updated_at: 2024-02-08T21:06:52Z
url: https://github.com/astral-sh/ruff/pull/9901
synced_at: 2026-01-12T15:55:30Z
```

# Filter out test rules in `RuleSelector` JSON schema

---

_@MichaReiser_

## Summary

Running `cargo test` fails today because `cargo dev generate-all` runs with the `test-rules` feature enabled (because `ruff` enables the feature), which results in the test rules showing up in the JSON schema. 

This PR fixes this by filtering out the test rules if the `test-rules` feature is enabled. I don't particularly like it but how it is today is very disruptive to the development workflow.

## Test Plan

`cargo test` no longer fails 


---

_@charliermarsh approved on 2024-02-08 20:48_

---

_Comment by @github-actions[bot] on 2024-02-08 20:53_

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

_@zanieb approved on 2024-02-08 21:02_

okeee thanks for cleaning up after me

---

_Label `internal` added by @zanieb on 2024-02-08 21:02_

---

_Merged by @MichaReiser on 2024-02-08 21:06_

---

_Closed by @MichaReiser on 2024-02-08 21:06_

---

_Branch deleted on 2024-02-08 21:06_

---
