```yaml
number: 11441
title: Add checkpoint logic for the parser
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/parser-checkpoint
created_at: 2024-05-16T09:29:01Z
updated_at: 2024-05-27T15:55:02Z
url: https://github.com/astral-sh/ruff/pull/11441
synced_at: 2026-01-12T15:55:38Z
```

# Add checkpoint logic for the parser

---

_@dhruvmanila_

## Summary

This PR adds the checkpoint logic to the parser and token source. It also updates the lexer checkpoint to contain the error position.


---

_Marked ready for review by @dhruvmanila on 2024-05-17 11:37_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-17 11:37_

---

_Merged by @dhruvmanila on 2024-05-17 11:37_

---

_Closed by @dhruvmanila on 2024-05-17 11:37_

---

_Branch deleted on 2024-05-17 11:37_

---

_@MichaReiser reviewed on 2024-05-27 09:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:653 on 2024-05-27 09:19_

Nit: I like to use destructuring here. It adds a compile time guarantee that all fields are used (at the cost of a bit more code)

```rust
let ParserCheckpoint { 
	tokens, 
	errors_position,
	prev_token_end,
	recovery_context
} = checkpoint; 

self.tokens.rewind(tokens);
self.errors.truncate(errors_position);
...
```

You can also do the destructuring right in the method signature, although it then becomes a bit hard to read.

---

_@dhruvmanila reviewed on 2024-05-27 15:55_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:653 on 2024-05-27 15:55_

Yeah, I recently realized that with the amount of fields increasing in the lexer checkpoint and will be making the changes in a follow-up PR. Thanks for the recommendation.


---
