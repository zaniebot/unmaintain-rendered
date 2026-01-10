```yaml
number: 13598
title: "Write the path of the parent environment to an `extends-environment` key in the `pyvenv.cfg` file of an ephemeral environment"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - virtualenv
assignees: []
merged: true
base: main
head: alex/ephemeral-venv-parent
created_at: 2025-05-22T17:18:02Z
updated_at: 2025-05-27T12:46:30Z
url: https://github.com/astral-sh/uv/pull/13598
synced_at: 2026-01-10T11:10:41Z
```

# Write the path of the parent environment to an `extends-environment` key in the `pyvenv.cfg` file of an ephemeral environment

---

_Pull request opened by @AlexWaygood on 2025-05-22 17:18_

## Summary

Ephemeral environments created by `uv run --with` extend a parent (virtual or system) environment by adding a `.pth` file to the ephemeral environment's `site-packages` directory. The `pth` file contains Python code to dynamically add the parent environment's `site-packages` directory to Python's import search paths in addition to the ephemeral environment's `site-packages` directory. This works well at runtime, but is not currently understood by static analysis tools like ty.

As discussed in https://github.com/astral-sh/ty/issues/320, ty _could_ implement support for these virtual environments by attempting to parse the Python code uv is writing into these `.pth` files. This has many disadvantages however:
- It seems somewhat error-prone. The specific Python code uv is writing into the `.pth` files could change at any time -- and if another tool implements a similar ephemeral-environment feature, it could use different Python idioms.
- It would not be particularly efficient for ty to have to attempt to parse Python snippets in every `.pth` file in the `site-packages` directory (it could hurt our ability to effectively implement a persistent cache even if it doesn't cost that much in wall time, since we need to know the import search paths very early on).
- Philosophically, it feels out of spirit with the kind of analysis a static type checker should be expected to do.

Instead, this PR implements a feature where uv additionally writes the `sys.prefix` of the parent environment to a new `extends-environment` key of the ephemeral environment's `pyvenv.cfg` file. This should make it much easier for ty -- and potentially other static-analysis tools -- to statically and reliably understand the relationship between the two environments. If ty sees that a `pyvenv.cfg` file has a `uv` key in it and also has a `extends-environment` key in it, it will be able to easily add the `site-packages` directory of the parent environment to its resolved search paths as well as the `site-packages` directory of the ephemeral child environment.

A question was raised in https://github.com/astral-sh/ty/issues/320#issuecomment-2877865480 about whether it was standards-compliant to add new keys to `pyvenv.cfg` files created by uv. I think it is. `pyvenv.cfg` files are in general very under-specified: only a small handful of keys are currently consistent between virtual-environment creation tools. Different tools already often add bespoke keys to their `pyvenv.cfg` files -- see https://github.com/astral-sh/ty/issues/320#issuecomment-2877881334 for some comparisons between uv, the virtualenv Python library, and the standard-library `venv` module.

## Test Plan

I added a test that demonstrates that the `extends-environment` key is written with the correct value to the `pyvenv.cfg` file of an ephemeral environment.

---

_Label `virtualenv` added by @AlexWaygood on 2025-05-22 17:18_

---

_@konstin reviewed on 2025-05-22 17:20_

---

_Review comment by @konstin on `crates/uv/tests/it/run.rs`:1269 on 2025-05-22 17:20_

```suggestion
/// Test that an ephemeral environment writes the path of its parent environment to the `extends-environment` key
/// of its `pyvenv.cfg` file. This feature makes it easier for static-analysis tools like ty to resolve which import
/// search paths are available in these ephemeral environments.
```

---

_Comment by @zanieb on 2025-05-22 18:20_

As a note, this will suffer from the same issue as reported in https://github.com/astral-sh/uv/issues/12889 — but it won't be "worse".

---

_Comment by @AlexWaygood on 2025-05-22 18:25_

Argh, well, it seems I need some help with getting the venv directory filtering to work on Windows. (I haven't actually changed anything to do with the filtering there -- not sure why it's not working on this snapshot?)

Other than that, this is ready for review!

---

_Marked ready for review by @AlexWaygood on 2025-05-22 18:25_

---

_Comment by @zanieb on 2025-05-22 18:30_

> (I haven't actually changed anything to do with the filtering there -- not sure why it's not working on this snapshot?)

I don't think we've ever filtered `escape_for_python` paths (which I guess differ on Windows).




---

_Comment by @AlexWaygood on 2025-05-22 20:22_

Well, the test is finally passing on all platforms... If there's a better way to write a test for this, LMK!

---

_@charliermarsh approved on 2025-05-27 12:41_

---

_Merged by @AlexWaygood on 2025-05-27 12:46_

---

_Closed by @AlexWaygood on 2025-05-27 12:46_

---

_Branch deleted on 2025-05-27 12:46_

---
