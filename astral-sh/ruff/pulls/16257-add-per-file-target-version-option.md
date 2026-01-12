```yaml
number: 16257
title: "Add `per-file-target-version` option"
type: pull_request
state: merged
author: ntBre
labels:
  - configuration
assignees: []
merged: true
base: main
head: brent/per-file-target-version
created_at: 2025-02-19T15:08:31Z
updated_at: 2025-02-24T13:47:17Z
url: https://github.com/astral-sh/ruff/pull/16257
synced_at: 2026-01-12T15:55:54Z
```

# Add `per-file-target-version` option

---

_@ntBre_

## Summary

This PR is another step in preparing to detect syntax errors in the parser. It introduces the new `per-file-target-version` top-level configuration option, which holds a mapping of compiled glob patterns to Python versions. I intend to use the `LinterSettings::resolve_target_version` method here to pass to the parser:

https://github.com/astral-sh/ruff/blob/f50849aeef51a381af6c27df8595ac0e1ef5a891/crates/ruff_linter/src/linter.rs#L491-L493

## Test Plan

I added two new CLI tests to show that the `per-file-target-version` is respected in both the formatter and the linter.

---

_Comment by @github-actions[bot] on 2025-02-19 15:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-02-19 16:27_

We should respect the `per-file-target-version` when resolving the target version for a file being linted or formatted. Ideally, we'd add a CLI test that demonstrates that the `per-file-target-version` override is respected both for linting and formatting.

---

_Label `configuration` added by @MichaReiser on 2025-02-19 16:27_

---

_Comment by @ntBre on 2025-02-19 16:33_

That makes sense to me. How does the approach of making the current `target_version` fields private and requiring callers to use the `resolve_target_version` method sound to you? I only added `LinterSettings::resolve_target_version` so far, but I'm assuming a formatter analog would be easy too.

I wasn't sure how many changes to put in this PR, but I think it definitely makes sense to wire it up somehow for useful tests.

---

_Converted to draft by @ntBre on 2025-02-19 16:33_

---

_Comment by @ntBre on 2025-02-19 16:34_

I'll convert to a draft because I think it's incomplete based on your suggestion.

---

_Comment by @MichaReiser on 2025-02-19 17:31_

Hmm. I haven't really thought about it. We probably want to avoid calling `resolve_target_version` everywhere where we compare the target versions. That would become expensive very quickly. Ideally, we would compute the target version for the current file once and then store it somewhere (on `Checker`?)

---

_Comment by @ntBre on 2025-02-19 19:33_

Reopening for review. I added
* `Checker::target_version` as a field and `const` method to return the resolved `PythonVersion` [^1] to be used in lint rules
* Target version resolution to the formatter
* Two new CLI tests showing that both the linter and formatter respect the new option

[^1]: I tried also making the `LinterSettings::target_version` private, as I mentioned above, but it disrupts a ton of tests that create them directly with code like `LinterSettings { ..LinterSettings::default() }`. This could be solved with a bunch of `with_*` methods to build a `LinterSettings` without `pub` fields, but after adding two of those and seeing the number of remaining errors, I just made them `pub` again. I think I caught all of the (current) uses in lint rules while they were private, though.

---

_Marked ready for review by @ntBre on 2025-02-19 19:33_

---

_Review requested from @AlexWaygood by @ntBre on 2025-02-19 19:33_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/settings.rs`:168 on 2025-02-20 08:00_

I'd probably rename this field to `default_target_version` or `unresolved_target_version` mainly so that it isn't the *perfect fit* for what someone might need when writing formatter or linter code. This ensures we take a moment to think about: Is this the right field and maybe consult the documentation. I'm otherwise concerned that it's too easy to accidentally use `target_version` over the resolved target version. 

We should document this field and hint the user toward how they can get the correct target version. The same applies for the `LinterSettings`.

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:337 on 2025-02-20 08:02_

We might want to extend this documentation by mentioning that it is to override the global default `target-version` or `requires-python` and mention a use case where this is desired (and what the implications are)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:666 on 2025-02-20 08:04_

Do I understand it correctly that we don't support negated patterns for `per-file-target-version`? I'm worried that this might be confusing, considering that `per-file-ignore` does support them.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:801 on 2025-02-20 08:05_

We should use `.context` or `with_context` or the error messages will be very unhelpful (e.g. invalid glob, good luck figuring out which glob).

We may also want to use `context` or `with_context` in `from_options` 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:670 on 2025-02-20 08:06_

Do we need to implement `Display` similar to `PerFileIgnores`? How does the output in `--show-settings` look like?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:805 on 2025-02-20 08:09_

I think it's fine to ignore this for now, considering that this is a pre-existing issue with `per-file-ignore` and it has never come up before. But TOML tables have no defined ordering. This could be problematic if two patterns are overlapping but specify different versions. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:639 on 2025-02-20 08:11_

I don't think this comment is useful for future readers, considering that `PerFileIgnore` has no documentation. It only tells users: Here are two versions of this. 

We should instead document whta this struct does (It specifies the target version for a glob pattern). We could also consider extracting a `PerFile<T>` that's shared between `PerFileIgnore` and `PerFileVersion`.

I'd suggest renaming this struct to `PerFileTargetVersion`. It helps clarifying what kind of version it is.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/isort/rules/organize_imports.rs`:85 on 2025-02-20 08:13_

