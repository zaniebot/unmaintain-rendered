```yaml
number: 14753
title: "Use `Invalid` context when keyword used as identifier"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
draft: true
base: main
head: dhruv/identifier-parse-result
created_at: 2024-12-03T11:12:44Z
updated_at: 2024-12-03T11:41:48Z
url: https://github.com/astral-sh/ruff/pull/14753
synced_at: 2026-01-12T15:55:48Z
```

# Use `Invalid` context when keyword used as identifier

---

_@dhruvmanila_

## Summary

This PR is related to #13778 and updates the return type of `parse_identifier` in the parser to return a result-like structure where both `Ok` and `Err` variant uses an `Identifier`. The type of the variant will indicate the type of `ExprContext` to use by the caller.

Previously, the `ctx` field on `ExprName` would only be set to `Invalid` when the identifier itself was invalid or missing. But, in case a keyword was found, then it would still set it to `Load` / `Store`. This becomes problematic because red knot will start displaying diagnostics for unresolved reference:

```py
# Name `pass` used when not defined
for x in pass:
	pass
```

This PR resolves that by setting the `Invalid` context even in those scenarios so that the semantic model will avoid recording a use of this "keyword as identifier".

## Test Plan

<!-- How was it tested? -->


---

_Label `internal` added by @dhruvmanila on 2024-12-03 11:12_

---

_Label `parser` added by @dhruvmanila on 2024-12-03 11:12_

---

_Comment by @MichaReiser on 2024-12-03 11:25_

> This PR resolves that by setting the Invalid context even in those scenarios so that the semantic model will avoid recording a use of this "keyword as identifier".

The downside of doing this will be that Red Knot won't be able to offer a refactoring to rename all uses of `pass` in identifier positions to a non-keyword name. 

---

_Comment by @MichaReiser on 2024-12-03 11:26_

For example, typescript reports two diagnostics for:

```
if (let > 0) {

}
```

```
    Identifier expected. 'let' is a reserved word in strict mode.
    Cannot find name 'let'.
```

Which I think is correct because I can now use the editor features to find all references to `let` or even jump to its definition and rename the variable and all its references using the rename refactor.

---

_Comment by @github-actions[bot] on 2024-12-03 11:33_

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

_Comment by @dhruvmanila on 2024-12-03 11:41_

> Which I think is correct because I can now use the editor features to find all references to `let` or even jump to its definition and rename the variable and all its references using the rename refactor.

Ok, I think that might be useful.

For context, I remember we discussed this in our 1:1 but I made this change quite a while ago. When I wrote some tests in #14754, I saw those unresolved reference diagnostics which I didn't find helpful.

I'll close this for now, we can revisit if there's any requirement.

---

_Closed by @dhruvmanila on 2024-12-03 11:41_

---
