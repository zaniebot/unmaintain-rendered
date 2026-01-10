```yaml
number: 18096
title: "[ty] fix missing '>' in HTML anchor tags in CLI reference"
type: pull_request
state: merged
author: Usul-Dev
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: fix-html-anchor-tag-syntax
created_at: 2025-05-14T15:33:05Z
updated_at: 2025-05-14T15:51:00Z
url: https://github.com/astral-sh/ruff/pull/18096
synced_at: 2026-01-10T18:51:01Z
```

# [ty] fix missing '>' in HTML anchor tags in CLI reference

---

_Pull request opened by @Usul-Dev on 2025-05-14 15:33_

## Summary

This PR is a follow-up to [ty-issue #383](https://github.com/astral-sh/ty/pull/383), fixing the root cause of the HTML syntax errors in the CLI documentation generator. Instead of modifying the generated documentation directly, this PR fixes the generation script itself.

Changes:
- Add missing '>' after href attributes in anchor tags in generate_ty_cli_reference.rs
- Regenerate CLI documentation with the fixed generator
- Ensures all future documentation generations will have proper HTML syntax

## Test Plan
1. Modified the documentation generator to fix HTML syntax
2. Regenerated the CLI documentation to verify the changes
3. Confirmed that the HTML anchor tags are now properly formatted in both the generator and output

---

_@MichaReiser approved on 2025-05-14 15:36_

Thanks. You have to commit the updated docs as well

---

_Label `documentation` added by @MichaReiser on 2025-05-14 15:36_

---

_Label `ty` added by @MichaReiser on 2025-05-14 15:36_

---

_Review requested from @carljm by @Usul-Dev on 2025-05-14 15:46_

---

_Review requested from @AlexWaygood by @Usul-Dev on 2025-05-14 15:46_

---

_Review requested from @sharkdp by @Usul-Dev on 2025-05-14 15:46_

---

_Review requested from @dcreager by @Usul-Dev on 2025-05-14 15:46_

---

_Merged by @MichaReiser on 2025-05-14 15:50_

---

_Closed by @MichaReiser on 2025-05-14 15:50_

---

_Comment by @github-actions[bot] on 2025-05-14 15:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---
