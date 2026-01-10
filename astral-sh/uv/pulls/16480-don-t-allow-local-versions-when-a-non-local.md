```yaml
number: 16480
title: "Don't allow local versions when a non-local version is pinned"
type: pull_request
state: open
author: konstin
labels:
  - bug
assignees: []
draft: true
base: main
head: konsti/mismatch-local-versions-in-sync
created_at: 2025-10-28T11:42:39Z
updated_at: 2025-12-05T18:37:51Z
url: https://github.com/astral-sh/uv/pull/16480
synced_at: 2026-01-10T05:49:14Z
```

# Don't allow local versions when a non-local version is pinned

---

_Pull request opened by @konstin on 2025-10-28 11:42_

See https://github.com/astral-sh/uv/issues/16368

Current behavior of `uv sync`:
* Requested: `1.0.0+cpu`, Installed: `1.0.0`: Install new package
* Requested: `1.0.0+cpu`, Installed: `1.0.0+cu128`: Install new package
* Requested: `1.0.0`, Installed: `1.0.0+cpu`: Keep installed package

The new behavior is to always reinstall when the local version part is different, for consistency and to fix torch.

This behavior happens because internally, we translate the version from the lockfile to an `=={version}` request, and version matching says local versions are allowed if the specifier has no local version.

This is a (minor) behavior change: When running `uv sync` in a venv with a package after installing the same package with the same version except the local version, uv will now remove the installed package and use the one from the lockfile instead. This seems more correct, as the other version was not matching the lockfile. There is still a gap as what we actually want to track is the index URL (or even better, the package hash), but as that information is not tracked in the venv, checking the local version is the next bests thing we can do. The main motivation is fixing torch, our main user of packages with both local and non-local versions (https://github.com/astral-sh/uv/issues/16368).

Fix #16368


---

_Review requested from @charliermarsh by @konstin on 2025-10-28 11:42_

---

_Label `bug` added by @konstin on 2025-10-28 11:42_

---

_Comment by @zanieb on 2025-10-28 12:12_

So

- Requested: 1.0.0, Installed: 1.0.0+cpu: Keep installed package

Will now install a new package?

I'm a little worried that specific case will break use-cases where people are using local versions for non-accelerator purposes.

Does this change `uv pip install` behavior?

---

_Comment by @konstin on 2025-10-28 12:30_

> So
> 
>     * Requested: 1.0.0, Installed: 1.0.0+cpu: Keep installed package
> 
> 
> Will now install a new package?

Yes

> I'm a little worried that specific case will break use-cases where people are using local versions for non-accelerator purposes.

Do you have a specific example?


> Does this change `uv pip install` behavior?

All cases of using `uv pip install` with direct dependencies or `requirements.txt` I know use a specifier, and we don't change specifier semantics. The change only applies to cases using a lockfile, afaik exactly `pylock.toml`, where the same change as `uv.lock` applies.


```
$ uv pip list
Using Python 3.14.0 environment at: debug/.venv
Package Version
------- -----------
project 1.0.0+local
$ uv pip install --find-links debug/local/dist/ --no-index "project==1.0.0"
Using Python 3.14.0 environment at: debug/.venv
Audited 1 package in 0.91ms
```


For `pylock.toml`, we can replicate the `uv.lock` case, starting with the setup in the test case:
```
uv export -o pylock.local.toml --extra local
uv export -o pylock.non-local.toml --extra non-local
uv pip install -r pylock.local.toml
# Installs project==1.0.0+local
uv pip install -r pylock.non-local.toml
# main: noop
# branch: Installs project==1.0.0
```



---

_Comment by @zanieb on 2025-10-29 14:29_

> Do you have a specific example?

Unfortunately no. I think the user story would be roughly that the override a dependency in their requirements file with some local version then perform a sync with the requirements file and expect their locally patched version to be retained.

---

_Comment by @konstin on 2025-10-30 09:09_

Another case where we observe this is https://github.com/karpathy/nanochat (`at1ccbaf4416bc39dfb75f0e4fe934363278adcfda`). There's a PyPI torch version (from `project.dependencies`) and there are CPU and GPU extras that point to the respective torch indexes:
* `uv sync --extra cpu` and `uv sync --extra gpu` switch between CPU and GPU versions `torch==2.9.0+cpu` and `torch==2.8.0+cu128`
* `uv sync --extra gpu`, then `uv sync` will switch from `torch==2.8.0+cu128` to `torch==2.9.0` (different release)
* `uv sync --extra cpu`, then `uv sync` does NOT switch from `torch==2.9.0+cpu` to `torch==2.9.0` (same release)

It's arguable that you don't actually want the PyPI version, but it seems counterintuitive that `uv sync` would install it when run first, but not after running with `--extra cpu`.

---

_Converted to draft by @konstin on 2025-12-05 18:37_

---
