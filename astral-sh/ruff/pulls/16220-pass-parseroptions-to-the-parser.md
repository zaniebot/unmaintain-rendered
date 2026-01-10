```yaml
number: 16220
title: "Pass `ParserOptions` to the parser"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: brent/pyfilesource
created_at: 2025-02-17T22:25:43Z
updated_at: 2025-02-19T15:52:25Z
url: https://github.com/astral-sh/ruff/pull/16220
synced_at: 2026-01-10T19:57:23Z
```

# Pass `ParserOptions` to the parser

---

_Pull request opened by @ntBre on 2025-02-17 22:25_

## Summary

This is part of the preparation for detecting syntax errors in the parser from https://github.com/astral-sh/ruff/pull/16090/. As suggested in [this comment](https://github.com/astral-sh/ruff/pull/16090/#discussion_r1953084509), I started working on a `ParseOptions` struct that could be stored in the parser. For this initial refactor, I only made it hold the existing `Mode` option, but for syntax errors, we will also need it to have a `PythonVersion`. For that use case, I'm picturing something like a `ParseOptions::with_python_version` method, so you can extend the current calls to something like

```rust
ParseOptions::from(mode).with_python_version(settings.target_version)
```

But I thought it was worth adding `ParseOptions` alone without changing any other behavior first.

Most of the diff is just updating call sites taking `Mode` to take `ParseOptions::from(Mode)` or those taking `PySourceType`s to take `ParseOptions::from(PySourceType)`. The interesting changes are in the new `parser/options.rs` file and smaller parts of `parser/mod.rs` and `ruff_python_parser/src/lib.rs`.

<details>
<summary>Outdated implementation details for future reference</summary>

NOTE: as the `details` summary says, this does *not* correspond to the current implementation, which preserves the original signature of `parse_unchecked_source` and uses a much simpler representation of `ParseOptions`. This is preserved just for historical purposes.

The `ParserOptions` implementation is complicated a bit by wanting to preserve the `SAFETY` comment in `parse_unchecked_source`:

https://github.com/astral-sh/ruff/blob/b5cd4f2f70408b8ba2ebd32e554d0fef2472e9c2/crates/ruff_python_parser/src/lib.rs#L289-L295

The only way I could see to enforce this while passing a `ParserOptions` instead of a `PySourceType` was by adding a generic type state to `ParserOptions`, so that `parse_unchecked_source` would take a `ParserOptions<KnownSource>` constructed by `ParserOptions::from_source_type`, as opposed to the `ParserOptions<UnknownSource>` produced by `ParserOptions::from_mode`.

On top of that, I didn't want to propagate the generics from `ParserOptions` to all uses of `Parser`, so there's another indirection through the `AsParserOptions` trait, which is used by `Parser` to hold a `Box<dyn AsParserOptions>` instead of a `ParserOptions<S: SourceType>`.

Finally, other public functions in the parser crate could be updated to take `ParserOptions` if we want, but `parse_unchecked_source` is the variant that ruff and red-knot call where I want to integrate the syntax error checks, so it was my main priority.
</details>

## Test Plan

Existing tests, this should not change any behavior.


---

_Label `internal` added by @ntBre on 2025-02-17 22:25_

---

_Review requested from @carljm by @ntBre on 2025-02-17 22:25_

---

_Review requested from @MichaReiser by @ntBre on 2025-02-17 22:25_

---

_Review requested from @AlexWaygood by @ntBre on 2025-02-17 22:25_

---

_Review requested from @sharkdp by @ntBre on 2025-02-17 22:25_

---

_Review requested from @dhruvmanila by @ntBre on 2025-02-17 22:25_

---

_Comment by @github-actions[bot] on 2025-02-17 22:35_

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

_@ntBre reviewed on 2025-02-17 22:37_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/options.rs`:42 on 2025-02-17 22:37_

This obviously doesn't need to be `pub(crate)`, I must have missed it when extracting the module. I'll fix this locally but hold off on pushing just to save some CI time.

---

_Comment by @dhruvmanila on 2025-02-18 06:34_

> The only way I could see to enforce this while passing a `ParserOptions` instead of a `PySourceType` was by adding a generic type state to `ParserOptions`, so that `parse_unchecked_source` would take a `ParserOptions<KnownSource>` constructed by `ParserOptions::from_source_type`, as opposed to the `ParserOptions<UnknownSource>` produced by `ParserOptions::from_mode`.

What's the motivation for changing the function signature for `parse_unchecked_source`? I think we should keep it as is and accept `PySourceType` which is then internally converted into a `ParserOptions`. This way we can maintain the invariant that `PySourceType` always parses into a `ModModule`. This will then avoid the need for `KnownSource`.

---

I've some additional thoughts that are related to this:
* I'm wondering if we should abstract away the `Mode` parameter i.e., replace `mode: Mode` with `options: ParserOptions` in `parse` and `parse_unchecked` functions
* We could possibly remove `Mode::Ipython` and use `PySourceType::Ipynb` instead because the `ParserOptions` will contain it and it seems a bit redundant that both `Mode` and `PySourceType`, which the parser and lexer have access to, contains an indicator that we're in a notebook context.
* I think we should also pass the options to the `Lexer`. I don't think the `TokenSource` needs any info from it but even if it did, it can just ask the lexer.

Curious to hear @MichaReiser thoughts on this.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:64 on 2025-02-18 07:57_

Nit: I'd add the `options` last because the `start_offset` seems more important (given that the signature is called `new_starts_at`). The other reason is that the `options` are "optional" (We would assign `ParserOptions::default` if Rust ever gets default parameters)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:74 on 2025-02-18 07:59_

What's the reason for boxing the options? I don't think the size of `Parser` matters much -- we only create very few instances -- but we'll access the values rather frequently.

Oh, I see. It's because you store a `AsParserOptions` trait. What's the motivation for having a trait... I think I need to take a closer look at `ParserOptions`

---

_@MichaReiser reviewed on 2025-02-18 08:03_

I agree with @dhruvmanila that I wouldn't make `ParserOptions` generic or add `ParserOptions` to `parse_unchecked_source`. It unnecessarily complicates things internally. 

We could consider adding a `ParseSourceOptions` struct which is `ParserOptions` with the source type prop omitted, but I'm not sure if it's worth exposing the options on `parse_unchecked_source`. Instead, advanced use cases should use `pare_unchecked` and then `map/unwrap` on the call site. 


I think it could make sense to expose the `ParserOptions` to the `Lexer`, or at least a subset of the options but I think this is something we can tackle separately. 

---

_Comment by @AlexWaygood on 2025-02-18 11:40_

I'll leave this one to Micha/Dhruv :-)

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-02-18 11:40_

---

_Comment by @ntBre on 2025-02-18 13:47_

The motivation for changing the signature of `parse_unchecked_source` in particular is that it's called by [ruff](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/linter.rs#L495) and by [red-knot](https://github.com/astral-sh/ruff/blob/main/crates/ruff_db/src/parsed.rs#L40) where I wanted to inject the Python version for syntax error detection. I wanted to avoid moving the `unwrap` call and accompanying `SAFETY` comments to those call sites, but that could certainly be preferable to the complication of the generics here.

And the `Box<dyn AsParserOptions>` follows from that to erase the generic on `ParserOptions` to avoid making every `Parser` a `Parser<T>`. That would obviously go away if `ParserOptions` were not generic.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/mod.rs`:533 on 2025-02-18 20:22_

