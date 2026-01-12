```yaml
number: 16917
title: "[red-knot] Discover local venv folder in cli"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: discovery-of-venv-folder
created_at: 2025-03-22T18:36:15Z
updated_at: 2025-03-29T00:11:10Z
url: https://github.com/astral-sh/ruff/pull/16917
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Discover local venv folder in cli

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes astral-sh/ty#171 

Code from 
https://github.com/astral-sh/uv/blob/bbf4f830b5b346d9bdba18b6bc319c9527e5e92c/crates/uv-python/src/virtualenv.rs#L124-L144

## Test Plan

Manual testing


---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-22 18:36_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-22 18:36_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-22 18:36_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-22 18:36_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-03-22 18:36_

---

_Comment by @github-actions[bot] on 2025-03-22 18:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @ntBre on 2025-03-22 19:13_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/options.rs`:116 on 2025-03-22 21:25_

I'm not sure if implementing the fallback here, at least how it's done now is the most ideal solution. I've two concerns:

1. This re-implements part of the discovery logic in `VirtualEnvironment::new`
2. It's now possible that Red Knot picks up a `.venv` for which `VirtualEnvironment::new` later fails (for example, because the `pyenv.cfg` is invalid). Red Knot will fail to start

I think my preferred behavior is that Red Knot ignores any errors (may log a warning or debug message) if a `.venv` directory exists but it isn't a valid venv. But I don't think Red Knot should fail to start in that case because it isn't an input explicitly set by the user.

One way to implement this is to have a `PythonPath::Discover` option where the fallback behavior is implemented in `SearchPaths::from_settings`

---

_@MichaReiser reviewed on 2025-03-22 21:25_

---

_@MatthewMckee4 reviewed on 2025-03-22 22:00_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_project/src/metadata/options.rs`:116 on 2025-03-22 22:00_

> It's now possible that Red Knot picks up a .venv for which VirtualEnvironment::new later fails (for example, because the pyenv.cfg is invalid). Red Knot will fail to start

Could this not happen already? It could pick up the env variable of an invalid .venv right?

---

_@MatthewMckee4 reviewed on 2025-03-22 22:19_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_project/src/metadata/options.rs`:116 on 2025-03-22 22:19_

Could we say we just don't error for site packages discovery for these?
```rs
SysPrefixPathOrigin::VirtualEnvVar | SysPrefixPathOrigin::LocalVenv
```

---

_@MichaReiser reviewed on 2025-03-23 08:00_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/options.rs`:116 on 2025-03-23 08:00_

> Could this not happen already? It could pick up the env variable of an invalid .venv right?

IMO, this is different because the user explicitly sets the `VIRTUAL_ENV` environment variable. They're explicitly telling Red Knot to use that folder.

> Could we say we just don't error for site packages discovery for these?

We could do that. But I wonder if it still leads to some more code duplication compared to deferring more to the resolver. 

---

_@MatthewMckee4 reviewed on 2025-03-23 13:06_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_project/src/metadata/options.rs`:116 on 2025-03-23 13:06_

Do you think its still bad to look for a venv there
```rs
                .or_else(PythonPath::from_local_venv)
```
Then in SearchPaths::from_settings, we allow an invalid venv, and fallback to no site packages?

---

_Review request for @carljm removed by @carljm on 2025-03-23 13:15_

---

_Renamed from "Discover local venv folder in cli" to "[red-knot] Discover local venv folder in cli" by @MatthewMckee4 on 2025-03-23 13:18_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-03-23 13:34_

---

_Comment by @MichaReiser on 2025-03-25 20:14_

Sorry, for being slow to respond. We're having an on-site and have very limited time on GitHub. I'll come back to your question no later than next week; I just don't know when. Sorry for that.

---

_Comment by @MatthewMckee4 on 2025-03-25 20:52_

All good, no worries at all, thank you.

---

_@MichaReiser reviewed on 2025-03-27 16:09_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/options.rs`:116 on 2025-03-27 16:09_

I don't think it's bad in itself. It even has some advantages from a persistent caching perspective (long-term) because we'd calculate a different cache key when running red knot when the `.venv` folder exists vs when it isn't. 

However, I do wonder if we could remove some code duplication (the code roughly repeats some of the discovery logic that we already have in resolver) by moving this logic entirely into the resolver. 

There's a last part to this feature, but I think it's okay to tackle it separately: We need to re-run the event discovery if the `.venv` folder gets deleted or created. 

---

_@MatthewMckee4 reviewed on 2025-03-27 16:20_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_project/src/metadata/options.rs`:116 on 2025-03-27 16:20_

Are you thinking of reverting to what we had before discovery of any venv:
```rs
            python_path: python
                .map(|python_path| {
                    PythonPath::SysPrefix(python_path.absolute(project_root, system))
                })
                .unwrap_or_else(|| PythonPath::KnownSitePackages(vec![])),
``` 

Then do all discovery of venvs inside SearchPathSettings, or in SearchPaths.from_settings?

---

_@MichaReiser reviewed on 2025-03-27 17:13_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/options.rs`:116 on 2025-03-27 17:13_

No, I'd introduce a new `PythonPath` variant because the resolver otherwise can't know that it should do the venv discovery (`KnownSitePackages` would then become testing only)

---

_@MatthewMckee4 reviewed on 2025-03-27 17:34_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_project/src/metadata/options.rs`:116 on 2025-03-27 17:34_

Okay I think understand, I'll have a go at this 

---

_@MatthewMckee4 reviewed on 2025-03-27 19:10_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_project/src/metadata/options.rs`:116 on 2025-03-27 19:10_

How does that look?

---

_@MatthewMckee4 reviewed on 2025-03-27 19:18_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_project/src/metadata/options.rs`:116 on 2025-03-27 19:18_

Sorry yeah now `KnownSitePackages` is only used in tests.

What is `KnownSitePackages` currently used for anyway, if its only used with an empty vec. Seems there is no use right now other than a fallback 

---

_@MichaReiser reviewed on 2025-03-27 19:48_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:140 on 2025-03-27 19:48_

I'm interested into @AlexWaygood's opinion but I don't think we should test if the current (or any parent) directory is a venv. It's unclear to me why you'd want to run red knot **inside** a venv. 

Let's also add some `debug` logs when we pick up a venv (here or in the `::Discover` match arm so that users understand that Red Knot picked up a venv.

---

_@MichaReiser reviewed on 2025-03-27 19:49_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:274 on 2025-03-27 19:49_

Nit
```suggestion
                                "Ignoring automatically detected virtual environment at '{}': {}",
```

---

_@MatthewMckee4 reviewed on 2025-03-27 19:53_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:140 on 2025-03-27 19:53_

Okay sure, i just took this straight from uv, i can clean it up.

Ill add some debug logs first

---

_@MatthewMckee4 reviewed on 2025-03-27 20:23_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:140 on 2025-03-27 20:23_

Also, isn't checking if your in a venv pointless. If you are in a venv, then you will eventually get to the parent of the venv which will detect the venv. I'll remove this

---

_Merged by @MichaReiser on 2025-03-28 17:59_

---

_Closed by @MichaReiser on 2025-03-28 17:59_

---

_Branch deleted on 2025-03-29 00:11_

---
