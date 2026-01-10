```yaml
number: 18471
title: Combine lint and syntax error handling
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/combine-lsp-diagnostic
created_at: 2025-06-04T20:32:10Z
updated_at: 2025-06-05T12:54:07Z
url: https://github.com/astral-sh/ruff/pull/18471
synced_at: 2026-01-10T18:45:04Z
```

# Combine lint and syntax error handling

---

_Pull request opened by @ntBre on 2025-06-04 20:32_

## Summary

This is a spin-off from https://github.com/astral-sh/ruff/pull/18447#discussion_r2125844669 to avoid using `Message::noqa_code` to differentiate between lints and syntax errors. I went through all of the calls on `main` and on the branch from #18447, and the instance in `ruff_server` noted in the linked comment was actually the primary place where this was being done. Other calls to `noqa_code` are typically some variation of `message.noqa_code().map_or(String::new, format!(...))`, with the major exception of the gitlab output format:

https://github.com/astral-sh/ruff/blob/a120610b5b01a9e7bb91740a23f6c2b5bbcd4b5f/crates/ruff_linter/src/message/gitlab.rs#L93-L105

which obviously assumes that `None` means syntax error. A simple fix here would be to use `message.name()` for `check_name` instead of the noqa code, but I'm not sure how breaking that would be. This could just be:

```rust
 let description = message.body();
 let description = description.strip_prefix("SyntaxError: ").unwrap_or(description).to_string();
 let check_name = message.name();
```

In that case. This sounds reasonable based on the [Code Quality report format](https://docs.gitlab.com/ci/testing/code_quality/#code-quality-report-format) docs: 

> | Name | Type | Description|
> |-----|-----|----|
> |`check_name` | String | A unique name representing the check, or rule, associated with this violation. |

## Test Plan

Existing tests


---

_Review requested from @MichaReiser by @ntBre on 2025-06-04 20:33_

---

_Review requested from @dhruvmanila by @ntBre on 2025-06-04 20:33_

---

_Label `internal` added by @ntBre on 2025-06-04 20:33_

---

_Label `diagnostics` added by @ntBre on 2025-06-04 20:33_

---

_Marked ready for review by @ntBre on 2025-06-04 20:33_

---

_Comment by @github-actions[bot] on 2025-06-04 20:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:267 on 2025-06-05 04:55_

nit: we could short-circuit if `code` is `None` and avoid generating the edits although I think currently it's only the syntax errors that doesn't have code and those doesn't have fixes, so maybe this isn't that useful

---

_@dhruvmanila approved on 2025-06-05 04:58_

This looks pretty straightforward!

---

_@MichaReiser approved on 2025-06-05 06:11_

Sweet

---

_@ntBre reviewed on 2025-06-05 12:45_

---

_Review comment by @ntBre on `crates/ruff_server/src/lint.rs`:267 on 2025-06-05 12:45_

Yeah, I think the `fix.is_some() || noqa_edit.is_some()` condition should virtually guarantee this is never `None`, but I can move it to the top of the closure just in case. Thanks!

---

_Merged by @ntBre on 2025-06-05 12:50_

---

_Closed by @ntBre on 2025-06-05 12:50_

---

_Branch deleted on 2025-06-05 12:50_

---
