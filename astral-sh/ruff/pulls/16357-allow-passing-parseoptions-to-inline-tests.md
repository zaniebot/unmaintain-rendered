```yaml
number: 16357
title: "Allow passing `ParseOptions` to inline tests"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: brent/parser-tests
created_at: 2025-02-24T19:50:13Z
updated_at: 2025-02-27T15:23:18Z
url: https://github.com/astral-sh/ruff/pull/16357
synced_at: 2026-01-12T15:55:54Z
```

# Allow passing `ParseOptions` to inline tests

---

_@ntBre_

## Summary

This PR adds support for a pragma-style header for inline parser tests containing JSON-serialized `ParseOptions`. For example,

```python
# parse_options: { "target-version": "3.9" }
match 2:
    case 1:
        pass
```

The line must start with `# parse_options: ` and then the rest of the (trimmed) line is deserialized into `ParseOptions` used for parsing the the test.

## Test Plan

Existing inline tests, plus two new inline tests for `match-before-py310`.


---

_Label `internal` added by @ntBre on 2025-02-24 19:50_

---

_Label `parser` added by @ntBre on 2025-02-24 19:50_

---

_Review requested from @MichaReiser by @ntBre on 2025-02-24 19:50_

---

_Review requested from @dhruvmanila by @ntBre on 2025-02-24 19:50_

---

_Comment by @github-actions[bot] on 2025-02-24 19:59_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:2272 on 2025-02-25 07:55_

I find the options json files somewhat noisy. I know, we do the same for the formatter and maybe that was already a bad choice from my side. 

Should we use a pragma comment instead?

```python
# parserOptions: { target_version: "3.10" }
```

and they are only allowed on the first line of the test.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@match_before_py310.py.snap`:63 on 2025-02-25 07:56_

```suggestion
  | |____________^ Syntax Error: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/Cargo.toml`:25 on 2025-02-25 07:57_

We should make `serde` an optional dependency. Not all downstream users need serde serialization

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/Cargo.toml`:16 on 2025-02-25 07:57_

We should only enable the `serde` feature for dev builds

---

_@MichaReiser reviewed on 2025-02-25 07:57_

This looks good. I do think I'd prefer a pragma comment over an external `options.json` file

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/fixtures.rs`:123 on 2025-02-25 09:57_

I think this is great and I'd also add this conditional check for `ParseError` so that we avoid adding an empty header in that case as well. Thus, I'd suggest to remove this TODO.

What about using the same name as the field? Like "Syntax Errors" or "Unsupported syntax errors"?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/fixtures.rs`:91 on 2025-02-25 10:03_

I think it'll be useful to add the non-default options in the snapshot. We do this in the formatter:

https://github.com/astral-sh/ruff/blob/aac79e453aeb77990bfcccc715a91870363523c3/crates/ruff_python_formatter/tests/snapshots/format%40statement__with_39.py.snap#L101-L115

It doesn't necessarily need to be in a pretty format and can directly use `{:#?}` instead.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:2272 on 2025-02-25 10:07_

Are you saying that the comment would be part of the source code? Like the following:
```py
# parse_options: { target_version: "3.10" }
match 2:
	case 1:
		pass
```

