```yaml
number: 9599
title: Allow arbitrary configuration options to be overridden via the CLI
type: pull_request
state: merged
author: AlexWaygood
labels:
  - cli
assignees: []
merged: true
base: main
head: arbitrary-cli-overrides
created_at: 2024-01-21T18:04:16Z
updated_at: 2024-02-19T03:39:25Z
url: https://github.com/astral-sh/ruff/pull/9599
synced_at: 2026-01-10T22:57:09Z
```

# Allow arbitrary configuration options to be overridden via the CLI

---

_Pull request opened by @AlexWaygood on 2024-01-21 18:04_

Fixes #8368
Fixes https://github.com/astral-sh/ruff/issues/9186

## Summary

Arbitrary TOML strings can be provided via the command-line to override configuration options in `pyproject.toml` or `ruff.toml`. As an example: to run over typeshed and respect typeshed's `pyproject.toml`, but override a specific isort setting and enable an additional pep8-naming setting:

```
cargo run -- check ../typeshed --no-cache --config ../typeshed/pyproject.toml --config "lint.isort.combine-as-imports=false" --config "lint.extend-select=['N801']"
```

This is what the error message currently looks like if you provide an invalid `--config` option:

```
>cargo run -- check ../typeshed --no-cache --config foo.toml
    Finished dev [unoptimized + debuginfo] target(s) in 0.77s
     Running `target\debug\ruff.exe check ../typeshed --no-cache --config foo.toml`
error: invalid value 'foo.toml' for '--config <CONFIG_OPTION>'

  tip: The `--config` flag must either be a path to a `.toml` configuation file or a TOML string providing configuration overrides
  tip: The path `foo.toml` does not exist on your filesystem

For more information, try '--help'.
error: process didn't exit successfully: `target\debug\ruff.exe check ../typeshed --no-cache --config foo.toml` (exit code: 2)
```

## Test Plan

~~So far, just manual testing. TODO: write proper tests and docs.~~

Several tests added to `crates/ruff/tests/format.rs` and `crates/ruff/tests/lint.rs`. Also, extensive manual testing.


---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:515 on 2024-01-22 08:14_

```suggestion
    /// Overrides provided via specific flags such as --line-length etc
```

These aren't legacy. It's just that we don't want to expose all options via CLI

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:525 on 2024-01-22 08:16_

I recommend just passing a `Vec<ConfigOption>` here. It only seems important to distinguish the cases: a) There are no options, b) there are some options.

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:543 on 2024-01-22 08:17_

We'll need to document the precedence of options. What binds higher: `--line-length` or `--config line-length`?

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:564 on 2024-01-22 08:18_

I'm not sure if it's worth special casing here. Seems like unnecessary complexity to handle a very specific, uncommon edge case.

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:744 on 2024-01-22 08:21_

I'm conflicted if we should use the TOML terminology here or should instead say that it needs to be a config option key `=` value pairs. I at least wouldn't immediately understand what a TOML string is (`"abcd"?`) 



---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/configuration.rs`:616 on 2024-01-22 08:23_

Nit: I think we can line these above (and avoid the `mut`)
```suggestion
						extend: extend.cloned(),
						required_version: required_version.cloned(),
            ..config
        };
```

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:36 on 2024-01-22 08:24_

It's probably not a huge concern because options are rarely constructed, but where do we need to clone them now (cloning is somewhat expensive because the structure contains so many allocated sub-structures)? 

---

_@MichaReiser reviewed on 2024-01-22 08:25_

Nice! I haven't done a full review and only skimmed through the code. 

Some CLI tests showing how overrides work for `check` and `format`, including some advanced use cases like specifying values for a table (e.g is there a way to "append" to `--per-file-ignores` without having to use a JSON value?) Can I do `--config "per-file-ignores.__init__.py" = ["RUF100"]`? See `ruff/tests/format` or `/ruff/tests/lint` etc for examples.

---

_@AlexWaygood reviewed on 2024-01-22 08:29_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:543 on 2024-01-22 08:29_

Currently `--line-length`. Agree it needs to be documented!

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/options.rs`:36 on 2024-01-22 08:34_

Yeah, I didn't like having to do this. It's because we initially use `clap` to parse `--config` arguments into variants of the `ConfigOption` enum, which is defined like this:

```rs
#[derive(Clone, Debug)]
pub enum ConfigOption {
    PathToConfigFile(PathBuf),
    ConfigOverride(Box<Options>),
}
```

One of `clap` or `serde` demands that the enum be clonable if we want to use it with the framework, and that means that `Options` needs to be clonable, which then cascades down into the fields of `Options`

---

_@AlexWaygood reviewed on 2024-01-22 08:34_

---

_@MichaReiser reviewed on 2024-01-22 08:38_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:36 on 2024-01-22 08:38_

I see. Do you need to mutate the `Options` value? If not, using a `Rc` (or `Arc` if it needs to be threadsafe) might be an option which also gives you O(1) cloning

---

_@AlexWaygood reviewed on 2024-01-22 08:44_

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/options.rs`:36 on 2024-01-22 08:44_

> Do you need to mutate the `Options` value?

I don't think so, no!

> If not, using a `Rc` (or `Arc` if it needs to be threadsafe) might be an option which also gives you O(1) cloning

I'll give that a try

---

_@AlexWaygood reviewed on 2024-01-22 09:21_

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/configuration.rs`:616 on 2024-01-22 09:21_

Good point -- I pushed a refactor in 18daf07bf907eb566283ff58586c6b9010bceca2

---

_@AlexWaygood reviewed on 2024-01-22 09:23_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:564 on 2024-01-22 09:23_

Deleted it in 721653de5580ce0c1c192f2625e971d2529f7b03 :)

---

_@AlexWaygood reviewed on 2024-01-22 09:28_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:525 on 2024-01-22 09:28_

7bb72f50ded913de373dffc32e3caf5c315f2267

---

_@AlexWaygood reviewed on 2024-01-22 13:09_

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/options.rs`:36 on 2024-01-22 13:09_

Looks like it does need to be threadsafe, so `Rc` won't work for us here.

I tried doing this:

<details>
<summary>Diff I tried</summary>

```diff
--- a/crates/ruff/src/args.rs
+++ b/crates/ruff/src/args.rs
@@ -1,4 +1,5 @@
 use std::path::PathBuf;
+use std::sync::Arc;

 use anyhow::anyhow;
 use clap::builder::{TypedValueParser, ValueParserFactory};
