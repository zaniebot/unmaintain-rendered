```yaml
number: 20441
title: "[ty] Search PYTHONPATH to find modules"
type: pull_request
state: merged
author: mmlb
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: push-tyvxlwmmrvxu
created_at: 2025-09-16T20:07:16Z
updated_at: 2025-09-22T13:47:10Z
url: https://github.com/astral-sh/ruff/pull/20441
synced_at: 2026-01-12T15:57:02Z
```

# [ty] Search PYTHONPATH to find modules

---

_@mmlb_

This brings ty in line with other type checkers and python interpreters, including on by default and no options to disable. Can be disabled by clearing the PYTHONPATH environment variable.

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Prepend PYTHONPATH to extra_paths so ty behaves similar to other type checkers and the interpreters. Fixes https://github.com/astral-sh/ty/issues/547

## Test Plan

I built ty locally and ran it in nix-shell environment that sets PYTHONPATH, without this PR ty errors with `unresolved-attribute`.


---

_Review requested from @carljm by @mmlb on 2025-09-16 20:07_

---

_Review requested from @MichaReiser by @mmlb on 2025-09-16 20:07_

---

_Review requested from @sharkdp by @mmlb on 2025-09-16 20:07_

---

_Review requested from @dcreager by @mmlb on 2025-09-16 20:07_

---

_Comment by @mmlb on 2025-09-16 20:10_

This is modification of #20016, I dropped the namespace package logic and opt-in as requested. Instead of opt-out via flag I went with opt-out by clearing PYTHONPATH as I think that makes sense but would be happy to add an explicit cli arg. Thanks to @natelust for the initial code/PR.

---

_Renamed from "Teach TY to search PYTHONPATH to find modules" to "[ty] Search PYTHONPATH to find modules" by @mmlb on 2025-09-16 20:31_

---

_Label `ty` added by @ntBre on 2025-09-16 20:35_

---

_@carljm reviewed on 2025-09-16 20:48_

This looks reasonable to me. Not sure if it's feasible to test this, or if we should have a CLI flag to disable reading `PYTHONPATH` (just unsetting `PYTHONPATH` does seem like a reasonable alternative.). Would prefer for @MichaReiser or @AlexWaygood to review.

---

_Comment by @github-actions[bot] on 2025-09-16 20:51_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-16 20:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @natelust on 2025-09-16 20:54_

Thanks for picking this up, I just have been underwater dealing with things lately, I really appreciate it.

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:287 on 2025-09-17 09:51_

As reported by clippy, we should use `system::env_var` here instead. This ensures proper test isolation.

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:318 on 2025-09-17 09:54_

```suggestion
                .filter_map(|str_path| {
                    let possible_path = SystemPathBuf::from(str_path);
                    system.path_exists(&possible_path).then_some(possible_path)
                })
```

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:318 on 2025-09-17 09:56_

Same here, we should use `system.path_exists` over `path.exists` to properly support in-memory file systems and WASM.  Or even better, use `is_directory` because adding a file would later fail

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:298 on 2025-09-17 10:01_

It's not really necessary to collect before calling `extend`. You can pass the iterator directly. However, as mentioned by @carljm on the issue, we should inform users that ty's adding paths from `PYTHONPATH`.

That's why I would rewrite this as:

```rust
for path in python_path.split(':') {
	let possible_path = SystemPathBuf::from(str_path);

	if system.path_exists(&possible_path) {
		tracing::debug!("Adding `{possible_path}` from the `PYTHONPATH` environment variable to `extra_paths`);
		extra_paths.push(possible_path);
	} else {
		tracing::debug!("Skipping `{possible_path}` listed in `PYTHONPATH` because the path doesn't exist)"
	}
}
```

We may even want 

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:299 on 2025-09-17 10:01_

I think we should only add `PYTHONPATH` if the user didn't explicitly set `extra_paths` (when `extra_paths` is `None`). 

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:331 on 2025-09-17 10:02_

@AlexWaygood any thoughts on whether we should move this logic into `SearchPaths::from_settings`? 

---

_@MichaReiser requested changes on 2025-09-17 10:03_

Thank you. I left a few comments. We should also add a CLI test for this in https://github.com/astral-sh/ruff/blob/1d2128f918a2315a5fc81042b5417f9d8cf34282/crates/ty/tests/cli/python_environment.rs#L9



---

_Label `configuration` added by @MichaReiser on 2025-09-17 10:03_

---

