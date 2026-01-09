---
number: 5190
title: Locking of build dependencies
type: issue
state: open
author: sbidoul
labels:
  - enhancement
assignees: []
created_at: 2024-07-18T15:55:38Z
updated_at: 2025-08-08T02:40:07Z
url: https://github.com/astral-sh/uv/issues/5190
synced_at: 2026-01-07T13:12:17-06:00
---

# Locking of build dependencies

---

_Issue opened by @sbidoul on 2024-07-18 15:55_

Coming from https://github.com/astral-sh/uv/issues/3611#issuecomment-2226921962 and following comments.

Caveat: this is certainly not a complete proposal, and I reckon I have not investigated the capabilities of uv lock/sync in any detail, so consider this as thoughts to start the conversation on the usefulness of such features.

**What**

The `uv.lock` format supports locking source distributions (from sdists, git source trees, ...). This is great!

I would find it useful that
- commands that install from a lock file fail (by default?) if they need to install build dependencies that are not locked
- the lock command grows a mean to lock the build system requirements of the source dependencies declared in the lock file it produces

**Why**

When installing from a lock file I expect that everything that the installation downloads (or obtains from cache) is checked against the hashes (or immutable VCS commit hashes) that are present in the lock file, for repeatability and security.

If, during installation, other artifacts are downloaded (or obtained from cache) without my knowledge (to build source distributions), the security and repeatability of the process is weakened. 

**Some thoughts**

