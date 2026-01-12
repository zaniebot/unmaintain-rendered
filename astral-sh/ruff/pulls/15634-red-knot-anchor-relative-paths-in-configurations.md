```yaml
number: 15634
title: "[red-knot] Anchor relative paths in configurations"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/path-anchoring
created_at: 2025-01-21T10:52:33Z
updated_at: 2025-01-23T09:14:04Z
url: https://github.com/astral-sh/ruff/pull/15634
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Anchor relative paths in configurations

---

_@MichaReiser_

## Summary

This PR adds support for relative paths in configurations and the CLI. 

The challenge with relative paths is that they are relative to a different root based on where the configuration value comes from:

* CLI: Paths are relative to the current working directory
* Configuration: Paths are relative to the project's root

While Red Knot already supported relative paths from the CLI, it incorrecty
resolved relative paths in configuration files as relative to the current working directory.
This PR fixes this by implementing path anchoring. It introduces a new `RelativePath` type that we
resolve to an absolute path when going from "raw" `Options` to a `*Settings` struct. 

A `RelativePath` is a combination of the path and a source indicating the origin of the path. This
tuple allows resolving the absolute path when given the `project_root` and `system`. 


I plan to generalize the *value with a source* in a follow up PR where we also start
tracking the source span for every value to enable rich diagnostics for configuration errors. 


Part of https://github.com/astral-sh/ty/issues/219

## Alternatives

An alternative to using the `thread_local` for passing the `ValueSource` would be
to instead have a `Options::apply_source` or similar method that changes the source 
for all values recursively. I decided against it because it would require a fair 
bit of boilerplate (or a macro). 

## Test Plan

Added integration tests.


---

_Label `red-knot` added by @MichaReiser on 2025-01-21 10:52_

---

_Marked ready for review by @MichaReiser on 2025-01-21 11:06_

---

_Review requested from @carljm by @MichaReiser on 2025-01-21 11:06_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-21 11:06_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-21 11:06_

---

_Comment by @github-actions[bot] on 2025-01-21 11:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sharkdp on 2025-01-22 08:59_

I did a coarse-grained review, but would like to understand the problem better before going into more detail.

Let me try to rephrase what you wrote just to see if I understand this correctly: The core problem is with options like `extra-search-path` which represents a list of paths. We could have one path in the list coming from a configuration file (anchored at the directory where the config file resides) and another path coming from the CLI (anchored at the CWD).

The approach you chose here is to attach that anchor *to each and every path* in the options struct, which leads to this slightly awkward solution with the thread-local because we create the options struct directly using serde.

Another approach might be to store a *global* anchor *next to* the options struct that we get from a particular source. For the scenario above, we would then have something like `(/path/to/project/root, <options read from config file>)` and `(/path/to/cwd, <options read from CLI>)` where each of the deserialized options structs would still contain bare relative paths. The obvious problem with this approach is that we can't `Combine` those, because for every path in the combined options struct, we wouldn't know what its original anchor was. But what if we turned those `Options` into `Settings` (with absolute paths) first, and *then* used `Combine` on the `Settings`? Would that be possible?

(Sorry if this is basically what you meant with the alternative solution in the PR description)

(Edit: the problem is even present for a config option with just a single path, not just with lists of paths)

---

_Comment by @MichaReiser on 2025-01-22 14:01_

Your summary is accurate and yes, the problem is that options get combined from multiple sources so that arrays end up with values relative to different roots. 

What you describe is definetely possible and is roughly what Ruff does, except that it requires one extra representatios:

* `Options`: The type we deserialize into. Matches the configuratino structure exactly
* `Configuration`: The `Options` are converted into a `Configuration`. Multiple `Configuration`s can be combined together. 
* `Settings`: The "resolved" values. That includes filling in default values or using representation optimized for reading (e.g. a configuration listing `allowed-types` uses a `Vec<String>` in `Options` but a `Vec<Type>` in `Settings). 

This separation has proven to be somewhat painful in Ruff because there's a lot of redundancy: Each structure roughly defines the same fields, only sometimes with slightly different types. 

There's a second problem that I only touched on in the summary: We want to validate the user configuration. Some of those validations are local to a single value and could be done when going from `Options` to `Configuration`, for example warning about unknown rules. However, other settings require a global view on all provided configuration values and can only be done once all configurations were merged. An example from Ruff is that we want to warn about conflicting rules. It isn't sufficient to only look at a single configuration before combining it because the user might disable one of the conflicting rules using the CLI. 





---

_Review comment by @sharkdp on `crates/red_knot/tests/cli.rs`:109 on 2025-01-22 14:38_

