```yaml
number: 18335
title: "[ty] Support ephemeral uv virtual environments"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/ephemeral-uv-envs
created_at: 2025-05-27T16:33:26Z
updated_at: 2025-05-28T14:55:00Z
url: https://github.com/astral-sh/ruff/pull/18335
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Support ephemeral uv virtual environments

---

_Pull request opened by @AlexWaygood on 2025-05-27 16:33_

## Summary

This PR implements support for the ephemeral environments uv creates when you use the `--with` argument with `uv run`. It will only work with a version of uv that includes https://github.com/astral-sh/uv/commit/7bba3d00d4ad1fb3daba86b98eb25d8d9e9836ae (currently you have to build uv with the `main` branch; it hasn't been included in a release yet).

Fixes https://github.com/astral-sh/ty/issues/320

## Test Plan

I added unit tests. I also manually tested the PR by doing the following:

1. I first built the main branch of uv using `cargo build --release` in my local uv clone.

2. I next checked that ty on the main branch was not able to resolve imports if it was invoked with `uv run --with=ty`. To do this, I:
   a. Applied this diff to the `main` branch of my local Ruff clone:

      ```diff
      diff --git a/pyproject.toml b/pyproject.toml
	  index 0b983b27a0..1bdf25692f 100644
	  --- a/pyproject.toml
	  +++ b/pyproject.toml
	  @@ -3,7 +3,7 @@ requires = ["maturin>=1.0,<2.0"]
	   build-backend = "maturin"
	  
	   [project]
	  -name = "ruff"
	  +name = "ty"
	   version = "0.11.11"
	   description = "An extremely fast Python linter and code formatter, written in Rust."
	   authors = [{ name = "Astral Software Inc.", email = "hey@astral.sh" }]
	  @@ -45,8 +45,8 @@ Changelog = "https://github.com/astral-sh/ruff/blob/main/CHANGELOG.md"
	  
	   [tool.maturin]
	   bindings = "bin"
	  -manifest-path = "crates/ruff/Cargo.toml"
	  -module-name = "ruff"
	  +manifest-path = "crates/ty/Cargo.toml"
	  +module-name = "ty"
	   python-source = "python"
	   strip = true
	   exclude = [
      ```

   b. `cd`'d into the local clone of a Python project with third-party dependencies (https://github.com/AlexWaygood/typeshed-stats)
   c. Ran `../uv/target/release/uv run --force-reinstall --with="ty@../ruff" ty check`
   d. Observed that ty ran on my project, but was not able to resolve any third-party dependencies, and reported 104 diagnostics

3. I next checked that this PR branch fixed the issue. I:
   a. Checked out this PR branch
   b. Applied the same `pyproject.toml` diff to this branch as I applied to the `main` banch in step (2)
   c. `cd`'d into my `typeshed-stats` again
   d. Again ran `../uv/target/release/uv run --force-reinstall --with="ty@../ruff" ty check`
   e. Observed that ty was able to resolve all third-party dependencies, and reported only 11 diagnostics


---

_Label `ty` added by @AlexWaygood on 2025-05-27 16:33_

---

_Comment by @github-actions[bot] on 2025-05-27 17:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-05-27 17:49_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-27 17:49_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-27 17:49_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-27 17:49_

---

_@MichaReiser reviewed on 2025-05-27 17:52_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:7 on 2025-05-27 17:52_

I haven't had time yet to review this PR but can you explain the motivation for using an `IndexSet`? Could we use `deduplicate_nested_paths`?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:7 on 2025-05-27 17:58_

Just to ensure that we didn't end up with duplicate `site-packages` paths being added if a virtual environment (somehow?) ends up being marked as a `--system-site-packages` path and also as having a parent environment, and they end up pointing to the same system installation. I don't know how we realistically end up in that situation; I was just being defensive.

> Could we use `deduplicate_nested_paths`?

I wasn't aware of this function! Do you think it offers significant advantages here, given that we will need to call `.collect()` on it anyway at some point (and therefore allocating memory is inevitable)?

One thing I do quite like about using an ordered set here is that the type itself guarantees that there will be no duplicates; it makes it easy for readers of the code to understand that invariant.

---

_@AlexWaygood reviewed on 2025-05-27 17:58_

---

_@carljm approved on 2025-05-27 18:06_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:7 on 2025-05-28 06:35_

I don't think there's a benefit of `deduplicate_nested_paths` over an `IndexSet` when the only goal is to remove exact duplicates. `deduplicate_nested_paths` does more. It eliminates entries that overlap:

* `/src/typeshed`
* `/src`

gets simplified to 

* `/src`

---

_@MichaReiser reviewed on 2025-05-28 06:35_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:352 on 2025-05-28 06:40_

Unrelated to your PR but we should make this proper diagnostics and not just tracing warnings (that are easy to miss in the LSP)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:1435 on 2025-05-28 06:41_

Could this have been an mdtest? Or is there still a feature that mdtests don't support that you need here?

---

_@MichaReiser approved on 2025-05-28 06:41_

---

_@AlexWaygood reviewed on 2025-05-28 06:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:7 on 2025-05-28 06:44_

That makes sense. I think it would be very strange to have one site-packages path that's a sub-directory of another, though — exact duplicates seem more plausible, but even they're pretty unlikely. I'll add a comment for why I'm using an IndexSet here.

---

_@AlexWaygood reviewed on 2025-05-28 11:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:352 on 2025-05-28 11:48_

I'll open a followup issue for this!

---

_@AlexWaygood reviewed on 2025-05-28 13:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:1435 on 2025-05-28 13:56_

It did require new features in mdtest 🙃 But I have now implemented said features in mdtest for this PR, and have converted this to be an mdtest.

---

_Merged by @AlexWaygood on 2025-05-28 14:54_

---

_Closed by @AlexWaygood on 2025-05-28 14:54_

---

_Branch deleted on 2025-05-28 14:55_

---