- this may have performance implications for the lock command if sdists have to be downloaded to know the build requirements (https://github.com/astral-sh/uv/issues/3611#issuecomment-2226993337)
- some build dependencies are dynamic (`get_requires_for_build_{wheel,editable}`), further complicating the locking operation
- different locked source distributions may require different versions of the same build dependency
- a first valuable step could be to lock build dependencies for which no wheel is available, and warn when an artifact that is not locked is downloaded when syncing


---

_Label `enhancement` added by @charliermarsh on 2024-07-19 01:36_

---

_Comment by @paveldikov on 2024-07-21 16:17_

I was going to raise this same issue as well. Currently `build-system.requires` is completely unconstrained/unconstrainable and this is causing problems with build reproducibility.

This is causing real-life inconveniences in our organisation as, for example, new versions of `setuptools` appear but are not yet given the all-clear by our scanning suite. Versions that are not cleared yield a HTTP 403 upon download. The end result is a de-facto intractable state where no one can build (or even editably install) packages. (unless the developer decides to hard-constrain @ `pyproject.toml`, which is terrible UX and also doesn't help with the dependency-on-sdist problem.)

(I will probably also raise a separate issue w.r.t the handling of 403 codes -- this is a pain point with `pip` and it would be awesome if `uv` could do better)

---

_Comment by @charliermarsh on 2024-07-28 00:35_

Definitely agree that we should do this.

---

_Referenced in [astral-sh/uv#5551](../../astral-sh/uv/issues/5551.md) on 2024-07-29 09:15_

---

_Referenced in [astral-sh/uv#5639](../../astral-sh/uv/pulls/5639.md) on 2024-08-01 19:43_

---

_Comment by @charliermarsh on 2024-09-10 00:06_

I really want to do this (lock the build requirements).

---

_Referenced in [astral-sh/python-build-standalone#300](../../astral-sh/python-build-standalone/issues/300.md) on 2024-09-10 08:20_

---

_Referenced in [pyproject-nix/pyproject.nix#169](../../pyproject-nix/pyproject.nix/issues/169.md) on 2024-11-01 00:33_

---

_Referenced in [pyproject-nix/uv2nix#44](../../pyproject-nix/uv2nix/pulls/44.md) on 2024-11-01 11:52_

---

_Comment by @vlad-ivanov-name on 2024-11-15 07:10_

This is becoming more relevant with newer python versions that don't ship setuptools by default anymore

---

_Comment by @charliermarsh on 2024-11-15 14:01_

I really want to do this and would like to spend time on it personally, but I'm worried about how it will intersect with PEP 751.

---

_Comment by @vlad-ivanov-name on 2024-11-15 14:09_

Hmm but that's more about lockfiles in general, not specific to build-time deps? 

Correct me if I'm wrong but to me this PEP looks to be in early stages, and uv.lock looks way more mature as it is now. I would expect the introduction of this PEP to be orthogonal to the goal of being able to lock build dependencies - in the end if uv does decide to adopt the format, the build dependency group will probably be locked in a similar way to other dependency groups regardless of the final lock file format 

---

_Comment by @notatallshaw on 2024-11-15 14:34_

> I really want to do this and would like to spend time on it personally, but I'm worried about how it will intersect with PEP 751.

Last I saw build time dependencies were dropped from PEP 751: https://discuss.python.org/t/pep-751-lock-files-again/59173/222 (end of that post).

I don't think anything changed: https://peps.python.org/pep-0751/#locking-build-requirements-for-sdists (FYI I really hope the "it confused enough people " isn't directed at me, I had some clarifying questions, and was mostly happy with the answers).

---

_Comment by @charliermarsh on 2024-11-15 16:50_

@notatallshaw -- No, I agree with you, it's just that: if we add support for it here, what happens if PEP 751 comes out and we can't translate that support to the PEP?

---

_Comment by @bluss on 2024-11-15 17:12_

Could you use the space for tool specific data in the PEP 751 lock file? Or even using one of the main fields like `groups` using some kind of convention that makes it possible for locked build deps to co-exist with other named things stored there.

---

_Comment by @notatallshaw on 2024-11-15 17:12_

Yeah, makes sense to see the finalized version of PEP 751 first.

---

_Comment by @vlad-ivanov-name on 2024-11-16 00:12_

>what happens if PEP 751 comes out and we can't translate that support to the PEP?

I mean, this wouldn't be the first feature that uv supports that vanilla python tooling doesn't. This is important enough to implement regardless of what python itself does; after all, even locking of regular dependencies is not really supported out of the box, doesn't mean that uv should be restricted by that 

---

_Comment by @dimbleby on 2024-11-19 19:20_

similar proposals in poetry, pdm: https://github.com/python-poetry/poetry/issues/8261 (and several duplicates), https://github.com/pdm-project/pdm/issues/2465

I mostly think that people who care about reproducible builds would be better served by avoiding sdists altogether with `--only-binary` or similar.

---

_Referenced in [astral-sh/uv#10233](../../astral-sh/uv/issues/10233.md) on 2025-01-03 12:14_

---

_Comment by @giftig on 2025-03-24 16:40_

+1 for this; I've just had an existing, locked dependency fail to install because a new version of `setuptools` refuses to install dependencies using deprecated `description-file` syntax in `setup.cfg`, which is confusing and slightly frustrating (though not the fault of `uv`)

---

_Comment by @sbidoul on 2025-03-24 17:32_

Yep, to me this would be the killer feature to switch to `uv.lock`.

Build contraints help but they'd need to be managed manually, won't work if there are conflicting build constraints in the project, and it's easy to forget a build constraint, which you only realize months or years later when a build fails.

---

_Comment by @purepani on 2025-04-01 01:04_

Since PEP 751 is now accepted, and the resulting lock file isn't immediately usable for `uv` as a project lock file, perhaps it makes sense to add this as a `uv.lock` feature.


---

_Comment by @charliermarsh on 2025-04-01 01:37_

We're hoping to start working on this soon.

---

_Referenced in [astral-sh/uv#7052](../../astral-sh/uv/issues/7052.md) on 2025-04-01 12:35_

---

_Referenced in [pyca/cryptography#11548](../../pyca/cryptography/issues/11548.md) on 2025-04-29 00:56_

---

_Referenced in [astral-sh/uv#13416](../../astral-sh/uv/issues/13416.md) on 2025-05-13 02:56_

---

_Assigned to @Gankra by @zanieb on 2025-05-18 12:57_

---

_Referenced in [astral-sh/uv#13798](../../astral-sh/uv/issues/13798.md) on 2025-06-03 08:43_

---

_Referenced in [hermetoproject/pybuild-deps#307](../../hermetoproject/pybuild-deps/issues/307.md) on 2025-07-03 18:43_

---

_Referenced in [NixOS/nixpkgs#425219](../../NixOS/nixpkgs/pulls/425219.md) on 2025-08-02 16:19_

---

_Comment by @jiridanek on 2025-08-06 17:40_

> Build contraints help but they'd need to be managed manually, won't work if there are conflicting build constraints in the project, and it's easy to forget a build constraint, which you only realize months or years later when a build fails.

Building with `--require-hashes` seems to be a decent check for a forgotten build constraint. With

```
❯ uv --version
uv 0.8.5
```

I get, when I use `--build-constraints=./requirements.txt --no-binary=:all:`

```
❯ uv pip install --strict --no-deps --no-cache --no-config --no-progress --require-hashes --index-strategy=unsafe-best-match --requirements=./requirements.txt --build-constraints=./requirements.txt --no-binary=:all:
Using Python 3.12.11 environment at: pepa
Resolved 71 packages in 268ms
   Building nbclient==0.10.2
   Building ptyprocess==0.7.0
   Building typing-extensions==4.14.1
   Building jupyter-core==5.7.2
   Building tinycss2==1.4.0
  × Failed to download and build `nbclient==0.10.2`
  ├─▶ Failed to resolve requirements from `build-system.requires`
  ├─▶ No solution found when resolving: `hatchling>=1.10.0`
  ╰─▶ In `--require-hashes` mode, all requirements must be pinned upfront with `==`, but found: `hatchling`
```

The above is actually a great inconvenience to me, but I see some may benefit from this behavior.

---

_Unassigned @Gankra by @konstin on 2025-08-06 18:02_

---

_Referenced in [astral-sh/uv#15146](../../astral-sh/uv/issues/15146.md) on 2025-08-07 19:31_

---

_Comment by @bruno-fs on 2025-08-08 02:40_

I find it a bit sad that new "universal lockfile format" from PEP 751 [won't support locking build requirements for sdists](https://peps.python.org/pep-0751/#locking-build-requirements-for-sdists), but I'm glad to hear uv is interested in solving this problem.


---

_Referenced in [opendatahub-io/notebooks#1882](../../opendatahub-io/notebooks/issues/1882.md) on 2025-08-11 19:38_

---
