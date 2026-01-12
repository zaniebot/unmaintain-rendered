```yaml
number: 16347
title: "Rename `venv-path` to `python`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/rename-venv-path-to-python
created_at: 2025-02-24T14:10:55Z
updated_at: 2025-02-24T18:41:10Z
url: https://github.com/astral-sh/ruff/pull/16347
synced_at: 2026-01-12T15:55:54Z
```

# Rename `venv-path` to `python`

---

_@MichaReiser_

## Summary

Closes https://github.com/astral-sh/ruff/issues/15530

This PR renames the CLI argument `--venv-path` and the `environment.venv-path` option to `--python` and `environment.python` 
to align with uv's python option. 

The Python option takes a path to a virtual environment or Python system installation (any directory that has a `lib/python3.x/site-packages` subdirectory on unix). 

This PR doesn't yet add support for:

* Specifying a path to a python executable
* Specifying a python version

uv supports both (and more). The CLI proposal does mention that we should support python executables. I recommend to add support for this, if wanted, as a separate PR


## Test plan

I ran Red Knot over Black and both main and this PR produce the same diagnostic when pointing it to a venv path.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:231 on 2025-02-24 14:12_

I think we have to adjust the code here as well to support system python installations. I don't have a good sense for what's involved here. It might just be that we have to make the `pyvenv.cfg` step optional. I can tackle this with some guidance as part of this PR or create a follow up issue to add system python support.

---

_Label `red-knot` added by @AlexWaygood on 2025-02-24 14:22_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:96 on 2025-02-24 14:28_

I noticed while testing that relative path arguments on the CLI were incorrectly anchored to the project directory instead of the cwd. For example, the `python` path for `--project ../test --python ../test/venv` resolved to `../test/test/venv`. 



---

_Marked ready for review by @MichaReiser on 2025-02-24 14:28_

---

_Review requested from @carljm by @MichaReiser on 2025-02-24 14:28_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-24 14:28_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-24 14:28_

---

_@MichaReiser reviewed on 2025-02-24 14:40_

---

_@sharkdp approved on 2025-02-24 14:42_

Thank you.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:231 on 2025-02-24 14:47_

I think probably best to defer this -- it seems like a separate issue

---

_@AlexWaygood reviewed on 2025-02-24 14:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/program.rs`:148 on 2025-02-24 14:49_

nit: maybe call this variant `KnownSitePackages` now, since the meaning of the wrapped path is different to the meaning of the wrapped path in the other variant?

And maybe document that this variant is only really for tests, where we want to skip having to resolve the `site-packages` from the `sys.prefix` path?

---

_@AlexWaygood approved on 2025-02-24 14:50_

Looks great, thank you!

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:71 on 2025-02-24 17:50_

This comment seems out of date (or at least badly located) now?

---

_@carljm approved on 2025-02-24 18:16_

---

_Comment by @carljm on 2025-02-24 18:19_

> any directory that has a `site-packages` sub-directory

This is from the PR summary, not the PR itself, but in case it represents an actual misunderstanding: `site-packages` is not a direct subdirectory of venvs or of system Python installations, it always lives under `lib/pythonX.X/site-packages`. (Or maybe you meant "sub-directory" in the transitive sense?)

---

_@MichaReiser reviewed on 2025-02-24 18:30_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:71 on 2025-02-24 18:30_

This is still true. We relativize the paths in `RelativePathBuf::absolute`. The main change here is that we don't use the project path as `cwd` when it is set (which is incorrect)

---

_Comment by @MichaReiser on 2025-02-24 18:33_

> This is from the PR summary, not the PR itself, but in case it represents an actual misunderstanding: site-packages is not a direct subdirectory of venvs or of system Python installations, it always lives under lib/pythonX.X/site-packages. (Or maybe you meant "sub-directory" in the transitive sense?

You give me too much credit. I was just still confused by how the directory structure looks.

---

_Merged by @MichaReiser on 2025-02-24 18:41_

---

_Closed by @MichaReiser on 2025-02-24 18:41_

---

_Branch deleted on 2025-02-24 18:41_

---
