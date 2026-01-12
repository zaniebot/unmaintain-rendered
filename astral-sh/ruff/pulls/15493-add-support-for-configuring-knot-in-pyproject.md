```yaml
number: 15493
title: "Add support for configuring knot in `pyproject.toml` files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/knot-configuration
created_at: 2025-01-15T10:55:55Z
updated_at: 2025-01-17T08:42:36Z
url: https://github.com/astral-sh/ruff/pull/15493
synced_at: 2026-01-12T15:55:51Z
```

# Add support for configuring knot in `pyproject.toml` files

---

_@MichaReiser_

## Summary

This PR adds support for configuring Red Knot in the `tool.knot` section of the project's
`pyproject.toml` section. Options specified on the CLI precede the
options in the configuration file.

This PR only supports the `environment` and the `src.root` options for now.
Other options will be added as separate PRs.

There are also a few concerns that I intentionally ignored as part of this PR:

* Handling of relative paths: We need to anchor paths relative to the current working directory (CLI), or the project (`pyproject.toml` or `knot.toml`)
* Tracking the source of a value. Diagnostics would benefit from knowing from which configuration a value comes so that we can point the user to the right configuration file (or CLI) if the configuration is invalid.
* Schema generation and there's a lot more; see https://github.com/astral-sh/ty/issues/219

This PR changes the default for first party codes: Our existing default was to only add the project root. Now, Red Knot adds the project root and `src` (if such a directory exists). 

Theoretically, we'd have to add a file watcher event that changes the first-party search paths if a user later creates a `src` directory. I think this is pretty uncommon, which is why I ignored the complexity for now but I can be persuaded to handle it if it's considered important.

Part of https://github.com/astral-sh/ty/issues/219

## Test Plan

Existing tests, new file watching test demonstrating that changing the python version and platform is correctly reflected. 


---

_Label `red-knot` added by @MichaReiser on 2025-01-15 10:56_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:82 on 2025-01-15 10:57_

Rename CLI option?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/testing.rs`:242 on 2025-01-15 11:02_

I renamed `site_packages` to `python` according to the CLI document (at least that's how I understood the change). @AlexWaygood let me know if that's what you intended.

---

_Comment by @github-actions[bot] on 2025-01-15 11:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/project/options.rs`:11 on 2025-01-15 11:11_

I renamed `Configuration` to `Options` to match uv's and ruff's terminology.

---

_@MichaReiser reviewed on 2025-01-15 11:15_

---

_Marked ready for review by @MichaReiser on 2025-01-15 13:04_

---

_Review requested from @carljm by @MichaReiser on 2025-01-15 13:04_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-15 13:04_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-15 13:04_

---

_@sharkdp reviewed on 2025-01-15 15:53_

---

_Review comment by @sharkdp on `crates/red_knot/src/main.rs`:46 on 2025-01-15 15:53_

Minor: do we want to be consistent on capitalizing "Python"? (see "about" above, for example)

---

_@sharkdp reviewed on 2025-01-15 15:57_

---

_Review comment by @sharkdp on `crates/red_knot/tests/cli.rs`:27 on 2025-01-15 15:57_

I suppose this is used because it was introduced in 3.12. Maybe add a comment here to make that more clear?
```suggestion
# `sys.last_exc` was introduced in 3.12
print(sys.last_exc)
```

---

_@sharkdp reviewed on 2025-01-15 15:58_

---

_Review comment by @sharkdp on `crates/red_knot/tests/cli.rs`:49 on 2025-01-15 15:58_

Can we also assert that the same command fails without the `--python-version 3.12` option? Otherwise, this could just spuriously succeed.

---

_@sharkdp reviewed on 2025-01-15 16:09_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/src/project/combine.rs`:22 on 2025-01-15 16:09_

This particular example is a bit confusing to me, as `--ignore` and `--warn` seem like two (related but) different options?

The inclusion/exclusion patterns might be a better example a merging of something like `exclude = ["*.png"]` and `exclude = ["!important.png"]` to `["*.png", "!important.png"]`?

---

_Review comment by @sharkdp on `crates/red_knot/tests/file_watching.rs`:912 on 2025-01-15 16:15_

Minor
```suggestion
    let diagnostics = case.db.check().context("Failed to check project.")?;
```

---

_@sharkdp reviewed on 2025-01-15 16:15_

---

_Review comment by @sharkdp on `crates/red_knot/tests/file_watching.rs`:935 on 2025-01-15 16:15_

```suggestion
    let diagnostics = case.db.check().context("Failed to check project.")?;
```

---

_@sharkdp reviewed on 2025-01-15 16:15_

---

_@sharkdp reviewed on 2025-01-15 16:22_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/src/project/combine.rs`:50 on 2025-01-15 16:22_

Minor naming comment: "combine" has no clear direction/order/precedence for me. Something like `OverrideWith`, `OverlayWith`, `ExtendWith` or similar might be a bit clearer in terms of which side is taking precedence? (after potentially swapping sides in the implementation)

---

