```yaml
number: 10678
title: "Implement `Display` for `TokenKind`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/tokenkind-display
created_at: 2024-03-31T03:09:17Z
updated_at: 2024-04-10T16:48:21Z
url: https://github.com/astral-sh/ruff/pull/10678
synced_at: 2026-01-10T22:37:01Z
```

# Implement `Display` for `TokenKind`

---

_Pull request opened by @dhruvmanila on 2024-03-31 03:09_

## Summary

This PR implements `Display` for `TokenKind` to improve error messages.


---

_Label `parser` added by @dhruvmanila on 2024-03-31 03:09_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-31 03:09_

---

_Merged by @dhruvmanila on 2024-03-31 03:09_

---

_Closed by @dhruvmanila on 2024-03-31 03:09_

---

_Branch deleted on 2024-03-31 03:09_

---

_Comment by @github-actions[bot] on 2024-03-31 03:28_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:907 on 2024-04-02 07:40_

Is it intentional that the display text is different from the token kind?

---

_@MichaReiser reviewed on 2024-04-02 07:43_

What's the motivation for implementing `Display` on `TokenKind`? Is it for better error messages? 

I'm asking because I agree that showing `.` instead of `Dot` is better, but I'm not sure if we have good display names for some of the tokens (e.g. `Name`, `Int`) and I would consider these names to be internal but not public terminology. 

Would it make sense to have a `as_str()` method that returns `Option<&str>` instead? Or is this not sufficient in case of the *Expected `x`, but at `y`* error message?

---

_@dhruvmanila reviewed on 2024-04-10 16:45_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token.rs`:907 on 2024-04-10 16:45_

No, just an oversight. I'll fix this.

---

_Comment by @dhruvmanila on 2024-04-10 16:48_

@MichaReiser 

> What's the motivation for implementing `Display` on `TokenKind`? Is it for better error messages?

Yes

> I'm asking because I agree that showing `.` instead of `Dot` is better, but I'm not sure if we have good display names for some of the tokens (e.g. `Name`, `Int`) and I would consider these names to be internal but not public terminology.
>
> Would it make sense to have a `as_str()` method that returns `Option<&str>` instead? Or is this not sufficient in case of the _Expected `x`, but at `y`_ error message?

Yeah, that could be done, we would need to fallback to the Debug implementation for the remaining tokens.


---
