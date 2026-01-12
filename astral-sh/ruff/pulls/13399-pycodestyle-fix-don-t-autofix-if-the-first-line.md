```yaml
number: 13399
title: "[pycodestyle] Fix: Don't autofix if the first line ends in a question mark? (D400)"
type: pull_request
state: merged
author: yahayaohinoyi
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: d400-not-autofix-some-chars
created_at: 2024-09-18T22:28:06Z
updated_at: 2024-09-20T11:14:43Z
url: https://github.com/astral-sh/ruff/pull/13399
synced_at: 2026-01-12T15:55:44Z
```

# [pycodestyle] Fix: Don't autofix if the first line ends in a question mark? (D400)

---

_@yahayaohinoyi_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/13365

> The autofix for D400 currently changes this:
> 
> ```python
> def should_frob() -> bool:
>     """Should we frob the bar?"""
> ```
> 
> To this:
> 
> ```python
> def should_frob() -> bool:
>     """Should we frob the bar?."""
> ```
> 
> Which seems pretty clearly worse! We probably just shouldn't offer an autofix if the first line ends in a question mark or exclamation mark?

## DESCRIPTION

Basically, we don't want a diagnostic when we don't set a fix for this rule https://github.com/astral-sh/ruff/blob/8b3da1867e02eb38c34b11b90969662f4506a65b/crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_period.rs#L113. 

## Test Plan

Holistically followed the instructions here https://docs.astral.sh/ruff/contributing/#rule-testing-fixtures-and-snapshots

---

_Marked ready for review by @yahayaohinoyi on 2024-09-18 23:11_

---

_Comment by @github-actions[bot] on 2024-09-18 23:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_period.rs`:112 on 2024-09-19 01:19_

```suggestion
            }
            checker.diagnostics.push(diagnostic);
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_punctuation.rs`:112 on 2024-09-19 01:20_

```suggestion
            }
            checker.diagnostics.push(diagnostic);
```


---

_@AlexWaygood reviewed on 2024-09-19 01:22_

Thanks for the PR! I think we should still emit a _diagnostic_ if the first line ends in a question mark or exclamation mark, though, since according to [PEP 257](https://peps.python.org/pep-0257/) the docstring isn't following the conventional style if it doesn't end in a period. We just shouldn't offer an _autofix_ for the diagnostic if it ends in a question mark or exclamation mark, since we don't know what the appropriate autofix would be in that situation.

I left some inline suggestions below that should be sufficient to address this -- though you'll need to regenerate the snapshots again!

---

_@MichaReiser requested changes on 2024-09-19 07:04_

See @AlexWaygood's comments

---

_Converted to draft by @yahayaohinoyi on 2024-09-19 07:16_

---

_Comment by @yahayaohinoyi on 2024-09-19 21:55_

Need some help!. I get failures like 
`failures:
    rules::pydocstyle::tests::rules::rule_endsinperiod_path_new_d_py_expects`
 after running the tests. How do I get visibility of what exactly fails? Even when I remove the content of the whole file the test still fails. Also doesn't generate any snapshots

---

_Comment by @MichaReiser on 2024-09-20 07:23_

Running the test gives me:

```
cargo test rules::pydocstyle::tests::rules::rule_endsinperiod_path_new_d_py_expects -p ruff_linter
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.08s
     Running unittests src/lib.rs (target/debug/deps/ruff_linter-93716c3568a00bf4)

running 1 test
test rules::pydocstyle::tests::rules::rule_endsinperiod_path_new_d_py_expects ... FAILED

failures:

---- rules::pydocstyle::tests::rules::rule_endsinperiod_path_new_d_py_expects stdout ----
thread 'rules::pydocstyle::tests::rules::rule_endsinperiod_path_new_d_py_expects' panicked at crates/ruff_linter/src/test.rs:254:21:
Rule EndsInPeriod is marked to always-fixable but the diagnostic has no fix.
Either ensure you always emit a fix or change `Violation::FIX_AVAILABILITY` to either `FixAvailability::Sometimes` or `FixAvailability::None`
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    rules::pydocstyle::tests::rules::rule_endsinperiod_path_new_d_py_expects

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 2108 filtered out; finished in 0.01s
```

The important bits are

```
thread 'rules::pydocstyle::tests::rules::rule_endsinperiod_path_new_d_py_expects' panicked at crates/ruff_linter/src/test.rs:254:21:
Rule EndsInPeriod is marked to always-fixable but the diagnostic has no fix.
Either ensure you always emit a fix or change `Violation::FIX_AVAILABILITY` to either `FixAvailability::Sometimes` or `FixAvailability::None`
```
It's telling you that the `Violation` should no longer implement `AlwaysFixableViolation` and instead explicitly set the `Fixability` to `Sometimes`


```rust
impl Violation for EndsInPeriod {
    /// `None` in the case a fix is never available or otherwise Some
    /// [`FixAvailability`] describing the available fix.
    const FIX_AVAILABILITY: FixAvailability = FixAvailability::Sometimes;

    #[derive_message_formats]
    fn message(&self) -> String {
        format!("First line should end with a period")
    }

    fn fix_title(&self) -> String {
        "Add period".to_string()
    }
}
```


---

_Review requested from @MichaReiser by @yahayaohinoyi on 2024-09-20 08:08_

---

_Review requested from @AlexWaygood by @yahayaohinoyi on 2024-09-20 08:08_

---

_Marked ready for review by @yahayaohinoyi on 2024-09-20 08:08_

---

_Label `bug` added by @MichaReiser on 2024-09-20 08:22_

---

_Label `rule` added by @MichaReiser on 2024-09-20 08:22_

---

_@MichaReiser reviewed on 2024-09-20 08:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_punctuation.rs`:112 on 2024-09-20 08:25_

I think we still need to move the `checker.diagnostic.push` call outside the `trimmed.ends_with` branch or we omit diagnostics for docstring summaries that end in `!`. We actually see this because I would expect to see more diagnostics in `ruff_linter__rules__pydocstyle__tests__D415_D415.py.snap`

---

_@yahayaohinoyi reviewed on 2024-09-20 09:17_

---

_Review comment by @yahayaohinoyi on `crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_punctuation.rs`:112 on 2024-09-20 09:17_

> I think we still need to move the `checker.diagnostic.push` call outside the `trimmed.ends_with` branch or we omit diagnostics for docstring summaries that end in `!`. We actually see this because I would expect to see more diagnostics in `ruff_linter__rules__pydocstyle__tests__D415_D415.py.snap`

Does it look any better now?

---

_@MichaReiser reviewed on 2024-09-20 09:36_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_punctuation.rs`:110 on 2024-09-20 09:36_

Looking at the code again, I think this change is unnecessary because line 107 already checks that the text doesn't end with `!` or `?`

---

_@yahayaohinoyi reviewed on 2024-09-20 09:38_

---

_Review comment by @yahayaohinoyi on `crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_punctuation.rs`:110 on 2024-09-20 09:38_

Yeah I the issue complains about fixing D400. D415 is just fine the way it is. I'd make a commit but leave the extra generated snapshot for D415

---

_@MichaReiser reviewed on 2024-09-20 11:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/ends_with_punctuation.rs`:49 on 2024-09-20 11:00_

Nice find

---

_@MichaReiser approved on 2024-09-20 11:00_

Thank you

---

_Merged by @MichaReiser on 2024-09-20 11:05_

---

_Closed by @MichaReiser on 2024-09-20 11:05_

---