I think I'd prefer to pass in the `TargetVersion` instead of the `path`, considering that the `path` isn't needed here otherwise.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:103 on 2025-02-20 08:15_

Could we pass in the target version here instead of resolving it over and over again for every file that's being linted?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/imports.rs`:23 on 2025-02-20 08:16_

I'd prefer if we pass the `PythonVersion` here rather than the `Path`. It currently feels inconsistent where almost all rules take a `PythonVersion` but there are a few exceptions where we pass the `Path` and the rule has to figure out how to get the target version.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:253 on 2025-02-20 08:17_

It could make sense to explicitly pass the `PythonVersion` as an argument and instead compute it once inside `lint_path` (or whatever the function is called that finally calls into `Checker`, there are too many of them)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:509 on 2025-02-20 08:17_

Let's add some documentation

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/settings.rs`:237 on 2025-02-20 08:19_

I can't comment on line 251 -- thanks GitHub -- but we need to include the new file in the `Display` implementation (same for `LinterSettings`)

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:351 on 2025-02-20 08:22_

I'd prefer if `to_format_options` takes an `Option<Path>` argument. That should also reveal that the `per-file-target-version` -- as is in this commit -- doesn't work in the LSP. It probably makes sense to quickly spin up a VS code instance, set the `ruff.path` setting to the debug build executable, and test if the option is respected correctfully in VS Code (I also suggest testing notebooks)

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:2628 on 2025-02-20 08:23_

You may want to add an assertion above without the `per-file-target-version` override that asserts that `UP046` triggers on this code. 

---

_Review comment by @MichaReiser on `crates/ruff/tests/format.rs`:2134 on 2025-02-20 08:25_

Same as for the linter. I think it would be good to have two assertions in this test:

1. One that asserts how the code gets formatted without the `--per-file-target-version` configuration set
2. One that asserts how the code gets formatted **with** the `--per-file-target-version` set. 


This ensures that the formatting change is, in fact, only due to the `--per-file-target-version` configuration and not because the formatter detected any other Python 3.9 syntax in the file and, therefore, decided that it's fine to parenthesize with items.

---

_@MichaReiser requested changes on 2025-02-20 08:26_

This is great! I suggest renaming the field on the setting structs to e.g. `default_target_version` or `global_target_version` so that its name raises questions: *Hmm, why global? maybe this is not the field I want. Let me check the documentation*

We should also test this feature in the editor. I think it currently doesn't work for the formatter. 

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:136 on 2025-02-20 11:47_

I'd prefer to pass around a `PythonVersion` in the `ruff_linter` crate where possible (only converting it to a `u8` representing the minor version when crossing the crate boundary and calling a function from the `ruff_python_stdlib` crate) as it's more strongly typed

```suggestion
fn is_allowed_module(settings: &LinterSettings, version: PythonVersion, module: &str) -> bool {
```

---

_@AlexWaygood reviewed on 2025-02-20 11:48_

I'll leave this one mostly to Micha, just a quick nit I spotted

---

_@ntBre reviewed on 2025-02-20 18:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:805 on 2025-02-20 18:48_

Ah that's a good point. Based on this outdated(?) comment, it looks like the `CompiledPerFileIgnoreList` used to be sorted, but I don't see it being sorted anymore.

https://github.com/astral-sh/ruff/blob/470f852f04f0e4284c78d15ce81637540e127558/crates/ruff_linter/src/settings/types.rs#L584-L588

I factored out a generic `CompiledPerFileList<T>` type, so if we end up needing to sort the lists, we should be able to handle them both at once, at least.

---

_@ntBre reviewed on 2025-02-20 18:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:666 on 2025-02-20 18:57_

I took your suggestion in another comment of factoring out a `PerFile<T>` type, so we should now inherit the same negated globbing behavior as `per-file-ignore`!

---

_@ntBre reviewed on 2025-02-20 19:12_

---

_Review comment by @ntBre on `crates/ruff_workspace/src/settings.rs`:168 on 2025-02-20 19:12_

Ooh, I like `unresolved_target_version`. I think that will capture most of what I hoped for with making it private.

---

_@ntBre reviewed on 2025-02-20 21:15_

---

_Review comment by @ntBre on `crates/ruff/src/commands/format.rs`:351 on 2025-02-20 21:15_

Nice catch! I tested a script and notebook manually in VS Code, and they both seem to be working now. Is there any way to test that here?

---

_Comment by @ntBre on 2025-02-20 22:45_

Wow I didn't know eliding the lifetime in a const was a recent feature. I thought clippy used to suggest that :shrug: Maybe I need to start building with 1.80 locally instead of 1.82...

---

_Review comment by @ntBre on `crates/ruff_wasm/src/lib.rs`:306 on 2025-02-20 22:56_

This is the one place I couldn't find a way to get a `Path`. I noticed some `Path::new(".")` calls up above, though. Is there something useful I could pass here, or do path-based settings not make sense in WASM?

---

_@ntBre reviewed on 2025-02-20 22:56_

---

_Comment by @ntBre on 2025-02-20 23:08_

Thanks @MichaReiser for the thorough review. I think it's in a much better place now. The only open question I see now is how to add tests for the editor integration (if that's possible).

I also feel like there might be a more elegant solution to the generic `PerFile<T>` stuff, but I haven't found it yet. Probably we could remove some of the wrapper newtypes and just use the generic versions directly at least, if we want to.

---

_@MichaReiser reviewed on 2025-02-21 07:10_

---

_Review comment by @MichaReiser on `crates/ruff_wasm/src/lib.rs`:306 on 2025-02-21 07:10_

The files in the wasm playground don't have a path, so I think this is fine

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/mod.rs`:232 on 2025-02-21 07:13_

I'd probably mention `Checker.target_version` first because that's what most devs should use. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:677 on 2025-02-21 07:15_

I'd probably add a third argument: `debug_label: &'static str` over introducing my own trait. Seems simpler

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:728 on 2025-02-21 07:16_

```suggestion
    T: Display,
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:625 on 2025-02-21 07:17_

```suggestion
pub struct CompiledPerFileList<T> {
```

I prefer to avoid having type constraints in struct and instead, only add them on the impls where needed. 

I think the downside of this is that you'd have to implement `CacheKey` manually (because we aren't using Rust edition 2024 yet)

