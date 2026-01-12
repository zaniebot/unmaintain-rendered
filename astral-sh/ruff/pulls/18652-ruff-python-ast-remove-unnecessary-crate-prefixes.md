```yaml
number: 18652
title: "[ruff_python_ast] remove unnecessary `crate::` prefixes"
type: pull_request
state: closed
author: chirizxc
labels:
  - internal
assignees: []
draft: true
base: main
head: fix/unnecessary-crate-prefix
created_at: 2025-06-12T20:41:38Z
updated_at: 2025-06-24T14:14:11Z
url: https://github.com/astral-sh/ruff/pull/18652
synced_at: 2026-01-12T15:56:23Z
```

# [ruff_python_ast] remove unnecessary `crate::` prefixes

---

_@chirizxc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
/closes #18651
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Renamed from "chore: remove unnecessary `crate::` prefixes" to "[ruff_python_ast] remove unnecessary `crate::` prefixes" by @chirizxc on 2025-06-12 20:46_

---

_Label `internal` added by @dhruvmanila on 2025-06-13 04:39_

---

_Review requested from @dcreager by @dhruvmanila on 2025-06-13 04:40_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/generate.py`:841 on 2025-06-13 04:41_

Let's keep the original formatting?

---

_@dhruvmanila reviewed on 2025-06-13 04:42_

Thank you! Can you fix the merge conflicts?

This looks reasonable to me, I'd prefer if @dcreager could take a quick look at this.

---

_Converted to draft by @chirizxc on 2025-06-13 10:23_

---

_Comment by @dcreager on 2025-06-13 20:17_

Which linter is generating the warnings on the original issue? This is hard to review without a CI job telling us whether all of the warnings are being removed. I'm still seeing some instances of `crate::FString`, for instance, in the generated code. Is that one of the original warnings that we're trying to silence? (I assume so given it being added to the config set in `generate.py`.) Will that continue to generate a warning if we don't catch them all?

---

_Comment by @chirizxc on 2025-06-13 20:27_

> Which linter is generating the warnings on the original issue? This is hard to review without a CI job telling us whether all of the warnings are being removed. I'm still seeing some instances of `crate::FString`, for instance, in the generated code. Is that one of the original warnings that we're trying to silence? (I assume so given it being added to the config set in `generate.py`.) Will that continue to generate a warning if we don't catch them all?

it's something built into rustrover (https://www.jetbrains.com/help/inspectopedia/RsUnnecessaryQualifications.html)

---

_Comment by @chirizxc on 2025-06-13 20:50_

i found this lint rule it's called [`unused_qualifications`](https://doc.rust-lang.org/rustc/lints/listing/allowed-by-default.html#unused-qualifications)

---

_Marked ready for review by @chirizxc on 2025-06-13 21:15_

---

_Comment by @github-actions[bot] on 2025-06-13 21:22_

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

_Comment by @chirizxc on 2025-06-13 21:45_

also can we add `unused_qualifications = "warn"` in [Cargo.toml](https://github.com/astral-sh/ruff/blob/main/Cargo.toml#L195) section `[workspace.lints.rust]`?

---

_Comment by @dcreager on 2025-06-16 18:55_

I'm going to move the discussion back over to #18651 so that we can discuss (there) whether we _want_ to do this before we discuss (here) _how_ to do it.

---

_Converted to draft by @chirizxc on 2025-06-16 18:57_

---

_Comment by @MichaReiser on 2025-06-24 12:25_

I'll close this for the reasons discussed on the issue. Thanks again for working on this.

---

_Closed by @MichaReiser on 2025-06-24 12:25_

---

_Branch deleted on 2025-06-24 14:14_

---