_Review comment by @sharkdp on `crates/ruff_macros/src/combine.rs`:41 on 2025-01-15 16:27_

Mostly just curious: Would it make sense (and is it possible?) to auto-derive `Combine` for enums by simply taking the value from the side which takes precendence and forgetting the other one?

Not sure if that's what we want in all cases though. In the `Option` case for example, we probably ignore overriding with `None`.

---

_@sharkdp reviewed on 2025-01-15 16:27_

---

_@sharkdp reviewed on 2025-01-15 16:27_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/src/project/combine.rs`:147 on 2025-01-15 16:27_

Minor readability:
```suggestion
                // The value from 'a' takes precedence
```

---

_@sharkdp reviewed on 2025-01-15 16:32_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/src/project/combine.rs`:50 on 2025-01-15 16:32_

Also: I want that trait in a separate crate so I can use it in my projects! :star_struck: 

---

_@sharkdp reviewed on 2025-01-15 16:37_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/src/project/combine.rs`:77 on 2025-01-15 16:37_

I could probably understand this from the code above... but what's the reason that we can't implement `Combine` for `Vec` and `HashMap` directly and then have one unified implementation for `Option<T>`?

---

_@MichaReiser reviewed on 2025-01-15 16:41_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/project/combine.rs`:50 on 2025-01-15 16:41_

I used this name because it's what Ruff and uv are using... I considered naming it `Merge` but it didn't seem an improvement enough to justify diverging.

---

_@MichaReiser reviewed on 2025-01-15 16:44_

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/combine.rs`:41 on 2025-01-15 16:44_

Neither ruff nor uv required deriving `Combine` for enums and I'm not 100% sure what the semantics should be for enum with fields (we may want to merge them?). 

I suggest that we defer this until we have a use case where we can discuss the desired semantics.

---

_@AlexWaygood reviewed on 2025-01-15 16:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/testing.rs`:242 on 2025-01-15 16:49_

Although we'll be using `--python` for the CLI argument, it _might_ be better to use variables such as `sys_prefix` and call the enum `SysPrefixPath`. That would be more precise and help me remember that the path needs to point to the grandparent directory of the Python binary rather than the path to the binary itself. See this doc-comment for a definition of `sys.prefix`:

https://github.com/astral-sh/ruff/blob/48e65418938da311b591bcd6e0f691f888bcb5e8/crates/red_knot_python_semantic/src/site_packages.rs#L360-L374

You might also want to rename this struct to something like `PythonInstallation`, update the `venv_path` field to `sys_prefix_path`, and update the doc-comments to note that it could point to a system installation or a virtual-environment "installation" https://github.com/astral-sh/ruff/blob/48e65418938da311b591bcd6e0f691f888bcb5e8/crates/red_knot_python_semantic/src/site_packages.rs#L22-L42

---

_@MichaReiser reviewed on 2025-01-15 17:35_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/project/combine.rs`:77 on 2025-01-15 17:35_

I can change it to have one `impl_combine_option` and another `impl_combine_rec` macro to make it more clear. 

What we'd need is:

```rust
impl<T> Combine for Option<T> {
	fn combine_with(&mut self, other: Self) {
		if self.is_none() {
			*self = other;
		}
	}
}

// Specialize for `T` where `T` implements Combine
// Recursively merges the value instead of just picking `self` over `other`
impl<T> Combine for Option<T> where T: Combine {
	fn combine_with(&mut self, other: Self) {
		if let (Some(a), Some(mut b)) = (self, other) {
			// recurse!
			a.combine_with(b);
		} else if self.is_none() {
			*self = other;
		}
	}
}
```

---

_@sharkdp reviewed on 2025-01-15 17:53_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/src/project/combine.rs`:50 on 2025-01-15 17:53_

Ah, I didn't realize it was an existing concept.

---

_@sharkdp reviewed on 2025-01-15 18:23_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/src/project/combine.rs`:77 on 2025-01-15 18:23_

I was thinking something like this, which seems to pass all tests, but I haven't thought it through completely. Feel free to disregard this comment.

```rs
impl<T> Combine for Vec<T> {
    fn combine_with(&mut self, mut other: Self) {
        // `self` takes precedence over `other` but values with higher precedence must be placed after.
        // Swap the vectors so that `other` is the one that gets extended, so that the values of `self` come after.
        std::mem::swap(self, &mut other);
        self.extend(other);
    }
}

impl<K, V, S> Combine for HashMap<K, V, S>
where
    K: Eq + std::hash::Hash,
    S: BuildHasher,
{
    fn combine_with(&mut self, mut other: Self) {
        // `self` takes precedence over `other` but `extend` overrides existing values.
        // Swap the hash maps so that `other` is the one that gets extended.
        std::mem::swap(self, &mut other);
        self.extend(other);
    }
}

impl<T: Combine> Combine for Option<T> {
    fn combine_with(&mut self, other: Self) {
        match self {
            None => *self = other,
            Some(a) => {
                if let Some(b) = other {
                    a.combine_with(b);
                }
            }
        }
    }
}

