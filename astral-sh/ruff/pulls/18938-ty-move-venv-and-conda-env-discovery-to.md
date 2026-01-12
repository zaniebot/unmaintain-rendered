```yaml
number: 18938
title: "[ty] Move venv and conda env discovery to `SearchPath::from_settings`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/python-discovery-search-paths
created_at: 2025-06-25T16:04:04Z
updated_at: 2025-06-26T14:46:02Z
url: https://github.com/astral-sh/ruff/pull/18938
synced_at: 2026-01-12T15:56:28Z
```

# [ty] Move venv and conda env discovery to `SearchPath::from_settings`

---

_@MichaReiser_

## Summary
This PR centralizes the auto discovery logic for a Python environment into `SearchPaths::from_settings`. 

This should make https://github.com/astral-sh/ty/issues/611 an easy change (manly prioritizing `.venv` over `CONDA_PREFIX`).

## Test Plan

Existing tests


---

_Label `internal` added by @MichaReiser on 2025-06-25 16:04_

---

_Label `ty` added by @MichaReiser on 2025-06-25 16:04_

---

_@MichaReiser reviewed on 2025-06-25 16:04_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/ranged_value.rs`:1 on 2025-06-25 16:04_

I extracted this from `ty_project/metadata/value` I didn't change the code other than adding the right feature gating and adjusting imports.

---

_Comment by @github-actions[bot] on 2025-06-25 16:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-06-25 16:14_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fpython-discovery-search-paths?runnerMode=WallTime)

### Merging #18938 will **not alter performance**

<sub>Comparing <code>micha/python-discovery-search-paths</code> (77242e7) with <code>main</code> (d04e63a)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Marked ready for review by @MichaReiser on 2025-06-25 16:33_

---

_Review requested from @carljm by @MichaReiser on 2025-06-25 16:33_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-25 16:33_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-25 16:33_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-25 16:33_

---

_Comment by @github-actions[bot] on 2025-06-25 16:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-06-25 16:50_

Hmm, not sure what's up with the benchmark. The auto discovery isn't run for benchmarks (it's a fixed environment) and the program settings are resolved outside the benchmark

I verified that both main and this PR perform the same search path discovery in the benchmark... 

Edit: It seems the benchmark triggers on other PRs too. So maybe just a flake? 

---

_@AlexWaygood reviewed on 2025-06-25 17:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:321 on 2025-06-25 17:50_

I'm not sure I understand the distinction between "explicit configuration" and "auto discovery" that this PR introduces. If I've explicitly activated an environment, or I've invoked ty via `uv run`, I'll have a strong expectation that ty is going to use that environment. It would be very surprising to me if ty silently fell back to some other environment because it failed to parse the `pyvenv.cfg` file in my activated virtual environment. I'd much prefer a hard error in this situation as an end user that tells me what the problem is, rather than lots of confusing diagnostics about unresolved imports. That's the behaviour we have on `main`, and I think it's good behaviour.

The only situation where I think we maybe want to do something like this (where we log the error and continue down the chain) is implicit discovery of unactivated virtual environments in local `.venv` directories. If the user hasn't explicitly activated the virtual environment, then we don't even know for sure that they actually want us to use that environment, so in _that_ situation I agree that it might make sense to just log the error and continue. But I think an explicitly activated virtual or conda environment is pretty different; I wouldn't classify that under "auto-discovery".

---

_Review request for @carljm removed by @carljm on 2025-06-25 19:58_

---

_@MichaReiser reviewed on 2025-06-25 20:19_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:321 on 2025-06-25 20:19_

Thanks for catching this. This wasn't an intentional behavior change. Or more, I thought I correctly mapped the existing `or_else` chain but I didn't notice that it maps over an `Option` and not a `Result` chain and I probably got mislead by how we handle the `.venv` case. I'll change this tomorrow. This should also simplify the implementation a bit.

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-26 06:26_

---

_Review request for @sharkdp removed by @sharkdp on 2025-06-26 07:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/program.rs`:210 on 2025-06-26 10:26_

The documentation here implies to me that this variant would only be used for the case where neither a virtual environment nor a conda environment has been activated but we find a `.venv` directory relative to the project root. But that doesn't seem to be the case. It appears to also be used for the case where we read the `sys.prefix` value from the `VIRTUAL_ENV` or `CONDA_PREFIX` variables. (It's sort of debatable whether reading environment variables that have either been explicitly set by the user or set as part of e.g. a `uv run` invocation really counts as "automatic" detection -- you could argue it's just as explicit configuration as a CLI flag or a pyproject.toml setting?)

I'm not totally sold that adding this new variant is necessary. But if we do add it, we need to rename the `IntoSysPrefix` variant, because the `VIRTUAL_ENV` and `CONDA_PREFIX` environment variables point directly to the `sys.prefix` path (in fact, they're the only `SysPrefixPathOrigin` variants that we can reliably count on _always_ pointing directly to the `sys.prefix` path!).

---

