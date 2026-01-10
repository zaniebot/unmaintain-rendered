```yaml
number: 19053
title: "Combine `OldDiagnostic` and `Diagnostic`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: alex-brent/combine-diagnostics
created_at: 2025-06-30T19:44:00Z
updated_at: 2025-07-07T14:17:31Z
url: https://github.com/astral-sh/ruff/pull/19053
synced_at: 2026-01-10T18:33:12Z
```

# Combine `OldDiagnostic` and `Diagnostic`

---

_Pull request opened by @ntBre on 2025-06-30 19:44_

## Summary

This PR is a collaboration with @AlexWaygood from our pairing session last Friday.

The main goal here is removing `ruff_linter::message::OldDiagnostic` in favor of
using `ruff_db::diagnostic::Diagnostic` directly. This involved a few major steps:

- Transferring the fields
- Transferring the methods and trait implementations, where possible
- Converting some constructor methods to free functions
- Moving the `SecondaryCode` struct
- Updating the method names

I'm hoping that some of the methods, especially those in the `expect_ruff_*`
family, won't be necessary long-term, but I avoided trying to replace them
entirely for now to keep the already-large diff a bit smaller.

### Related refactors

Alex and I noticed a few refactoring opportunities while looking at the code,
specifically the very similar implementations for `create_parse_diagnostic`,
`create_unsupported_syntax_diagnostic`, and `create_semantic_syntax_diagnostic`.
We combined these into a single generic function, which I then copied into
`ruff_linter::message` with some small changes and a TODO to combine them in the
future.

I also deleted the `DisplayParseErrorType` and `TruncateAtNewline` types for
reporting parse errors. These were added in #4124, I believe to work around the
error messages from LALRPOP. Removing these didn't affect any tests, so I think
they were unnecessary now that we fully control the error messages from the
parser.

On a more minor note, I factored out some calls to the `OldDiagnostic::filename`
(now `Diagnostic::expect_ruff_filename`) function to avoid repeatedly allocating
`String`s in some places.

### Snapshot changes

The `show_statistics_syntax_errors` integration test changed because the
`OldDiagnostic::name` method used `syntax-error` instead of `invalid-syntax`
like in ty. I think this (`--statistics`) is one of the only places we actually
use this name for syntax errors, so I hope this is okay. An alternative is to
use `syntax-error` in ty too.

