```yaml
number: 9927
title: Merge maps when combining options
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
  - breaking
  - configuration
assignees: []
base: ruff-0.5
head: charlie/merge
created_at: 2024-02-11T03:28:34Z
updated_at: 2024-06-26T08:11:59Z
url: https://github.com/astral-sh/ruff/pull/9927
synced_at: 2026-01-10T21:55:59Z
```

# Merge maps when combining options

---

_Pull request opened by @charliermarsh on 2024-02-11 03:28_

## Summary

If a user provides a value via a sub-table in a configuration file, we should combine that table when extending, rather than overwriting it.

Closes https://github.com/astral-sh/ruff/issues/9872.

## Test Plan

Creating a directory following the structure in #9872. Verified that `--show-settings` showed the custom section as a known package.


---

_Label `bug` added by @charliermarsh on 2024-02-11 03:28_

---

_Label `configuration` added by @charliermarsh on 2024-02-11 03:28_

---

_Marked ready for review by @charliermarsh on 2024-02-11 03:28_

---

_Comment by @charliermarsh on 2024-02-11 03:32_

Adding a test...

---

_Comment by @github-actions[bot] on 2024-02-11 03:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_macros/src/combine_options.rs`:53 on 2024-02-11 09:20_

I prefer "dumb" derive macros that, ideally, call a trait method rather than that they contain the entire implementation. 

This is achievable for `CombinePluginOptions`, although it requires more boilerplate code. 

`handle_field` in macro:

```rust
    Ok(quote_spanned!(
        ident.span() => #ident: crate::configuration::CombinePluginOptions::combine(self.#ident, other.#ident)
    ))
```

next to `CombinePluginOptions`

```
macro_rules! or_combine_plugin_impl {
    ($ty:ident) => {
        impl CombinePluginOptions for Option<$ty> {
            #[inline]
            fn combine(self, other: Self) -> Self {
                self.or(other)
            }
        }
    };
}

or_combine_plugin_impl!(bool);
or_combine_plugin_impl!(u8);
or_combine_plugin_impl!(u16);
or_combine_plugin_impl!(u32);
or_combine_plugin_impl!(u64);
or_combine_plugin_impl!(usize);
or_combine_plugin_impl!(i8);
or_combine_plugin_impl!(i16);
or_combine_plugin_impl!(i32);
or_combine_plugin_impl!(i64);
or_combine_plugin_impl!(isize);
or_combine_plugin_impl!(String);

impl<T: CombinePluginOptions> CombinePluginOptions for Option<T> {
    fn combine(self, other: Self) -> Self {
        match (self, other) {
            (Some(base), Some(other)) => Some(base.combine(other)),
            (Some(base), None) => Some(base),
            (None, Some(other)) => Some(other),
            (None, None) => None,
        }
    }
}

impl<T> CombinePluginOptions for Option<Vec<T>> {
    fn combine(self, other: Self) -> Self {
        self.or(other)
    }
}

impl<T, S> CombinePluginOptions for Option<HashSet<T, S>> {
    fn combine(self, other: Self) -> Self {
        self.or(other)
    }
}

impl<K, V, S> CombinePluginOptions for HashMap<K, V, S>
where
    K: Eq + Hash,
    S: BuildHasher,
{
    fn combine(mut self, other: Self) -> Self {
        self.extend(other);
        self
    }
}

