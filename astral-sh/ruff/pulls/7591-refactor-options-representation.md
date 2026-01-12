```yaml
number: 7591
title: "Refactor `Options` representation"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor-options
created_at: 2023-09-22T07:51:52Z
updated_at: 2023-09-22T16:20:00Z
url: https://github.com/astral-sh/ruff/pull/7591
synced_at: 2026-01-12T02:39:10Z
```

# Refactor `Options` representation

---

_Pull request opened by @MichaReiser on 2023-09-22 07:51_

## Summary

This PR refactors the `Options` representation. 

### Current Reprsentation

Our `ConfigureOptions` macro currently generates the following code (truncated):

```rust
impl Flake8BanditOptions {
    pub const fn metadata() -> crate::options_base::OptionGroup {
        const OPTIONS: [(&'static str, crate::options_base::OptionEntry); 3usize] = [
			("hardcoded-tmp-directory", crate::options_base::OptionEntry::Field(crate::options_base::OptionField { doc: &"A list of directories to consider temporary.", default: &"[\"/tmp\", \"/var/tmp\", \"/dev/shm\"]", value_type: &"list[str]", example: &"hardcoded-tmp-directory = [\"/foo/bar\"]" })), 
			("hardcoded-tmp-directory-extend", crate::options_base::OptionEntry::Field(crate::options_base::OptionField { doc: &"A list of directories to consider temporary, in addition to those\nspecified by `hardcoded-tmp-directory`.", default: &"[]", value_type: &"list[str]", example: &"extend-hardcoded-tmp-directory = [\"/foo/bar\"]" })), 
			("check-typed-exception", crate::options_base::OptionEntry::Field(crate::options_base::OptionField { doc: &"Whether to disallow `try`-`except`-`pass` (`S110`) for specific\nexception types. By default, `try`-`except`-`pass` is only\ndisallowed for `Exception` and `BaseException`.", default: &"false", value_type: &"bool", example: &"check-typed-exception = true" }))
		];
        crate::options_base::OptionGroup::new(&OPTIONS)
    }
}
```

You can see how it generates a static array and `metadata` returns an `OptionGroup(&slice)`  referring to the slice of the static array. 

Options can be nested (option-groups). The macro calls into the nested type's `metadata` function to get the `OptionGroup` and adds a `OptionEntry::Group()` to its static array

```rust
const OPTIONS: [(&'static str, crate::options_base::OptionEntry); 3usize] = [
			("hardcoded-tmp-directory", crate::options_base::OptionEntry::Field(crate::options_base::OptionField { doc: &"A list of directories to consider temporary.", default: &"[\"/tmp\", \"/var/tmp\", \"/dev/shm\"]", value_type: &"list[str]", example: &"hardcoded-tmp-directory = [\"/foo/bar\"]" })), 
			...
			("example-group", crate::options_base::OptionEntry::Group(ExampleGroup::metadata()))
		];
```

This works well enough and the API is simple. However, it requires that the macro statically determines all options to be able to set the length of the array.

### New Requirements
I'm about to introduce a new `lint` section to the configuration without removing the top-level options until the new configuration is stabilized. The simplest but very verbose approach is duplicating the `lint` options: Once at the top level and once under the `lint` section. But that's rather verbose. Ideally, I want to use serde's `flatten` feature and write:

```rust
struct Options {
	...

    // Lint options
    /// Options for the Ruff linter
    #[option_group]
    pub lint: Option<LintOptions>,

    /// Doc
    #[serde(flatten)]
    pub lint_legacy: LintOptions,
}
```

While serde and schemars support this nicely, our options macro does not, and adding it is impossible with the current representation. The problem is that macros operate on a tokens stream only. They can't resolve types (at least to my knowledge). But that's what's necessary for `Options` to statically determine the length of its `OPTIONS` array: It needs to expand the `LintOptions` options into `Options`. 

### New Representation

The new representation is inspired by serde and `tracing_subscribs`'s `Visit` trait. 

The basic idea is to implement the metadata abstraction as a visitor pass (to avoid allocations). 

