---
number: 7810
title: Pin to major versions for apps
type: issue
state: closed
author: vriesdemichael
labels: []
assignees: []
created_at: 2024-09-30T14:25:09Z
updated_at: 2025-01-03T18:40:30Z
url: https://github.com/astral-sh/uv/issues/7810
synced_at: 2026-01-07T13:12:17-06:00
---

# Pin to major versions for apps

---

_Issue opened by @vriesdemichael on 2024-09-30 14:25_

With the project API we can now lock applications down to specific versions before invocations and during syncs. Which is great!

But when you add a dependency using `uv add somedep` it will add an entry in `pyproject.toml` pinned to a lower bound patch version (e.g. `somedep >=1.2.3`).

I would like it if you could also add an upper bound to dependencies from the CLI.
Typically, we would use an upper bound for major versions, to prevent locking to new versions of dependencies with breaking changes (major version bumps indicate breaking changes to the API for projects with SemVer)

Given that the latest version of somedep is version `1.2.3` it would be nice to get `somedep >=1.2.3, <2` as entry in pyproject.toml.
The uv.lock file can then deal with the specific patch version to use together with the other dependencies.

Perhaps it could be a new resolution strategy to pass to uv add. 
`uv add somedep --resolution pin-major`







---

_Comment by @benlindsay on 2024-10-15 14:37_

This is the default behavior in Poetry and it would be great to have it available in uv too. Loving uv so far, thanks for all the hard work!

---

_Comment by @notatallshaw on 2024-10-15 14:49_

FYI there was a long discussion towards the end of #1398 about why adding upper bounds by default would have a lot of negative consequences.

I do think there are valid cases, either because a users knows they are tightly coupled on specific dependencies, or they know their application will never ever be a dependency itself and they want to put barriers on unintentional upgrades.

But in both cases I would personally recommend pinning your dependencies, as very few libraries strictly follow semver, or they don't follow semver at all but users think they do e.g. pip which has a form of calver. And even if a library follows semver it is only meant to guarantee intentional behavior, users often get caught out that they are depending on unintentional behavior and that breaks, which semver doesn't protect against.

---

_Comment by @benlindsay on 2024-10-15 15:22_

> adding upper bounds by default would have a lot of negative consequences.

I'm fine with not having it on by default. Having a cli option to enable it would be great, enabling a config option to opt-in per-project would be fantastic, and enabling a similar config option to set in .bashrc/.zshrc would be superb.

> But in both cases I would personally recommend pinning your dependencies

Isn't this what the lockfile is for? I prefer to leave the bounds relatively wide in pyproject.toml, then pin via the lockfile. This way, if I see a dependency pinned to a specific version in pyproject.toml, I'll know that I should be more wary of updating that one. If everything is pinned in pyproject.toml, updating project dependencies is a lot harder since you can't just run one update command.

---

_Comment by @edmorley on 2025-01-03 17:55_

This seems like a duplicate of #6783 (see also #10247).

---

_Closed by @zanieb on 2025-01-03 18:40_

---