---

_@MichaReiser approved on 2025-02-21 07:19_

---

_Comment by @MichaReiser on 2025-02-21 07:21_

>  The only open question I see now is how to add tests for the editor integration (if that's possible).

There are a few end to end tests in the VS code extension but it isn't something we have good infrastructure today. But @dhruvmanila knows better than I.

---

_@ntBre reviewed on 2025-02-21 13:08_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/mod.rs`:232 on 2025-02-21 13:08_

Oh, of course

---

_@ntBre reviewed on 2025-02-21 13:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:625 on 2025-02-21 13:35_

Ah that makes sense, I did find it annoying to have to  put `: CacheKey` everywhere. I took a brief look at adding the trait bounds in the derive macro, but I probably need a little more proc macro practice before tackling that!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:625 on 2025-02-21 13:38_

I'm not sure if that's something that you even can do in the proc macro? I'd suggest to simply implement `CacheKey` manually.

---

_@MichaReiser reviewed on 2025-02-21 13:38_

---

_@ntBre reviewed on 2025-02-21 13:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:625 on 2025-02-21 13:42_

That's what `Clone` does, so I thought it was possible. But I looked for the source code, and it said `Clone` was a compiler builtin, so maybe you're right. I already switched to the manual impl locally, I was just curious.

---

_@ntBre reviewed on 2025-02-21 13:59_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:625 on 2025-02-21 13:59_

I think [serde](https://github.com/serde-rs/serde/blob/e3eaa6a3dd6edd701476097182313cdbd73da78c/serde_derive/src/ser.rs#L130) might be able to do it too, again just for my curiositiy, I don't think we need to do this here. `CacheKey` is easy to implement.

---

_Comment by @dhruvmanila on 2025-02-21 14:02_

> There are a few end to end tests in the VS code extension but it isn't something we have good infrastructure today. But @dhruvmanila knows better than I.

I've talked with Brent in regards to this on Discord. Looking at the change that fixed the issue which you were pointing out earlier there are two options:
1. Add unit tests in https://github.com/astral-sh/ruff/blob/df9fbdce3af5af826115780f2d8c5cc4d91be650/crates/ruff_server/src/format.rs for `format` function specifically for per-file-target-version
2. Add a E2E test in https://github.com/astral-sh/ruff-vscode/blob/main/src/test/e2e.test.ts with a fixture that sets this new option but this would only work _after_ the Ruff release with this option is went out. I wouldn't recommend using this for a new option in Ruff.

I'd suggest (1) but I'm longing to create a mock server xD

---

_Comment by @ntBre on 2025-02-21 15:16_

Yes, thanks @dhruvmanila! I've added unit tests for `format` and `format_range` for scripts that parallel the other linter and formatter tests I added, so I think this was a really nice idea.

It looks like notebooks also go through this code path, but I'm happy to add notebook-specific tests if either of you want. It shouldn't be too much trouble if I reuse the `create_test_notebook` infrastructure from `notebook.rs`.

Similarly, it looks like linting goes straight through `check_path`, where the target version resolution happens and is tested in the linter. But again, I'd be happy to add linter tests in the server crate if you want.

---

_@dhruvmanila approved on 2025-02-24 10:42_

Thank you for adding those test cases in the language server, they look good.

---

_Merged by @ntBre on 2025-02-24 13:47_

---

_Closed by @ntBre on 2025-02-24 13:47_

---

_Branch deleted on 2025-02-24 13:47_

---
