```yaml
number: 11740
title: "Use `Tokens` from parsed type annotation or parsed source"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/parsed-tokens
created_at: 2024-06-04T16:44:23Z
updated_at: 2024-06-05T08:00:46Z
url: https://github.com/astral-sh/ruff/pull/11740
synced_at: 2026-01-10T21:56:00Z
```

# Use `Tokens` from parsed type annotation or parsed source

---

_Pull request opened by @dhruvmanila on 2024-06-04 16:44_

## Summary

This PR fixes a bug where the checker would require the tokens for an invalid offset w.r.t. the source code.

Taking the source code from the linked issue as an example:
```py
relese_version :"0.0is 64"
```

Now, this isn't really a valid type annotation but that's what this PR is fixing. Regardless of whether it's valid or not, Ruff shouldn't panic.

The checker would visit the parsed type annotation (`0.0is 64`) and try to detect any violations. Certain rule logic requests the tokens for the same but it would fail because the lexer would only have the `String` token considering original source code. This worked before because the lexer was invoked again for each rule logic.

The solution is to store the parsed type annotation on the checker if it's in a typing context and use the tokens from that instead if it's available. This is enforced by creating a new API on the checker to get the tokens.

But, this means that there are two ways to get the tokens via the checker API. I want to restrict this in a follow-up PR (#11741) to only expose `tokens` and `comment_ranges` as methods and restrict access to the parsed source code.

fixes: #11736 

## Test Plan

- [x] Add a test case for `F632` rule and update the snapshot
- [x] Check all affected rules
- [x] No ecosystem changes


---

_Label `bug` added by @dhruvmanila on 2024-06-04 16:44_

---

_Comment by @github-actions[bot] on 2024-06-04 17:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-05 06:46_

---

_@MichaReiser approved on 2024-06-05 07:07_

---

_Comment by @dhruvmanila on 2024-06-05 07:47_

I've added test cases for all affected rules as well. Even though the type annotations themselves aren't valid, Ruff shouldn't panic.

---

_Merged by @dhruvmanila on 2024-06-05 07:50_

---

_Closed by @dhruvmanila on 2024-06-05 07:50_

---

_Branch deleted on 2024-06-05 07:50_

---