macro_rules! impl_trivial_combine {
    ($name:ident) => {
        impl Combine for $name {
            fn combine_with(&mut self, _other: Self) {}
        }
    };
}

impl_trivial_combine!(SystemPathBuf);
impl_trivial_combine!(PythonPlatform);
impl_trivial_combine!(PythonPath);
impl_trivial_combine!(PythonVersion);

impl_trivial_combine!(bool);
impl_trivial_combine!(usize);
impl_trivial_combine!(u8);
impl_trivial_combine!(u16);
impl_trivial_combine!(u32);
impl_trivial_combine!(u64);
impl_trivial_combine!(u128);
impl_trivial_combine!(isize);
impl_trivial_combine!(i8);
impl_trivial_combine!(i16);
impl_trivial_combine!(i32);
impl_trivial_combine!(i64);
impl_trivial_combine!(i128);
impl_trivial_combine!(String);
```

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:167 on 2025-01-15 18:54_

`program_settings` looks maybe like an out-of-date variable name here?

---

_@MichaReiser reviewed on 2025-01-15 19:03_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/project/combine.rs`:77 on 2025-01-15 19:03_

Oh that's clever

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/program.rs`:141 on 2025-01-15 19:24_

I'm not sure if this is new behavior or just moving around existing behavior, but this doesn't look right. The semantics of `PythonPath::Derived` are that we store just the root of the python installation (`sys.prefix` -- which as @AlexWaygood mentioned above, would probably be a better name than `venv_path`, since it could also be a system Python installation), and then we can derive the actual site-packages path from that. But the implementation here suggests that "derivation" is just "use the prefix path", which is wrong. If the prefix/venv path is e.g. `/usr`, then the actual site-packages path would be `/usr/lib/python3.XX/site-packages`, where `XX` is the Python minor version. That is the "derivation" that we need to do to correctly go from a `PythonPath::Derived` to an actual path to put on the import search path, and which seems to be skipped here.

Maybe this should be fixed in a separate PR that is actually focused on making sure we can import from things installed in a real venv (or Python installation), but in that case I would still put a TODO here, because the current code looks definitely wrong.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/module_resolver/testing.rs`:242 on 2025-01-15 19:28_

I think either `site_packages / SitePackages` or `python_path / PythonPath` are reasonable names for this field and the enum that occupies it. I think just `python` is a bit less clear / more confusing. (I'm not clear whether there's any reason the name of this field needs to correspond to the CLI option name.)

I think renaming the `venv_path` field in the `Derived` variant of the enum to `sys_prefix` makes sense; that clarifies what path it holds, and that it can be either for a venv or a system Python installation.

I think renaming the entire enum to `SysPrefixPath` would not make sense, because the `Known` variant of the enum does not hold a `sys.prefix` path at all, it holds actual site-packages path(s). Only the `Derived` variant holds a `sys.prefix` path.

---

_@carljm reviewed on 2025-01-15 19:40_

---

_@MichaReiser reviewed on 2025-01-15 21:40_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/program.rs`:141 on 2025-01-15 21:40_

I'll rever the `PythoPath` changes and keep the `venv` option for now. I'll takle this in a separate PR (seems more involved than just renaming)

---

_Comment by @MichaReiser on 2025-01-15 21:40_

Reading through the comments. It looks like this is good to land for as long as I undo the rename from `venv-path` to `python`

---

_@MichaReiser reviewed on 2025-01-16 12:19_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/file_watching.rs`:935 on 2025-01-16 12:19_

Thanks copilot strikes again.

---

_@MichaReiser reviewed on 2025-01-16 13:20_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/project/combine.rs`:22 on 2025-01-16 13:20_

I updated the comment and also pointed out a potential "shortcoming" of this approach. I'm interested to hear if there are any concerns or if we feel comfortable moving forward with how it is

---

_@MichaReiser reviewed on 2025-01-16 13:21_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:167 on 2025-01-16 13:21_

I moved it into `ProjectDatabase::new` where this now fits better

---

_@MichaReiser reviewed on 2025-01-16 13:41_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/testing.rs`:242 on 2025-01-16 13:41_

I created a follow up issue https://github.com/astral-sh/ruff/issues/15530 Thanks for all the feedback

---

_@sharkdp reviewed on 2025-01-16 19:18_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/src/project/options.rs`:93 on 2025-01-16 19:18_

see …? :smile: 

---

_@sharkdp approved on 2025-01-16 19:22_

Thank you, looks great!

I'm not yet familiar with the `file_watching` part, so I only skimmed that. Let me know if I should do a more detailed review.

---

_@sharkdp reviewed on 2025-01-16 19:24_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/src/project/combine.rs`:22 on 2025-01-16 19:24_

I can certainly imagine cases where we would want a "replace" logic for lists, instead of a "extend" logic. But I think it's completely fine to try and see how far this approach takes us. Thank you for the updated description!

---

_Merged by @MichaReiser on 2025-01-17 08:41_

---

_Closed by @MichaReiser on 2025-01-17 08:41_

---

_Branch deleted on 2025-01-17 08:41_

---
