```yaml
number: 16319
title: "Fallback to `requires-python` in certain cases when `target-version` is not found"
type: pull_request
state: merged
author: dylwil3
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: micha/ruff-0.10
head: requires-python
created_at: 2025-02-22T19:14:02Z
updated_at: 2025-03-13T11:49:11Z
url: https://github.com/astral-sh/ruff/pull/16319
synced_at: 2026-01-12T15:55:54Z
```

# Fallback to `requires-python` in certain cases when `target-version` is not found

---

_@dylwil3_

# Summary

This PR introduces the following modifications in configuration resolution:

1. In the event where we are reading in a configuration file `myconfig` with no `target-version` specified, we will search for a `pyproject.toml` in the _same directory_ as `myconfig` and see if it has a `requires-python` field. If so, we will use that as the `target-version`.

2. In the event where...
  - we have not used the flag `--isolated`, and
  - we have not found any configuration file, and
  - we have not passed `--config` specifying a target version

then we will search for a `pyproject.toml` file with `required-python` in an ancestor directory and use that if we find it.

We've also added some debug logs to indicate which of these paths is taken.

## Implementation
Two small things:

1. I have chosen a method that will sometimes re-parse a `pyproject.toml` file that has already been parsed at some earlier stage in the resolution process. It seemed like avoiding that would require more drastic changes - but maybe there is a clever way that I'm not seeing!
2. When searching for these fallbacks, I suppress any errors that may occur when parsing `pyproject.toml`s rather than propagate them. The reasoning here is that we have already found or decided upon a perfectly good configuration, and this is just a "bonus" to find a better guess for the target version.

Closes #14813, #16662

## Testing

The linked issue contains a repo for reproducing the behavior, which we used for a manual test:

```console
ruff-F821-repro on  main
❯ uvx ruff check --no-cache
hello.py:9:11: F821 Undefined name `anext`. Consider specifying `requires-python = ">= 3.10"` or `tool.ruff.target-version = "py310"` in your `pyproject.toml` file.
  |
8 | async def main():
9 |     print(anext(g()))
  |           ^^^^^ F821
  |

Found 1 error.

ruff-F821-repro on  main
❯ ../ruff/target/debug/ruff check --no-cache
All checks passed!
```

In addition, we've added snapshot tests with the CLI output in some examples. Please let me know if there are some additional scenarios you'd like me to add tests for!

---

_Label `breaking` added by @dylwil3 on 2025-02-22 19:14_

---

_Label `configuration` added by @dylwil3 on 2025-02-22 19:14_

---

_Label `do-not-merge` added by @dylwil3 on 2025-02-22 19:14_

---

_Added to milestone `v0.10` by @dylwil3 on 2025-02-22 19:14_

---

_Comment by @github-actions[bot] on 2025-02-22 19:24_

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

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:205 on 2025-02-23 09:29_

Let's log at least a debug message when this happens

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:171 on 2025-02-23 09:30_

Is this function also called for user-level configurations? I'm asking because we should only apply this fallback to project configurations. 

We should also avoid this fallback for inherited configuration. 

---

_@MichaReiser reviewed on 2025-02-23 09:37_

Thanks for looking into this. 

We may need to restructure the code more. I believe the current implementation also applies the fallback for inherited and maybe even user configurations. 

We only want the fallback to apply to top-level project configurations.

---

_Comment by @dylwil3 on 2025-02-23 14:57_

> We only want the fallback to apply to top-level project configurations.

