```yaml
number: 16372
title: "`uv pip compile` is pinning cyclic dependecies to oldest released version"
type: issue
state: open
author: Czaki
labels:
  - bug
assignees: []
created_at: 2025-10-20T11:26:12Z
updated_at: 2025-11-07T06:34:51Z
url: https://github.com/astral-sh/uv/issues/16372
synced_at: 2026-01-12T16:02:30Z
```

# `uv pip compile` is pinning cyclic dependecies to oldest released version

---

_@Czaki_

### Summary

We use `uv pip compile` to provide a constraints file for reproducible builds and testing (even for `pip install`). 

Most recent release of `napari-plugin-manager` added a `napari` as a dependency, where `napari` depends on `napari-plugin-manager`. 

So when we run `uv pip compile` on the napari project (`pyproject.toml`) 
https://github.com/napari/napari/blob/e8abcaf5fea49e872b07a998f1a3bcde628dee80/.github/workflows/upgrade_test_constraints.yml#L168

It ends with adding `napari==0.6.6` to the constraints file. https://github.com/napari/napari/commit/86ea1478982ceca52b311f22b8978a6e634e6433#diff-347d546af1ecc381608b9b2a5635a7daeb1a4740c6db0550e39fe735da7e79dbR246

Could I request not to include the constraints in the package, which `pyproject.toml` is input to `uv pip compile`? And especially do not use constraints of the released version when calculating compile output.
 
Sometimes, cyclic dependencies are not possible to avoid (we will try to work around it).

### Platform

linux GHA

### Version

0.9.4

### Python version

3.12.12

---

_Label `bug` added by @Czaki on 2025-10-20 11:26_

---

_Comment by @konstin on 2025-10-20 14:34_

Sorry, I'm not following, what package should not be include for which specific command, and how is that related to having a cyclic dependency?

---

_Renamed from "`uv pip compile` is pining cyclic dependecies to oldest release version" to "`uv pip compile` is pining cyclic dependecies to oldest released version" by @Czaki on 2025-10-20 15:12_

---

_Comment by @Czaki on 2025-10-20 15:19_

If I run `uv pip compile pyproject.toml` in the napari project main directory, I do no not expect to see `napari==0.6.6` in the output (where napari is a dependency of one napari dependency) I do not even expect to `napari==0.6.6` dependencies (with constraints) to be taken as input to constraint resolving. 



---

_Renamed from "`uv pip compile` is pining cyclic dependecies to oldest released version" to "`uv pip compile` is pinning cyclic dependecies to oldest released version" by @Czaki on 2025-10-27 08:28_

---

_Comment by @jni on 2025-10-28 13:39_

@konstin we're trying to compile a lock file for the current dependencies of the project **napari** on the **main** branch, ie, napari@main, several commits ahead of the last released version, napari 0.6.6.

One of our *optional* dependencies is **napari-plugin-manager**, which itself depends on napari (this has temporarily been reverted to avoid this issue). That's the circular dependency.

Then when we run `uv pip compile pyproject.toml` on [napari@main's pyproject.toml](https://github.com/napari/napari/blob/a0ba263b2c72b89786fb2e6ea93cde2c9b3ff2f5/pyproject.toml#L9), it results in a lockfile that includes napari==0.6.6, which is of course unsatisfiable when installing napari@main.

Does that make sense?

So the request is for a way for the root project (the project name in the pyproject.toml used to invoke `uv pip compile`) to be excluded from the resultant lockfile.

What do you think?

---

_Comment by @konstin on 2025-11-05 19:33_

It would be incorrect for us to strip the circular dependency by default, even if it's the `project.name` in `pyproject.toml`. We're adding better support for manually excluding dependencies (https://github.com/astral-sh/uv/pull/16528), while currently, you can manually exclude dependencies through an override dependency with `napari; python_version < "0"`.

---

_Comment by @Czaki on 2025-11-07 06:34_

As I read, the exclude from #16528 will exclude the package from installation, but documentation do not mention anything about local package. Does put `napari` in exclude (for example via `UV_EXCLUDE` env variable) will prevent from install napari using `pip install .` in root of project? 

---