Since this is a slightly involved test setup, I think I would appreciate something like
```suggestion

    // Make sure that the CLI fails when the `libs` directory is not in the search path.
    insta::with_settings!({filters => vec![(&*tempdir_filter(&tempdir), "<temp_dir>/")]}, {
        assert_cmd_snapshot!(knot().current_dir(&child), @r#"
        success: false
        exit_code: 1
        ----- stdout -----
        error[lint:unresolved-import] <temp_dir>/child/test.py:2:1 Cannot resolve import `utils`

        ----- stderr -----
        "#);
    });

```

---

_@sharkdp reviewed on 2025-01-22 14:38_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata/value.rs`:30 on 2025-01-22 14:42_

Is this needed?

---

_@sharkdp reviewed on 2025-01-22 14:42_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata/value.rs`:35 on 2025-01-22 14:44_

Maybe
```suggestion
    /// but we want to associate each deserialized [`RelativePath`] with the source from
    /// which it originated. We use a thread local variable to work around this limitation.
```

---

_@sharkdp reviewed on 2025-01-22 14:44_

---

_@sharkdp reviewed on 2025-01-22 14:49_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata/value.rs`:50 on 2025-01-22 14:49_

Is this needed because we might have multiple `let _guard = ValueSourceGuard::new(source);` calls in different depths of a call stack? Or would it be enough to make sure that the value is `None` when creating a guard?

---

_@MichaReiser reviewed on 2025-01-22 14:51_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/value.rs`:50 on 2025-01-22 14:51_

It isn't needed but I considered it good practice to leave the environment in the same state as before. It should lead to fewer surprises (e.g. if we ever end up with nested calls for whatever reason.

---

_@sharkdp reviewed on 2025-01-22 14:51_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata/value.rs`:20 on 2025-01-22 14:51_

Minor: Do we need this for something else besides path anchoring? If not, I think I would prefer if this struct were a little more tailored to this use case in terms of naming (`ValueSource` could mean a lot of things). This would also make the comments a bit simpler(?). Something like
```rs
pub enum PathAnchor {
    /// …
    RelativeTo(Arc<SystemPathBuf>)
    /// …
    RelativeToCwd
}
```

---

_@sharkdp reviewed on 2025-01-22 14:54_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata/value.rs`:97 on 2025-01-22 14:54_

Maybe
```suggestion
    /// Resolves the absolute path for `self` based on its origin
```
or
```suggestion
    /// Resolves the absolute path for `self` based on the anchor that it originates from
```

(and similar below)

---

_@sharkdp approved on 2025-01-22 14:55_

Thank you — looks great!

I don't *love* the thread-local solution, but your reasoning makes sense — thank you for writing it down!

---

_@MichaReiser reviewed on 2025-01-22 14:56_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/value.rs`:20 on 2025-01-22 14:56_

The idea is to also use it in diagnostic. E.g. the rule `x` that you specified via the CLI is unknown or this rule is checked because it was enabled via the CLI. That's why I designed the enum around where the value comes from rather than how it influences path anchoring.

---

_@sharkdp reviewed on 2025-01-22 14:58_

---

_Review comment by @sharkdp on `crates/red_knot_project/src/metadata/value.rs`:20 on 2025-01-22 14:58_

Makes sense, thank you.

---

_Comment by @MichaReiser on 2025-01-22 15:00_

> I don't love the thread-local solution, but your reasoning makes sense — thank you for writing it down!

Neither do I :( It's a shame that there's no other solution for passing down state to serde. 

We'll face a similar problem when implementing the JSON schema for rules because the schema also doesn't allow passing down any additional data but we'll need access to the rule registry

---

_Comment by @dcreager on 2025-01-22 15:15_

> Neither do I :( It's a shame that there's no other solution for passing down state to serde.

Is there a way to do it using [`DeserializeSeed`](https://docs.rs/serde/1.0.217/serde/de/trait.DeserializeSeed.html)?

---

_Comment by @MichaReiser on 2025-01-22 16:02_

> Is there a way to do it using [DeserializeSeed](https://docs.rs/serde/1.0.217/serde/de/trait.DeserializeSeed.html)?

Possibly. I didn't look too much into it, but it seems that you'd have to write your own `Deserializer` and recognize whenever a `RelativePath` gets deserialized but maybe this isn't too hard? I can explore that if we want as a follow up PR. 

---

_Comment by @dcreager on 2025-01-22 16:24_

> Possibly. I didn't look too much into it, but it seems that you'd have to write your own `Deserializer` and recognize whenever a `RelativePath` gets deserialized but maybe this isn't too hard? I can explore that if we want as a follow up PR.

If it's a pain, and the thread-local version is working, I'd say it's not worth it.

---

_Merged by @MichaReiser on 2025-01-23 09:14_

---

_Closed by @MichaReiser on 2025-01-23 09:14_

---

_Branch deleted on 2025-01-23 09:14_

---