@@ -691,7 +692,7 @@ fn resolve_bool_arg(yes: bool, no: bool) -> Option<bool> {
 #[derive(Clone, Debug)]
 pub enum ConfigOption {
     PathToConfigFile(PathBuf),
-    ConfigOverride(Box<Options>),
+    ConfigOverride(Arc<Options>),
 }

 #[derive(Clone)]
@@ -722,7 +723,7 @@ impl TypedValueParser for ConfigOptionParser {
             return Ok(ConfigOption::PathToConfigFile(path_to_config_file));
         }
         if let Ok(option) = toml::from_str(value) {
-            Ok(ConfigOption::ConfigOverride(option))
+            Ok(ConfigOption::ConfigOverride(Arc::new(option)))
         } else {
             let mut new_error =
                 clap::Error::new(clap::error::ErrorKind::ValueValidation).with_cmd(cmd);
diff --git a/crates/ruff_workspace/src/options.rs b/crates/ruff_workspace/src/options.rs
index 9eb54688a..8964d999b 100644
--- a/crates/ruff_workspace/src/options.rs
+++ b/crates/ruff_workspace/src/options.rs
@@ -31,7 +31,7 @@ use ruff_python_formatter::{DocstringCodeLineWidth, QuoteStyle};

 use crate::settings::LineEnding;

-#[derive(Clone, Debug, PartialEq, Eq, Default, OptionsMetadata, Serialize, Deserialize)]
+#[derive(Debug, PartialEq, Eq, Default, OptionsMetadata, Serialize, Deserialize)]
 #[serde(deny_unknown_fields, rename_all = "kebab-case")]
 #[cfg_attr(feature = "schemars", derive(schemars::JsonSchema))]
 pub struct Options {
```

</details>

But the compiler complains at me:

```
error[E0507]: cannot move out of an `Arc`
   --> crates\ruff\src\args.rs:539:53
    |
539 |                         Configuration::from_options(*overridden_option, &path_dedot::CWD)?,
    |                                                     ^^^^^^^^^^^^^^^^^^ move occurs because value has type `ruff_workspace::options::Options`, which does not implement the `Copy` trait
```

It seems we really do need to own an `Options` instance at that point, in order to be able to pass it to `Configuration::from_options()`. To achieve that, I think we _have_ to clone the underlying `Options` instance at that point, unfortunately -- but I might be missing something

---

_@AlexWaygood reviewed on 2024-01-22 13:13_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:515 on 2024-01-22 13:13_

True. In general, I'm not sure how to refer to this group of flags. On `main`, the struct providing information on the flags is called `CliOverrides`, but that's not a great name anymore now that we're introducing a separate mechanism allowing for arbitrary options to be overridden via the CLI. `DedicatedCliOverrideFlags`? Not sure...

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:515 on 2024-01-22 13:45_

Maybe "explicit" overrides? I think "cli" is already inherent in the context here, so just `ExplicitOverrides` and `explicit_overrides` seems okay to me.

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:627 on 2024-01-22 13:50_

I would probably write this as:

```rust
if let Some(ref config_file) = new.config_file {
    // return error
}
new.config_file = Some(path);
```

That way, you stick with the linear flow of code: check for error, return, check for error, return, finally success.

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:684 on 2024-01-22 14:27_

rustdoc uses Markdown, so the newlines you're using here to indicate pauses/breaks/end-of-sentence will disappear when rendered. If you want to keep the newlines-as-breaks, you'll need an empty line separating them.

Because rustdoc also generates short previews from the first "paragraph" of a Markdown block, my convention for docs is to write a single short sentence (usually one line) that very tersely describes what something is or does. Then insert an empty line and add additional context as necessary.

(Previews are only generated for free functions, types, macros and modules. Notably absent from that list is methods.)

See for example the previews here: https://docs.rs/rkyv/0.7.43/rkyv/index.html

Almost all of them are very short and easy to read. They're nicely done. The one exception is the `out_field` macro that (IMO) puts a little too much content in that first paragraph.

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:725 on 2024-01-22 17:50_

I would do an early return here.

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:742 on 2024-01-22 17:53_

This is a pretty long line. Unfortunately, `rustfmt` won't wrap strings for you automatically. I'd suggest this:

```rust
"The `--config` flag must either be a path to a `.toml` \
 configuration file or a TOML string providing \
 configuration overrides".into()
```

The forward slash causes all whitespace after it (up to the first non-whitespace character) to be elided.

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:744 on 2024-01-22 17:54_

I think it might be reasonable if we advertise this option as "taking a TOML key-and-value pair."

---

_Review comment by @BurntSushi on `crates/ruff_workspace/src/options.rs`:36 on 2024-01-22 18:11_

You can maybe do a little better here with [`Arc::try_unwrap`](https://doc.rust-lang.org/std/sync/struct.Arc.html#method.try_unwrap):

```
diff --git a/crates/ruff/src/args.rs b/crates/ruff/src/args.rs
index 9d72c6168..4e74891a1 100644
--- a/crates/ruff/src/args.rs
+++ b/crates/ruff/src/args.rs
@@ -1,4 +1,4 @@
-use std::path::PathBuf;
+use std::{path::PathBuf, sync::Arc};
 
 use anyhow::anyhow;
 use clap::builder::{TypedValueParser, ValueParserFactory};
@@ -534,8 +534,10 @@ impl ConfigArgs {
         for option in config_options {
             match option {
                 ConfigOption::ConfigOverride(overridden_option) => {
+                    let overridden_option =
+                        Arc::try_unwrap(overridden_option).unwrap_or_else(|x| Options::clone(&x));
                     new.config_overrides = new.config_overrides.combine(
-                        Configuration::from_options(*overridden_option, &path_dedot::CWD)?,
+                        Configuration::from_options(overridden_option, &path_dedot::CWD)?,
                     );
                 }
                 ConfigOption::PathToConfigFile(path) => {
@@ -691,7 +693,7 @@ fn resolve_bool_arg(yes: bool, no: bool) -> Option<bool> {
 #[derive(Clone, Debug)]
 pub enum ConfigOption {
     PathToConfigFile(PathBuf),
-    ConfigOverride(Box<Options>),
+    ConfigOverride(Arc<Options>),
 }
 
 #[derive(Clone)]
@@ -722,7 +724,7 @@ impl TypedValueParser for ConfigOptionParser {
             return Ok(ConfigOption::PathToConfigFile(path_to_config_file));
         }
         if let Ok(option) = toml::from_str(value) {
-            Ok(ConfigOption::ConfigOverride(option))
+            Ok(ConfigOption::ConfigOverride(Arc::new(option)))
         } else {
             let mut new_error =
                 clap::Error::new(clap::error::ErrorKind::ValueValidation).with_cmd(cmd);
```

That way, there shouldn't be any unnecessary clones if nothing is actually cloning the `ConfigOption`. But it does still unfortunately require that `Options` implements `Clone`.

I don't see a simple way of avoiding the clone here without some other kind of bigger refactoring. I can think of some reasons why the clone is probably fine:

* This only applies when options are overridden on the CLI, which is probably not the common case.
* An `Options` in this case likely only has one non-`None` value, and so the actual heap allocation happening here is probably pretty small.

---

_Review comment by @BurntSushi on `crates/ruff_workspace/src/options.rs`:36 on 2024-01-22 18:24_

With that said, it is unfortunate that this makes a bunch of non-`Clone` types `Clone`. That in turn will make it easier to clones to slip in in the future. But I don't see a simple way of avoiding that other than not using `Options` here. But that would require duplicating all of `Options` fields into another type (probably an enum of the options instead of a struct of the options). But then you'd have to impl `Clone` for that type, which would probably mean that you'd still need most of the added `Clone` impls in this PR.

The only way I see out of this is to abstain from using clap's `TypedValueParser`, which is the actual thing requiring a `Clone` implementation here. But I'm actually not familiar enough with Clap 4 to know how to do that off-hand. I took a quick look at Clap's API, and it doesn't look like there are any escape hatches. Which to me means the only way around this is to specifically opt out from Clap's "value parsing" and do it as a second step after getting the parsed args from Clap. Which will be supremely annoying I suspect.

The last idea is that `ConfigOption` be some kind of facsimile of the specific option being set from `Options`. Like for example, an `Option<(String, String)>`, with the first `String` being the key and the second `String` a value. Then only after Clap parsing would you parse that second `String` into an actual value. That would let you retain the use of Clap's value parsing machinery by side-stepping the need to involve `Options` into the trait impl. But... this is likely to prove annoying.

I think overall, on balance, doing what you did here is probably the right path even though it adds a bunch of `Clone` impls.

---

_@BurntSushi reviewed on 2024-01-22 18:29_

Nice work!

---

_@AlexWaygood reviewed on 2024-01-23 14:22_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:742 on 2024-01-23 14:22_

Thanks, I pushed your changes!

I'm a bit worries about how long this error message is in general, though. It doesn't look great on narrow screens:

![image](https://github.com/astral-sh/ruff/assets/66076021/f0c0a00f-6941-426a-9724-f6ee95963bb2)


---

_@AlexWaygood reviewed on 2024-01-23 14:34_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:742 on 2024-01-23 14:34_

This is also an issue for the `--help` text, though some of ruff's existing `--help` docs are already quite long:

![image](https://github.com/astral-sh/ruff/assets/66076021/6f5f37cc-bc9f-4f8d-b230-73fed0af8b2c)


---

_@AlexWaygood reviewed on 2024-01-23 14:45_

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/options.rs`:36 on 2024-01-23 14:45_

Nice, thanks @BurntSushi! I switched to using an `Arc`, as you recommended.

I basically agree with everything you said above -- I tried to find a way to do it without cloning, but couldn't see a way without a large refactor of lots of other code. The fact that it's already done in various similar places also made me feel a little better about things:

https://github.com/astral-sh/ruff/blob/395bf3dc98a726eed64280211f06e92b3e309467/crates/ruff/src/args.rs#L666-L749

I'll leave this conversation "unresolved", though, in case anybody else wants to chime in on the approach here

---

_@AlexWaygood reviewed on 2024-01-23 17:35_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:576 on 2024-01-23 17:35_

Using `clap` here means we get nicely formatted multiline error messages with the `tip: ` bit at the beginning:

![image](https://github.com/astral-sh/ruff/assets/66076021/9c080a05-e872-4bd0-b34b-8dbd680f33d5)

Unfortunately, because we're using `clap` _after_ the initial argument parsing here, we don't get the nice colours that we do elsewhere -- this is what it looks like when you use `clap` in the initial argument parsing:

![image](https://github.com/astral-sh/ruff/assets/66076021/12e2d6b3-fbd1-498a-b322-49d8998546c4)

I can't really see an easy solution to that

---

_@AlexWaygood reviewed on 2024-01-24 01:14_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:576 on 2024-01-24 01:14_

Hmm, I have an idea. Will try it out tomorrow...

---

_@BurntSushi reviewed on 2024-01-24 14:26_

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:742 on 2024-01-24 14:26_

I wonder if there's a bug somewhere related to Windows where the help generation isn't able to query the terminal size. I'd suspect the help generation would get the terminal size and then wrap the output. Let me try...

![ruff-help-no-wrap](https://github.com/astral-sh/ruff/assets/456674/c0fc1cd1-34f9-48b5-9710-76af823d8e9e)

Welp. Nope. Something else is going wrong. Or the auto-wrapping isn't enabled.

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:742 on 2024-01-24 14:35_

Hopefully this should fix it: https://github.com/astral-sh/ruff/pull/9633

---

_@BurntSushi reviewed on 2024-01-24 14:35_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:742 on 2024-01-24 17:01_

#9633 fixed the wrapping for `--help`, but didn't do anything for error messages displayed by clap.

We could wrap the long error message here by doing something like this:

```diff
--- a/crates/ruff/src/args.rs
+++ b/crates/ruff/src/args.rs
@@ -7,6 +7,8 @@ use clap::{command, Parser};
 use path_absolutize::path_dedot;
 use regex::Regex;
 use rustc_hash::FxHashMap;
+use terminal_size::terminal_size;
+use textwrap;
 use toml;

 use ruff_linter::line_width::LineLength;
@@ -754,10 +756,16 @@ impl TypedValueParser for ConfigOptionParser {
             clap::error::ContextKind::InvalidValue,
             clap::error::ContextValue::String(value.to_string()),
         );
+        let clap_indent = 7;
+        let terminal_width = usize::from(terminal_size().map_or(80, |(width, _)| width.0))
+            .saturating_sub(clap_indent);
+        let join = format!("\n{}", " ".repeat(clap_indent));
+        let long_first_tip = "The `--config` flag must either be \
+        a path to a `.toml` configuration file or a TOML `<KEY> = <VALUE>` \
+        pair overriding a specific config setting";
         let tips = vec![
-            "The `--config` flag must either be a path to a `.toml` \
-            configuration file or a TOML `<KEY> = <VALUE>` pair overriding \
-            a specific config setting"
+            textwrap::wrap(long_first_tip, terminal_width)
+                .join(&join)
                 .into(),
             format!("The path `{value}` does not exist on your filesystem").into(),
         ];
```

It works very nicely:

![image](https://github.com/astral-sh/ruff/assets/66076021/54590bdc-c0f4-4b47-a6fc-bb01f78c0d5d)

But adding two dependencies (`terminal_size` and `textwrap`) feels possibly over-the-top for such a minor feature... though, we do already have `terminal_size` as a transitive dependency following #9633!

---

_@AlexWaygood reviewed on 2024-01-24 17:01_

---

_@AlexWaygood reviewed on 2024-01-24 17:03_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:742 on 2024-01-24 17:03_

> It works very nicely:

Admittedly, `let clap_indent = 7;` is a bit of a hack lol

---

_@BurntSushi reviewed on 2024-01-24 17:08_

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:742 on 2024-01-24 17:08_

I'd say go for it and put up a PR. :-)

---

_@AlexWaygood reviewed on 2024-01-24 19:48_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:742 on 2024-01-24 19:48_

I cleaned up the patch for wrapping multiline error messages a little, but it still feels somewhat hacky:

<details>

```diff
--- a/crates/ruff/src/args.rs
+++ b/crates/ruff/src/args.rs
@@ -1,12 +1,14 @@
 use std::path::PathBuf;
 use std::sync::Arc;

-use anyhow::anyhow;
+use anyhow;
 use clap::builder::{TypedValueParser, ValueParserFactory};
 use clap::{command, Parser};
 use path_absolutize::path_dedot;
 use regex::Regex;
 use rustc_hash::FxHashMap;
+use terminal_size::terminal_size;
+use textwrap;
 use toml;

 use ruff_linter::line_width::LineLength;
@@ -543,14 +545,16 @@ impl ConfigArgs {
                 }
                 ConfigOption::PathToConfigFile(path) => {
                     if isolated {
-                        let error = anyhow!(
-                            "Cannot specify `--isolated` and also specify a configuration file"
-                        );
-                        let context = format!(
+                        let error = wrap_error_message(&format!(
                             "Both `--isolated` and `--config={}` were specified on the command line",
-                            path.display()
+                            path.display()),
+                            ANYHOW_CAUSE_INDENT,
+                        );
+                        let context = wrap_error_message(
+                            "Cannot specify `--isolated` and also specify a configuration file",
+                            ANYHOW_CAUSE_INDENT,
                         );
-                        return Err(error.context(context));
+                        return Err(anyhow::Error::msg(error).context(context));
                     }
                     if let Some(ref config_file) = new.config_file {
                         let (first, second) = (config_file.display(), path.display());
@@ -563,15 +567,17 @@ impl ConfigArgs {
                             clap::error::ContextKind::InvalidValue,
                             clap::error::ContextValue::String(second.to_string()),
                         );
-                        let tips = vec![
-                            "Cannot specify more than one configuration file on the command line"
-                                .into(),
-                            format!("Remove either `--config={first}` or `--config={second}`")
-                                .into(),
+                        let tips = [
+                            "Cannot specify more than one configuration file on the command line",
+                            &format!("Remove either `--config={first}` or `--config={second}`"),
                         ];
                         error.insert(
                             clap::error::ContextKind::Suggested,
-                            clap::error::ContextValue::StyledStrs(tips),
+                            clap::error::ContextValue::StyledStrs(
+                                tips.iter()
+                                    .map(|tip| wrap_error_message(tip, CLAP_TIP_INDENT).into())
+                                    .collect(),
+                            ),
                         );
                         return Err(error.into());
                     }
@@ -697,6 +703,29 @@ fn resolve_bool_arg(yes: bool, no: bool) -> Option<bool> {
     }
 }

+/// If for whatever reason we can't calculate the terminal size,
+/// assume it has a width of 80
+const FALLBACK_TERMINAL_SIZE: u16 = 80;
+
+/// The length of the "tip:" prefix `clap` gives the first line
+/// of error messsages produced by `clap::error::ErrorKind::Suggested`
+const CLAP_TIP_INDENT: usize = "  tip: ".len();
+
+/// The length of the "Cause:" prefix `anyhow` gives
+/// to messages passed to the `.context()` method
+/// on `anyhow::Result` instances
+const ANYHOW_CAUSE_INDENT: usize = "  Cause: ".len();
+
+/// Wrap error messages to the width of the terminal,
+/// and apply a consistent indent to all but the first line
+fn wrap_error_message(string: &str, indent_length: usize) -> String {
+    let terminal_width =
+        usize::from(terminal_size().map_or(FALLBACK_TERMINAL_SIZE, |(width, _)| width.0));
+    let max_width = terminal_width.saturating_sub(indent_length);
+    let join = format!("\n{}", " ".repeat(indent_length));
+    textwrap::wrap(string, max_width).join(&join)
+}
+
 /// `--config` arguments passed via the CLI.
 ///
 /// Users may pass 0 or 1 paths to a configuration file,
@@ -754,16 +783,21 @@ impl TypedValueParser for ConfigOptionParser {
             clap::error::ContextKind::InvalidValue,
             clap::error::ContextValue::String(value.to_string()),
         );
-        let tips = vec![
-            "The `--config` flag must either be a path to a `.toml` \
-            configuration file or a TOML `<KEY> = <VALUE>` pair overriding \
-            a specific config setting"
-                .into(),
-            format!("The path `{value}` does not exist on your filesystem").into(),
+        // We want subsequent lines of multiline error messages
+        // to be aligned with the "tip:" prefix clap gives the first line
+        let tips = [
+            "The `--config` flag must either be \
+            a path to a `.toml` configuration file or a TOML `<KEY> = <VALUE>` \
+            pair overriding a specific config setting",
+            &format!("The path `{value}` does not exist on your filesystem"),
         ];
         new_error.insert(
             clap::error::ContextKind::Suggested,
-            clap::error::ContextValue::StyledStrs(tips),
+            clap::error::ContextValue::StyledStrs(
+                tips.iter()
+                    .map(|tip| wrap_error_message(tip, CLAP_TIP_INDENT).into())
+                    .collect(),
+            ),
         );
         Err(new_error)
```

</details>

I think I'll leave it for now, but possibly propose it as a followup after this PR is merged :)

---

_@AlexWaygood reviewed on 2024-01-24 20:16_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:576 on 2024-01-24 20:16_

On second though, I think I will leave this, too, to a subsequent PR.

---

_Comment by @github-actions[bot] on 2024-01-24 20:42_

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

_Comment by @codspeed-hq[bot] on 2024-01-25 16:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:arbitrary-cli-overrides)

### Merging #9599 will **not alter performance**

<sub>Comparing <code>AlexWaygood:arbitrary-cli-overrides</code> (0f8a583) with <code>main</code> (d387d0b)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @AlexWaygood on 2024-01-25 23:44_

Looks like a test case that I've added will fail if I make this modification:

```diff
diff --git a/crates/ruff/tests/lint.rs b/crates/ruff/tests/lint.rs
index 95f068b19..493641f13 100644
--- a/crates/ruff/tests/lint.rs
+++ b/crates/ruff/tests/lint.rs
@@ -570,7 +570,7 @@ x = "longer_than_90_charactersssssssssssssssssssssssssssssssssssssssssssssssssss
         .arg("--config")
         .arg(&ruff_toml)
         .args(["--config", "line-length=90"])
-        .args(["--config", "extend-select=['E501']"])
+        .args(["--config", "extend-select=['E501','F481']"])
         .args(["--config", "lint.isort.combine-as-imports = false"])
         .arg("-")
         .pass_stdin(fixture), @r###"
```

It fails to parse the TOML (possibly because `clap` thinks the comma is being used to delimit multiple `--config` values? Not sure), and gives this message:

```
error: invalid value 'extend-select=['E501','F481']' for '--config <CONFIG_OPTION>'

  tip: The `--config` flag must either be a path to a `.toml` configuration file or a TOML `<KEY> = <VALUE>` pair overriding a specific config setting
  tip: The path `extend-select=['E501','F481']` does not exist on your filesystem

For more information, try '--help'.
```

I'm not yet sure exactly why...

---

_Marked ready for review by @AlexWaygood on 2024-01-26 13:51_

---

_Comment by @AlexWaygood on 2024-01-26 13:52_

Alrighty, I've had a stab at writing docs and tests, and I think I'm pretty happy with the error messages. This should be (finally!) ready for proper review!

---

_Label `cli` added by @MichaReiser on 2024-01-29 07:29_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/add_noqa.rs`:21 on 2024-01-29 07:31_

Nit: Rename the variable to match the new type name


```suggestion
    arguments: &ConfigArgs,
```

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:318 on 2024-01-29 07:33_

Can you explain the reasoning why `--isolated` no longer conflicts when `--config` is a file or is this now handled manually?

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:510 on 2024-01-29 07:35_

Nit: I don't think we ever made an explicit decision on this as a team but I prefer to avoid abbreviations (I know we use a lot in the AST, we should change this. You can't know what STMT or EXPR is without having worked with ASTs before)

```suggestion
pub struct ConfigArguments {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:514 on 2024-01-29 07:36_

Nit: I'm leaning towards removing the `config_` prefix as this is already given by `ConfigArgs`: `config_arguments.file` vs `config_arguments.config_file`. 

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:523 on 2024-01-29 07:37_


```suggestion
    pub fn config_file(&self) -> Option<&Path> {
        self.config_file.as_ref()
    }

```

`PathBuf` and `Path` are the same as `String` (owned) and `&str` (Reference). It should be sufficient to return a `Path`

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:538 on 2024-01-29 07:39_

What's the reason to use `Options::clone` vs `options.clone`? I had to look up the `Option::clone` definition to understand if it implements the `Clone` trait or if it is a special static method called `clone`.

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:596 on 2024-01-29 07:44_

Can we add a comment explaining why it only applies the `per_flag_overrides` but not the manual passed config options `--config line-length=1`? I assume because they have a different precedence but it isn't immediately obvious to me when reading the code (nor explained in the struct documentation).

Maybe it's better to extend the `ConfigArgs` struct or field documentations to explain in more detail what the fields are about and how they 
are scoped differently. 

Edit: I just saw that you apply both transformation. That's sneeky and the "nesting" makes spotting the precedence difficult:

```suggestion
    fn transform(&self, config: Configuration) -> Configuration {
        let with_config_overrides = self.config_overrides.combine(config);
        self.per_flag_overrides.transform(with_config_overrides)
    }
```

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:661 on 2024-01-29 07:45_

Nit: 

```suggestion
        let format_arguments = FormatArguments {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:702 on 2024-01-29 07:47_

I would find it helpful if the documentation explains the precedence between `--config <path>` and `--config <option>`. Are they applied in order or do `--config <option>` always override `--config <path>`?

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:715 on 2024-01-29 07:53_

Nit: 


```suggestion
    FilePath {
        path: PathBuf,
        arg: Option<String>,
        cmd: clap::Command,
    },
    Override(Arc<Options>),
```

To avoid repeating the `Config` from the enum name

(Instead of `FilePath` maybe even just `Path`)

We could even consider renaming the enum. I don't know if `Option` is terminology. If not, then I would recommend removing the `Option` postfix because it would be nice if we can use `Option` for the variant name to match the terminology in the enum documentation (Ideally we have a single terminology for a concept in code and user documentation and in code)

```rust
enum ConfigArgument {
    File(PathBuf),
    Option(Arg<Options>)
}
```

or 

```rust
enum ConfigValue {
    File(PathBuf),
    Option(Arg<Options>)
}
```

which seems to match with Clap's `ValueParser` semantics. 

(Nit: You would need to rename `ConfigArguments::from_cli_options` to `ConfigArguments::from_(cli_)arguments(..)`)

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:713 on 2024-01-29 07:54_

Nit: Maybe explain what `arg` and `cmd` is and why we need to track them (I would have expected that the path itself is sufficient)

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:753 on 2024-01-29 07:56_

Non UTF8 values are okay for Paths
```suggestion
        let path_to_config_file = PathBuf::from(value);
        if path_to_config_file.exists() {
            return Ok(ConfigOption::PathToConfigFile {
                path: path_to_config_file,
                arg: arg.map(clap::Arg::to_string),
                cmd: cmd.clone(),
            });
        }

        let value = value
            .to_str()
            .ok_or_else(|| clap::Error::new(clap::error::ErrorKind::InvalidUtf8))?;
        
        let toml_parse_error = match toml::from_str(value) {
            Ok(option) => return Ok(ConfigOption::ConfigOverride(Arc::new(option))),
            Err(toml_error) => toml_error,
        };
        let mut new_error = clap::Error::new(clap::error::ErrorKind::ValueValidation).with_cmd(cmd);
```

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:755 on 2024-01-29 08:00_

Nit: Some empty lines can help to make the early return easier to spot
```suggestion
        };
        
        let mut new_error = clap::Error::new(clap::error::ErrorKind::ValueValidation).with_cmd(cmd);
        if let Some(arg) = arg {
            new_error.insert(
```

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:797 on 2024-01-29 08:01_

Nice!

---

_Review comment by @MichaReiser on `crates/ruff/src/main.rs`:64 on 2024-01-29 08:03_

How do the errors look like? Is the exit message and formatting consistent with what Ruff does otherwise?

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/configuration.rs`:581 on 2024-01-29 08:09_

What's the difference between `Configuration::transform` and `Configuration::combine`? Could we use `Configuration.combine` instead? (rather than implementing `Configuratin::transform`)?

---

_Review comment by @MichaReiser on `docs/configuration.md`:463 on 2024-01-29 08:15_

Nit: I would explain one concept at a time by

1. Explain the configuration options that can be overriden globally (they also have different semantics)
2. Explain the `--config` option

```suggestion
Some configuration options can be provided or overriden via the command-line, such as those related to rule enablement and disablement, file discovery, logging level, and more:

```shell
ruff check path/to/code/ --select F401 --select F403 --quiet
```

Configuration options that do not have a dedicated flag can be set using the `--config` flag. The `--config` flag is most often
used to point to the configuration file that you would like ruff to use,
```

I think it would be worth explaining in more detail how the config options are overriden. Does `--config line-length=100` override the `line-length` settings for all files or do configuration files in sub-directories have priority (because they're closer to the file)?

---

_@MichaReiser approved on 2024-01-29 08:19_

Nice work and I love the care you gave the error messages. 

The one thing that isn't entirely clear to m is how `--config option=value` overrides values: Does it override the option in all configuration files? Even if a directory has its own configuration file that could be considered "closer" to the file in question?

---

_@AlexWaygood reviewed on 2024-01-29 11:08_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:318 on 2024-01-29 11:08_

Yeah, `--isolated` still conflicts if `--config` is a file:

![image](https://github.com/astral-sh/ruff/assets/66076021/3af739ca-2ad2-4a46-87e3-b119b3e62123)

However, we now produce this error manually in `ConfigArgs.from_cli_options`, rather than using clap's out-of-the-box mechanism. The reason is that at this stage, we don't yet know whether the `--config` flag is being used to specify a config file (which _does_ conflict with `--isolated`) or whether it's being used to specify a config override (which _doesn't_ conflict with `--isolated`).

I'll add a clarifying comment.

---

_@AlexWaygood reviewed on 2024-01-29 11:15_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:538 on 2024-01-29 11:15_

The reason was that I applied a suggestion from @BurntSushi's last review, and assumed he knew best 😄

I also like your suggestion slightly more, so I'll go with it.

---

_@AlexWaygood reviewed on 2024-01-29 11:48_

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/configuration.rs`:581 on 2024-01-29 11:48_

The difference is that `combine()` consumes and invalidates `self`, whereas `transform` only takes a reference to `self`. I innitially spent quite a while seeing if I could reuse and/or modify `Configuration::combine` so that it would work for our purposes here, but it's quite hard to do without a much bigger refactor, I think.

In any event, I'll add a clarifying comment!

---

_@AlexWaygood reviewed on 2024-01-29 11:59_

---

_Review comment by @AlexWaygood on `crates/ruff/src/main.rs`:64 on 2024-01-29 11:59_

The issue here is that most argument-parsing-related errors are produced on this line, before we start "properly" running ruff itself:

https://github.com/astral-sh/ruff/blob/c8074b0e181063f2f0aaf71ac01416d049c69e89/crates/ruff/src/main.rs#L47

If any errors fail during clap's argument parsing there, clap immediately exits and produces a very pretty error message for the user. As an example, here's an existing error message produced by `clap` that's the same as it is on `main`:

![image](https://github.com/astral-sh/ruff/assets/66076021/4dd42969-b970-4f18-b52a-065adbf7bac5)

With the change I'm making here in `main.rs`, we allow clap to produce beautifully formatted error messages if they relate to argument parsing, even when those error messages are created manually after the initial argument-parsing stage:

![image](https://github.com/astral-sh/ruff/assets/66076021/b724f3f7-fc60-48ce-be48-de7c23a2692c)

![image](https://github.com/astral-sh/ruff/assets/66076021/ec093b45-3451-4940-8d52-8cc6dd33e0ba)

_Without_ the change I'm making here to `main.rs`, here's what those two error messages would look like:

![image](https://github.com/astral-sh/ruff/assets/66076021/efd7621b-e434-4fd9-87ce-95a90bd97d8c)

![image](https://github.com/astral-sh/ruff/assets/66076021/24ea0eb7-799f-4638-90f0-c687f1f61bf1)

It wouldn't be the end of the world, but it's not quite as pretty, not as consistent with the way ruff displays other argument-parsing-related error messages to users, and the "ruff failed" message makes it look like there's some kind of internal error here rather than the user passing in an incorrect argument from the CLI

---

_@AlexWaygood reviewed on 2024-01-29 12:06_

---

_Review comment by @AlexWaygood on `crates/ruff/src/main.rs`:64 on 2024-01-29 12:06_

While I obviously can't assert the colour of the error messages in our test suite, I _have_ added tests that assert that the error messages in these cases start with "error: " rather than "ruff failed" :)

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:702 on 2024-01-29 12:07_

`--config <option>` always overrides `--config <path>`. I'll add some more docs.

---

_@AlexWaygood reviewed on 2024-01-29 12:07_

---

_@MichaReiser reviewed on 2024-01-29 12:42_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/configuration.rs`:581 on 2024-01-29 12:42_

You can do `configuration.clone().combine(...)`. It shouldn't be more expensive than the `transform` implementation that you have now because it clones all field values (which is what `configuration.clone()` does too.

---

_@MichaReiser reviewed on 2024-01-29 12:43_

---

_Review comment by @MichaReiser on `crates/ruff/src/main.rs`:64 on 2024-01-29 12:43_

I see. What we need is our own proper error types/diagnostics. I assumed you return `ClapErrors` because this runs as part of Clap. I feel a bit conflicted about passing around Clap errors in the rest of our code. The issue is that we may need to depend on clap if we e.g have to move any of this logic into `ruff_workspace` when rewriting our LSP into rust. 

---

_@AlexWaygood reviewed on 2024-01-29 12:43_

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/configuration.rs`:581 on 2024-01-29 12:43_

oh wow, I think I really wasn't seeing the wood for the trees here. Thanks!

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:713 on 2024-01-29 12:52_

This is essentially the same issue we're discussing in https://github.com/astral-sh/ruff/pull/9599#discussion_r1469202408: we're tracking the original command and arg here so that we can use `clap` to create pretty argument-parsing-related error messages even when they occur after the initial argument-parsing phase

---

_@AlexWaygood reviewed on 2024-01-29 12:52_

---

_@AlexWaygood reviewed on 2024-01-29 13:03_

---

_Review comment by @AlexWaygood on `crates/ruff/src/main.rs`:64 on 2024-01-29 13:03_

I get what you mean; I wasn't sure about this either. We're just talking about two error messages here -- while I much prefer clap's formatting for these, since they _are_ to do with argument parsing, I could possibly just use `anyhow::Error` for them for now rather than `clap::Error`, and open a followup issue/PR after this PR goes in to discuss how to improve the UX for these errors.

---

_@BurntSushi reviewed on 2024-01-29 14:34_

---

_Review comment by @BurntSushi on `crates/ruff/src/args.rs`:538 on 2024-01-29 14:34_

I think that was a slip on my part. I probably had `Arc::clone` there and then changed it to `Options::clone`. It is I'd say weakly idiomatic to use `Arc::clone` and not `arc.clone()` though. See: https://doc.rust-lang.org/std/sync/struct.Arc.html#deref-behavior

---

_@AlexWaygood reviewed on 2024-01-29 17:59_

---

_Review comment by @AlexWaygood on `crates/ruff/src/main.rs`:64 on 2024-01-29 17:59_

I switched to using `anyhow` for these two errors in 1501eeb4c0c9463056bc8cca33f1d28731034405. After this PR is merged, I'll consider opening an issue to discuss whether it's worth trying to improve the UX for those error paths, and if so, how to do so.

The new errors look like this:

![image](https://github.com/astral-sh/ruff/assets/66076021/9bf55e0c-f619-409f-a9fc-5e62fe941bab)

![image](https://github.com/astral-sh/ruff/assets/66076021/90e15277-f2c7-4784-804c-63ccd83e2173)


---

_Comment by @AlexWaygood on 2024-01-29 18:06_

Thanks @MichaReiser! Would you mind giving the docs another look? I revamped them a fair bit following your comments. Also @zanieb did you still want to take a look? :)

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-01-29 18:07_

---

_Review requested from @zanieb by @AlexWaygood on 2024-01-29 18:07_

---

_@MichaReiser approved on 2024-01-30 06:51_

Nice work and I love the care you gave the error messages. 

The one thing that isn't entirely clear to m is how `--config option=value` overrides values: Does it override the option in all configuration files? Even if a directory has its own configuration file that could be considered "closer" to the file in question?

---

_Comment by @AlexWaygood on 2024-01-30 18:42_

> The one thing that isn't entirely clear to m is how `--config option=value` overrides values: Does it override the option in all configuration files? Even if a directory has its own configuration file that could be considered "closer" to the file in question?

Yeah -- I added some docs about this in 0ccc608b6ec4886682c626bf9794d6a8d57bb358. Essentially, overriding a setting using `--config` currently works identically to an override using a dedicated flag (except that a dedicated flag always takes priority over `--config` if they're _both_ specified. Lmk if you think that doesn't make sense, or if you think the docs need any additional clarity!

---

_Comment by @zanieb on 2024-02-01 20:49_

Sorry I haven't looked at this yet I've been focused on v0.2.0. I'm looking forward to reading through this once that's out.

---

_Comment by @AlexWaygood on 2024-02-01 20:55_

> Sorry I haven't looked at this yet I've been focused on v0.2.0. I'm looking forward to reading through this once that's out.

No worries! Yeah, I figured :)

---

_@zanieb reviewed on 2024-02-06 21:21_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:170 on 2024-02-06 21:21_

This is interesting, it's different than cargo's `--config` behavior. Why are we doing it this way instead of always applying in-order?

---

_@zanieb reviewed on 2024-02-06 21:24_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:313 on 2024-02-06 21:24_

Why does specifying a config file conflict with `--isolated`? Shouldn't `--isolated` just ignore any _discovered_ config files? I don't see any problem with respecting config files that are explicitly passed.

---

_@AlexWaygood reviewed on 2024-02-06 21:31_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:313 on 2024-02-06 21:31_

I don't have a strong opinion here; this just preserves the pre-existing behaviour where it was an error to pass `--config <config file>` and also pass `--isolated` on the CLI. I added the comment because we now have to generate that error manually rather than using `clap` to generate it, so it's less obvious from the definitions here that this would be the case 

---

_@AlexWaygood reviewed on 2024-02-06 21:38_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:170 on 2024-02-06 21:38_

Ah, I didn't realise that was cargo's behaviour!

I figured it would be easier for users to understand if overrides specified using `--config` worked as similarly as possible to the dedicated CLI flags such as `--line-length` etc — it's fewer concepts to hold in your head if they have ~basically the same rules. If cargo has different behaviour here I'm open to reconsidering this — though a lot of Python developers won't be familiar with cargo, so maybe they won't even notice the behaviour difference between our CLI and cargo!

The way I have it now is probably also slightly simpler in terms of the implementation, but that's obviously very much a secondary concern to the UX :)

---

_@zanieb reviewed on 2024-02-06 21:46_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:557 on 2024-02-06 21:46_

Nit: I wouldn't include the "Struct to encapsulate" since that's kind of redundant

---

_@zanieb reviewed on 2024-02-06 21:46_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:560 on 2024-02-06 21:46_

I would also say this is implied by the datastructure and can easily become outdated, I wouldn't include it.

---

_@zanieb reviewed on 2024-02-06 21:47_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:564 on 2024-02-06 21:47_

Just wondering, will we allow more in the future?

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:170 on 2024-02-06 21:47_

I guess this makes more sense with https://github.com/astral-sh/ruff/pull/9599/files#r1480577234 where we are limiting to a single config file. If we'll support more though I think "in-order" is the clearest way to talk about precedence.

---

_@zanieb reviewed on 2024-02-06 21:47_

---

_@zanieb reviewed on 2024-02-06 21:48_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:571 on 2024-02-06 21:48_

```suggestion
    /// Overrides provided via dedicated flags such as `--line-length` etc.
```

---

_@zanieb reviewed on 2024-02-06 21:50_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:313 on 2024-02-06 21:50_

I don't see a reason to ban the behavior. Our config file inference is really complicated and passing in `--isolated` to disable that doesn't seem like it should prevent you from passing in a single configuration file.

Curious for @charliermarsh's thoughts though because I do not have context on the origin of these options.

---

_@zanieb reviewed on 2024-02-06 21:52_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:615 on 2024-02-06 21:52_

I wonder if we should use structured errors for these e.g. introduce an `Error` enum and define the messages there. I'm not sure what the current state of that is for our Ruff CLI.

cc @MichaReiser 

---

_Review comment by @zanieb on `crates/ruff/tests/format.rs`:104 on 2024-02-06 21:55_

I would say "configuration setting" instead of "config setting" for consistency with "configuration file".

I also have a minor preference for "option" over "setting", not sure if we're consistent about that terminology in general.

---

_@zanieb reviewed on 2024-02-06 21:55_

---

_@zanieb reviewed on 2024-02-06 21:56_

---

_Review comment by @zanieb on `crates/ruff/tests/format.rs`:104 on 2024-02-06 21:56_

You may be able to drop "TOML" from this as well

---

_@zanieb reviewed on 2024-02-06 21:56_

---

_Review comment by @zanieb on `crates/ruff/tests/format.rs`:123 on 2024-02-06 21:56_

It feels weird that these are separate tips, is it feasible to combine the message into one?

Similar to https://github.com/astral-sh/ruff/pull/9599/files#r1480584476 you should use "configuration option" consistently.

---

_@zanieb reviewed on 2024-02-06 21:58_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:313 on 2024-02-06 21:58_

Another question is... does `--config=...` imply isolated? or does it apply that configuration file on top of any discovered configuration files?

---

_@zanieb reviewed on 2024-02-06 21:59_

---

_Review comment by @zanieb on `crates/ruff/tests/format.rs`:155 on 2024-02-06 21:59_

This is a confusing message. Why do we have two `{}`? Shouldn't these have file paths?

---

_@zanieb reviewed on 2024-02-06 22:00_

---

_Review comment by @zanieb on `crates/ruff/tests/format.rs`:183 on 2024-02-06 22:00_

I would omit the `={}` here if we retain this error and write as "The `--config` argument cannot be used with `--isolated`"

---

_@zanieb reviewed on 2024-02-06 22:01_

---

_Review comment by @zanieb on `crates/ruff/tests/lint.rs`:526 on 2024-02-06 22:01_

Maybe this should be prefaced with.. "It looks like you passed a configuration file, but the path ... does not exist."

"on your filesystem" feels redundant.

---

_@zanieb reviewed on 2024-02-06 22:06_

---

_Review comment by @zanieb on `crates/ruff/tests/lint.rs`:715 on 2024-02-06 22:06_

I'd make this a little less verbose, maybe?

"Failed to parse `...` as a configuration option"

Additionally, won't the TOML parse error _always_ include the line that the user passed? I guess it's possible that they try to pass multiple lines but that seems very rare. Perhaps including what we tried to parse in the tip message is overkill, and it should just be "It looks like an option was passed, but it was not valid TOML"

---

_@AlexWaygood reviewed on 2024-02-06 22:06_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:615 on 2024-02-06 22:06_

Micha and I had a longish discussion about this in https://github.com/astral-sh/ruff/pull/9599#discussion_r1469202408. I originally used `clap::error::Error` here, for precisely this reason, but Micha successfully persuaded me that it was a bad idea to use `clap` after the initial argparsing phase :)

I'd quite like to look at introducing more structured errors generally so that it's easier for us to distinguish between errors where the user input from the CLI is the problem and errors that are "our fault". I think there are other places where we use `anyhow` for bubbling up CLI errors that we detect after the initial argparsing; I'd quite like to look at it holistically and defer it to a future issue/PR, if that's laright?

---

_@zanieb reviewed on 2024-02-06 22:06_

---

_Review comment by @zanieb on `crates/ruff/tests/lint.rs`:526 on 2024-02-06 22:06_

Or to be less "you": "It looks like a configuration file was passed, but..."

---

_@zanieb reviewed on 2024-02-06 22:09_

---

_Review comment by @zanieb on `crates/ruff/tests/lint.rs`:693 on 2024-02-06 22:09_

Can we have different error messages for invalid TOML and TOML that does not conform to _our_ specification? e.g. for this one "It looks like an option was passed, but the value is not valid"

---

_@zanieb reviewed on 2024-02-06 22:09_

---

_Review comment by @zanieb on `crates/ruff/tests/lint.rs`:693 on 2024-02-06 22:09_

If this is hard, maybe just "It looks like an option was passed, but it was not valid" to generalize over all sorts of invalidities :)

---

_@AlexWaygood reviewed on 2024-02-06 22:09_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/format.rs`:155 on 2024-02-06 22:09_

The message the user sees will have file paths:

![image](https://github.com/astral-sh/ruff/assets/66076021/596533bf-f91a-4c71-acd3-897171711af4)

This test doesn't use `assert_cmd_snapshot`, because we don't know what the file paths are going to be ahead of time in this test (it uses a TempDir), and `assert_cmd_snapshot` needs a literal string to be passed in. So this is a format string that I'm using to generate the expected stderr given the path to the TempDir, and then I'm just using an `assert_eq` call lower down once I've generated the expected stderr.

---

_@AlexWaygood reviewed on 2024-02-06 22:11_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:564 on 2024-02-06 22:11_

We could do; I think it would be feasible implementation-wise, and I don't think it would be particularly _confusing_ for users to have that behaviour. Has it been asked for by users, do you know?

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:313 on 2024-02-06 22:12_

> or does it apply that configuration file on top of any discovered configuration files?

I _believe_ this is the current behaviour

---

_@AlexWaygood reviewed on 2024-02-06 22:12_

---

_@zanieb reviewed on 2024-02-06 22:13_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:1466 on 2024-02-06 22:13_

Is there test coverage for this?

---

_@AlexWaygood reviewed on 2024-02-06 22:13_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/lint.rs`:693 on 2024-02-06 22:13_

I'd love to do this if possible; I'll investigate!

---

_@zanieb reviewed on 2024-02-06 22:16_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:615 on 2024-02-06 22:16_

Ah yeah I'm thinking you'd just `thiserror` but I am still an error handling noob in Rust. Definitely feel free to defer, especially if you already talked with Micha about it.

---

_@zanieb reviewed on 2024-02-06 22:18_

---

_Review comment by @zanieb on `crates/ruff/tests/format.rs`:155 on 2024-02-06 22:18_

Oh. I would use insta filters for this e.g. https://github.com/astral-sh/ruff/pull/9764/commits/d467bc6344b7b83546b24a4f6b576e76497afdfe (although it's kind of a pain for Windows https://github.com/astral-sh/ruff/pull/9764/commits/f46c2e4edc7861024a690c8324b07a97d5d346af)

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:564 on 2024-02-06 22:21_

I'm just thinking about the cargo implementation. I haven't heard an ask for it but I wouldn't be surprised! We can punt on it for now though we might want to be careful about precedence (i.e. as discussed in https://github.com/astral-sh/ruff/pull/9599#discussion_r1480577993) to avoid having it be a breaking change in the future.

---

_@zanieb reviewed on 2024-02-06 22:21_

---

_@AlexWaygood reviewed on 2024-02-06 22:23_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:564 on 2024-02-06 22:23_

Cool -- let's punt on it for now. Very much open to this if it is asked for, though!

---

_@AlexWaygood reviewed on 2024-02-06 22:36_

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/configuration.rs`:1466 on 2024-02-06 22:36_

There is now :)

7d6f9dd1216f7160e95e6d6d999d6cc32bcc21d5

---

_@zanieb reviewed on 2024-02-06 22:39_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:313 on 2024-02-06 22:39_

I think allowing it _with_ `--isolated` makes a lot of sense then.

---

_@AlexWaygood reviewed on 2024-02-06 23:29_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/format.rs`:155 on 2024-02-06 23:29_

Ahh makes sense, thanks! I looked for something like that in the insta_cmd docs, but obviously I didn't look thoroughly enough 😄

---

_@AlexWaygood reviewed on 2024-02-07 14:03_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/format.rs`:155 on 2024-02-07 14:03_

Fixed in https://github.com/astral-sh/ruff/pull/9599/commits/7739440e40eaebab41b3fed68a0e25b9b458777e

---

_@AlexWaygood reviewed on 2024-02-07 14:48_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/format.rs`:123 on 2024-02-07 14:48_

I had a go at combining them into a single tip in https://github.com/astral-sh/ruff/pull/9599/commits/88e7273abf26f1f38e970a058699aa293737a0a4. I also introduced some more line breaks, as I realised the error messages looked terrible if the terminal had a narrow width

---

_@AlexWaygood reviewed on 2024-02-07 15:39_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/lint.rs`:693 on 2024-02-07 15:39_

Managed to get this working in 6eea7977945ee160697033f03cfde4156fcace4b

---

_Comment by @AlexWaygood on 2024-02-07 15:41_

Thanks so much @zanieb! I think I tackled nearly all of your points, except for https://github.com/astral-sh/ruff/pull/9599#discussion_r1480557151 where we're waiting for Charlie to chime in.

Some of the error messages might still be a bit verbose for your liking, but I'm not sure how to trim them down... suggestions welcome :)

---

_Review requested from @zanieb by @AlexWaygood on 2024-02-07 15:42_

---

_@AlexWaygood reviewed on 2024-02-07 17:01_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/lint.rs`:715 on 2024-02-07 17:01_

So the reason I'm specifically saying "`ruff.toml` configuration option" rather than just "configuration option" is because a `pyproject.toml` configuration option would need to be specified differently. E.g. you don't need to specify `--config "tool.ruff.lint.extend-select = ['F841']"` (a la `pyproject.toml`), you just need to specify `--config "lint.extend-select = ['F841']"` (a la `ruff.toml`)

---

_@AlexWaygood reviewed on 2024-02-07 17:02_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/format.rs`:183 on 2024-02-07 17:02_

Hmm -- _some_ `--config` arguments are allowed, though. It's okay to use the `--config` flag alongside `--isolated` if `--config` is being used to override a specific configuration option. It's just not okay to use `--config` alongside `--isolated` if `--config` is being used to provide a configuration file. So the value being passed to `--config` does matter here

---

_@AlexWaygood reviewed on 2024-02-07 17:06_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/format.rs`:104 on 2024-02-07 17:06_

I've kept "TOML" in here for now, as otherwise I think it's a little confusing in some of the error messages when we immediately start talking about a "TOML parse error" in the following lines

---

_Comment by @charliermarsh on 2024-02-09 14:06_

Sorry, didn't realize this was waiting on me! I'll take a look today, then we should merge :)

---

_@charliermarsh reviewed on 2024-02-09 20:26_

---

_Review comment by @charliermarsh on `docs/configuration.md`:486 on 2024-02-09 20:26_

Nit: capitalize "Ruff" here.

---

_@charliermarsh reviewed on 2024-02-09 20:26_

---

_Review comment by @charliermarsh on `docs/configuration.md`:488 on 2024-02-09 20:26_

Nit: capitalize "Ruff" here.

---

_@charliermarsh reviewed on 2024-02-09 20:26_

---

_Review comment by @charliermarsh on `docs/configuration.md`:493 on 2024-02-09 20:26_

Nit: can omit "always" here.

---

_@charliermarsh reviewed on 2024-02-09 20:27_

---

_Review comment by @charliermarsh on `docs/configuration.md`:501 on 2024-02-09 20:27_

Nit: capitalize "Ruff"

---

_Review comment by @charliermarsh on `docs/configuration.md`:463 on 2024-02-09 20:27_

Should there be some transitional sentence between "Some configuration options can be provided or overridden via dedicated flags on the command line." and this next section?

---

_@charliermarsh reviewed on 2024-02-09 20:27_

---

_@charliermarsh approved on 2024-02-09 20:29_

Looks great overall.

---

_@charliermarsh reviewed on 2024-02-09 20:31_

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:858 on 2024-02-09 20:31_

A little concerning how much implicit coupling there is to Clap, but I don't have better suggestions (and at least there's test coverage, I think, such that if we upgrade Clap and the output format changes, we will know that there's a disconnect here).

---

_@AlexWaygood reviewed on 2024-02-09 20:35_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:858 on 2024-02-09 20:35_

Yeah. Ideally I feel like clap would have a feature that allows long error messages to be wrapped to the terminal width, much as it does with the output of `--help`. But it doesn't seem to have that feature right now :/

Initially I had each sentence as a separate tip, which obviated the need for this kind of hack, but Zanie (correctly!) pointed out that that looked pretty weird

---

_@AlexWaygood reviewed on 2024-02-09 20:41_

---

_Review comment by @AlexWaygood on `docs/configuration.md`:463 on 2024-02-09 20:41_

Something like this?

```suggestion
One flag that deserves special mention is the `--config` flag.

### The `--config` CLI flag
```

---

_@charliermarsh reviewed on 2024-02-09 21:06_

---

_Review comment by @charliermarsh on `docs/configuration.md`:463 on 2024-02-09 21:06_

Yeah, or even like... "For everything else, there's the `--config` flag."

---

_@AlexWaygood reviewed on 2024-02-09 21:17_

---

_Review comment by @AlexWaygood on `docs/configuration.md`:463 on 2024-02-09 21:17_

Added in 789d1d0d17e569f4a9ac96be3473af2fa0f66b25 👍

---

_@charliermarsh reviewed on 2024-02-09 21:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:313 on 2024-02-09 21:17_

I'm pretty sure `--config` "implies" `--isolated`, in that if you use `--config`, we ignore any discovered configuration files.

---

_Comment by @AlexWaygood on 2024-02-09 21:17_

Thanks all for the reviews! I'll land this now

---

_@charliermarsh reviewed on 2024-02-09 21:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:313 on 2024-02-09 21:18_

I think it's fine to allow them to be used together (even if that differs from the previous conflict settings).

---

_@AlexWaygood reviewed on 2024-02-09 21:27_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:313 on 2024-02-09 21:27_

So this PR is currently going out of its way to _preserve_ the behaviour on `main`, which is that `--config <config_file>` and `--isolated` aren't allowed to be specified simultaneously. On `main` we just use `clap` to forbid them both from being specified at once, but with this PR it ends up being a bunch of extra code to achieve that prohibition, because we _do_ want to allow `--config "<setting> = <value>"` in combination with `--isolated`.

The question is whether this PR _should_ be going out of its way to preserve the behaviour on `main` (which is what it currently does), or if it would actually be _desirable_ to change things so that it's now legal to specify `--isolated` in combination with `--config <config_file>`.

---

_@AlexWaygood reviewed on 2024-02-09 21:42_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:313 on 2024-02-09 21:42_

My inclination is to keep this PR how it is, and consider any possible behaviour changes relating to this in a separate issue/PR, if at all. There's not _that_ much extra code being added here to maintain the previous behaviour, and it should be pretty easy to rip it out if we _do_ want to change the behaviour

---

_Merged by @AlexWaygood on 2024-02-09 21:56_

---

_Closed by @AlexWaygood on 2024-02-09 21:56_

---

_Branch deleted on 2024-02-09 21:56_

---

_Comment by @ofek on 2024-02-19 03:39_

For posterity: https://github.com/astral-sh/ruff/issues/10035

---