Since configurations are resolved relative the to the file being checked, I just want to confirm that what you mean here is something like the below. (Maybe the implementation is not what you'd prefer, but is the logic doing what you meant?)

```diff
diff --git a/crates/ruff_workspace/src/pyproject.rs b/crates/ruff_workspace/src/pyproject.rs
index 9d9bc4199..cbc3958ab 100644
--- a/crates/ruff_workspace/src/pyproject.rs
+++ b/crates/ruff_workspace/src/pyproject.rs
@@ -152,7 +152,10 @@ pub fn find_user_settings_toml() -> Option<PathBuf> {
 }
 
 /// Load `Options` from a `pyproject.toml` or `ruff.toml` file.
-pub(super) fn load_options<P: AsRef<Path>>(path: P) -> Result<Options> {
+pub(super) fn load_options<P: AsRef<Path>>(
+    path: P,
+    version_strategy: TargetVersionStrategy,
+) -> Result<Options> {
     if path.as_ref().ends_with("pyproject.toml") {
         let pyproject = parse_pyproject_toml(&path)?;
         let mut ruff = pyproject
@@ -172,16 +175,21 @@ pub(super) fn load_options<P: AsRef<Path>>(path: P) -> Result<Options> {
         if let Ok(ref mut ruff) = ruff {
             if ruff.target_version.is_none() {
                 debug!("No `target-version` found in `ruff.toml`");
-                if let Some(dir) = path.as_ref().parent() {
-                    let fallback = get_fallback_target_version(dir);
-                    if fallback.is_some() {
-                        debug!(
+                match version_strategy {
+                    TargetVersionStrategy::Standard => {}
+                    TargetVersionStrategy::RequiresPythonFallback => {
+                        if let Some(dir) = path.as_ref().parent() {
+                            let fallback = get_fallback_target_version(dir);
+                            if fallback.is_some() {
+                                debug!(
                             "Deriving `target-version` from `requires-python` in `pyproject.toml`"
                         );
-                    } else {
-                        debug!("No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified");
+                            } else {
+                                debug!("No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified");
+                            }
+                            ruff.target_version = fallback;
+                        }
                     }
-                    ruff.target_version = fallback;
                 }
             }
         }
@@ -236,6 +244,13 @@ fn get_minimum_supported_version(requires_version: &VersionSpecifiers) -> Option
     PythonVersion::iter().find(|version| Version::from(*version) == minimum_version)
 }
 
+#[derive(Debug, Default)]
+pub(super) enum TargetVersionStrategy {
+    #[default]
+    Standard,
+    RequiresPythonFallback,
+}
+
 #[cfg(test)]
 mod tests {
     use std::fs;
diff --git a/crates/ruff_workspace/src/resolver.rs b/crates/ruff_workspace/src/resolver.rs
index f550f810f..ef10119c3 100644
--- a/crates/ruff_workspace/src/resolver.rs
+++ b/crates/ruff_workspace/src/resolver.rs
@@ -23,7 +23,7 @@ use ruff_linter::package::PackageRoot;
 use ruff_linter::packaging::is_package;
 
 use crate::configuration::Configuration;
-use crate::pyproject::settings_toml;
+use crate::pyproject::{settings_toml, TargetVersionStrategy};
 use crate::settings::Settings;
 use crate::{pyproject, FileResolverSettings};
 
@@ -319,7 +319,15 @@ pub fn resolve_configuration(
         }
 
         // Resolve the current path.
-        let options = pyproject::load_options(&path).with_context(|| {
+        let version_strategy = if configurations.is_empty() {
+            TargetVersionStrategy::RequiresPythonFallback
+        } else {
+            // For inherited configurations, we do not attempt to
+            // fill missing `target-version` with `requires-python`
+            // from a nearby `pyproject.toml`
+            TargetVersionStrategy::Standard
+        };
+        let options = pyproject::load_options(&path, version_strategy).with_context(|| {
             if configurations.is_empty() {
                 format!(
                     "Failed to load configuration `{path}`",
```

---

_Comment by @MichaReiser on 2025-02-23 15:14_

Uhm, hard to say because I'm mostly unfamiliar with this part of Ruff.

Different strategies could be a solution or we start using different methods based on what configuration we're resolving (I think that's what we do in Red Knot)

---

_Comment by @dylwil3 on 2025-02-23 15:29_

I guess what I'm trying to say is: what do you mean by "project root"? It doesn't seem to me like that's really a first-class notion in Ruff in the same way that it kind of has to be in `uv` or Red Knot, as far as I can tell.

Probably I'm just not understanding the design spec here - sorry! So far I think I understand these requirements:

- [x] (from the issue) If we can't find any configuration but there's a pyproject.toml without a tool.ruff section, don't ignore it and instead use the requires-python from there
- [x] (from the issue) If we find a ruff.toml without a target-version, fallback to the pyproject.toml's requires-python at the same level (if there's any).
- [ ] Do not use the fallback when resolving user-level configuration