_Review request for @sharkdp removed by @sharkdp on 2025-09-17 10:21_

---

_@carljm reviewed on 2025-09-17 14:04_

---

_Review comment by @carljm on `crates/ty_project/src/metadata/options.rs`:299 on 2025-09-17 14:04_

I'm not sure about this. I think I would find it surprising for extra paths option on CLI to override `PYTHONPATH` entirely, since they aren't named the same. I think if we want a way to disable respecting `PYTHONPATH` it should be its own separate option.  There may be use cases for respecting `PYTHONPATH` and setting unrelated additional extra paths in config or CLI. 

---

_@mmlb reviewed on 2025-09-17 14:09_

---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:287 on 2025-09-17 14:09_

fixed

---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:318 on 2025-09-17 14:10_

fixed, also converted to `is_directory`

---

_@mmlb reviewed on 2025-09-17 14:10_

---

_@mmlb reviewed on 2025-09-17 14:11_

---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:298 on 2025-09-17 14:11_

you cut off there at the end, but I did grab from your re-write.

---

_@mmlb reviewed on 2025-09-17 14:12_

---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:299 on 2025-09-17 14:12_

fixed, I also added a log message for when both --extra-paths and PYTHONPATH are set

---

_Comment by @mmlb on 2025-09-17 14:16_

> Thank you. I left a few comments. We should also add a CLI test for this in
> 
> https://github.com/astral-sh/ruff/blob/1d2128f918a2315a5fc81042b5417f9d8cf34282/crates/ty/tests/cli/python_environment.rs#L9

I'll take a look at this and give it a go.

---

_@AlexWaygood reviewed on 2025-09-17 14:39_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:299 on 2025-09-17 14:39_

Hmm, I agree with @carljm though -- I'm not sure I understand why we should ignore `PYTHONPATH` just because `extra-_paths` has been set

---

_@AlexWaygood reviewed on 2025-09-17 14:42_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:331 on 2025-09-17 14:42_

No strong opinion really!

---

_@mmlb reviewed on 2025-09-17 15:02_

---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:299 on 2025-09-17 15:02_

Hmm yeah I can see that. I was going off of cli-args > env vars but it can be confusing if the names don't match. I think the intention was that PYTHONPATH and --extra-paths both set extra_paths. I can see it either way, though I think I do prefer avoiding the implentation leakage and instead just allow both PYTHONPATH and --extra-paths, the `extra` part is pretty telling.

---

_@carljm reviewed on 2025-09-17 15:04_

---

_Review comment by @carljm on `crates/ty_project/src/metadata/options.rs`:299 on 2025-09-17 15:04_

Yes -- for me, using `extra-paths` internally to implement respecting `PYTHONPATH` is an implementation detail, just because the behavior of the paths happens to be the same. It doesn't mean that `PYTHONPATH` is simply an alias of the extra-paths configuration; they are separate features.

---

_@mmlb reviewed on 2025-09-17 15:06_

---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:299 on 2025-09-17 15:06_

I think I'll go back to respecting both PYTHONPATH and --extra-paths, PYTHONPATH first --extra-paths second. I want to continue avoiding adding an argument to ignore PYTHONPATH, I think it'll be confusing and you can wait until someone shows a need for it.

---

_@mmlb reviewed on 2025-09-17 15:40_

---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:299 on 2025-09-17 15:40_

Back to taking both PYTHONPATH and --extra-paths.

---

_@MichaReiser reviewed on 2025-09-17 15:47_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:313 on 2025-09-17 15:47_

Let's make `possible_path` absolute here (or on line 289), similar to what we do on line 310. 

---

_Review comment by @mmlb on `crates/ty_project/src/metadata/options.rs`:313 on 2025-09-17 16:24_

done

---

_@mmlb reviewed on 2025-09-17 16:24_

---

_Comment by @MichaReiser on 2025-09-18 08:24_

Thank you. This looks good now and we can merge it once there's a CLI test. 

---

_Comment by @mmlb on 2025-09-19 21:28_

> Thank you. This looks good now and we can merge it once there's a CLI test.

Done!

---

_Review requested from @MichaReiser by @mmlb on 2025-09-19 21:28_

---

_@MichaReiser approved on 2025-09-20 11:15_

Perfect, thank you

---

_Merged by @MichaReiser on 2025-09-20 11:40_

---

_Closed by @MichaReiser on 2025-09-20 11:40_

---

_Branch deleted on 2025-09-22 13:47_

---