or_combine_plugin_impl!(ParametrizeNameType);
or_combine_plugin_impl!(ParametrizeValuesType);
or_combine_plugin_impl!(ParametrizeValuesRowType);
or_combine_plugin_impl!(Quote);
or_combine_plugin_impl!(Strictness);
or_combine_plugin_impl!(RelativeImportsOrder);
or_combine_plugin_impl!(LineLength);
or_combine_plugin_impl!(Convention);
or_combine_plugin_impl!(IndentStyle);
or_combine_plugin_impl!(QuoteStyle);
or_combine_plugin_impl!(LineEnding);
or_combine_plugin_impl!(DocstringCodeLineWidth);
```

We could avoid all the boilerplate if Rust supported specialization...  (it would boil down to `impl<T> CombinePluginOptions for Option<T>` and one specialization `impl<T: CombinePluginOptions> for Option<T>`). Maybe @BurntSushi knows a better way to avoid some of the boilerplate. 

The advantage of being explicit about how `CombinePluginOptions` is implemented is that we can have custom implementations (like for HashMap) or even other types we don't know yet. It also ensures that we explicitly consider how `Option<T>` should be merged rather than assuming `Option::or` is the right choice.



---

_@MichaReiser approved on 2024-02-11 09:21_

This is one of those bug fixes that are, theoretically, a breaking change 😟 

---

_@charliermarsh reviewed on 2024-02-11 15:21_

---

_Review comment by @charliermarsh on `crates/ruff_macros/src/combine_options.rs`:53 on 2024-02-11 15:21_

Sounds good, I can do this.

---

_Comment by @charliermarsh on 2024-02-11 15:53_

Yeah. I think this behavior is more intuitive when you consider how TOML is structured, but it also means there's now no way to "unset" values in a map when extending.


---

_@BurntSushi reviewed on 2024-02-12 13:06_

---

_Review comment by @BurntSushi on `crates/ruff_macros/src/combine_options.rs`:53 on 2024-02-12 13:06_

Yeah I like @MichaReiser's approach here.

> We could avoid all the boilerplate if Rust supported specialization... (it would boil down to `impl<T> CombinePluginOptions for Option<T>` and one specialization `impl<T: CombinePluginOptions> for Option<T>`). Maybe @BurntSushi knows a better way to avoid some of the boilerplate.

Nothing is really coming to me here. And [specialization is unfortunately probably not going to land any time soon](https://github.com/rust-lang/rust/issues/31844).

---

_Comment by @LTourinho on 2024-02-13 11:09_

> Yeah. I think this behavior is more intuitive when you consider how TOML is structured, but it also means there's now no way to "unset" values in a map when extending.

You should still be able to "unset" by explicitly writing the extended value and setting it to a null value, perhaps. The issue would be that there isn't (I am not aware of) a "universal safe value". I think TOML explicitly doesn't like or expects to handle empty/null/nil values.

I think an explicit unset could still be expressed by:
./.ruff.toml:
>[sometag]
a = "a"

./subdir/.ruff.toml:
>extend "../ruff.toml"
[sometag]
a = {}


---

_@MichaReiser reviewed on 2024-02-14 14:38_

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/combine_options.rs`:53 on 2024-02-14 14:38_

I applied the changes for you 

---

_Label `breaking` added by @charliermarsh on 2024-02-26 15:54_

---

_Added to milestone `v0.3.0` by @charliermarsh on 2024-02-26 15:54_

---

_Removed from milestone `v0.3.0` by @MichaReiser on 2024-02-29 13:28_

---

_Added to milestone `v0.4` by @MichaReiser on 2024-02-29 13:28_

---

_Closed by @MichaReiser on 2024-03-06 09:00_

---

_Branch deleted on 2024-03-06 09:00_

---

_Branch restored on 2024-03-08 11:43_

---

_Reopened by @MichaReiser on 2024-03-08 11:44_

---

_Removed from milestone `v0.4.0` by @dhruvmanila on 2024-04-18 19:49_

---

_Added to milestone `v0.5.0` by @dhruvmanila on 2024-04-18 19:49_

---

_Comment by @MichaReiser on 2024-06-25 08:32_

To confirm the behavior change. We'll now start merging the following options:

* `per_file_ignores`
* `extend-per-file-ignores` (Do we still need the `extend`?) 
* `flake8_import_convention_options.aliases`
* `flake8_import_convention_options.extend_aliases`
* `flake8_import_convention_options.banned_aliases`
* `flake8_import_convention_options.banned_from`
* `flake8_tidy_imports.banned_api`
* `isort.sections`

I'm not sure whether the new behavior is correct for all settings, or at least it becomes less clear why we would still need the `extend-` options when "extending" becomes the new default for tables. 

The new `CombineOptions` allows us to use different merging behaviors for different settings, but I'm worried this will be confusing. 

This makes me hesitant to merge this change. @charliermarsh what are your thoughts on this?

---

_Assigned to @MichaReiser by @MichaReiser on 2024-06-25 08:38_

---

_Comment by @MichaReiser on 2024-06-26 08:11_

I don't feel confident that this is the right solution to justify a breaking change because it doesn't play nice with Ruff's `extend` semantics. We need to holistically rethink the semantics of extending configurations. 

---

_Closed by @MichaReiser on 2024-06-26 08:11_

---
