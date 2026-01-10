```yaml
number: 15885
title: "[`flake8-pyi`] Rename `PYI019` and improve its diagnostic message"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: alex/pyi019-message
created_at: 2025-02-02T21:07:49Z
updated_at: 2025-02-03T14:24:00Z
url: https://github.com/astral-sh/ruff/pull/15885
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-pyi`] Rename `PYI019` and improve its diagnostic message

---

_Pull request opened by @AlexWaygood on 2025-02-02 21:07_

## Summary

Currently the name of `PYI019` is `custom-type-var-return-type` and its diagnostic message is:

> "Methods like `{method_name}` should return `Self` instead of a custom `TypeVar`"

There are two issues that this PR addresses:
- Often if you're converting a method to use `Self` rather than a custom `TypeVar`, you'll need to change other annotations in the method's non-self parameters, not just the `self` annotation and the return annotation. Indeed, our preview-mode autofix for stub files will change some of these annotations. But the message and name imply that the rule is just about the return annotation.
- It should be pretty obvious which method the diagnostic is complaining about from the diagnostic's range, so I don't think we really need to mention the method name in the diagnostic message. It's more useful to mention the name of the TypeVar that we're suggesting the user should get rid of.

I think the changes here should all be backwards-compatible, but we'll need to remember to add a redirect for the documentation after the next release (whether it is a minor release or patch release). In my opinion, the diagnostic's range should also cover the whole of the function's header, but this would be backwards-incompatible, so I'll propose it separately (it will either need to be preview-only or wait until the next minor release).

## Test Plan

`cargo test -p ruff_linter --lib`


---

_Label `rule` added by @AlexWaygood on 2025-02-02 21:07_

---

_Comment by @github-actions[bot] on 2025-02-02 21:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-02-02 21:17_

Strange, I would have expected some ecosystem hits here...

---

_@MichaReiser approved on 2025-02-03 14:18_

---

_Comment by @MichaReiser on 2025-02-03 14:19_

I assume you meant that we have to set up the redirect with the next release, whether it is a patch or minor release. 



---

_Comment by @AlexWaygood on 2025-02-03 14:22_

> I assume you meant that we have to set up the redirect with the next release, whether it is a patch or minor release.

Correct, thank you! I'll update the PR description (edit: now updated)

---

_Merged by @AlexWaygood on 2025-02-03 14:23_

---

_Closed by @AlexWaygood on 2025-02-03 14:23_

---

_Branch deleted on 2025-02-03 14:24_

---