That is a good alternative but then I would recommend (not in this PR) to add the file content (if it's not too noisy) in the snapshot to make sure that the snapshot contains all the necessary details (content, parse options) to validate it.

---

_@dhruvmanila reviewed on 2025-02-25 10:08_

This is great! Thanks for taking this on quickly and implementing it.

---

_@dhruvmanila reviewed on 2025-02-25 10:08_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/fixtures.rs`:91 on 2025-02-25 10:08_

This won't be required if we go with the pragma comment instead of `.options.json`

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@match_before_py310.py.snap`:63 on 2025-02-25 15:27_

I'm going to apply this one to #16090.

---

_@ntBre reviewed on 2025-02-25 15:27_

---

_@ntBre reviewed on 2025-02-25 15:35_

---

_Review comment by @ntBre on `crates/ruff_python_parser/Cargo.toml`:16 on 2025-02-25 15:35_

I tried this a couple of times, but making `serde` a `dev-dependency` led to errors like `Deserialize is not implemented` in the integration tests. From some searching online, I think integration tests don't activate the `test` feature, so `#[cfg_attr(test, derive(serde::Deserialize))]` doesn't work, for example.

I'll give it another try in case I'm missing something here, though. I definitely agree that I'd rather not make it a full dependency. Maybe I can just add a `serde` feature to the parser?

---

_@ntBre reviewed on 2025-02-25 15:36_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:2272 on 2025-02-25 15:36_

I like this idea, I'll give it a try!

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/Cargo.toml`:16 on 2025-02-25 16:18_

I see. Yeah, a feature might work but you probably still need to enable that feature for the one integration test.

---

_@MichaReiser reviewed on 2025-02-25 16:18_

---

_Comment by @codspeed-hq[bot] on 2025-02-25 18:08_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fparser-tests)

### Merging #16357 will **not alter performance**

<sub>Comparing <code>brent/parser-tests</code> (62b43a7) with <code>main</code> (dd6f623)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Comment by @ntBre on 2025-02-25 18:09_

I rebased on #16090 to get the new `SyntaxError` name and switched to the pragma implementation. It seems quite nice and ended up being less code too. The only piece I might be missing is including the code file contents in the snapshot, but I wasn't quite sure if that was still needed. It sounds like that could be a follow-up either way.

I made `serde` an optional dependency gated behind the `serde` feature in the parser. You have to pass `--features serde` to run the parser tests with something like `cargo test -p ruff_python_parser --features serde`, but I think CI runs everything with `--all-features`. This feels a bit hacky but maybe it's okay? I left that commit last so it would be easy to revert.

---

_Comment by @ntBre on 2025-02-25 18:24_

My other branch must be missing Doug's alist changes, at least I assume that's where the codspeed failure is coming from. I expect that to go away when I change the base branch to `main`.

---

_Comment by @ntBre on 2025-02-26 00:10_

While trying to use this branch as a base for detecting some syntax errors, I realized that I forgot to actually check for the new errors in the *valid* syntax case. It's difficult to test this case because it requires a version-related syntax error in a `test_ok` inline test. I guess a `#[should_panic]` test calling `test_valid_syntax` directly could work, but I can also confirm that these changes now detect the false-positive I was debugging on the other branch.

---

_Review requested from @AlexWaygood by @ntBre on 2025-02-26 04:03_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:667 on 2025-02-26 07:23_

We normally use kebab case everywhere

```suggestion
    serde(rename_all = "kebab_case")
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:216 on 2025-02-26 07:26_

I don't understand this change. Why are we appending the `unsupported_syntax_errors` on line 189 and here?

---

_@MichaReiser approved on 2025-02-26 07:26_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:667 on 2025-02-26 08:29_

I think it's `kebab-case` (hyphen, not an underscore) (https://serde.rs/container-attrs.html#rename_all)

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:216 on 2025-02-26 08:31_

I think this PR probably just needs to be rebased on `main`? 

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/options.rs`:24 on 2025-02-26 08:34_

I'd use `#[serde(rename_all = "kebab-case")]` for consistency

---

_@dhruvmanila approved on 2025-02-26 08:36_

---

_@ntBre reviewed on 2025-02-26 13:39_

---

_Review comment by @ntBre on `crates/ruff_server/src/lint.rs`:216 on 2025-02-26 13:39_

Oh yes, I was using git too late last night!

---

_Comment by @ntBre on 2025-02-26 15:27_

Based on a discussion with @MichaReiser on Discord, I backed out the `serde` feature changes and added `JsonParseOptions` and `JsonMode` types in `fixtures.rs` to keep `serde` as a `dev-dependency`.

This avoids needing to run the parser integration tests with the `serde` feature enabled or requiring `serde` as a full dependency.

---

_Merged by @ntBre on 2025-02-27 15:23_

---

_Closed by @ntBre on 2025-02-27 15:23_

---

_Branch deleted on 2025-02-27 15:23_

---
