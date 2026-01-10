```yaml
number: 17222
title: "[red-knot] Fix `python` setting in mdtests, and rewrite a `site-packages` test as an mdtest"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/rewrite-test-2
created_at: 2025-04-05T16:04:25Z
updated_at: 2025-04-06T17:24:34Z
url: https://github.com/astral-sh/ruff/pull/17222
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Fix `python` setting in mdtests, and rewrite a `site-packages` test as an mdtest

---

_Pull request opened by @AlexWaygood on 2025-04-05 16:04_

## Summary

This PR does the following things:
- Fixes the `python` configuration setting for mdtest (added in https://github.com/astral-sh/ruff/pull/17221) so that it expects a path pointing to a venv's `sys.prefix` variable rather than the a path pointing to the venv's `site-packages` subdirectory. This brings the `python` setting in mdtest in sync with our CLI `--python` flag.
- Tweaks mdtest so that it automatically creates a valid `pyvenv.cfg` file for you if you don't specify one. This makes it much more ergonomic to write an mdtest with a custom `python` setting: red-knot will reject a `python` setting that points to a directory that doesn't have a `pyvenv.cfg` file in it
- Tweaks mdtest so that it doesn't check a custom `pyvenv.cfg` as Python source code if you _do_ add a custom `pyvenv.cfg` file for your mock virtual environment in an mdtest. (You get a lot of diagnostics about Python syntax errors in the `pyvenv.cfg` file, otherwise!)
- Rewrites the test added in https://github.com/astral-sh/ruff/pull/17178 as an mdtest, and deletes the original test that was added in that PR

## Test Plan

I verified that the new mdtest fails if I revert the changes to `resolver.rs` that were added in https://github.com/astral-sh/ruff/pull/17178


---

_Label `testing` added by @AlexWaygood on 2025-04-05 16:04_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-05 16:04_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-05 16:04_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-05 16:04_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-05 16:04_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-04-05 16:04_

---

_Comment by @github-actions[bot] on 2025-04-05 16:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:243 on 2025-04-05 17:06_

Hmm, GitHub doesn't allow me to add a suggestion. 

Can you move the `let python_version = test.configuration().python_version().unwrap_or_default();` out of the `if let Some(python_path) = python_path` and use the same version in the `ProgramSettings` initializer. Just to ensure that we use the same version.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:278 on 2025-04-05 17:07_

I only noticed this now. The `PythonCliFlag` variant isn't very accurate because the source is either `environment.python` or `--python`. But that's unrelated to this PR

---

_@MichaReiser approved on 2025-04-05 17:08_

Nice. This was a bit more work than expected. Thank you

---

_@AlexWaygood reviewed on 2025-04-06 11:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:271 on 2025-04-06 11:44_

this doesn't work on Windows, because on Windows the `site-packages` subdirectory is in a different path relative to `sys.prefix` (and red-knot knows this, which is what is causing the test to fail!). For the test to pass on Windows, this would need to be

````suggestion
`/src/.venv/Lib/site-packages/foo/__init__.py`:

```py
from .a import A as A
```

`/src/.venv/Lib/site-packages/foo/a.py`:

```py
class A: ...
```
````

(which is honestly more sane; I wish it were like this on non-Windows platforms too).

I'm wondering about adding a "magic path segment" like this, which mdtest would automatically replace with whatever the path to `site-package` is on the platform that the test is being run on, before it writes the file to the memory file system:

````suggestion
`/src/.venv/<path-to-site-packages>/foo/__init__.py`:

```py
from .a import A as A
```

`/src/.venv/<path-to-site-packages>/foo/a.py`:

```py
class A: ...
```
````

But this also feels like a certain amount of spiralling complexity. It might be best to say that tests which need to mock out `site-packages` should stay written in Rust rather than use mdtests?

---

_@AlexWaygood reviewed on 2025-04-06 12:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:271 on 2025-04-06 12:11_

I implemented the "magic path segment" solution (and documented it) in https://github.com/astral-sh/ruff/pull/17222/commits/33976270346528217fe59c9cd09db9f7c7b37bb5. It's not _too_ much added complexity, though I'm still not totally sure it's worth it. The alternative is just to close this PR, though (and maybe revert https://github.com/astral-sh/ruff/pull/17221), so I figured I'd push it to this PR so it can be reviewed. And it is nice to have as many tests as possible be mdtests.

---

_Converted to draft by @AlexWaygood on 2025-04-06 12:20_

---

_Marked ready for review by @AlexWaygood on 2025-04-06 12:33_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-04-06 12:33_

---

_@MichaReiser approved on 2025-04-06 17:18_

uff, this is annoying... but I remember that I ran into this before. 

I do like your solution and, thanks for documenting it. An alternative is to have platform-specific tests (only run them on UNIX), but it's not that we want one or the other. It's more that we might want both and use magic paths if we want to test some generic venv behavior and platform-specific tests if we want to test platform-specific venv behavior.

---

_Merged by @AlexWaygood on 2025-04-06 17:24_

---

_Closed by @AlexWaygood on 2025-04-06 17:24_

---

_Branch deleted on 2025-04-06 17:24_

---
