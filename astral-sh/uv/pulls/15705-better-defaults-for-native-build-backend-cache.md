```yaml
number: 15705
title: Better defaults for native build backend cache keys
type: pull_request
state: merged
author: rimathia
labels:
  - enhancement
assignees: []
merged: true
base: main
head: scikit-init-suggestion
created_at: 2025-09-06T10:20:58Z
updated_at: 2025-09-12T13:10:37Z
url: https://github.com/astral-sh/uv/pull/15705
synced_at: 2026-01-12T16:11:53Z
```

# Better defaults for native build backend cache keys

---

_@rimathia_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This change extends the default initialized projects with scikit and maturin build backends to ensure that the rust or C++ build gets invoked when a source file changes.

## Test Plan

`cargo run init cppextension-test --build-backend=scikit` followed by manual testing of the behaviour of the resulting project under `uv run --with jupyter jupyter lab` and changes to the source file of the extension.
Analogous for `cargo run init maturin-test --build-backend=maturin `.

## Relevant Issues

The question of why the python extension is not rebuilt on source changes has been discussed in https://github.com/astral-sh/uv/issues/15701#issuecomment-3258714942.


---

_Comment by @rimathia on 2025-09-06 10:24_

It is of course a judgement call whether the rebuild on rust or C++ source file changes is more likely to meet developer expectations than the rebuild on `--reinstall`. Just decline the PR if you disagree with my premise.

---

_Comment by @zanieb on 2025-09-08 13:24_

cc @charliermarsh / @konstin 

I don't have strong feelings here.

---

_Comment by @konstin on 2025-09-08 13:26_

I think this is a good idea, but we should change the cache keys to something that captures all Rust/C/C++ source files, such as:

```toml
cache-keys = [{ file = "pyproject.toml" }, { file = "src/**/*.rs" }, { file = "Cargo.toml" }, { file = "Cargo.lock" }]
```

---

_Comment by @rimathia on 2025-09-08 19:02_

Thanks for the suggestion, I was deterred by the documentation mentioning that glob patterns can be costly to check, but in a source directory it definitely makes sense. Unfortunately, C++ doesn't have a strong filename convention, so I just went with {cpp,h}, hoping that people will become aware of that line in the pyproject.toml before wondering why a change to their .hpp file doesn't trigger recompilation.

---

_Comment by @konstin on 2025-09-09 07:08_

We can do `{h,c,hpp,cpp}`. The costly part of the glob is traversing many files, the regex part of it is really fast. For each directory we look, we need to make a syscall, and for each syscall, there needs to be a random IO read. These are still fast for most source tree, but it's easy to misconfigure the glob and e.g. traverse an entire venv or data or cache directory with tenthousands of small files, where it becomes slow, hence the warning.

---

_Label `enhancement` added by @konstin on 2025-09-09 07:08_

---

_Comment by @Ravencentric on 2025-09-09 09:39_

Should `pyproject.toml` and `requirements.txt` be a part of the `cache-key` by default in this case? I don't think adding/removing/updating a python dependency or changing the configuration of a completely unrelated tool should rebuild the expensive native code.

---

_Comment by @konstin on 2025-09-09 12:31_

Speaking from a maturin perspective, `pyproject.toml` should be included, settings can be defined in this file, but `requirements.txt` isn't used.

CC @henryiii Do you have thoughts about which files should cause a scikit-build-core project to be rebuilt?

---

_Review comment by @konstin on `crates/uv/src/commands/project/init.rs`:1005 on 2025-09-09 12:31_

Don't we need the double `{{` and `}} ` here also?

---

_@konstin reviewed on 2025-09-09 12:31_

---

_Comment by @henryiii on 2025-09-09 14:11_

`pyproject.toml` (at least some tables, like `project`, `build-system`, and `tool.scikit-build`) makes sense for scikit-build-core as well. `**/CMakeLists.txt`, `**.cmake` would make sense too. We don't have dedicated support for presets yet, but `CMakePreset.json` and `CMakeUserPreset.json` also can affect the builds (and we'll have better support at some point). I've also seen the `.C` convention for C++ (pretty rare, but was somewhat common in the early nineties, so still pops up occasionally, especially with ROOT in HEP). I've also seen `.inc` for includes that aren't headers.

---

_Comment by @henryiii on 2025-09-09 14:14_

Oh, also the fortran extensions (`.f`, `.f90`, `.F90`, `.F`), and cuda (`.cu`, `.cuh`). I think those are the most common languages, though CMake also supports others, like Swift.

---

_Comment by @konstin on 2025-09-09 17:51_

Thanks @henryiii!

For the PR, further file extensions are cheap to support, we can cover as many of them as we want. I would avoid `**/CMakeLists.txt` and `**.cmake` as the require traversing everything, we need to limit those more if we want to include them.

---

_Comment by @henryiii on 2025-09-09 20:34_

How is that different from source file extensions? At least `.cmake` literally is an extension.

---

_Comment by @henryiii on 2025-09-09 20:37_

Oh, I think I see. Hadn’t looked at the code. `src/**/{*.h,*.c,*.hpp,*.cpp,*.cmake,CMakeLists.txt}` might work?

---

_Comment by @zanieb on 2025-09-10 02:04_

I'm sort of wary to go far beyond the files types that we're generating in the initial template.

---

_Comment by @henryiii on 2025-09-10 03:00_

I think a nice minimal set then might be `..., "{ file = "src/**/*.{h,c,hpp,cpp}" }, { file = "CMakeLists.txt" }]"`, then? Since it's part of the template, that seems fine to have it minimal.

---

_@rimathia reviewed on 2025-09-10 05:34_

---

_Review comment by @rimathia on `crates/uv/src/commands/project/init.rs`:1005 on 2025-09-10 05:34_

It turns out that at present, the Scikit project string does not use format! like the Maturin project string does. As a result, `{wheel_tag}` is still present in the resulting pyproject.toml. I'll have to investigate why this even works.

---

_@henryiii reviewed on 2025-09-10 14:18_

---

_Review comment by @henryiii on `crates/uv/src/commands/project/init.rs`:1005 on 2025-09-10 14:18_

That's correct, scikit-build-core has to inject the wheel tag there when it builds, since you don't know the wheel tag before you start building a wheel. You want `{wheel_tag}` in the pyproject.toml.

---

_Comment by @rimathia on 2025-09-10 16:37_

Thanks for giving me specifics, I hope I've understood the emerging consensus.

---

_Renamed from "Add source files of python extension to tool.uv cache-keys when initializing projects with build backend maturin or scikit" to "Better defaults for native build backend cache keys" by @konstin on 2025-09-12 09:09_

---

_Comment by @konstin on 2025-09-12 09:13_

Let's start with these cache keys and see how it goes.

---

_@konstin approved on 2025-09-12 09:14_

Thank you!

---

_Merged by @konstin on 2025-09-12 09:15_

---

_Closed by @konstin on 2025-09-12 09:15_

---

_Comment by @zanieb on 2025-09-12 13:10_

I think this is missing a docs update in https://docs.astral.sh/uv/concepts/projects/init/#projects-with-extension-modules where we say you'll need to use `--reinstall` when `main.cpp` changes

---
