```yaml
number: 8877
title: "feat: add environment variable to disable writing installer metadata files"
type: pull_request
state: merged
author: adisbladis
labels: []
assignees: []
merged: true
base: main
head: reproducible-builds-no-dist-info
created_at: 2024-11-07T04:48:59Z
updated_at: 2024-12-04T01:29:34Z
url: https://github.com/astral-sh/uv/pull/8877
synced_at: 2026-01-12T16:08:32Z
```

# feat: add environment variable to disable writing installer metadata files

---

_@adisbladis_

## Summary

This change introduces the `UV_NO_INSTALLER_METADATA` environment variable
as a way to opt out of the extra installer metadata files that `uv` is creating.

This is important to achieve reproducible builds in distribution
packaging, allowing to replace usage of
[installer](https://pypi.org/project/installer) with `uv pip install`.

At the time of writing these files are:
- `uv_cache.json`
    Contains timestamps which are non-reproducible.
    These hashes also leak in to the `RECORD` file.

- `direct_url.json`
    Contains the path to the installed wheel.
    While not non-reproducible it's not required for distribution packaging.

- `INSTALLER`
    Again, not non-reproducible, but of no value in distribution packaging.

## Test Plan

Automated test added.

---

_Comment by @zanieb on 2024-11-07 05:16_

Thanks for putting up the PR!

Note to other reviewers, we briefly discussed using an environment variable for this in [Discord](https://discord.com/channels/1039017663004942429/1207998321562619954/1303899841994424330).

I don't have strong opinions on the name, but we might want to use `UV_NO_EXTRA_DIST_INFO=1` rather than `UV_EXTRA_DIST_INFO=0`?

Otherwise, I'm not well suited to be the primary reviewer for this. Perhaps @konstin or @charliermarsh would be interested.

---

_@j178 reviewed on 2024-11-07 05:20_

---

_Review comment by @j178 on `crates/uv-static/src/env_vars.rs`:555 on 2024-11-07 05:20_

"Set to false to skip writing `uv` cache & installer files to site-packages dist-info." ?

---

_Review comment by @adisbladis on `crates/uv-static/src/env_vars.rs`:555 on 2024-11-07 05:34_

There are no other env vars written in that style, and I believe the wording makes more sense since the name was swapped from `UV_EXTRA_DIST_INFO` to `UV_NO_EXTRA_DIST_INFO`, with the logic inverted from previously.

---

_@adisbladis reviewed on 2024-11-07 05:34_

---

_Comment by @adisbladis on 2024-11-07 05:34_

> I don't have strong opinions on the name, but we might want to use UV_NO_EXTRA_DIST_INFO=1 rather than UV_EXTRA_DIST_INFO=0?

It made more sense to me when writing this to think about it the other way around, but it's indeed seems more consistent with other options to use `UV_NO_EXTRA_DIST_INFO=1`.

---

_Comment by @konstin on 2024-11-07 08:52_

What's the motivation for going through wheel installation for repackaging over re-zipping the wheel into the target format you are interested in?

---

_Comment by @adisbladis on 2024-11-07 09:41_

> What's the motivation for going through wheel installation for repackaging over re-zipping the wheel into the target format you are interested in?

Sorry, I don't understand the question? I'll explain my use case in more detail, hopefully we'll find some understanding.

The use case for this PR is to replace usage of `installer` with `uv` in distribution packaging scripts.
These scripts need to:
- Invoke a Python build system, such as `pypa/build` which takes a source tree and outputs a wheel
  - Or download a pre-built wheel
- Invoke an installer that can install said wheel into a prefix

In [nixpkgs](https://github.com/nixos/nixpkgs/) we use:
- [`pypa/installer`](https://github.com/NixOS/nixpkgs/blob/57c2e56/pkgs/development/interpreters/python/hooks/pypa-install-hook.sh#L11) to install Python wheels.
- [`pypa/build`](https://github.com/NixOS/nixpkgs/blob/57c2e5683de7cd80ce44702b56fb22c614407a7e/pkgs/development/interpreters/python/hooks/pypa-build-hook.sh#L9) to build Python packages from source
- In some rare cases download pre-built wheels

For distribution packaging we want [reproducible builds](https://reproducible-builds.org/), meaning that the outputs produced by a packaging script should be bit-for-bit identical.
Nix comes with tooling to perform these checks: `nix-build ./. -A somePackage --check`.

I have implemented an alternative Python build infrastructure for Nix [where I use uv](https://github.com/nix-community/pyproject.nix/blob/eba6cb2/build/hooks/pyproject-install-hook.sh#L11).
These builds end up failing reproducibility checks because of extra dist info files.

At some point in the future I'd like to replace the nixpkgs install hook with a uv implementation too. A requisite for that is that outputs are reproducible.

---

_Review requested from @charliermarsh by @konstin on 2024-11-07 12:45_

---

_Comment by @adisbladis on 2024-11-12 00:39_

Ping @charliermarsh

---

_Comment by @charliermarsh on 2024-11-12 02:30_

I'm not super excited to maintain this but I see the value. I think we should call this "installer metadata" rather than "extra dist-info", since the latter is just a term we made up within the wheel installer crate. How's that sound?

---

_@charliermarsh reviewed on 2024-11-12 02:31_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/linker.rs`:48 on 2024-11-12 02:31_

I think this should be after `installer`.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:246 on 2024-11-12 02:32_

I'm not gonna make the case that anything here is well-ordered but we do consistently end with cache and then printer. And generally, we try to put the global arguments at the bottom. I think this should be after logger maybe?

---

_@charliermarsh reviewed on 2024-11-12 02:32_

---

_@adisbladis reviewed on 2024-11-22 05:22_

---

_Review comment by @adisbladis on `crates/uv/src/commands/project/sync.rs`:246 on 2024-11-22 05:22_

I didn't know what the right call was, so I just put it last.
I've now realigned this with your comments as best as I could.

It's indeed not perfectly consistent, but I think it makes more organisational sense now.

---

_Comment by @adisbladis on 2024-11-22 05:39_

> I'm not super excited to maintain this but I see the value. 

Thank you. I understand that the use case is a bit niche from a Python perspective.

> I think we should call this "installer metadata" rather than "extra dist-info", since the latter is just a term we made up within the wheel installer crate. How's that sound?

Yep, that sounds much better!

---

_Renamed from "feat: add environment variable to disable writing extra dist-info files" to "feat: add environment variable to disable writing installer metadata files" by @adisbladis on 2024-11-22 05:46_

---

_Comment by @adisbladis on 2024-12-03 22:50_

Ping again @charliermarsh 

---

_Assigned to @charliermarsh by @zanieb on 2024-12-03 22:54_

---

_Comment by @charliermarsh on 2024-12-04 00:47_

Ack, will review (and hopefully merge) soon.

---

_@charliermarsh approved on 2024-12-04 01:18_

---

_Merged by @charliermarsh on 2024-12-04 01:29_

---

_Closed by @charliermarsh on 2024-12-04 01:29_

---
