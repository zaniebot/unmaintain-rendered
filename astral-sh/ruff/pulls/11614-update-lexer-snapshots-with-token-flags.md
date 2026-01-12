```yaml
number: 11614
title: Update lexer snapshots with token flags
type: pull_request
state: merged
author: dhruvmanila
labels:
  - testing
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/lexer-tests
created_at: 2024-05-30T08:27:08Z
updated_at: 2024-05-30T16:09:14Z
url: https://github.com/astral-sh/ruff/pull/11614
synced_at: 2026-01-12T15:55:38Z
```

# Update lexer snapshots with token flags

---

_@dhruvmanila_

## Summary

This PR updates the lexer test snapshots to include the `TokenFlags`.

## Test Plan

Verify the snapshots.


---

_Label `testing` added by @dhruvmanila on 2024-05-30 08:27_

---

_Comment by @codspeed-hq[bot] on 2024-05-30 08:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/lexer-tests)

### Merging #11614 will **not alter performance**

<sub>Comparing <code>dhruv/lexer-tests</code> (66953c5) with <code>dhruv/parser-phase-2</code> (d55bfcc)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@MichaReiser reviewed on 2024-05-30 08:31_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__empty_fstrings.snap`:49 on 2024-05-30 08:31_

what's the usage of the `FStringFlag`. Can't we tell that it is an `FString` by looking at the `TokenKind`?

---

_@dhruvmanila reviewed on 2024-05-30 08:34_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__empty_fstrings.snap`:49 on 2024-05-30 08:34_

Yeah, good point. Currently, it's only used inside the lexer to call either `lex_fstring_start` or `lex_string` method.

---

_@dhruvmanila reviewed on 2024-05-30 11:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:597 on 2024-05-30 11:24_

Set the token flags only if we know that it's `FStringEnd`.

---

_@dhruvmanila reviewed on 2024-05-30 11:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:720 on 2024-05-30 11:24_

Set the token flags only if we know that it's `FStringMiddle`.

---

_@dhruvmanila reviewed on 2024-05-30 11:35_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__empty_fstrings.snap`:49 on 2024-05-30 11:35_

I think it's required because we represent regular string with no string kind flag set. And, `prefix` method would also not be able to differentiate because the flags themselves don't have access to the token kind.

---

_Comment by @dhruvmanila on 2024-05-30 11:35_

(I'll look at the remaining test cases in a separate PR.)

---

_Marked ready for review by @dhruvmanila on 2024-05-30 11:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1905 on 2024-05-30 12:27_

Nit: At least debug_map etc. mutate the state in place. 
```suggestion
						if matches!(self.value, TokenValue::None) {
                tuple.field(&self.kind);
            } else {
                tuple.field(&self.value);
            };
						tuple.field(&self.range);
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1906 on 2024-05-30 12:28_

Nit: Maybe

```rust
if !self.flags.is_empty() {
	tuple.field(&self.flags);
}

tuple.finish()
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__lexer__tests__empty_fstrings.snap`:49 on 2024-05-30 12:31_

Hmm okay. I would need to check out the code to better understand it. We could move the `prefix` method to the `Token`? And could we only keep it for `FStringFlags`? But it's probably not that important.

---

_@MichaReiser approved on 2024-05-30 12:31_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1905 on 2024-05-30 15:11_

Are you suggesting to use `debug_map`? I want to keep the tuple here to have minimal diff which makes it easier to verify the changes. We can explore other formats as a follow-up.

---

_@dhruvmanila reviewed on 2024-05-30 15:11_

---

_Merged by @dhruvmanila on 2024-05-30 15:11_

---

_Closed by @dhruvmanila on 2024-05-30 15:11_

---

_Branch deleted on 2024-05-30 15:11_

---

_@MichaReiser reviewed on 2024-05-30 16:09_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1905 on 2024-05-30 16:09_

Oh no, I'm suggesting to not use `let mut tuple = tuple.field` because I think it might not be necessary (it's not necessary for `debug_map`)

---
