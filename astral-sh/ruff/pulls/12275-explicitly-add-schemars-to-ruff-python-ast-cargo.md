```yaml
number: 12275
title: Explicitly add schemars to ruff_python_ast Cargo.toml
type: pull_request
state: merged
author: GaetanLepage
labels:
  - internal
assignees: []
merged: true
base: main
head: cargo
created_at: 2024-07-10T12:38:48Z
updated_at: 2024-07-11T06:46:44Z
url: https://github.com/astral-sh/ruff/pull/12275
synced_at: 2026-01-10T21:47:02Z
```

# Explicitly add schemars to ruff_python_ast Cargo.toml

---

_Pull request opened by @GaetanLepage on 2024-07-10 12:38_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Context: [Updating ruff from 0.5.0 to 0.5.1 on nixpkgs](https://github.com/NixOS/nixpkgs/pull/324823).

We noticed that since https://github.com/astral-sh/ruff/commit/5109b50bb3847738eeb209352cf26bda392adf62, `ruff` was not building successfully in our pipeline:
```
thread 'main' panicked at cargo-auditable/src/collect_audit_data.rs:77:9:
error: Package `ruff_python_ast v0.0.0 (/build/source/crates/ruff_python_ast)` does not have feature `schemars`. It has an optional dependency with that name, but that dependency us>

note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
error: could not compile `ruff_python_formatter` (bin "ruff_python_formatter")
```

This patch by @eljamm seems to fix it on our end.

## Test Plan

We were able to build `ruff` 0.5.1 with this patch and its test suite is passing.


---

_Comment by @MichaReiser on 2024-07-10 12:40_

Thanks for the PR. 

I'm hesitant merging the PR because it's unclear to me where this feature is used (I can't find any usage in our code). Ideally we would change the usage rather than the features exposed by the crate.

---

_Comment by @MichaReiser on 2024-07-10 12:42_

Do you have a link to the CI log where the build on nix fails? I wonder what command it is using

---

_Comment by @GaetanLepage on 2024-07-10 12:49_

> Do you have a link to the CI log where the build on nix fails? I wonder what command it is using

Here is the entire log. I am afraid that it won't help you much more than the error message I shared.
https://pastebin.com/RCDPtNmj (the build command is at line 50).

It is weird, because manually running the exact same `cargo build` command on the exact same source files works fine.
Our sandbox might be doing something weird here, but I have no idea what.

---

_Comment by @eljamm on 2024-07-10 16:41_

After a bit more of digging, this is my best guess of what's going on. According to [Features - The Cargo Book](https://doc.rust-lang.org/cargo/reference/features.html#optional-dependencies):

> By default, this optional dependency implicitly defines a feature that looks like this:
> ```rust
> [features]
> gif = ["dep:gif"]
> ```

But, it also says that:

> If you specify the optional dependency with the dep: prefix anywhere in the [features] table, that disables the implicit feature

Which is exactly what's happening here:

```rust
[features]
serde = ["dep:serde", "ruff_text_size/serde", "dep:ruff_cache", "compact_str/serde", "dep:ruff_macros", "dep:schemars"]
```

This is what the error is complaining about :

> error: Package `ruff_python_ast v0.0.0 (/build/source/crates/ruff_python_ast)` does not have feature `schemars`. It has an optional dependency with that name, but that dependency uses the "dep:" syntax in the features table, so it does not have an implicit feature with that name.

Adding `schemars = ["dep:schemars"]` to the features satisfies this condition and makes the program compile correctly.

But why is this happening? I have no clue :shrug:

schemars is apparently needed in `ruff_python_ast/src/name.rs#L183`

```rust
#[cfg(feature = "serde")]
impl schemars::JsonSchema for Name {
    fn is_referenceable() -> bool {
        String::is_referenceable()
    }
...
```

Because when I remove `"dep:schemars"` from the features:

```diff
 [features]
-serde = ["dep:serde", "ruff_text_size/serde", "dep:ruff_cache", "compact_str/serde", "dep:ruff_macros", "dep:schemars"]
+serde = ["dep:serde", "ruff_text_size/serde", "dep:ruff_cache", "compact_str/serde", "dep:ruff_macros"]
```

That's where it complains about it being missing in [ruff.log](https://github.com/user-attachments/files/16165022/ruff-schemars.log)

But even though schemars is included as a dep of serde, it's not being used. In [other crates](https://github.com/search?q=repo%3Aastral-sh%2Fruff+language%3Atoml+schemars&type=code) like [ruff_python_formatter](https://github.com/astral-sh/ruff/blob/abcf07c8c5188400adaa75a60712ce97a78ee9ef/crates/ruff_python_formatter/Cargo.toml#L63), we can see that schemars is defined independently from serde, so I don't know why it's different for `ruff_python_ast`.

**Note:** Another solution is to make schemars non-optional:

<details>

<summary> non-optional-schemars.patch </summary>

```diff
---
 crates/ruff_python_ast/Cargo.toml | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)

diff --git a/crates/ruff_python_ast/Cargo.toml b/crates/ruff_python_ast/Cargo.toml
index bd41c71b6..e4e8e4304 100644
--- a/crates/ruff_python_ast/Cargo.toml
+++ b/crates/ruff_python_ast/Cargo.toml
@@ -25,12 +25,12 @@ is-macro = { workspace = true }
 itertools = { workspace = true }
 once_cell = { workspace = true }
 rustc-hash = { workspace = true }
-schemars = { workspace = true, optional = true }
+schemars = { workspace = true }
 serde = { workspace = true, optional = true }
 compact_str = { workspace = true }
 
 [features]
-serde = ["dep:serde", "ruff_text_size/serde", "dep:ruff_cache", "compact_str/serde", "dep:ruff_macros", "dep:schemars"]
+serde = ["dep:serde", "ruff_text_size/serde", "dep:ruff_cache", "compact_str/serde", "dep:ruff_macros"]
 
 [lints]
 workspace = true
-- 
2.45.1
```

</details>

---

_Comment by @MichaReiser on 2024-07-10 17:06_

Your analysis is correct. The part that I don't understand is that the error can only occur if some code adds a dependency to `ruff_python_ast` with `features=["schemars"]` or has a feature that enables `ruff_python_ast`s `schemars` feature, which doesn't exist.

I haven't found any code that enables that feature or requires that feature from `ruff_python_ast`. The only reference I've seen reference the `serde` feature, which does exist.

---

_Comment by @eljamm on 2024-07-10 19:12_

I finally found an explanation of why this is happening, from https://github.com/parcel-bundler/lightningcss/pull/713#issuecomment-2130870582:

> [cargo-auditable](https://github.com/NixOS/nixpkgs/pull/223320) is a tool to store the exact versions of dependencies in rust binaries and makes it easier/possible to audit the dependency tree of rust binaries.
> 
> It is a [drop-in replacement](https://www.reddit.com/r/rust/comments/zgz96m/cargo_auditable_can_now_be_used_as_a_dropin/) of cargo. Mainly this lets easier identification of vulnerabilities (CVEs). Nixpkgs by default uses cargo auditable to build rust binaries.
> 
> Stricter checks like these are implemented by cargo-auditable. And hence this PR. cargo auditable is compatible with cargo itself, so this change won't affect builds with cargo.

As proposed by the PR above, to fix this issues for cargo-auditable, it's enough to just remove the `dep:` prefix:

```diff
 [features]
-serde = ["dep:serde", "ruff_text_size/serde", "dep:ruff_cache", "compact_str/serde", "dep:ruff_macros", "dep:schemars"]
+serde = ["dep:serde", "ruff_text_size/serde", "dep:ruff_cache", "compact_str/serde", "dep:ruff_macros", "schemars"]
```

With this, ruff compiled successfully both manually (with cargo) and in nixpkgs (with cargo-auditable).

I tracked the main issue behind this down to https://github.com/rust-secure-code/cargo-auditable/issues/124 which appears to be caused by cargo in which it passes the wrong features to cargo-auditable.

It would be nice to fix this in ruff, but I'm also worried that it would be confusing as `schemars` is marked optional but is not called with the `dep:` prefix. If this is wrongly assumed to be a mistake in the future, it can be re-added and thus break the compilation again in nixpkgs.

What do you think @MichaReiser @GaetanLepage ?

---

_Comment by @MichaReiser on 2024-07-10 20:11_

Wow nice find! I think what I'll do is to split the feature into three:
- schemars
- Serde
- Cache

Which is also more consistent with other crates and should reduce the risk of breaking in the future because someone readds the dep prefix 

---

_Comment by @GaetanLepage on 2024-07-10 21:49_

Thanks so much @eljamm for this amazing deep dive !
What should we do now ? Should I apply [your patch](https://github.com/astral-sh/ruff/pull/12275#issuecomment-2221245280) in this PR ?

---

_Comment by @eljamm on 2024-07-10 21:59_

I'll leave that call to @MichaReiser if he wants to add this change now or wait until he implements his suggestion.

---

_Comment by @MichaReiser on 2024-07-11 06:36_

The exact command to reproduce the error is `cargo auditable build  --target x86_64-unknown-linux-gnu` Building without a target succeeds

---

_Label `internal` added by @MichaReiser on 2024-07-11 06:42_

---

_Merged by @MichaReiser on 2024-07-11 06:46_

---

_Closed by @MichaReiser on 2024-07-11 06:46_

---

_Branch deleted on 2024-07-11 06:46_

---
