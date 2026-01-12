```yaml
number: 15779
title: "Dependency groups ignore `project.name` and tries to pull it from registry"
type: issue
state: open
author: ZzEeKkAa
labels:
  - bug
assignees: []
created_at: 2025-09-10T21:39:08Z
updated_at: 2025-09-14T12:59:24Z
url: https://github.com/astral-sh/uv/issues/15779
synced_at: 2026-01-12T16:02:17Z
```

# Dependency groups ignore `project.name` and tries to pull it from registry

---

_@ZzEeKkAa_

### Summary

I want to include project's extra into a dependency group, but it looks like uv ignores it and tries to pull the project from registry.

`pyproject.toml`:
```toml
[project]
name = "uv_test"
version = "0.0.1"

[project.optional-dependencies]
foo = ["foo>=1.0.0"]

[dependency-groups]
foo = ["uv_test[foo]"]
```

```bash
uv pip compile -o pylock.foo-group.toml --group foo pyproject.toml
```
results into a lock file with a dependency:

```toml
[[packages]]
name = "uv-test"
version = "0.1.0"
sdist = { url = "https://files.pythonhosted.org/packages/70/8b/ac9819d2d6c2071b8e2bbafc37943013d8e7cdad669eea27fbf00e20acd3/uv_test-0.1.0.tar.gz", upload-time = 2024-11-01T07:12:39Z, size = 27479, hashes = { sha256 = "d0b75903d4e479a1dde7d13600bb49b3102e13661e7786ea1b7bcaa88c334dbd" } }
wheels = [{ url = "https://files.pythonhosted.org/packages/23/9a/677b8c914aeb441458d6af75aaac6b7bf9dc1809016aec92054b4ac872da/uv_test-0.1.0-py3-none-any.whl", upload-time = 2024-11-01T07:18:02Z, size = 1339, hashes = { sha256 = "784c63e7fc406a312bf768407c0a029cdaded028e82fc6e4e7452c1d493cc879" } }]
```

However,

```bash
uv pip compile -o pylock.foo-extra.toml --extra foo pyproject.toml
```

fails due to 

```
× No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of foo and uv-test depends on foo>=1.0.0, we can conclude that your requirements are unsatisfiable.
```

which is correct, since there is no `foo>=1.0.0` package in registry

So I see here 2 issues:
1. dependency group resolution ignores project name and looks registry
2. extra dependency is ignored if not found with only a warning (`warning: The package `uv-test==0.1.0` does not have an extra named `foo`), however it feels like it should be an error

This could explain https://github.com/astral-sh/uv/issues/14645 issue

### Platform

Ubuntu 20.04

### Version

uv 0.8.16

### Python version

Python 3.11.13

---

_Label `bug` added by @ZzEeKkAa on 2025-09-10 21:39_

---

_Comment by @zanieb on 2025-09-10 21:41_

When using the `pyproject.toml` as an input to `uv pip compile`, you are not including the project itself, just its dependencies, so uv does not know the project is local.

I think you want `uv export` instead?

---

_Comment by @zanieb on 2025-09-10 21:41_

Regarding (2), this is very common in Python packaging, otherwise it's a breaking change for projects to remove extras.

---

_Comment by @ZzEeKkAa on 2025-09-10 21:48_

> When using the `pyproject.toml` as an input to `uv pip compile`, you are not including the project itself, just its dependencies, so uv does not know the project is local.

I can argue that. If we add `bar = ["uv_test[foo]"]` to `project.optional-dependencies`, I'm getting exactly same issue as with foo:

```bash
uv pip compile -o pylock.bar-extra.toml --extra bar pyproject.toml
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of foo and uv-test depends on foo>=1.0.0, we can conclude that your requirements are unsatisfiable.
```

That means if referred to itself from `project.optional-dependencies` it knows about itself, but from `dependency-groups` - it does not

---

_Comment by @zanieb on 2025-09-10 22:02_

mm that's a good point! I'm still not sure it should work, but... the distinction is sort of subtle. I'll need to think about it.

---

_Comment by @ZzEeKkAa on 2025-09-10 22:13_

Thank you! Unfortunately there is not WAR for this. The way around is [deferred](https://peps.python.org/pep-0735/#why-not-support-dependency-group-includes-in-project-dependencies-or-project-optional-dependencies) for now and duplicating the list of dependencies is highly undesirable.

---

_Comment by @zanieb on 2025-09-11 01:58_

Doesn't `uv export` work though?

---

_Comment by @konstin on 2025-09-14 12:59_

We're already resolving the package name, but we drop it again instead of using it in the resolver. We need to keep the package name -> local directory URL association around. I confirmed that this is possible by using a constraint (https://github.com/astral-sh/uv/compare/main...konsti/keep-source-tree-around), but we should solve this properly, for example in the lookahead resolver.

---
