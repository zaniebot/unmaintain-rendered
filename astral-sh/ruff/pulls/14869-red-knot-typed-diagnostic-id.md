```yaml
number: 14869
title: "[red-knot] Typed diagnostic id"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: micha/diagnostic-id
created_at: 2024-12-09T13:13:20Z
updated_at: 2024-12-10T21:29:20Z
url: https://github.com/astral-sh/ruff/pull/14869
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Typed diagnostic id

---

_Pull request opened by @MichaReiser on 2024-12-09 13:13_

## Summary

This PR introduces a structured `DiagnosticId` instead of using a plain `&'static str`. It is the first of three in a stack that implements a basic rules infrastructure for Red Knot.

`DiagnosticId` is an enum over all known diagnostic codes. A closed enum reduces the risk of accidentally introducing two identical diagnostic codes. It also opens the possibility of generating reference documentation from the enum in the future (not part of this PR). 

The enum isn't *fully closed* because it uses a `&'static str` for lint names. This is because we want the flexibility to define lints in different crates, and all names are only known in `red_knot_linter` or above. Still, lower-level crates must already reference the lint names to emit diagnostics. We could define all lint-names in `DiagnosticId` but I decided against it because:

* We probably want to share the `DiagnosticId` type between Ruff and Red Knot to avoid extra complexity in the diagnostic crate, and both tools use different lint names.
* Lints require a lot of extra metadata beyond just the name. That's why I think defining them close to their implementation is important. 

In the long term, we may also want to support plugins, which would make it impossible to know all lint names at compile time. The next PR in the stack introduces extra syntax for defining lints. 

A closed enum does have a few disadvantages:

* rustc can't help us detect unused diagnostic codes because the enum is public
* Adding a new diagnostic in the workspace crate now requires changes to at least two crates: It requires changing the workspace crate to add the diagnostic and the `ruff_db` crate to define the diagnostic ID. I consider this an acceptable trade. We may want to move `DiagnosticId` to its own crate or into a shared `red_knot_diagnostic` crate.


## Preventing duplicate diagnostic identifiers

One goal of this PR is to make it harder to introduce ambiguous diagnostic IDs, which is achieved by defining a closed enum. However, the enum isn't fully "closed" because it doesn't explicitly list the IDs for all lint rules. That leaves the possibility that a lint rule and a diagnostic ID share the same name. 

I made the names unambiguous in this PR by separating them into different namespaces by using `lint/<rule>` for lint rule codes. I don't mind the `lint` prefix in a *Ruff next* context, but it is a bit weird for a standalone type checker. I'd like to not overfocus on this for now because I see a few different options:

* We remove the `lint` prefix and add a unit test in a top-level crate that iterates over all known lint rules and diagnostic IDs to ensure the names are non-overlapping. 
* We only render `[lint]` as the error code and add a note to the diagnostic mentioning the lint rule. This is similar to clippy and has the advantage that the header line remains short (`lint/some-long-rule-name` is very long ;))
* Any other form of adjusting the diagnostic rendering to make the distinction clear

I think we can defer this decision for now because the `DiagnosticId` contains all the relevant information to change the rendering accordingly. 


## Why `Lint` and not `LintRule`

I see three kinds of diagnostics in Red Knot:

* Non-suppressable: Reveal type, IO errors, configuration errors, etc. (any `DiagnosticId`)
* Lints: code-related diagnostics that are suppressable. 
* Lint rules: The same as lints, but they can be enabled or disabled in the configuration. The majority of lints in Red Knot and the Ruff linter.

Our current implementation doesn't distinguish between lints and Lint rules because we aren't aware of a suppressible code-related lint that can't be configured in the configuration. The only lint that comes to my mind is maybe `division-by-zero` if we're 99.99% sure that it is always right. However, I want to keep the door open to making this distinction in the future if it proves useful. 

Another reason why I chose lint over lint rule (or just rule) is that I want to leave room for a future lint rule and lint phase concept:

* lint is the *what*: a specific code smell, pattern, or violation 
* the lint rule is the *how*: I could see a future  `LintRule` trait in `red_knot_python_linter` that provides the necessary hooks to run as part of the linter. A lint rule produces diagnostics for exactly one lint. A lint rule differs from all lints in `red_knot_python_semantic` because they don't run as "rules" in the Ruff sense. Instead, they're a side-product of type inference.
* the lint phase is a different form of *how*: A lint phase can produce many different lints in a single pass. This is a somewhat common pattern in Ruff where running one analysis collects the necessary information for finding many different lints
* diagnostic is the *presentation*: Unlike a lint, the diagnostic isn't the what, but how a specific lint gets presented. I expect that many lints can use one generic `LintDiagnostic`, but a few lints might need more flexibility and implement their custom diagnostic rendering (at least custom `Diagnostic` implementation). 


## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-12-09 13:13_

---

_Label `diagnostics` added by @MichaReiser on 2024-12-09 13:19_

---

_Review requested from @BurntSushi by @MichaReiser on 2024-12-09 13:19_

---

_Comment by @github-actions[bot] on 2024-12-09 13:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2024-12-09 14:14_

---

_Review requested from @carljm by @MichaReiser on 2024-12-09 14:14_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-09 14:14_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-09 14:14_

---

_Review comment by @carljm on `crates/ruff_db/src/diagnostic.rs`:69 on 2024-12-10 00:11_

```suggestion
    /// Lints can be suppressed and some lints can be enabled or disabled in the configuration.
