---
number: 8932
title: Pruned versions make specific uv versions unable to compile
type: issue
state: closed
author: Flamefire
labels:
  - bug
assignees: []
created_at: 2024-11-08T11:37:43Z
updated_at: 2024-11-08T15:18:38Z
url: https://github.com/astral-sh/uv/issues/8932
synced_at: 2026-01-10T01:24:34Z
---

# Pruned versions make specific uv versions unable to compile

---

_Issue opened by @Flamefire on 2024-11-08 11:37_

I recently tried to recompile uv 0.2.30 (for testing) which has a dependency on reqwest-middleware and reqwest-retry:  
https://github.com/astral-sh/uv/blob/0.2.30/Cargo.lock#L3123

However that specific commit does no longer exist/is dangling: https://github.com/astral-sh/reqwest-middleware/commit/21ceec9a5fd2e8d6f71c3ea2999078fecbd13cbe

It will be removed from GitHub when the reflog expires (about 90 days IIRC)

Can you please do either of

1. avoid removing commits from repositories that are used elsewhere
2. tag such commits (e.g. as `reqwests-uv-2.x`) to avoid them from expiring
3. avoid using commits not in some branch that will not be deleted, such as `main`

Otherwise this creates headaches for maintainers of "build scripts", e.g. packages for distros, container images or us at EasyBuild (software build software for HPC)

---

_Comment by @charliermarsh on 2024-11-08 13:41_

I don't know what happened there but in general we always tag commits that we use in VCS for this reason. It looks like an oversight by @konstin. We can tag that commit to fix this one up.

---

_Label `bug` added by @charliermarsh on 2024-11-08 14:10_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-08 14:35_

---

_Comment by @charliermarsh on 2024-11-08 14:39_

Is there a way to get Cargo to fail when building this? Or rather, how did you even detect this?

---

_Comment by @charliermarsh on 2024-11-08 14:40_

Thanks @konstin (https://github.com/astral-sh/reqwest-middleware/releases/tag/uv-0.2.30).

---

_Closed by @charliermarsh on 2024-11-08 14:40_

---

_Comment by @Flamefire on 2024-11-08 14:53_

> Is there a way to get Cargo to fail when building this? Or rather, how did you even detect this?

I would expect cargo to fail unless it has the dependency cached. I.e. deleting `$HOME/.cargo` or setting `$CARGOHOME` to some empty folder should make it try to download it and fail. Unless it fetches the specific commit directly, which I guess it does. That will work until the reflog is pruned.

In our case we detected that after cloning the repo and trying to checkout the commit (in some generic code that vendors crates)

---

_Comment by @charliermarsh on 2024-11-08 14:56_

(I think it worked for me because Konsti had already fixed it, and I didn't realize.)

---

_Referenced in [TrueLayer/reqwest-middleware#198](../../TrueLayer/reqwest-middleware/pulls/198.md) on 2024-11-08 15:17_

---

_Comment by @konstin on 2024-11-08 15:18_

Upstreamed this to ask for crates.io releases: https://github.com/TrueLayer/reqwest-middleware/pull/198

I've also tagged the version we're using in uv 0.5.0 in the meantime.

---

_Referenced in [easybuilders/easybuild-framework#4680](../../easybuilders/easybuild-framework/pulls/4680.md) on 2025-04-10 07:24_

---