```rust
/// Returns metadata for its options.
pub trait OptionsMetadata {
    /// Visits the options metadata of this object by calling `visit` for each option.
    fn record(visit: &mut dyn Visit);

    /// Returns the extracted metadata.
    fn metadata() -> OptionSet
    where
        Self: Sized + 'static,
    {
        OptionSet::of::<Self>()
    }
}

struct Root;

impl OptionsMetadata for Root {
    fn record(visit: &mut dyn Visit) {
        visit.record_field("ignore-git-ignore", OptionField {
            doc: "Whether Ruff should respect the gitignore file",
            default: "false",
            value_type: "bool",
            example: "",
        });

        visit.record_set("format", Nested::metadata());
    }
}

struct Nested;

impl OptionsMetadata for Nested {
    fn record(visit: &mut dyn Visit) {
        visit.record_field("hard-tabs", OptionField {
            doc: "Use hard tabs for indentation and spaces for alignment.",
            default: "false",
            value_type: "bool",
            example: "",
        });
    }
}
```

* Each struct that has options implements `OptionsMetadata`
* `OptionsMetadata::record` calls `record_field` or `record_set` for each option

where `Visit` is defined as

```rust
/// Visits [`OptionsMetadata`].
///
/// An instance of [`Visit`] represents the logic for inspecting an object's options metadata.
pub trait Visit {
    /// Visits an [`OptionField`] value named `name`.
    fn record_field(&mut self, name: &str, field: OptionField);

    /// Visits an [`OptionSet`] value named `name`.
    fn record_set(&mut self, name: &str, group: OptionSet);
}
```

This new representation has a few advantages:

* The `OptionsMetadata` trait can be implemented manually for cases where the derive macro isn't flexible enough
* The `OptionsMetadata` provides all relevant abstractions so that the macro is optional. It means you can now understand the system without understanding the macro.
* Implementing `flatten` becomes a simple `Nested::record(visit)` call (instead of calling `visit.record_set(name, Nested::metadata())`. Meaning we traverse into the sub-set without telling the visitor that it now enters a new set. 

The main downside of the new API is that writing visitors is awkward. This is evident by the longer implementations for `has`, `get`, and `Display`. I think that's a fair tradeoff, considering that these methods are implemented once and that a similar API is provided to consumers. 

## Test Plan

* Verified that `cargo dev generate-options` produces the exact same output
* Verfieid that the `config` command continues working


---

_Comment by @MichaReiser on 2023-09-22 07:52_

Current dependencies on/for this PR:
* main
  * **PR #7566** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7566" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7591** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7591" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #7593** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7593" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #7549** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7549" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #7599** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7599" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7591?utm_source=stack-comment).

---

_Comment by @codspeed-hq[bot] on 2023-09-22 08:01_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/refactor-options)

### Merging #7591 will **not alter performance**

<sub>Comparing <code>refactor-options</code> (f63df7d) with <code>main</code> (9d16e46)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_@MichaReiser reviewed on 2023-09-22 08:13_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_options.rs`:25 on 2023-09-22 08:13_

I don't know how I feel about sorting options. It disallows grouping options that are semantically related (all resolver settings)

---

_@MichaReiser reviewed on 2023-09-22 08:14_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:2415 on 2023-09-22 08:14_

This also feels less hacky now since we have a trait. 

---

_Marked ready for review by @MichaReiser on 2023-09-22 08:19_

---

_Review requested from @konstin by @MichaReiser on 2023-09-22 08:19_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-09-22 08:19_

---

_Label `internal` added by @MichaReiser on 2023-09-22 08:19_

---

_@MichaReiser reviewed on 2023-09-22 08:27_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_options.rs`:108 on 2023-09-22 08:27_

The old implementation only supports one level of nesting. This would have become a problem with `lint.pydocstyle.option` where we have two level nesting.

---

_Comment by @github-actions[bot] on 2023-09-22 08:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @konstin on `crates/ruff_dev/src/generate_options.rs`:17 on 2023-09-22 12:37_

`writeln!` with trailing `\n` looks strange

---

_@konstin approved on 2023-09-22 12:41_

---

_@MichaReiser reviewed on 2023-09-22 15:29_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_options.rs`:17 on 2023-09-22 15:29_

Agree but everything else feels verbose just to get two newlines

---

_@charliermarsh reviewed on 2023-09-22 16:02_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_options.rs`:17 on 2023-09-22 16:02_

(I typically do two `writeln` for this but don't feel strongly.)

---

_@charliermarsh approved on 2023-09-22 16:03_

---

_Merged by @MichaReiser on 2023-09-22 16:19_

---

_Closed by @MichaReiser on 2023-09-22 16:19_

---

_Branch deleted on 2023-09-22 16:20_

---
