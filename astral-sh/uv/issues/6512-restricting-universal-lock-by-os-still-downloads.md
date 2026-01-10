---
number: 6512
title: Restricting universal lock by OS still downloads wheels from all OSes
type: issue
state: closed
author: tmct
labels:
  - enhancement
  - good first issue
  - lock
assignees: []
created_at: 2024-08-23T13:04:29Z
updated_at: 2024-10-15T07:13:52Z
url: https://github.com/astral-sh/uv/issues/6512
synced_at: 2026-01-10T01:24:02Z
---

# Restricting universal lock by OS still downloads wheels from all OSes

---

_Issue opened by @tmct on 2024-08-23 13:04_

Hi - I'm very glad to see uv's recent universal locking features, and engagement with the lockfile standard - but I've found one behaviour that seems a little odd.

Perhaps I have misunderstood how [environment markers](https://docs.astral.sh/uv/reference/settings/#environments) are used in [universal resolution](https://docs.astral.sh/uv/concepts/resolution/#universal-resolution), but I am surprised by this behaviour:

```
mkdir temp
cd temp
uv init -p 3.10

# I only want to produce a lockfile for Linux, across Python versions
cat <<EOL >> pyproject.toml

[tool.uv]
environments = ["sys_platform == 'linux'"]
EOL

uv add torch==2.1.0 -p 3.10
```

Result: I still get plently Windows-specific wheels in my `uv.lock` lockfile, e.g. `torch-2.1.0-cp310-cp310-win_amd64.whl` - presumably these have all still been downloaded.

Would it be possible please not to download these Mac/Windows-only wheels for linux-only universal locks, as an example?

Many thanks,
Tom

---

_Label `enhancement` added by @konstin on 2024-08-23 13:06_

---

_Comment by @zanieb on 2024-08-23 13:36_

Thanks for the report!

@konstin you marked this as an enhancement, is this not a bug?

cc @charliermarsh 

---

_Label `lock` added by @zanieb on 2024-08-23 13:36_

---

_Comment by @konstin on 2024-08-23 13:43_

We already never install those wheels, but we're missing the pruning of the wheels list for platforms. We're currently only prune the wheels list for the python version (#4696), but we can extend this to the platform markers also. It may not work 100% for platform marker them since markers and wheels tags don't have a perfect 1:1 mapping, but we would still trim the lockfile down by a lot.

---

_Comment by @zanieb on 2024-08-23 13:51_

Thanks for the additional context. Is this hard?

---

_Comment by @konstin on 2024-08-23 14:25_

Hard to tell, hopefully it's a just a lookup table and some filtering. Ideally, you create a mapping from a marker to what tags it supports, e.g. `sys_platform == 'linux'` -> `manylinux`, `musllinnux` and `linux`, and then filter the wheels by that wheel tag.

---

_Comment by @charliermarsh on 2024-08-23 14:52_

This would be a good improvement.

---

_Label `good first issue` added by @konstin on 2024-08-23 14:53_

---

_Comment by @tmct on 2024-08-23 15:17_

Thanks very much all - would be very happy to see this enhancement - none of the other "universal" lockfile tools seems to support this filtering...

---

_Comment by @charliermarsh on 2024-08-29 15:47_

This is a bit more tedious because we don't track "all the markers under which this package could be relevant" in the lockfile directly -- you have to compute it by propagating markers. So at this point in the lockfile:
```rust
// Remove wheels that don't match `requires-python` and can't be selected for
// installation.
if let Some(requires_python) = &requires_python {
    package
        .wheels
        .retain(|wheel| requires_python.matches_wheel_tag(&wheel.filename));
}
```

We'd need to track the supported platforms.

---

_Assigned to @konstin by @konstin on 2024-09-02 12:35_

---

_Referenced in [astral-sh/uv#6957](../../astral-sh/uv/pulls/6957.md) on 2024-09-03 07:39_

---

_Comment by @charliermarsh on 2024-09-04 20:14_

Closed by https://github.com/astral-sh/uv/pull/6957.

---

_Closed by @charliermarsh on 2024-09-04 20:14_

---

_Comment by @tmct on 2024-09-04 21:18_

Thank you! Though that PR is described as "Prep for fixing https://github.com/astral-sh/uv/issues/6512. No functional changes."?

---

_Comment by @tmct on 2024-09-04 21:20_

Ah, now I see the subsequent #6961 and #6959 in the release notes. Very nice

---

_Comment by @charliermarsh on 2024-09-04 21:25_

Apologies, I linked the wrong PR!

---

_Comment by @tmct on 2024-10-15 07:12_

~Hi, quick question, which caches do I need to clear to make this happen if I forgot to restrict by OS before first sync? Thanks~

Apologies, was using an old version of uv 😂 

---