But I'm not understanding the requirement around the project root. Would it be possible to give an example of the behavior you're looking for?

---

_Comment by @MichaReiser on 2025-02-23 15:38_

That's a good point and using the term project root was probably more confusing than helpful (because Ruff doesn't know that concept). 

What I meant is that this fallback shouldn't apply to user-level configurations or an extended (`extend = "dont_fallback_to_pyproject_toml_here.toml"`) configuration. 



---

_Comment by @dylwil3 on 2025-02-23 15:41_

Excellent, thank you for clarifying! That's what I suspected (and is handled by the diff above). I'll tackle the user-config and inherited config adjustments now. Sorry for the confusion!

---

_Comment by @dylwil3 on 2025-02-23 18:02_

@dhruvmanila no urgency here, but before we add this to v0.10 it'd be great if you could double-check that the changes to `ruff server` make sense. The hope is that the changes will ensure that the settings will be resolved the same way whether you are running the server or passing in paths on the command line.

---

_Review comment by @MichaReiser on `crates/ruff/src/resolve.rs`:40 on 2025-02-24 09:23_

Do we need both `Relativity` and `ConfigurationProvenance`. I get the impression that `Relativity` can be derived from `Provenance`.

---

_Review comment by @MichaReiser on `crates/ruff/src/resolve.rs`:143 on 2025-02-24 09:25_

Can we extract the path into a variable and use it both in `find_fallback_target_version` and when calling `into_settings`. It required me few cycles to understand whether `path_dedot::CWD` is correct but I then noticed that it's already what `into_setting` uses

---

_Review comment by @MichaReiser on `crates/ruff/src/resolve.rs`:126 on 2025-02-24 09:27_

can you explain me the case where we end up in this branch and we'll find a `pyproject.toml`? 