```

---

_@carljm approved on 2024-12-10 00:18_

Code looks good!

My main question about this PR is the use of the term "lint" to encompass core type checker diagnostics. This was discussed extensively in the CLI proposal, without a clear resolution. I don't think this usage is wrong (Python type checkers really are linters, since they don't integrate with runtime), but I suspect that even if we use this terminology internally, we will need to be careful to avoid it in the UI, because I think existing mypy/pyright users will likely find it confusing to have type errors referred to as "lints".

But you said you don't want to over-focus on this now, and we can change it, so not a blocking issue.

---

_Comment by @MichaReiser on 2024-12-10 07:32_

> My main question about this PR is the use of the term "lint" to encompass core type checker diagnostics. This was discussed extensively in the CLI proposal, without a clear resolution. I don't think this usage is wrong (Python type checkers really are linters, since they don't integrate with runtime), but I suspect that even if we use this terminology internally, we will need to be careful to avoid it in the UI, because I think existing mypy/pyright users will likely find it confusing to have type errors referred to as "lints".

I agree. We should use the term rule as the user-facing term and I also intend to use it in code when the context only allows rules but not other lints. For example, the configuration only allows rule names, not arbitrary lints. One possible design is to define a `RuleName` struct that rejects non-rule lints during deserialization. I only want to avoid using the term `Rule` in contexts where something isn't guaranteed to be backed by a rule or isn't configurable.



---

_@sharkdp reviewed on 2024-12-10 07:51_

---

_Review comment by @sharkdp on `crates/ruff_db/src/diagnostic.rs`:114 on 2024-12-10 07:51_

This seems to only be used in tests? We probably can't mark it `#[cfg(test)]`, because the `red_knot_test` infrastructure is not `#[cfg(test)]` either?

But considering it's only used in tests, would it make sense to change the implementation to
```suggestion
    pub fn matches(&self, name: &str) -> bool {
        self.to_string() == name
    }
```
to enforce consistency with the `Display` impl, which is not test-only?

---

_@sharkdp reviewed on 2024-12-10 07:55_

---

_Review comment by @sharkdp on `crates/ruff_db/src/diagnostic.rs`:110 on 2024-12-10 07:55_

> I'd like to not overfocus

Okay, just a minor stylistic preference then: I think I would prefer `lint:…` over `lint/…`, to make it visually distinct from the path names that are also on the same line:
```
error[lint:unresolved-import] /src/tomllib/_parser.py:7:29 Module `collections.abc` has no member `Iterable`
```
vs
```
error[lint/unresolved-import] /src/tomllib/_parser.py:7:29 Module `collections.abc` has no member `Iterable
```

---

_@sharkdp approved on 2024-12-10 07:55_

Thank you!

---

_@MichaReiser reviewed on 2024-12-10 10:18_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:114 on 2024-12-10 10:18_

I introduced a `as_str` method that returns a `Result` where it is `Ok` for all ids that don't belong to a sub category and `Err(DiagnosticIdAsStr::Category { category: "lint", name })` for lints. this allows us to implement `Display` and `matches` without repeating the id strings and avoids unnecessary allocations

---

_Merged by @MichaReiser on 2024-12-10 15:58_

---

_Closed by @MichaReiser on 2024-12-10 15:58_

---

_Branch deleted on 2024-12-10 15:58_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic.rs`:104 on 2024-12-10 18:34_

Would it make more sense to just return a `DiagnosticAsStr` type that implements `Display`? So, basically, a `DiagnosticAsStrError` like you have, but for all the variants. And in this case, you'd probably rename it to `display()` or some such.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic.rs`:104 on 2024-12-10 18:35_

Another pattern I like for things like this is to bite the bullet on the alloc, but do it at construction time. Then you can have an `as_str()` method that unconditionally returns `&str`.

---

_@BurntSushi reviewed on 2024-12-10 18:35_

Makes sense to me at this point in my journey. :-) Thank you for tagging me!

---

_@MichaReiser reviewed on 2024-12-10 21:29_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:104 on 2024-12-10 21:29_

I introduced it to avoid repeating the variant to name matching code for the `matches` method. I think the easiest would be to just call `to_string()` in matches which I'm open to do when we have other use cases for it. 

Another solution is to have a macro that generates a `name` method for it. 

Overall, I don't have a very good understanding yet of how this type will be used, and I suggest we change it as we see fit. 

---