_Review comment by @AlexWaygood on `crates/ty_test/src/lib.rs`:277 on 2025-06-26 10:29_

is the `sys_prefix` method useful if this is all it does? 😆

---

_@AlexWaygood reviewed on 2025-06-26 10:31_

Thanks! This looks basically good. I still feel like there's some confusing distinctions being introduced that I'm not totally sold on.

It would also be ideal if we could add a regression test for https://github.com/astral-sh/ruff/pull/18938#discussion_r2167288287, but it was obviously a pre-existing issue that we had no coverage there :-)

---

_Review comment by @MichaReiser on `crates/ty_test/src/lib.rs`:277 on 2025-06-26 10:49_

It wasn't introduced by me!

---

_@MichaReiser reviewed on 2025-06-26 10:49_

---

_@AlexWaygood reviewed on 2025-06-26 10:52_

---

_Review comment by @AlexWaygood on `crates/ty_test/src/lib.rs`:277 on 2025-06-26 10:52_

I know! 😆

---

_@MichaReiser reviewed on 2025-06-26 10:53_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:210 on 2025-06-26 10:53_

> The documentation here implies to me that this variant would only be used for the case where neither a virtual environment nor a conda environment has been activated but we find


I'm not sure where the comment implies this. All it says is that it will to an automated discovery but it's not specific about what that means (we could add it but we then end up repeating the same documentation in even more places).

> I'm not totally sold that adding this new variant is necessary.

What alternative would you suggest? E.g. what should we specify in `to_search_path_settings` when no python path was specified in the Options. We can't use `IntoSysPrefix` because we don't have a virtual env path... 

> I'm not totally sold that adding this new variant is necessary. But if we do add it, we need to rename the IntoSysPrefix variant, because the VIRTUAL_ENV and CONDA_PREFIX environment variables point directly to the sys.prefix path (in fact, they're the only SysPrefixPathOrigin variants that we can reliably count on always pointing directly to the sys.prefix path!).

I had a version that used `Option<RangedValue<SystemPathBuf>>` for the `SysPrefix` variant but it resulted in a large refactor. So I reverted the change. That means it's now still possible to construct `IntoSysPrefix` with a `SysPrefixPathOrigin::CondaPrefix` even if we aren't constructing this anyway. I think that's fine. But regardless, I'm not sure how the environment variables influence the naming here because a) a caller has to resolve them in which case they create an `IntoSysPrefix` or the variable is resoloved in `SearchPaths::from_settings`, in which case it isn't relevant for the enum variants here.

---

_@MichaReiser reviewed on 2025-06-26 10:58_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:210 on 2025-06-26 10:58_

Overall, all this enum does is distinguish between 3 cases:

* Use a known list of search paths (testing only)
* Use a specific path that can be converted to a sys prefix but it's up to the caller to construct it
* Use ty's heuristics to find the python path

---

_@AlexWaygood reviewed on 2025-06-26 11:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/program.rs`:210 on 2025-06-26 11:19_

> All it says is that it will to an automated discovery

Right, but why is reading configuration from an environment variable the user has set more "automated" than reading configuration from a pyproject.toml file? It feels just as explicit to me; the `VIRTUAL_ENV` variable is the conventional way in Python that you tell the language and all tooling which environment you're currently using. It doesn't feel like much of a heuristic to me; it feels pretty distinct to searching around on the filesystem for an unactivated virtual environment the user might have hanging around locally!

> What alternative would you suggest?

I guess I sort-of like the current design where we convert `PythonPath` quite eagerly to `SysPrefixPathOrigin` in `options.rs`. I think https://github.com/astral-sh/ty/issues/611 should be pretty fixable even with that structure: we can do a quick check upfront to see if `.venv/pyvenv.cfg` is a file on disk, and if so, assume the `.venv` directory is a virtual environment and use it; if not, fallback to the default conda environment. I believe basically the validation [uv](https://github.com/astral-sh/uv/pull/14214/files) does to see if something should be treated as a virtual environment is just to check whether a `./pyvenv.cfg` file exists relative to that directory, and if so, it runs with it.

---

_@MichaReiser reviewed on 2025-06-26 13:20_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:210 on 2025-06-26 13:20_

We discussed this offline. The main issue are the variant names. I'll try to refine them but we both struggled to come up with good names

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:210 on 2025-06-26 13:39_

I did some renaming. Let me know if this is better.

---

_@MichaReiser reviewed on 2025-06-26 13:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/program.rs`:209 on 2025-06-26 13:44_

```suggestion
    /// The path to the Python environment isn't known. Try to discover the Python environment
```

---

_@AlexWaygood approved on 2025-06-26 13:45_

Thank you! The new names make it much clearer

---

_Comment by @MichaReiser on 2025-06-26 14:22_

> Thank you! The new names make it much clearer

It's a shame that I'll delete the entire enum in my next PR :D

---

_Merged by @MichaReiser on 2025-06-26 14:39_

---

_Closed by @MichaReiser on 2025-06-26 14:39_

---

_Branch deleted on 2025-06-26 14:39_

---
