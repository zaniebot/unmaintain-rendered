```yaml
number: 14130
title: add a UV_ONLY_GLOBAL_LOCKS flag for uv add/sync/remove
type: pull_request
state: closed
author: jpetrucciani
labels: []
assignees: []
base: main
head: global_lock
created_at: 2025-06-18T15:11:43Z
updated_at: 2025-06-25T20:32:59Z
url: https://github.com/astral-sh/uv/pull/14130
synced_at: 2026-01-12T16:11:02Z
```

# add a UV_ONLY_GLOBAL_LOCKS flag for uv add/sync/remove

---

_@jpetrucciani_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

After the recent change in #13869, I've found that certain python environments can no longer run things like `uv sync`, `uv add`, and `uv remove`. In particular, I use [nix](https://nixos.org/) to get my python and uv binaries - and when I specify paths for these with `UV_PYTHON=`, it attempts to write these locks into the nix store next to the interpreter! This is problematic since things in the nix store are read-only - so I am unable to use uv for my environments beyond that change at the moment.

I did look around for other options I might be able to use to force this behavior, but I wasn't able to find anything that looked like it would behave how I wanted - if this is not the case, please let me know!

I also am not entirely sure I've added this correctly, or if there's some other pattern I should be following. I am not tied to any particular name or style of this, so please let me know if there's anything you'd like me to change!

## Test Plan

<!-- How was it tested? -->
I've been using a patched version of uv in my local environments using this commit's `.patch` file as a patch to the `uv` build in nixpkgs - adding `UV_ONLY_GLOBAL_LOCKS=1` with this change allows me to use uv again for operations that use these locks!

Here's the overlay:
```nix
final: prev: {
  uv = prev.uv.overrideAttrs (_: {
    patches = [
      (final.fetchpatch {
        url = "https://github.com/astral-sh/uv/pull/14130/commits/f332ef0f7b698b6db69c00a87fb8ab6797bb74f6.patch";
        sha256 = "sha256-sJcPyO/R+rxS2x4vOyogtY9zVU9mN/TjaBQLjECnjQI=";
      })
    ];
  });
}
```

---

_Renamed from "add a UV_ONLY_GLOBAL_LOCKS flag for " to "add a UV_ONLY_GLOBAL_LOCKS flag for uv add/sync/remove" by @jpetrucciani on 2025-06-18 15:11_

---

_Review requested from @oconnor663 by @zanieb on 2025-06-18 15:14_

---

_Review requested from @oconnor663 by @zanieb on 2025-06-18 15:14_

---

_Review requested from @oconnor663 by @zanieb on 2025-06-18 15:14_

---

_Review requested from @oconnor663 by @zanieb on 2025-06-18 15:14_

---

_Assigned to @zanieb by @zanieb on 2025-06-18 15:14_

---

_Assigned to @oconnor663 by @zanieb on 2025-06-18 15:14_

---

_Unassigned @oconnor663 by @zanieb on 2025-06-18 15:14_

---

_Unassigned @zanieb by @zanieb on 2025-06-18 15:14_

---

_Assigned to @konstin by @zanieb on 2025-06-18 15:14_

---

_Unassigned @oconnor663 by @zanieb on 2025-06-18 15:14_

---

_Unassigned @konstin by @zanieb on 2025-06-18 15:14_

---

_Unassigned @zanieb by @zanieb on 2025-06-18 15:14_

---

_Review requested from @konstin by @zanieb on 2025-06-18 15:14_

---

_Review requested from @konstin by @zanieb on 2025-06-18 15:14_

---

_Review requested from @konstin by @zanieb on 2025-06-18 15:14_

---

_Review requested from @konstin by @zanieb on 2025-06-18 15:14_

---

_Comment by @jpetrucciani on 2025-06-18 15:26_

Fixed the linter errors - though I'm hitting a weird panic trying to run `cargo dev generate-cli-reference` - probably an env issue on my side - will dig in more later today

---

_Comment by @konstin on 2025-06-18 15:30_

When you're using `uv sync` with a Python interpreter in nix, doesn't it need to write to the interpreter directory anyway to install packages, i.e., which specific path is write-only and failing for you?

---

_Comment by @jpetrucciani on 2025-06-18 16:02_