I'd expect that it will always take the `Ancestor` branch above because there's a `pyproject.toml` in the current (or any parent) directory unless no such file exists, in which case the override never applies

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/resolver.rs`:410 on 2025-02-24 09:48_

nit: I had to Google what "provenance" meant (TIL), I'd possibly name it `ConfigurationOrigin` or `ConfigurationSource`

> provenance /prŏv′ə-nəns, -näns″/
> noun
> 1. Place of origin; derivation.

---

_Review comment by @dhruvmanila on `crates/ruff/tests/lint.rs`:2094 on 2025-02-24 09:50_

nit: I'd move the attribute right above the function

```suggestion
/// ```
/// tmp
/// ├── pyproject.toml #<--- no `[tool.ruff]`
/// └── test.py
/// ```
#[test]
fn requires_python_no_tool() -> Result<()> {
```

Same for other tests and structs

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/pyproject.rs`:264 on 2025-02-24 09:53_

Can we document these variants? It might be hard to infer what a standard strategy is without looking at how it's being used 😅

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/resolver.rs`:334 on 2025-02-24 09:54_

I think I'd move this as documentation on the variants instead

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/resolver.rs`:414 on 2025-02-24 09:59_

Is this to indicate https://github.com/astral-sh/ruff/blob/5eaf225fc3baf738493ad052575d544e565d74d3/crates/ruff_workspace/src/pyproject.rs#L100-L103 ?

This is in a user specific config directory like `~/.config/ruff.toml`.

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/pyproject.rs`:186 on 2025-02-24 10:18_

I think I'll reword it to indicate we've _derived_ the target-version from a pyproject.toml

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/resolver.rs`:306 on 2025-02-24 10:22_

Can we update the function documentation to indicate what does `None` mean here?

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:174 on 2025-02-24 10:25_

Nit
```suggestion
				let path = path.as_ref();
        let mut ruff = parse_ruff_toml(path);
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/resolve.rs`:119 on 2025-02-24 10:25_

nit: It's unclear to me what does this convert into, it might be useful to explicitly use the `from` syntax

---

_Review comment by @dhruvmanila on `crates/ruff/src/resolve.rs`:117 on 2025-02-24 10:26_

Same as earlier comment about using the past tense

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:186 on 2025-02-24 10:26_

```suggestion
			                            "Deriving `target-version` from `requires-python` in `pyproject.toml`"
			                        );
```

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:191 on 2025-02-24 10:27_

```suggestion
                            if let Some(fallback) = get_fallback_target_version(dir) {
                                debug!(
			                            "Deriving `target-version` from `requires-python` in `pyproject.toml`"
			                        );
			                        ruff.target_version = Some(fallback);
                            } else {
                                debug!("No `pyproject.toml` with `requires-python` in same directory; `target-version` unspecified");
                            }
                        }
```

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:263 on 2025-02-24 10:28_

Let's add some documentation to what either strategy means.

I find `Standard` a bit confusing, considering that it isn't the standard for most `ruff.toml` files. Maybe `NoFallback`, `RequiresPythonFallback` 

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:258 on 2025-02-24 10:29_

I suggest removing the default bacause it's important that we make a deliberate decision for every call site

---

_@dhruvmanila reviewed on 2025-02-24 10:29_

---

_Comment by @dhruvmanila on 2025-02-24 10:31_

> @dhruvmanila no urgency here, but before we add this to v0.10 it'd be great if you could double-check that the changes to `ruff server` make sense. The hope is that the changes will ensure that the settings will be resolved the same way whether you are running the server or passing in paths on the command line.

Yeah, I think it makes sense although I'd also test it out in an editor context. I can add it to my todo but I was wondering if you had done any testing in an editor (VS Code or any other)?

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/resolver.rs`:306 on 2025-02-24 11:11_

Let's make `ConfigurationProvenance` `Copy` instead of passing it by reference

```suggestion
    provenance: Option<ConfigurationProvenance>,
```

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/resolver.rs`:387 on 2025-02-24 11:11_

```suggestion
    provenance: Option<ConfigurationProvenance>,
```

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/resolver.rs`:335 on 2025-02-24 11:18_

Let's add a test what happens for:

`pyproject.toml`

```toml
requires-python = ">=3.9"
```

*ruff.toml*

```toml
extend = "./shared/base_config.toml"
```

`shared/base_config.toml`

```toml
target-version = "py310"
```

I think red-knot respects the 3.9 in this case, even though the extended configuration sets the `target-version` to 3.10. This is mainly because it simplifies the resolution and there's an argument that either behavior is correct. 



---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/resolver.rs`:735 on 2025-02-24 11:19_

It's not clear to me why using `None` here is correct (or when using `None` is correct in general). Can you tell me a bit more about it?

---

_@MichaReiser reviewed on 2025-02-24 11:20_

---

_@dylwil3 reviewed on 2025-02-28 23:15_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/resolver.rs`:410 on 2025-02-28 23:15_

https://github.com/astral-sh/ruff/pull/16319/commits/abc6af21d5bde0ba30d961b257895288c7b46a05

---

_Review comment by @dylwil3 on `crates/ruff/tests/lint.rs`:2094 on 2025-02-28 23:16_

https://github.com/astral-sh/ruff/pull/16319/commits/d117da27a5fad0a2bcf06fe36d84d5684253ba5a

---

_@dylwil3 reviewed on 2025-02-28 23:16_

---

_@dylwil3 reviewed on 2025-02-28 23:17_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/pyproject.rs`:264 on 2025-02-28 23:17_

https://github.com/astral-sh/ruff/pull/16319/commits/0a64d65b0c586407050d212d38fc17d1bf65c1d2

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/resolver.rs`:334 on 2025-02-28 23:18_

I did both, with a more abbreviated version on the enum itself

---

_@dylwil3 reviewed on 2025-02-28 23:18_

---

_@dylwil3 reviewed on 2025-02-28 23:21_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/resolver.rs`:414 on 2025-02-28 23:21_

yep! added this as an example to doc comment in this commit: https://github.com/astral-sh/ruff/pull/16319/commits/af82f19da8c125ae21cb90341c59142cbfffbf63

---

_@dylwil3 reviewed on 2025-02-28 23:21_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/pyproject.rs`:186 on 2025-02-28 23:21_

https://github.com/astral-sh/ruff/pull/16319/commits/edcebd55dccd34cbac081f737e74c9646bc10849

---

_@dylwil3 reviewed on 2025-02-28 23:22_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/resolver.rs`:306 on 2025-02-28 23:22_

added `Unknown` variant with doc-comment; removed `Option`s: https://github.com/astral-sh/ruff/pull/16319/commits/af82f19da8c125ae21cb90341c59142cbfffbf63

---

_@dylwil3 reviewed on 2025-02-28 23:22_

---

_Review comment by @dylwil3 on `crates/ruff/src/resolve.rs`:119 on 2025-02-28 23:22_

https://github.com/astral-sh/ruff/pull/16319/commits/5c1b8def4bf6510cad79ce4c60b64999b473b0eb

---

_Review comment by @dylwil3 on `crates/ruff/src/resolve.rs`:117 on 2025-02-28 23:22_

https://github.com/astral-sh/ruff/pull/16319/commits/edcebd55dccd34cbac081f737e74c9646bc10849

---

_@dylwil3 reviewed on 2025-02-28 23:22_

---

_@dylwil3 reviewed on 2025-02-28 23:23_

---

_Review comment by @dylwil3 on `crates/ruff/src/resolve.rs`:40 on 2025-02-28 23:23_

https://github.com/astral-sh/ruff/pull/16319/commits/fe825a71dab8d72e84d5df4488f8c5cd67b6d6c7

---

_@dylwil3 reviewed on 2025-02-28 23:26_

---

_Review comment by @dylwil3 on `crates/ruff/src/resolve.rs`:126 on 2025-02-28 23:26_

Added clarification here: https://github.com/astral-sh/ruff/pull/16319/commits/80501d04548e877d37cdc53230a91b4604b9dc37

You'll end up here if you had a `pyproject.toml` file that did not contain a `[tool.ruff]` table. This is implicit in the `Ancestor` branch above because `find_settings_toml` only looks for `pyproject.toml` files that contain a `[tool.ruff]`.

---

_@dylwil3 reviewed on 2025-02-28 23:27_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/pyproject.rs`:174 on 2025-02-28 23:27_

made this change in various places where there were multiple calls to `as_ref` in situations like this: https://github.com/astral-sh/ruff/pull/16319/commits/5c2ef01e0ccd448eb818e8331895be8f30b56d6b

---

_@dylwil3 reviewed on 2025-02-28 23:27_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/pyproject.rs`:186 on 2025-02-28 23:27_

instead I made the spacing match the `debug` call in the other branch: https://github.com/astral-sh/ruff/pull/16319/commits/ffd2dbf679d01f3bbd071ec30c836c166fcc46f0

---

_@dylwil3 reviewed on 2025-02-28 23:28_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/pyproject.rs`:191 on 2025-02-28 23:28_

I don't think I can quite do this because I need to use `fallback` (as an `Option<_>`) right below this.

---

_@dylwil3 reviewed on 2025-02-28 23:29_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/pyproject.rs`:263 on 2025-02-28 23:29_

https://github.com/astral-sh/ruff/pull/16319/commits/0a64d65b0c586407050d212d38fc17d1bf65c1d2

---

_@dylwil3 reviewed on 2025-02-28 23:29_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/pyproject.rs`:258 on 2025-02-28 23:29_

https://github.com/astral-sh/ruff/pull/16319/commits/0a64d65b0c586407050d212d38fc17d1bf65c1d2

---

_@dylwil3 reviewed on 2025-02-28 23:30_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/resolver.rs`:306 on 2025-02-28 23:30_

https://github.com/astral-sh/ruff/pull/16319/commits/ccadd49bd09f0f6ac96363427d3f5e8cedcdd0d4

---

_@dylwil3 reviewed on 2025-02-28 23:30_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/resolver.rs`:335 on 2025-02-28 23:30_

yep the behavior here matches red knot https://github.com/astral-sh/ruff/pull/16319/commits/d9e144e8d82563128e57bd523ccd060bbd0b0f5d

---

_@dylwil3 reviewed on 2025-02-28 23:31_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/resolver.rs`:735 on 2025-02-28 23:31_

tried to clarify here (replacing `None` with new variant `Unknown` which signals that the configuration origin is unknown to the caller: https://github.com/astral-sh/ruff/pull/16319/commits/af82f19da8c125ae21cb90341c59142cbfffbf63

---

_@dylwil3 reviewed on 2025-02-28 23:36_

---

_Review comment by @dylwil3 on `crates/ruff/src/resolve.rs`:143 on 2025-02-28 23:36_

I'm not sure, it depends on what behavior we want.

The way I have it currently set up I'm trying to make the behavior change as little as possible, so I've kept the settings resolution path the same no matter what, but attempted to find a fallback version using the same logic as we did when searching for a user configuration file. That logic would start from the stdin filename directory if it was passed in.

In other words, I don't think I can extract a `path` variable here since I may be passing a different value to `find_fallback_target_version` than I am to `into_settings`.

---

_Comment by @dylwil3 on 2025-02-28 23:38_

> > @dhruvmanila no urgency here, but before we add this to v0.10 it'd be great if you could double-check that the changes to `ruff server` make sense. The hope is that the changes will ensure that the settings will be resolved the same way whether you are running the server or passing in paths on the command line.
> 
> Yeah, I think it makes sense although I'd also test it out in an editor context. I can add it to my todo but I was wondering if you had done any testing in an editor (VS Code or any other)?

I haven't tried it in an editor yet but I will!

---

_Review comment by @MichaReiser on `crates/ruff/src/resolve.rs`:119 on 2025-03-03 10:34_

Nit: I think I'd find it helpful to include the fallback version in the log message: *Derived `target-version` from `requires-python` setting ({fallback})*

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:2126 on 2025-03-03 10:35_

It's unconventional to assert on the log messages. Could we use `--show-settings` instead? 

It may also be useful to include an assertion before that explicitly asserts that the `target-version` is not `3.11` if the `pyproject.toml` doesn't exist.

The same applies for other tests.

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:2752 on 2025-03-03 10:39_

It seems all tests are specific to when the `pyproject.toml` has no `tool.ruff` table. Do we need a test for when the `pyproject.toml` has a `tool.ruff` section?

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:186 on 2025-03-03 10:40_

Nit: Same as above, I'd find it useful to include the parsed version in here.

---

_@MichaReiser approved on 2025-03-03 10:40_

---

_Review comment by @dylwil3 on `crates/ruff/tests/lint.rs`:2126 on 2025-03-04 12:35_

I did this but then I had to change some of the explicit lower versions to 3.10 so that you could tell the difference between "defaulting to 3.9" and "reading 3.9 from a toml file"

---

_@dylwil3 reviewed on 2025-03-04 12:35_

---

_Comment by @MichaReiser on 2025-03-10 14:25_

@dylwil3 feel free to merge this into the ruff-0.10 feature branch once it's ready (I already changed the base)

---

_Label `do-not-merge` removed by @MichaReiser on 2025-03-10 14:25_

---

_@dhruvmanila approved on 2025-03-12 04:17_

The changes all look correct to me for the server settings.

Edit: Wait, I'm seeing some discrepancies in the editor context. 

---

_Comment by @dhruvmanila on 2025-03-12 04:39_

I'm currently testing out this in an editor context using some of the test cases mentioned in https://github.com/astral-sh/ruff/blob/8aa8c56cad2697c0e4c6a86aa997221e842dcfea/crates/ruff/tests/lint.rs

https://github.com/astral-sh/ruff/blob/8aa8c56cad2697c0e4c6a86aa997221e842dcfea/crates/ruff/tests/lint.rs#L2088-L2094

For this test case, the target version in the editor is still giving me 3.9. As per the test case, it should be 3.11 ? The debug information gives me that the indexed configuration file is empty even though the project has a `pyproject.toml` file (without `tool.ruff` heading). I think this is because the server uses the `settings_toml` function that skips the `pyproject.toml` file without a `tool.ruff` heading:

https://github.com/astral-sh/ruff/blob/ba44e9de13edca36041bfe258f728ebbe3916970/crates/ruff_workspace/src/pyproject.rs#L81-L85

I believe this isn't expected? Sorry for taking this on at the last moment before the 0.10 release.

https://github.com/astral-sh/ruff/blob/8aa8c56cad2697c0e4c6a86aa997221e842dcfea/crates/ruff/tests/lint.rs#L2716-L2724

For this test case, I'm seeing the incorrect behavior where the target version is still 3.9.

https://github.com/astral-sh/ruff/blob/8aa8c56cad2697c0e4c6a86aa997221e842dcfea/crates/ruff/tests/lint.rs#L2397-L2404

For this test case, I'm seeing the correct behavior where the target version is 3.11.

---

_Comment by @MichaReiser on 2025-03-12 07:16_

@dylwil3 can you also take a look at https://github.com/astral-sh/ruff/issues/16662 to verify if it is fixed with your changes.

---

_Comment by @dylwil3 on 2025-03-12 10:30_

Thanks Dhruv, looking into this now!

---

_Comment by @codspeed-hq[bot] on 2025-03-12 11:40_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dylwil3%3Arequires-python)

### Merging #16319 will **degrade performances by 4.6%**

<sub>Comparing <code>dylwil3:requires-python</code> (dc26d44) with <code>micha/ruff-0.10</code> (d622137)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dylwil3%3Arequires-python)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.6% |


