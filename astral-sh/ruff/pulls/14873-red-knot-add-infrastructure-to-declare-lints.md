```yaml
number: 14873
title: "[red-knot] Add infrastructure to declare lints"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/declare-lint
created_at: 2024-12-09T14:09:04Z
updated_at: 2024-12-10T16:19:11Z
url: https://github.com/astral-sh/ruff/pull/14873
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Add infrastructure to declare lints

---

_@MichaReiser_

## Summary

This is the second PR  out of three that adds support for enabling/disabling lint rules in Red Knot. You may want to take a look at the [first PR](https://github.com/astral-sh/ruff/pull/14869) in this stack to familiarize yourself with the used terminology. 

This PR adds a new syntax to define a lint: 

```rust
declare_lint! {
    /// ## What it does
    /// Checks for references to names that are not defined.
    ///
    /// ## Why is this bad?
    /// Using an undefined variable will raise a `NameError` at runtime.
    ///
    /// ## Example
    ///
    /// ```python
    /// print(x)  # NameError: name 'x' is not defined
    /// ```
    pub(crate) static UNRESOLVED_REFERENCE = {
        summary: "detects references to names that are not defined",
        status: LintStatus::preview("1.0.0"),
        default_level: Level::Warn,
    }
}
```

A lint has a name and metadata about its status (preview, stable, removed, deprecated), the default diagnostic level (unless the configuration changes), and documentation. I use a macro here to derive the kebab-case name and extract the documentation automatically. 

This PR doesn't yet add any mechanism to discover all known lints. This will be added in the next and last PR in this stack. 


## Documentation
I documented some rules but then decided that it's probably not my best use of time if I document all of them now (it also means that I play catch-up with all of you forever). That's why I left some rules undocumented (marked with TODO)

## Where is the best place to define all lints?

I'm not sure. I think what I have in this PR is fine but I also don't love it because most lints are in a single place but not all of them. If you have ideas, let me know. 


## Why is the message not part of the lint, unlike Ruff's `Violation`

I understand that the main motivation for defining `message` on `Violation` in Ruff is to remove the need to repeat the same message over and over again. I'm not sure if this is an actual problem. Most rules only emit a diagnostic in a single place and they commonly use different messages if they emit diagnostics in different code paths, requiring extra fields on the `Violation` struct.  

That's why I'm not convinced that there's an actual need for it and there are alternatives that can reduce the repetition when creating a diagnostic:

* Create a helper function. We already do this in red knot with the `add_xy` methods
* Create a custom `Diagnostic` implementation that tailors the entire diagnostic and pre-codes e.g. the message

Avoiding an extra field on the `Violation` also removes the need to allocate intermediate strings as it is commonly the place in Ruff. Instead, Red Knot can use a borrowed string with `format_args`

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-12-09 14:14_

---

_Marked ready for review by @MichaReiser on 2024-12-09 14:31_

---

_Review requested from @carljm by @MichaReiser on 2024-12-09 14:31_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-09 14:31_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-09 14:31_

---

_Comment by @github-actions[bot] on 2024-12-09 14:40_

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

_Review comment by @carljm on `crates/red_knot_python_semantic/src/lint.rs`:119 on 2024-12-10 00:34_

Is it the version it was added, or the version it became stable?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/lint.rs`:152 on 2024-12-10 00:35_

This is just so we can give more useful error messages on user configurations referencing the rule?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/lint.rs`:74 on 2024-12-10 00:38_

This says "leading and trailing whitespace removed", but that doesn't describe the behavior, it only removes a single leading space. I assume this is because the doc comments we get from the macro parser have a leading space on every line (the space between `///` and the text)?

Removing all leading whitespace from each line (as the wording currently implies) would be bad, as it would break many kinds of Markdown formatting.

```suggestion
    /// Returns the documentation line by line with one leading space and all trailing whitespace removed.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:34 on 2024-12-10 00:40_

```suggestion
    /// Checks for references to names that are possibly not defined.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:48 on 2024-12-10 00:41_

```suggestion
        summary: "detects references to possibly undefined names",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:68 on 2024-12-10 00:42_

```suggestion
        summary: "detects iteration over an object that is not iterable",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:86 on 2024-12-10 00:42_

```suggestion
    /// Checks for subscripting objects that do not support subscripting.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:96 on 2024-12-10 00:43_

```suggestion
        summary: "detects subscripting objects that do not support subscripting",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:317 on 2024-12-10 00:45_

```suggestion
        summary: "detects binary, unary, or comparison expressions where the operands don't support the operator",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:357 on 2024-12-10 00:47_

Currently this diagnostic is only emitted for `type[]` but it is intended to be more general.

```suggestion
    /// Checks for invalid type expressions.
```

---

_@carljm approved on 2024-12-10 00:49_

Nice!

---

_@MichaReiser reviewed on 2024-12-10 07:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/lint.rs`:152 on 2024-12-10 07:34_

The main use case I have in mind is the reference documentation. It would be nice if we can list when rules were added/stablized/deprecated/removed. Another use case is for us. One thing that's somewhat involved today is to find all rule changes that were made since version X. With this information available it becomes possible to build a command into `ruff_dev` that lists all rule changes since version X.

---

_@sharkdp reviewed on 2024-12-10 08:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/lint.rs`:24 on 2024-12-10 08:15_

Do we want this to be part of a release binary? Having potentially hundreds of `"crates/red_knot_python_semantic/src/types/diagnostic.rs:123"` strings in the `.rodata` section seems like a bit of a waste?

---

_@MichaReiser reviewed on 2024-12-10 10:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/lint.rs`:24 on 2024-12-10 10:31_

This is as good point. I consider it fairly small compared to e.g. the documentation of every rule and all vendored stubs but I don't mind gating it behind a feature that we only activate when generating the reference documentation if you think it's worth doing

---

_@sharkdp reviewed on 2024-12-10 10:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/lint.rs`:24 on 2024-12-10 10:56_

>  consider it fairly small compared to e.g. the documentation of every rule

Right. For 1000 lints, it would be ~ 100 KiB, so feel free to ignore it :smile: 

---

_@MichaReiser reviewed on 2024-12-10 11:07_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/lint.rs`:74 on 2024-12-10 11:07_

> I assume this is because the doc comments we get from the macro parser have a leading space on every line (the space between /// and the text)?

Yes, this is the reason. We could remove the whitespace if we convert the macro to a proc-macro but I find proc-macros harder to maintain (and read), so I opted to do this at runtime. 

---

_@MichaReiser reviewed on 2024-12-10 11:10_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/lint.rs`:24 on 2024-12-10 11:10_

I could change it to two fields: `file` and `line` to possibly safe some space for the `:` and line number :laughing: 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/lint.rs`:24 on 2024-12-10 12:25_

I'd also expect LTO to remove the data if it isn't referenced anywhere... But I haven't verified if LTO is capable of doing this.

---

_@MichaReiser reviewed on 2024-12-10 12:25_

---

_Merged by @MichaReiser on 2024-12-10 16:14_

---

_Closed by @MichaReiser on 2024-12-10 16:14_

---

_Branch deleted on 2024-12-10 16:14_

---
