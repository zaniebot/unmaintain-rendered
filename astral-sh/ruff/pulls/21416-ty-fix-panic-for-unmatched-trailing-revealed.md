```yaml
number: 21416
title: "[ty] Fix panic for unmatched trailing revealed assertions in mdtests"
type: pull_request
state: merged
author: Hugo-Polloli
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: fix-mdtest-revealed-line
created_at: 2025-11-12T23:14:48Z
updated_at: 2025-11-16T08:42:39Z
url: https://github.com/astral-sh/ruff/pull/21416
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Fix panic for unmatched trailing revealed assertions in mdtests

---

_Pull request opened by @Hugo-Polloli on 2025-11-12 23:14_

## Summary

- Prevent mdtest from panicking when an inline assertion refers to a non-existent line (e.g., a trailing `# revealed: ...` with no matching `reveal_type`)
- `EmbeddedFileSourceMap::to_absolute_line_number` now returns `Result` with a clear diagnostic, plus a helper to report the last valid embedded code line
- Mdtest CLI/GitHub output surfaces both the original assertion failure and the new explanation, and parser unit tests cover both the success and error paths

Fixes [ty#1512](https://github.com/astral-sh/ty/issues/1512).

## Test Plan

```shell
cargo clippy --workspace --all-targets --all-features -- -D warnings  # Rust linting
cargo test  # Rust testing
uvx pre-commit run --all-files --show-diff-on-failure  # Rust and Python formatting, Markdown and Python linting, etc...
``` 
Everything ok except the 2 mentioned in #10084 but on WSL so no issue here

---

_Review requested from @carljm by @Hugo-Polloli on 2025-11-12 23:14_

---

_Review requested from @MichaReiser by @Hugo-Polloli on 2025-11-12 23:14_

---

_Review requested from @AlexWaygood by @Hugo-Polloli on 2025-11-12 23:14_

---

_Review requested from @sharkdp by @Hugo-Polloli on 2025-11-12 23:14_

---

_Review requested from @dcreager by @Hugo-Polloli on 2025-11-12 23:14_

---

_Label `testing` added by @AlexWaygood on 2025-11-12 23:24_

---

_Label `ty` added by @AlexWaygood on 2025-11-12 23:24_

---

_Review comment by @MichaReiser on `crates/ty_test/src/lib.rs`:112 on 2025-11-13 08:21_

Nit: to reduce code duplication, I suggest adding a method to `OutputFormat` to print an error


```rust
impl OutputFormat {
	fn print_error(self, file: &str, line: Option<OneIndexed>, failure: impl Display) {
		match self {
			Self::Cli => {
				let cli_line_info = match absolute_line_number_value {
		        Some(line) => format!("{relative_fixture_path}:{line}"),
		        None => format!("{relative_fixture_path}:?"),
		    };
				println!(" {} {failure}", cli_line_info.as_str().cyan());
			}
			Self::GitHub => {
				if let Some(line) = absolute_line_number_value {
					println!("::error file={absolute_fixture_path},line={line}::{err}");
				} else {
					println!("::error file={absolute_fixture_path}::{err}");
				}
			}
		}
	}
}

---

_Review comment by @MichaReiser on `crates/ty_test/src/parser.rs`:279 on 2025-11-13 08:25_

I think I would change this to be an `std::result::Result<OneIndexed, OneIndexed>` where
the error case returns the last line number (`last_absolute_line_number`). 

This leaves deciding on what error message to use up to the caller

---

_Review comment by @MichaReiser on `crates/ty_test/src/lib.rs`:122 on 2025-11-13 08:26_

I'm slightly leaning towards skipping the `failures` if the line number can't be mapped because I suspect it leads to slightly simpler code but I'm curious to hear why you decide to emit them even if the file mapping failed.

---

_@MichaReiser reviewed on 2025-11-13 08:26_

Thank you.

this overall looks great. I've a few smaller comments.

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 08:28_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-13 08:29_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-11-13 14:30_

---

_Review comment by @MichaReiser on `crates/ty_test/src/lib.rs`:97 on 2025-11-13 14:30_

Can you help me understand how `total_relative_line_count` is different from `last_absolute_line_number`?

---

_Review comment by @Hugo-Polloli on `crates/ty_test/src/lib.rs`:112 on 2025-11-13 23:27_

Thanks for the implementation, I used it as is

---

_@Hugo-Polloli reviewed on 2025-11-13 23:27_

---

_@Hugo-Polloli reviewed on 2025-11-13 23:27_

---

_Review comment by @Hugo-Polloli on `crates/ty_test/src/parser.rs`:279 on 2025-11-13 23:27_

Oh yes, much cleaner, done

---

_@Hugo-Polloli reviewed on 2025-11-13 23:29_

---

_Review comment by @Hugo-Polloli on `crates/ty_test/src/lib.rs`:122 on 2025-11-13 23:29_

No real reason haha, to be honest I kinda emitted them "by default" and did not think about just not skipping them, done

---

_Review comment by @Hugo-Polloli on `crates/ty_test/src/lib.rs`:97 on 2025-11-13 23:43_

This was supposed to be complementary information, `total_relative_line_count` showing the number of lines in the embedded snippet, `relative_line_number` being in our case equal to `total_relative_line_count + 1`, showing that the assertion engine tried to find a line that just does not exist due to the trailing assertion comment.
Decided to discard it because `last_absolute_line_number` and the error message are plenty enough information for the user to find what's happening

(Separate process question: when I’m still iterating on feedback and not ready for another review yet, is it preferable to convert the PR to a draft?)

btw the changes in [suggestions follow-up](https://github.com/astral-sh/ruff/pull/21416/commits/4170fa988fbc3a7c9f9a0cc3074d7a7b05b3a4d1) commit are, this time, ready for review, I changed the error message to be more generic than just about revealed, because this happens for any type of assertion comment

---

_@Hugo-Polloli reviewed on 2025-11-13 23:43_

---

_@MichaReiser approved on 2025-11-16 08:29_

Thank you

---

_Renamed from "[ty] Provide proper error on dangling revealed" to "[ty] Fix panic for unmatched trailing revealed assertions in mdtests" by @MichaReiser on 2025-11-16 08:30_

---

_Merged by @MichaReiser on 2025-11-16 08:34_

---

_Closed by @MichaReiser on 2025-11-16 08:34_

---

_Comment by @codspeed-hq[bot] on 2025-11-16 08:42_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Hugo-Polloli%3Afix-mdtest-revealed-line?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21416 will **improve performances by 4.39%**

<sub>Comparing <code>Hugo-Polloli:fix-mdtest-revealed-line</code> (8330a21) with <code>main</code> (3065f8d)</sub>



### Summary

`⚡ 1` improvement  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/Hugo-Polloli%3Afix-mdtest-revealed-line?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 21.6 s | 20.6 s | +4.39% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/Hugo-Polloli%3Afix-mdtest-revealed-line?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---
