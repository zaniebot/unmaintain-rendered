```yaml
number: 18232
title: "Return `DiagnosticGuard` from `Checker::report_diagnostic`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/report-violation-builder
created_at: 2025-05-20T20:38:46Z
updated_at: 2025-05-28T11:41:33Z
url: https://github.com/astral-sh/ruff/pull/18232
synced_at: 2026-01-10T18:51:02Z
```

# Return `DiagnosticGuard` from `Checker::report_diagnostic`

---

_Pull request opened by @ntBre on 2025-05-20 20:38_

Summary
--

This PR adds a `DiagnosticGuard` type to ruff that is adapted from the `DiagnosticGuard` and `LintDiagnosticGuard` types from ty. This guard is returned by `Checker::report_diagnostic` and derefs to a `ruff_diagnostics::Diagnostic` (`OldDiagnostic`), allowing methods like `OldDiagnostic::set_fix` to be called on the result. On `Drop` the `DiagnosticGuard` pushes its contained `OldDiagnostic` to the `Checker`.

The main motivation for this is to make a following PR adding a `SourceFile` to each diagnostic easier. For every rule where a `Checker` is available, this will now only require modifying `Checker::report_diagnostic` rather than all the rules.

In the few cases where we need to create a diagnostic before we know if we actually want to emit it, there is a `DiagnosticGuard::defuse` method, which consumes the guard without emitting the diagnostic. I was able to restructure about half of the rules that naively called this to avoid calling it, but a handful of rules still need it.

One of the fairly common patterns where `defuse` was needed initially was something like

```rust
let diagnostic = Diagnostic::new(DiagnosticKind, range);

if !checker.enabled(diagnostic.rule()) {
    return;
}
```

So I also added a `Checker::checked_report_diagnostic` method that handles this check internally. That helped to avoid some additional `defuse` calls. The name is a bit repetitive, so I'm definitely open to suggestions there. I included a warning against using it in the docs since, as we've seen, the conversion from a diagnostic to a rule is actually pretty expensive.

Test Plan
--

Existing tests


---

_Label `internal` added by @ntBre on 2025-05-20 20:38_

---

_Label `diagnostics` added by @ntBre on 2025-05-20 20:38_

---

_Comment by @github-actions[bot] on 2025-05-20 20:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-05-20 20:47_

---

_Review requested from @AlexWaygood by @ntBre on 2025-05-20 20:47_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-05-20 20:47_

---

_Review requested from @MichaReiser by @ntBre on 2025-05-20 20:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:408 on 2025-05-21 06:41_

It's unfortunate that this has to go through `diagnostic.rule` which requires a lookup. I'm leaning towards adding a `rule` method to `ViolationMetadata`. That should be easy enough to derive and does make sense to me. This will also allow us to resolve the noqa code in a future version.

That makes me wonder if `report_diagnostic` should always to the `self::enabled` call but I suspect that handling with the `Option` return type is too annoying in many cases that it isn't worth it.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3103 on 2025-05-21 06:43_

we shouldn't emit the diagnostic if the thread is unwinding:

```suggestion
        if std::thread::panicking {
            return;
        }
        
        if let Some(diagnostic) = self.diagnostic.take() {
            self.checker.diagnostics.borrow_mut().push(diagnostic);
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:1217 on 2025-05-21 06:45_

It would be nice if we could avoid creating the diagnsotic by moving this check before the `report_diagnostic `call. Diagnostics are rather heavy weight and creating them for what's valid code is non ideal.


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:980 on 2025-05-21 06:46_

Same here. It would be great if we could move this check before creating the diagnostic.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:300 on 2025-05-21 06:47_

Same as for the other airflow rule

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:1182 on 2025-05-21 06:53_

Maybe: `report_diagnostic_if_enabled`. THe `checker.checked` reads a bit strangely

---

_@MichaReiser approved on 2025-05-21 06:55_

This is great. I didn't review all rule changes (there are too many). 

I think it would be good to add `rule` to `Violation` to avoid the need to go through `diagnostic.rule` only to check if the rule is enabled (which could lead to a somewhat significant perf regression).

---

_@ntBre reviewed on 2025-05-21 12:52_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:3103 on 2025-05-21 12:52_

Makes sense, I saw that in ty but thought it might be ty-specific. Do you know if I also need the second one after the `take` call?

https://github.com/astral-sh/ruff/blob/839c637abb8c7f684f7577885a53432bff949169/crates/ty_python_semantic/src/types/context.rs#L503-L516

---

_@ntBre reviewed on 2025-05-21 13:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:408 on 2025-05-21 13:03_

Oh nice, I'll try to clean https://github.com/astral-sh/ruff/pull/18234 up for review at some point then. That makes sense to resolve the `NoqaCode` and store _that_ on the `Diagnostic` instead but use the `Rule` directly here.

It does sound pretty appealing to always do the `enabled` check if it's cheap enough too. I think in most cases it should just be a `let-else` that returns if `None`, but it will be another big refactor to apply that everywhere for sure. I'm getting better at using ast-grep for these, though 😄 

---

_@MichaReiser reviewed on 2025-05-21 13:07_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3103 on 2025-05-21 13:07_

Hmm no, I think that's just wrong.

---

_@ntBre reviewed on 2025-05-27 14:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:3103 on 2025-05-27 14:29_

Added one check before the `take` call.

---

_@ntBre reviewed on 2025-05-27 14:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_function_call.rs`:1182 on 2025-05-27 14:35_

Nice, I like that.

---

_@ntBre reviewed on 2025-05-27 19:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:1217 on 2025-05-27 19:50_

I reworked this a bit to avoid the `defuse` call. I also stored a `QualifiedName` directly on the rule struct to avoid a `to_string` call, but I needed to pass along a lifetime parameter in the derive macro using `split_for_impl` too. I could revert that part if we wanted, but it seemed like it could be helpful in general.

---

_@ntBre reviewed on 2025-05-27 20:15_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:1217 on 2025-05-27 20:15_

On second thought, I could definitely revert this. 2/4 airflow rules I updated actually needed `String`s, so I couldn't change those to `QualifiedName` and the mix is a bit inconsistent.

---

_@MichaReiser reviewed on 2025-05-28 06:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:408 on 2025-05-28 06:08_

> It does sound pretty appealing to always do the enabled check if it's cheap enough too. I think in most cases it should just be a let-else that returns if None, but it will be another big refactor to apply that everywhere for sure. I'm getting better at using ast-grep for these, though 😄

Let's keep it at what we have for now. We can consider this if it proves necessary

---

_@MichaReiser approved on 2025-05-28 06:11_

---

_Merged by @ntBre on 2025-05-28 11:41_

---

_Closed by @ntBre on 2025-05-28 11:41_

---

_Branch deleted on 2025-05-28 11:41_

---