Apologies - to clarify, I called out `uv sync` just since it was associated with the change as well - I am only using `uv add` and `uv remove` (and occasionally `uv lock`). When I am using uv inside nix environments, I am essentially using it only for the version solving and generating the lockfile, which then is parsed by [uv2nix](https://github.com/pyproject-nix/uv2nix) to provide a reproducible tree of the dependencies. I am running with:

```
UV_PYTHON=/nix/store/78s3fidv1bhy5pj71sqff8y5cmzs1g1l-coco-env/bin/python   # this is an example from this particular environment - this would be a random /nix/store/* path depending on the derivation (which is read-only)
UV_SYSTEM_PYTHON=true
UV_NO_MANAGED_PYTHON=true
UV_ONLY_GLOBAL_LOCKS=1            # this is the new flag i added to let this continue to work how i've been using uv
UV_NO_SYNC=1
UV_PYTHON_DOWNLOADS=never
```

and then as the lockfile changes, my setup rebuilds the entire derivation based on what's parsed from the lockfile

---

_Comment by @oconnor663 on 2025-06-18 16:58_

I wonder if the underlying issue here is that `uv` is asking "is this a virtual environment?" when it should (also?) be asking "is the environment [_externally managed_](https://packaging.python.org/en/latest/specifications/externally-managed-environments)?":

https://github.com/astral-sh/uv/blob/1fc65a1d9de51b2dd567733208f4595dc442db89/crates/uv-python/src/interpreter.rs#L593-L599

@jpetrucciani, could you check whether your Nix environment has an `EXTERNALLY-MANAGED` marker file? Here's how I find that file in my Arch Linux environment, based on the `sysconfig` path rule [described in the docs here](https://packaging.python.org/en/latest/specifications/externally-managed-environments/#marking-an-interpreter-as-using-an-external-package-manager):

```
$ ls -l $(python -c "import sysconfig; print(sysconfig.get_path('stdlib', sysconfig.get_default_scheme()))")/EXTERNALLY-MANAGED
-rw-r--r-- 1 root root 553 Apr  9 00:44 /usr/lib/python3.13/EXTERNALLY-MANAGED
```

If a similar file exists for you, the best fix might be for us to add a check for it in the logic above. (If it doesn't exist, maybe it should? But I don't have any Nix experience myself.)

---

_Comment by @zanieb on 2025-06-18 18:01_

Those are technically mutually exclusive, `EXTERNALLY-MANAGED` can't be used in virtual environments (though I think it should be usable).

---

_Comment by @oconnor663 on 2025-06-18 18:26_

Ah, I was missing the "if both of these conditions are true" part [in the docs](https://packaging.python.org/en/latest/specifications/externally-managed-environments/#marking-an-interpreter-as-using-an-external-package-manager). In that sense, is this situation in some sense "Nix's fault" for causing Python to believe it's in a virtual environment, when it's...mostly not?

---

_Comment by @jpetrucciani on 2025-06-18 19:00_

Ah interesting - thank you for the links! I wasn't aware of this particular functionality being a part of the default python configuration/installation!

@oconnor663 the default python derivations in nix do not have the `EXTERNALLY-MANAGED` marker file, though it sounds like that wouldn't solve this particular case alone - but that could be something else to explore adding for other reasons!

This is interesting as well since the whole `sys.prefix != sys.base_prefix` check makes sense in most cases, but with how nix is managing the store paths, this would always be the case, since the base `python` derivation is its own store path, and then the uv2nix wrapper creates a symlink tree of that combined with the packages formed from the `uv.lock` file. I wonder if this is something I can actually patch as well - if that main check is the thing that determines "being in a virtual environment", then perhaps I could fix that?

![image](https://github.com/user-attachments/assets/5fce173b-d2d7-442c-91d1-6a6273f715bc)


---

_Comment by @matko on 2025-06-20 11:05_

@jpetrucciani, thank you for putting in effort into getting this issue resolved. I too started running into this in the last 2 days in my nix environments, and had to pin an earlier version of uv to keep things working.

Regarding environment testing, maybe another option is checking if the environment directory is read-only. This will never be the case for a uv-managed environment, and always the case for a virtual environment in the nix store. Since environment-local locking will always fail for read-only environments, and concurrent modification of a read-only environment is very unlikely, defaulting to a global lock in that case makes sense.

@oconnor663,
> In that sense, is this situation in some sense "Nix's fault" for causing Python to believe it's in a virtual environment, when it's...mostly not?

Nix environments are certainly 'weird' and it's understandable that tool expectations are broken some times. That said, this is not a case of causing python to believe it's a virtual environment when it is not. The uv2nix tooling really does produce a virtual environment, with a pyvenv.cfg one directory above a python binary, and a site-packages directory. It is just read-only and mostly composed out of symlinks. To my knowledge, that is still [PEP 405](https://peps.python.org/pep-0405/) compliant.

---

_Comment by @jpetrucciani on 2025-06-21 16:37_

fixed my local rust env and ran:

```bash
cargo dev generate-all
```

then updated the snapshot tests:
```bash
cargo insta test --features pypi,python,python-patch --accept --test-runner nextest --test it -- help::help
```

then updated this branch - though CI still seems to think I've not regenerated some things 🤔 from the diff, it looks like I'm [missing a section](https://github.com/astral-sh/uv/actions/runs/15796998330/job/44530342903?pr=14130#step:9:11279) of the `cli.md`, but I cannot find that section locally (`uv-python-upgrade`)

---

_Comment by @jpetrucciani on 2025-06-25 20:32_

I think this ended up being user error on my part - I was using the wrong location for `UV_PYTHON=` for my environment, causing this to act in this way. Following the [uv2nix's maintainer's advice](https://github.com/pyproject-nix/uv2nix/issues/208#issuecomment-3000838609) makes it work for me again, without needing this PR - Apologies for wasting anyone's time!

for context, in my nix environment (inside my `buildEnv`), I needed to use:
```nix
UV_PYTHON = python.interpreter;
```
instead of
```nix
UV_PYTHON = "${virtualenv}/bin/python";
```

---

_Closed by @jpetrucciani on 2025-06-25 20:32_

---
