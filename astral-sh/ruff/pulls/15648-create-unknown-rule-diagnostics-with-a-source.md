```yaml
number: 15648
title: Create Unknown rule diagnostics with a source range
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/ranged-rule-diagnostics
created_at: 2025-01-21T15:59:44Z
updated_at: 2025-01-23T11:50:46Z
url: https://github.com/astral-sh/ruff/pull/15648
synced_at: 2026-01-10T20:05:43Z
```

# Create Unknown rule diagnostics with a source range

---

_Pull request opened by @MichaReiser on 2025-01-21 15:59_

## Summary

Track the source range of values deserialized from the TOML configuration and use the source range to create unknown-rule diagnostics that point to the right location in the configuration file. 

```
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: false
    1     1 │ exit_code: 1
    2     2 │ ----- stdout -----
    3       │-warning[unknown-rule] Unknown lint rule `division-by-zer`
          3 │+warning[unknown-rule] <temp_dir>/pyproject.toml:3:1 Unknown lint rule `division-by-zer`
    4     4 │ 
    5     5 │ ----- stderr -----
```

This PR makes use of [`toml`'s spanned](https://docs.rs/toml/latest/toml/struct.Spanned.html) functionality to get a text range for every deserialized value in combination with the `ValueSource` that I introduced in [previ](https://github.com/astral-sh/ruff/pull/15634). 

Toml's spanned feature has one significant drawback: It doesn't support `flatten` or `untagged`, at least when using the derived deserializer. The [`toml-span`](https://crates.io/crates/toml-span) crate explains why in more detail. 

I considered using `toml-span` instead, but it requires you to write all `Deserializer`s manually. I ultimately decided to go with `toml`'s span because:

* We can always migrate to `toml-span` if we **have** to use `flatten` or similar (either for the entire configuration or only the problematic values)
* Writing deserializer's manually brings the risk that the JSON schema and `Deserializer` diverge
* We want to avoid `flatten` as much as possible because it leads to very unhelpful error messages when the configuration can't be deserialized because of an unknown field or invalid type.
* It's simply easier :)


I do expect that using this approach will require some hackery to support e.g. a `string | vec<string>` value but this has been solved by [cargo-vet](https://github.com/Gankra/cargo-vet/blob/989b65b0d63bb3dfd974d737fce0ef7397825b00/src/serialization.rs#L26-L39). 


This PR is inspired by https://github.com/mozilla/cargo-vet/pull/230

Part of https://github.com/astral-sh/ty/issues/219

## Test Plan

See the changes in the CLI test

---

_Label `red-knot` added by @MichaReiser on 2025-01-21 15:59_

---

_Comment by @github-actions[bot] on 2025-01-21 16:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-01-22 16:36_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata.rs`:58 on 2025-01-22 16:36_

How long before it becomes `&*********` 😆 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/lint.rs`:417 on 2025-01-22 16:38_

This is not strictly necessary for what we need today but the intent here is that we show a note in diagnostics similar to clippy, telling users why a check was reported (you enabled this rule in the configuration, you enabled it in the cli, it's enabled by default, etc)

---

_@MichaReiser reviewed on 2025-01-22 16:38_

---

_@MichaReiser reviewed on 2025-01-22 16:40_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/options.rs`:172 on 2025-01-22 16:40_

For now, it would be sufficient to only make the key in the `Rules` table ranged because we don't use the range or source for any other value. I instead opted to make all values `Ranged` because the overhead should be small and having the range will make it easier to write good diagnostics.

---

_Marked ready for review by @MichaReiser on 2025-01-22 16:59_

---

_Review requested from @carljm by @MichaReiser on 2025-01-22 16:59_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-22 16:59_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-22 16:59_

---

_Review comment by @carljm on `crates/red_knot_project/src/metadata.rs`:58 on 2025-01-22 21:40_

all I see is `&hunter2`

---

_Review comment by @carljm on `crates/red_knot_project/src/metadata/value.rs`:82 on 2025-01-22 23:04_

This allows representing the invalid states where a value with CLI source has Some range, or a value with file source has None range. What would be the tradeoffs of instead recording the range as part of the `ValueSource`? (I realize this would mean splitting the `ValueSource` type used here from the `ValueSource` type stored in thread-locals, but that doesn't necessarily seem like a problem).

---

_Review comment by @carljm on `crates/red_knot_project/src/metadata/value.rs`:142 on 2025-01-22 23:06_

This comment doesn't make sense to me, because we are defining the `into_iter` method for the `RangedValue` type here. Did you mean to say that it already has an `iter` method?

---

_@carljm approved on 2025-01-22 23:10_

Cool!

---

_Renamed from "WIP: Create Unknown rule diagnostics with a source range" to "Create Unknown rule diagnostics with a source range" by @MichaReiser on 2025-01-23 06:42_

---

_@MichaReiser reviewed on 2025-01-23 10:53_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/value.rs`:82 on 2025-01-23 10:53_

I considered that but decided against it for two reasons:

1. It doesn't simplify downstream code because no code depends on the enforced constraint. However, enforcing it here does require more code because we'd need a `ValueSourceKind` (which is what we set in the thread local) and ` ValueSource`. 
2. We'll need to deserialize a `serde::Value` when supporting more complex types from the CLI using `--config complex.key = "{ sub = [... ] }"` and the `toml` crate can't emit text ranges in that case. I'm not sure if we have a similar case for configurations coming from a file, but maybe when working on the uv integration?

Overall, there are too many unknowns for cases where we might not have a range even if the value comes from a file and there are no "downstream" improvements to enforce the constraint which is why I'd prefer to keep it as is for now. 

---

_Merged by @MichaReiser on 2025-01-23 11:50_

---

_Closed by @MichaReiser on 2025-01-23 11:50_

---

_Branch deleted on 2025-01-23 11:50_

---
