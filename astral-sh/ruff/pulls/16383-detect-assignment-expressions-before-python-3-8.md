```yaml
number: 16383
title: Detect assignment expressions before Python 3.8
type: pull_request
state: merged
author: ntBre
labels:
  - preview
assignees: []
merged: true
base: main
head: brent/syntax-walrus-38
created_at: 2025-02-25T20:31:53Z
updated_at: 2025-02-28T22:13:49Z
url: https://github.com/astral-sh/ruff/pull/16383
synced_at: 2026-01-10T19:49:01Z
```

# Detect assignment expressions before Python 3.8

---

_Pull request opened by @ntBre on 2025-02-25 20:31_

## Summary
This PR is the first in a series derived from https://github.com/astral-sh/ruff/pull/16308, each of which add support
for detecting one version-related syntax error from https://github.com/astral-sh/ruff/issues/6591. This one should be
the largest because it also includes a couple of additional changes:
1. the `syntax_errors!` macro, which makes adding more variants a bit easier
2. the `Parser::add_unsupported_syntax_error` method

Otherwise I think the general structure will be the same for each syntax error:
* Detecting the error in the parser
* Inline parser tests for the new error
* New ruff CLI tests for the new error

Because of the second point here, this PR is currently stacked on https://github.com/astral-sh/ruff/pull/16357.

## Test Plan
As noted above, there are new inline parser tests, as well as new ruff CLI
tests. Once https://github.com/astral-sh/ruff/pull/16379 is resolved, there should also be new mdtests for red-knot,
but this PR does not currently include those.


---

_Comment by @github-actions[bot] on 2025-02-25 20:40_

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

_Label `preview` added by @ntBre on 2025-02-27 21:24_

---

_Marked ready for review by @ntBre on 2025-02-27 21:24_

---

_Review requested from @MichaReiser by @ntBre on 2025-02-27 21:24_

---

_Review requested from @dhruvmanila by @ntBre on 2025-02-27 21:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:478 on 2025-02-28 08:07_

I think I prefer not to use a macro here because it makes the code much harder to read (I rather have some repetition) and more difficult to extend in the future. 

It also doesn't seem that the macro helps to avoid much repetition. Each information provided to the macro is only used once:

```rust
#[derive(Debug, PartialEq, Clone, Copy)]
pub enum UnsupportedSyntaxErrorKind {
    MatchBeforePy310,
    WalrusBeforePy38,
}

impl Display for UnsupportedSyntaxError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        let kind = match self.kind {
            UnsupportedSyntaxErrorKind::MatchBeforePy310 => "`match` statement",
            UnsupportedSyntaxErrorKind::WalrusBeforePy38 => "named assignment expression (`:=`)",
        };

        write!(
            f,
            "Cannot use {kind} on Python {} (syntax was added in Python {})",
            self.target_version,
            self.minimum_version(),
        )
    }
}

impl UnsupportedSyntaxError {
    /// The earliest allowed version for the syntax associated with this error.
    pub const fn minimum_version(&self) -> PythonVersion {
        match self.kind {
            UnsupportedSyntaxErrorKind::MatchBeforePy310 => PythonVersion::PY310,
            UnsupportedSyntaxErrorKind::WalrusBeforePy38 => PythonVersion::PY38,
        }
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:482 on 2025-02-28 08:08_

I think I'd name the variants `Match` and `Walrus` because the *Before<Version>* part is covered by the `minimum_version` method. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:2170 on 2025-02-28 08:09_

```suggestion
        // test_ok walrus_py38
```

---

_@MichaReiser approved on 2025-02-28 08:09_

---

_Review comment by @dhruvmanila on `crates/ruff/tests/lint.rs`:2639 on 2025-02-28 08:26_

We can just use `(x := 1)` to avoid unrelated diagnostics in the snapshot. Same for other test cases.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/error.rs`:478 on 2025-02-28 08:28_

Additionally, not using a macro also has the possible benefit to customize the entire error message and not rely on a template.

---

_@dhruvmanila approved on 2025-02-28 08:31_

---

_@ntBre reviewed on 2025-02-28 18:28_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:478 on 2025-02-28 18:28_

That's fair. I liked the little table of errors at the bottom, but I've already been annoyed that my goto-definition was jumping into the macro. I'll revert this.

Your variant name suggestions will also make them a bit less repetitive to begin with!

---

_@ntBre reviewed on 2025-02-28 18:32_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:2170 on 2025-02-28 18:32_

I changed the `test_err` version to `walrus_py37` too.

---

_Merged by @ntBre on 2025-02-28 22:13_

---

_Closed by @ntBre on 2025-02-28 22:13_

---

_Branch deleted on 2025-02-28 22:13_

---