Nit: We could implement `from_source_type` (or `From<SourceType> for ParserOptions`)

---

_Comment by @ntBre on 2025-02-18 20:23_

I've reverted the generic stuff and put `source_type` back in `parse_unchecked_source`. I can just change ruff and red-knot to call and unwrap `parse_unchecked` when I actually integrate the syntax error checks.

I also changed `parse` and `parse_unchecked` to take `ParserOptions` instead of `Mode`, as  suggested. That will work nicely with the point above.

> We could possibly remove Mode::Ipython and use PySourceType::Ipynb instead because the ParserOptions will contain it and it seems a bit redundant that both Mode and PySourceType, which the parser and
lexer have access to, contains an indicator that we're in a notebook context.

I looked into this briefly, but I don't think the lexer currently has access to a `PySourceType`, so this might make more sense to combine with passing other options to the lexer too.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/mod.rs`:517 on 2025-02-18 20:25_

I'd prefer `ParseOptions` over `ParserOptions` to align it with `FormatOptions` and because its the options passed to the `parse_*` methods (the parser is an internal abstraction)

---

_@MichaReiser approved on 2025-02-18 20:25_

Sweet. I recommend giving @dhruvmanila some time to review the PR too.

---

_@ntBre reviewed on 2025-02-18 20:31_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/mod.rs`:533 on 2025-02-18 20:31_

Okay, I was thinking about that too. Should I implement `From<Mode> for ParseOptions` too? I guess I was thinking we might have multiple `from_*` methods, but I'm realizing now that they won't actually conflict with each other.

---

_@ntBre reviewed on 2025-02-18 20:35_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/mod.rs`:533 on 2025-02-18 20:35_

This gives me more to put in the separate `options.rs` file anyway. I thought it looked a bit sparse before 😄 

---

_@MichaReiser reviewed on 2025-02-18 20:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/mod.rs`:533 on 2025-02-18 20:39_

I have a love-hate relationship with `From` and I've been very inconsistent in the past with having explicit `from_` methods vs `From` implementations... 

I do like that `from_mode` is explicit but it also is a bit verbose... 🤷 

---

_@ntBre reviewed on 2025-02-18 21:10_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/comments/mod.rs`:533 on 2025-02-18 21:10_

I know what you mean. I already switched to `From` immediately after commenting, but I'm happy to go back to the explicit versions if you and/or Dhruv prefer. I think I tend toward `from_` methods usually, so I can really go either way.

---

_@MichaReiser reviewed on 2025-02-18 21:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/mod.rs`:533 on 2025-02-18 21:32_

I defer to you and @dhruvmanila 

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/comments/mod.rs`:533 on 2025-02-19 05:16_

I think `From` is fine. In the future, when there are more options and if it turns out a bit difficult to read, we could switch to `from_` methods. Another option would be to use a builder pattern with the default impl so that it reads as `ParseOptions::default().with_mode(mode).with_python_version(version)`.

---

_@dhruvmanila reviewed on 2025-02-19 05:16_

---

_@dhruvmanila approved on 2025-02-19 05:19_

LGTM! I'd recommend updating the PR description to reflect the current state of the PR for future readers. We can keep the "Details" content as a collapsed section for historical purposes.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/options.rs`:8 on 2025-02-19 05:20_

nit: I think I'd remove this from public docs as it's mainly an implementation detail

---

_@dhruvmanila reviewed on 2025-02-19 05:20_

---

_Label `parser` added by @dhruvmanila on 2025-02-19 05:21_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/options.rs`:8 on 2025-02-19 12:46_

Nice catch. Hopefully that will be outdated very soon anyway!

---

_@ntBre reviewed on 2025-02-19 12:46_

---

_Merged by @ntBre on 2025-02-19 15:50_

---

_Closed by @ntBre on 2025-02-19 15:50_

---

_Branch deleted on 2025-02-19 15:50_

---

_Comment by @ntBre on 2025-02-19 15:52_

I forgot to update the `ParserOptions` in the title :facepalm: but I did update the description!

---