The other snapshot changes are from removing this code, as discussed on
[Discord](https://discord.com/channels/1039017663004942429/1228460843033821285/1388252408848847069):

https://github.com/astral-sh/ruff/blob/34052a1185392963c465737e9647f404abdfe8d5/crates/ruff_linter/src/message/mod.rs#L128-L135

I think both of these are technically breaking changes, but they only affect
syntax errors and are very narrow in scope, while also pretty substantially
simplifying the refactor, so I hope they're okay to include in a patch release.

## Test plan

Existing tests, with the adjustments mentioned above


---

_Label `internal` added by @ntBre on 2025-06-30 19:44_

---

_Label `diagnostics` added by @ntBre on 2025-06-30 19:44_

---

_Comment by @github-actions[bot] on 2025-06-30 19:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
-     memo fields = ~45MB
+     memo fields = ~41MB

aioredis (https://github.com/aio-libs/aioredis)
-     memo fields = ~54MB
+     memo fields = ~60MB

Expression (https://github.com/cognitedata/Expression)
-     memo fields = ~54MB
+     memo fields = ~49MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

bandersnatch (https://github.com/pypa/bandersnatch)
-     memo fields = ~66MB
+     memo fields = ~72MB

mkdocs (https://github.com/mkdocs/mkdocs)
-     memo fields = ~97MB
+     memo fields = ~106MB

tornado (https://github.com/tornadoweb/tornado)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~171MB
-     memo fields = ~129MB
+     memo fields = ~142MB

psycopg (https://github.com/psycopg/psycopg)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB

django-stubs (https://github.com/typeddjango/django-stubs)
- TOTAL MEMORY USAGE: ~189MB
+ TOTAL MEMORY USAGE: ~171MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~304MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-06-30 19:58_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex-brent%2Fcombine-diagnostics?runnerMode=WallTime)

### Merging #19053 will **not alter performance**

<sub>Comparing <code>alex-brent/combine-diagnostics</code> (46d690c) with <code>main</code> (352b896)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-06-30 19:59_

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

_Comment by @ntBre on 2025-06-30 21:00_

I'm not too sure about the failing benchmark, but I think this is ready for review otherwise.

---

_Marked ready for review by @ntBre on 2025-06-30 21:00_

---

_Review requested from @carljm by @ntBre on 2025-06-30 21:00_

---

_Review requested from @AlexWaygood by @ntBre on 2025-06-30 21:00_

---

_Review requested from @sharkdp by @ntBre on 2025-06-30 21:00_

---

_Review requested from @dcreager by @ntBre on 2025-06-30 21:00_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-30 21:00_

---

_Review requested from @dhruvmanila by @ntBre on 2025-06-30 21:00_

---

_Review request for @dcreager removed by @ntBre on 2025-06-30 21:01_

---

_Review request for @carljm removed by @ntBre on 2025-06-30 21:01_

---

_Review request for @sharkdp removed by @ntBre on 2025-06-30 21:01_

---

_Review requested from @BurntSushi by @ntBre on 2025-06-30 21:01_

---

_Comment by @AlexWaygood on 2025-06-30 21:01_

> I'm not too sure about the failing benchmark, but I think this is ready for review otherwise.

That one's been very flaky; I wouldn't worry about it

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/snapshots/ruff_linter__rules__flake8_implicit_str_concat__tests__ISC002_ISC_syntax_error.py.snap`:124 on 2025-07-01 08:46_

I know you mentioned this on Discord but for this specific error, I'm worried that the user experience will be bad because for a large file, all of the lines in the editor starting from the f-string start token will be underlined. I don't think we need to do anything here but just wanted to point this out.

---

_@dhruvmanila approved on 2025-07-01 08:48_

This looks like a pretty straightforward change to me but I'd prefer if someone else who's more familiar can take a look as well.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/snapshots/ruff_linter__rules__flake8_implicit_str_concat__tests__ISC002_ISC_syntax_error.py.snap`:124 on 2025-07-01 13:03_

Oh wow, that's a good point, I didn't realize this continued to the end of the file. I'm realizing that we could actually revert these since I couldn't combine the two `create_snapshot_diagnostic` functions anyway. That seems better for now, even if this matches ty's behavior.

---

_@ntBre reviewed on 2025-07-01 13:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/snapshots/ruff_linter__rules__flake8_implicit_str_concat__tests__ISC002_ISC_syntax_error.py.snap`:124 on 2025-07-01 13:08_

Oh, never mind. The current version is shared between error types, so adding this back would truncate other error ranges too. I guess I'll leave this as is for now.

---

_@ntBre reviewed on 2025-07-01 13:08_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:1393 on 2025-07-02 17:16_

It's interesting to me here that the name of this function is very specific, but the parameters are very generic. That is, there is really nothing specific about the parameters here that ties this to creating a syntax error diagnostic specifically. Does it make sense to rename this? Or even put it as a convenience constructor on `Diagnostic`?

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:1391 on 2025-07-02 17:17_

Should this be named `span`?

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:1393 on 2025-07-02 17:17_

Oh I see, this is still using `DiagnosticId::InvalidSyntax`. So maybe it's fine as-is.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/message/mod.rs`:50 on 2025-07-02 17:21_

Makes sense!

---

_@BurntSushi approved on 2025-07-02 17:24_

This all looks reasonable to me. I'm impressed at how few snapshot changes there are here. Nice work. :-)

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:1393 on 2025-07-03 04:14_

Ah I see what you mean. It's just a `DiagnosticId` and `Severity` away from constructing any diagnostic. I think I'll leave the arguments as-is for now if that's good with you, but it does sound nice to make it a constructor!

Do you think it would still be better to implement this on the syntax errors themselves, or should I delete this part of the comment too? 

```rust
/// This should _probably_ be a method on the syntax errors, but
/// at time of writing, `ruff_db` depends on `ruff_python_parser` instead of
/// the other way around. And since we want to do this conversion in a couple
/// places, it makes sense to centralize it _somewhere_. So it's here for now.
```

(It originally referred to `ParseError`s specifically, so I may have mangled it a bit already by making it refer to "syntax errors" instead)

---

_@ntBre reviewed on 2025-07-03 04:14_

---

_@BurntSushi reviewed on 2025-07-03 11:39_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:1393 on 2025-07-03 11:39_

I _think_ so, yeah. But I don't feel strongly at all. And doing that I think requires some refactoring of the module structure.

---

_@ntBre reviewed on 2025-07-03 13:08_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:1393 on 2025-07-03 13:08_

Sounds good, I'll keep the comment. Thanks for the review!

---

_Merged by @ntBre on 2025-07-03 17:01_

---

_Closed by @ntBre on 2025-07-03 17:01_

---

_Branch deleted on 2025-07-03 17:01_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/check.rs`:132 on 2025-07-07 08:59_

Not as part of this PR: But we should probably change the diagnostic here to use a `LintId::IOError` rather than a `LintName`

---

_@MichaReiser reviewed on 2025-07-07 08:59_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:368 on 2025-07-07 09:03_

We should alignt he naming here to `invalid_syntax`. This otherwise feels inconsistent. The same applies for the `syntax_error` constructor (we should probalby use `invalid_syntax` everywhere or rename `invalid_syntax` to `syntax_error` but I sort of prefer the former)

---

_@MichaReiser reviewed on 2025-07-07 09:03_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:383 on 2025-07-07 09:04_

This method should have a `ruff` prefix or does it work for ty too? If not, I'm sort of inclined to make it a standalone function instead.

---

_@MichaReiser reviewed on 2025-07-07 09:04_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:465 on 2025-07-07 09:06_

I think this implementation is different from ty's ordering. I'd suggest to remove the `Ord` implementation and instead add a `start_ordering(&self, other: &Diagnostic)` method that returns an `Ordering` which you can use in places where you used `Ord` before.

For example, there's `RenderingSortKey` below and it's unclear to users now why/how the two sortings differ

---

_@MichaReiser reviewed on 2025-07-07 09:06_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:454 on 2025-07-07 09:08_

Assuming that two diagnostics are Equal that have no range but come from different files seems incorrect.

---

_@MichaReiser reviewed on 2025-07-07 09:08_

---

_Review comment by @MichaReiser on `crates/ruff_db/Cargo.toml`:16 on 2025-07-07 09:10_

I think we should move `Fix` and `Edit` (and remove `ruff_diagnostics`?). But it was the right move to not do this as part of this PR

---

_@MichaReiser reviewed on 2025-07-07 09:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3161 on 2025-07-07 09:12_

I think I'd find it more idiomatic if `Violation` has a `into_diagnostic(TextRange, &SourceFile)` method. 

---

_@MichaReiser reviewed on 2025-07-07 09:12_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/mod.rs`:774 on 2025-07-07 09:18_

I'm slightly worried by all the `expect_range` calls because we'll very likely start adding diagnostics that don't have a start range (e.g. IOError!). I think it was the right call to use `expect_range` as part of this PR but it would be great if we could make (at least the production code) more forgiving if the source file or range is missing.

---

_@MichaReiser reviewed on 2025-07-07 09:18_

---

_@MichaReiser reviewed on 2025-07-07 09:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2510_invalid_characters_syntax_error.py.snap`:19 on 2025-07-07 09:18_

These are nice improvements!

---

_Review comment by @MichaReiser on `crates/ruff_linter/Cargo.toml`:18 on 2025-07-07 09:21_

Huh, what's the reason for changing to the version (I can't comment on line 3 for whatever reason)

---

_@MichaReiser reviewed on 2025-07-07 09:21_

---

_Comment by @MichaReiser on 2025-07-07 09:22_

This is great!

---

_@ntBre reviewed on 2025-07-07 12:28_

---

_Review comment by @ntBre on `crates/ruff_linter/Cargo.toml`:18 on 2025-07-07 12:28_

I'm not seeing a diff on line 3 of this file. Both versions have `0.12.1`. Is that what you mean?

---

_@MichaReiser reviewed on 2025-07-07 12:29_

---

_Review comment by @MichaReiser on `crates/ruff_linter/Cargo.toml`:18 on 2025-07-07 12:29_

Weird, maybe a bug in the online VS code diff viewer

---

_@ntBre reviewed on 2025-07-07 13:25_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:454 on 2025-07-07 13:25_

I agree, I just wasn't sure what to do about missing files/ranges. I took your other suggestion about `start_ordering`. Is it okay to `unwrap` those calls in Ruff, or should I still fall back to `Equal` just in case?

I also tried using `RenderingSortKey` directly, but I'm not sure we want to fall back to comparing the diagnostic IDs in Ruff. It changed a handful of snapshots when many diagnostics were on the same line.

---

_@ntBre reviewed on 2025-07-07 14:17_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:383 on 2025-07-07 14:17_

I'll try to resolve this one in my revisions to #19133.

---