---

_Comment by @dylwil3 on 2025-03-12 12:10_

@dhruvmanila Thanks for catching the error! I was able to verify my attempted fix with the server in Helix, but I wasn't sure how to use my local build to test in VSCode. Is there documentation for how to do that somewhere?

---

_Comment by @MichaReiser on 2025-03-12 12:22_

> @dhruvmanila Thanks for catching the error! I was able to verify my attempted fix with the server in Helix, but I wasn't sure how to use my local build to test in VSCode. Is there documentation for how to do that somewhere?

It's relatively straightforward for as long as it doesn't require changes to the extension itself. 

* Install Ruff's VS code extension
* Open the settings and set `ruff.path` to your debug ruff binary:


The following is my configuration

```json
{
    "ruff.path": [
        "/Users/micha/astral/ruff/target/debug/ruff",
    ]
}

---

_Comment by @dylwil3 on 2025-03-12 13:13_

Update: It looks like the case where there's no `ruff.toml` or `[tool.ruff]` in any `pyproject.toml` (but there is a `requires-python`) is still not working as expected for the server. I believe it's because the logic here for the "fallback to default in the absence of any config files" case was not reproduced in the `server`:

https://github.com/astral-sh/ruff/blob/b098255d2ef14b92a024b141368e1c4f3f876e14/crates/ruff/src/resolve.rs#L104-L115

Looking into it now - sorry for all the last minute hullabaloo close to the release!

---

_Comment by @dylwil3 on 2025-03-12 15:28_

I believe the `server` behavior is now fixed! 🤞  I wish there were a way to write tests for it, but I tried out the failing examples in VSCode and printed the configs and they work now.

---

_@dhruvmanila reviewed on 2025-03-12 16:50_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:305 on 2025-03-12 16:50_

If I'm understanding this change correctly, we'd now have a fallback settings for _every_ directory in the project. I think this might create an unexpected behavior because for a file in a nested directory, it would use this fallback settings instead of the settings from the project root? Let me test this out.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:305 on 2025-03-12 16:54_

Yeah, I tested out, I think this is an incorrect fix. Consider the following directory structure:
```
.
├── nested
│   └── foo
│       └── test.py
├── pyproject.toml
└── test.py
```

The `nested/foo/test.py` does not consider any settings from `pyproject.toml`, it's only the `test.py` file that will consider it.

---

_@dhruvmanila reviewed on 2025-03-12 16:54_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:92 on 2025-03-12 17:06_

Does that mean that we'll pick up the `requires-python` constraint even if a user uses `editor-only` in their settings. I'd be a bit confused if `editor-only` loads any project settings. @dhruvmanila what's your take on this?

---

_@MichaReiser reviewed on 2025-03-12 17:06_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:305 on 2025-03-12 17:07_

It's not entirely clear to me why the change here is necessary. Shouldn't the changes above be sufficient?

---

_@MichaReiser reviewed on 2025-03-12 17:07_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:92 on 2025-03-12 17:13_

Yeah, I don't think we should be looking at any of the config files if the configuration preference is `editorOnly` unless the user has explicitly asks us using the `ruff.configuration`.

---

_@dhruvmanila reviewed on 2025-03-12 17:13_

---

_@dylwil3 reviewed on 2025-03-12 17:30_

---

_Review comment by @dylwil3 on `crates/ruff_server/src/session/index/ruff_settings.rs`:92 on 2025-03-12 17:30_

I think we won't have gotten this far in the code in that case, though, right? We exited earlier here:

https://github.com/astral-sh/ruff/blob/dd2313ab0faea90abf66a75f1b5c388e728d9d0a/crates/ruff_server/src/session/index/ruff_settings.rs#L114-L123

---

_@MichaReiser reviewed on 2025-03-13 09:09_

---

_Review comment by @MichaReiser on `crates/ruff/src/resolve.rs`:92 on 2025-03-13 09:09_

It seems inconsistent that we don't apply the fallback behavior if a user only has the user-level configuration. It means that we won't pick up the right python version if you have:

* a user level ruff configuration
* but use a `pyproject.toml` without a `tool.ruff` section at the project level

I think it would be good to apply the `requires-python` fallback in this case as well. 




---

_@dhruvmanila reviewed on 2025-03-13 09:56_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:92 on 2025-03-13 09:56_

That branch will be calling `RuffSettings::editor_only` to create a default configuration so we'll be getting there in this case.

---

_Comment by @MichaReiser on 2025-03-13 11:00_

I pushed three new commits: 

1. This is more a quality of life improvement: The server now shows logs emitted with `log::debug` (instead of `tracing::debug`)
2. Changes the behavior for user-level configurations: A `pyproject.toml` now always overrides the `target-version` from the user-level configuration. I'm not a 100% if this is the right change but it's what we do in Red Knot. However, Red Knot's model is slightly different so I'm not sure if this behavior makes sense for Ruff. Maybe it's best to revert it but i'm interested in feedback
3. The main changes on the server side

@dhruvmanila and @dylwil3 could yo uboth take a look

---

_Comment by @MichaReiser on 2025-03-13 11:05_

@dylwil3 we should also update https://docs.astral.sh/ruff/configuration/#config-file-discovery or have a new section explaining the `target-version` discovery but we can do this in a separate PR

---

_Comment by @dhruvmanila on 2025-03-13 11:22_

> 2\. Changes the behavior for user-level configurations: A `pyproject.toml` now always overrides the `target-version` from the user-level configuration. I'm not a 100% if this is the right change but it's what we do in Red Knot. However, Red Knot's model is slightly different so I'm not sure if this behavior makes sense for Ruff. Maybe it's best to revert it but i'm interested in feedback

IIUC, between a user-level config (`~/.config/ruff.toml`) and the `pyproject.toml` in the project, Ruff will prefer to use the `requires-python` in the project config if present. If not, it'll use the one in the user-level config if present.

Why are you not sure about the behavior in Ruff? I think preferring a project specific version makes sense intuitively, I'd be surprised to see that the `requires-python` isn't being picked up by Ruff even though it's defined in the project config.



---

_Comment by @dylwil3 on 2025-03-13 11:31_

> I pushed three new commits:

Thank you, this looks awesome!

> 1. This is more a quality of life improvement: The server now shows logs emitted with `log::debug` (instead of `tracing::debug`)

✅ 

> 2. Changes the behavior for user-level configurations: A `pyproject.toml` now always overrides the `target-version` from the user-level configuration. I'm not a 100% if this is the right change but it's what we do in Red Knot. However, Red Knot's model is slightly different so I'm not sure if this behavior makes sense for Ruff. Maybe it's best to revert it but i'm interested in feedback

This seems intuitive to me. 

> 3. The main changes on the server side
> 

Tested this in VSCode and looks good! The behavior Dhruv pointed out before with a nested file no longer occurs, and the fallback matches the CLI behavior in the edge case where there is no `[tool.ruff]` or `ruff.toml` anywhere (so, in Dhruv's example, if you open VSCode from `tmp` you'll get a different target version than you would by opening VSCode from `nested/foo`).

I will work on some documentation!


---

_Comment by @MichaReiser on 2025-03-13 11:32_

Okay, so I think this PR is ready to land, assuming my changes look good.

---

_Comment by @dhruvmanila on 2025-03-13 11:44_

The code looks good, thank you for addressing the issues. I also did some quick testing (not the same ones as Dylan).


---

_Merged by @dylwil3 on 2025-03-13 11:49_

---

_Closed by @dylwil3 on 2025-03-13 11:49_

---

_Comment by @dylwil3 on 2025-03-13 11:49_

Thanks so much @dhruvmanila  and @MichaReiser !!

---
