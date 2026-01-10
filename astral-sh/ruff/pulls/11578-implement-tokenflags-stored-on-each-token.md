```yaml
number: 11578
title: "Implement `TokenFlags` stored on each `Token`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/token-flags
created_at: 2024-05-28T06:32:36Z
updated_at: 2024-05-29T06:13:03Z
url: https://github.com/astral-sh/ruff/pull/11578
synced_at: 2026-01-10T21:56:00Z
```

# Implement `TokenFlags` stored on each `Token`

---

_Pull request opened by @dhruvmanila on 2024-05-28 06:32_

## Summary

This PR implements the `TokenFlags` which will be stored on each `Token` and certain flags will be set depending on the token kind. Currently, it's equivalent to `AnyStringFlags` but it will help in the future to provide additional information regarding certain tokens like unterminated string, number kinds, etc.

The main motivation to add a `TokenFlags` is to store certain information related to the token which will then be used by downstream tools. Currently, the information is only related to string tokens. The downstream tools should not be allowed access to the flags directly, it's an implementation detail. Instead, methods will be provided on `Token` to query certain information. An example can be seen in the follow-up PR (https://github.com/astral-sh/ruff/pull/11592).

For example, the `Stylist` and `Indexer` uses the string flags stored on `String`/`FStringStart` token to get certain information. They will be updated to use these flags instead, thus removing the need for `Tok` completely.

Prior art in TypeScript: https://github.com/microsoft/TypeScript/blob/16beff101ae1dae0600820ebf22632ac8a40cfc8/src/compiler/types.ts#L2788-L2827


---

_Label `parser` added by @dhruvmanila on 2024-05-28 06:32_

---

_@AlexWaygood reviewed on 2024-05-28 07:10_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/lexer.rs`:1390 on 2024-05-28 07:10_

I might be missing something, but it looks like this is identical to the `AnyStringFlags` type we already have in `nodes.rs` in terms of its interface and capabilities. Couldn't we just reuse that type? It was originally designed for use in the lexer

---

_@dhruvmanila reviewed on 2024-05-28 08:12_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1390 on 2024-05-28 08:12_

Yes, currently it is equivalent to the `AnyStringFlags` but we do plan to add additional flags which might not be related to a string.

---

_@AlexWaygood reviewed on 2024-05-28 08:19_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/lexer.rs`:1390 on 2024-05-28 08:19_

Got it, thanks!

---

_@dhruvmanila reviewed on 2024-05-28 09:44_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:40 on 2024-05-28 09:44_

These are here just to help me with finding the remaining references and will be removed at the end.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:161 on 2024-05-28 10:04_

We could go from `AnyStringPrefix` -> `TokenFlags` but that seemed like an unnecessary computation. So, instead of `char` -> `AnyStringPrefix` -> `TokenFlags`, we go directly from `char` -> `TokenFlags`.

---

_@dhruvmanila reviewed on 2024-05-28 10:04_

---

_@dhruvmanila reviewed on 2024-05-28 10:06_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:592 on 2024-05-28 10:06_

This means that all three f-string tokens (`FStringStart`, `FStringMiddle`, `FStringEnd`) will have the flag set without any additional cost.

---

_Renamed from "WIP: Implement `TokenFlags`" to "Implement `TokenFlags`" by @dhruvmanila on 2024-05-28 10:07_

---

_Renamed from "Implement `TokenFlags`" to "Implement `TokenFlags` stored on each `Token`" by @dhruvmanila on 2024-05-28 10:07_

---

_Marked ready for review by @dhruvmanila on 2024-05-28 10:10_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-28 10:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:146 on 2024-05-28 10:58_

Nit: I think I would move this method after `lex_identifier`. I was surprised to find this as the very first non-infrastructure method (maybe we should reorganize the lexer methods in a separate PR once we're done with your parser work. E.g. move `next_token` to the top, followed by `lex_token`).

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:146 on 2024-05-28 11:00_

Nit: You could consider changing the method to `Option<TokenFlags>` to remove the side effect from it. 

```rust
if let Some(prefix_flags) = self.try_single_char_prefix(first) {
	self.token_flags |= prefix_flags;
  self.lex_string(...)
} else {
	...
}
```

Although it might not be worth it...

---

_@MichaReiser approved on 2024-05-28 11:03_

Nice!

I think it would be helpful to add some additional context to the PR summary why we need this. What I understand is that we need it for some token based lint rules to remove the dependency on `Tok`.

---

_@dhruvmanila reviewed on 2024-05-29 05:56_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:146 on 2024-05-29 05:56_

> Although it might not be worth it...

Yeah, not sure if it's worth doing.

> Nit: I think I would move this method after `lex_identifier`. I was surprised to find this as the very first non-infrastructure method (maybe we should reorganize the lexer methods in a separate PR once we're done with your parser work. E.g. move `next_token` to the top, followed by `lex_token`).

Yes, I'm going to follow-up on this.

---

_Merged by @dhruvmanila on 2024-05-29 06:13_

---

_Closed by @dhruvmanila on 2024-05-29 06:13_

---

_Branch deleted on 2024-05-29 06:13_

---
