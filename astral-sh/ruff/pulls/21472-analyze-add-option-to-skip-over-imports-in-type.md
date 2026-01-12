```yaml
number: 21472
title: "`analyze`: Add option to skip over imports in `TYPE_CHECKING` blocks"
type: pull_request
state: merged
author: gauthsvenkat
labels:
  - analyze
assignees: []
merged: true
base: main
head: feat/ruff-graph-type-checking-imports
created_at: 2025-11-15T13:03:26Z
updated_at: 2025-11-29T08:37:20Z
url: https://github.com/astral-sh/ruff/pull/21472
synced_at: 2026-01-12T15:57:25Z
```

# `analyze`: Add option to skip over imports in `TYPE_CHECKING` blocks

---

_@gauthsvenkat_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

`ruff analyze graph` naively detects all imports. This MR adds a feature where `TYPE_CHECKING` imports can be detected and optionally excluded (using a CLI flag). 

Closes: #20736 

### Main changes

- [collector.rs] `CollectedImport` is now a `struct` instead of an `enum`. This was done so that a new `type_checking` field could be added.
- [collector.rs] Refactored the logic in `Stmt::Import{,From}` into helper functions `collect_import{,_from}`. Done so they can be reused in `Stmt::If` to detect type checking imports.
- [collector.rs] Added a new helper function `is_type_checking_condition`. This can detect very simple type-checking conditions, but, practically, it would cover a lot of the cases.

## Test Plan

- Added a new test to check that type-checking imports are detected and filtered, iff `--exclude-type-checking-imports` flag is set.

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2025-11-15 13:10_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @gauthsvenkat on 2025-11-15 13:12_

Pipeline fails cause Clippy is complaining there are too many bools in `AnalyzeGraphCommand`. But it is a CLI option, so not sure how to deal with it.

---

_Label `analyze` added by @AlexWaygood on 2025-11-15 15:22_

---

_Renamed from "[ruff] Option to detect (and exclude) `TYPE_CHECKING` imports in `ruff analyze graph`" to "`analyze`: Add option to skip over imports in `TYPE_CHECKING` blocks" by @MichaReiser on 2025-11-16 07:22_

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/collector.rs`:103 on 2025-11-16 07:29_

I think a simpler approach is to pass the `exclude_type_checking` option to `Collector` and simply skip the entire body if `test` is a `TYPE_CHECKING` condition. 

This removes the need to split `collect_import_from` and `collect_import` out of `visit_stmt` or that we need to set `type_checking` on `CollectedImport`

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:198 on 2025-11-16 07:31_

I think I'd name this `type_checking_only_imports` or just `type_checking_imports` where `true` includes them and `false` skips them (should default to `true`).


We should also add this as a configuration optiont to https://github.com/astral-sh/ruff/blob/6e4b15b6ca72285a2f509076f3871adca90e923a/crates/ruff_workspace/src/options.rs#L3816

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:196 on 2025-11-16 07:32_

You can add `#[expect(clippy::struct_excessive_bools)]` to the struct definition to silence the excessive bool lint

---

_@MichaReiser requested changes on 2025-11-16 07:32_

Thank you. I left a few inline comments with a few suggestions

---

_@gauthsvenkat reviewed on 2025-11-16 07:40_

---

_Review comment by @gauthsvenkat on `crates/ruff_graph/src/collector.rs`:103 on 2025-11-16 07:40_

Oh yeah, that would be simpler. 

Then we don't track which imports are type-checking only for other uses. e.g., if we want to show only type-checking imports? 
Is that okay?  

---

_@MichaReiser reviewed on 2025-11-16 07:47_

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/collector.rs`:103 on 2025-11-16 07:47_

I think that's okay for now, given that we don't show that information anywhere. 


---

_@gauthsvenkat reviewed on 2025-11-16 11:00_

---

_Review comment by @gauthsvenkat on `crates/ruff/src/args.rs`:198 on 2025-11-16 11:00_

I've done both. 

Adding it as a config option ended up being a bigger change than I expected. I tried to follow how it was done for `detect-string-imports`. Please let me know if that is good. 

---

_Review requested from @MichaReiser by @gauthsvenkat on 2025-11-16 11:02_

---

_@gauthsvenkat reviewed on 2025-11-16 11:29_

---

_Review comment by @gauthsvenkat on `crates/ruff/src/args.rs`:198 on 2025-11-16 11:29_

I also just noticed that `preview` and `string_imports` config options are not boolean, but instead "enabled" or "disabled". Should I model it that way instead?

---

_@MichaReiser reviewed on 2025-11-16 12:28_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:198 on 2025-11-16 12:28_

Yeah, it requires a lot of boilerplate code. 

> t preview and string_imports config options are not boolean, but instead "enabled" or "disabled". Should I model it that way instead?

I think a boolean is fine.

---

_@MichaReiser reviewed on 2025-11-16 12:29_

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/collector.rs`:110 on 2025-11-16 12:29_

We could consider handling 

```py
if !TYPE_CHECKING:
	...
else: 
	...
```

and 

```py
if condition:
	...
elif TYPE_CHECKING:
	...
```

but we can do this as a separate PR

---

_@MichaReiser approved on 2025-11-16 12:29_

Thank you

---

_Merged by @MichaReiser on 2025-11-16 12:30_

---

_Closed by @MichaReiser on 2025-11-16 12:30_

---

_Branch deleted on 2025-11-29 08:37_

---
