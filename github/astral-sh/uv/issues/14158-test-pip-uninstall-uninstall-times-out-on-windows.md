---
number: 14158
title: "`test pip_uninstall::uninstall*` times out on Windows (has been running for over 60 seconds)"
type: issue
state: closed
author: Gankra
labels:
  - ci-flake
assignees: []
created_at: 2025-06-20T15:23:38Z
updated_at: 2025-07-18T13:16:41Z
url: https://github.com/astral-sh/uv/issues/14158
synced_at: 2026-01-07T13:12:18-06:00
---

# `test pip_uninstall::uninstall*` times out on Windows (has been running for over 60 seconds)

---

_Issue opened by @Gankra on 2025-06-20 15:23_

* https://github.com/astral-sh/uv/actions/runs/15781861061/job/44489244834?pr=14156

*  `test pip_uninstall::uninstall_by_path` has been running for over 60 seconds
*  `test pip_uninstall::uninstall_duplicate_by_path` has been running for over 60 seconds

---

_Label `ci-flake` added by @Gankra on 2025-06-20 15:23_

---

_Referenced in [astral-sh/uv#14156](../../astral-sh/uv/pulls/14156.md) on 2025-06-20 15:24_

---

_Comment by @Gankra on 2025-06-20 15:54_

Again in https://github.com/astral-sh/uv/actions/runs/15782642131/job/44491854297?pr=14161

---

_Comment by @zanieb on 2025-06-20 20:08_

See also #14114 

I hope these timeouts aren't related to #14122

---

_Comment by @zanieb on 2025-06-20 20:09_

Also, the timeout is set to 90s

https://github.com/astral-sh/uv/blob/647f70505ef4c0e6a2207f6f27a8f42d918b8297/.config/nextest.toml#L2-L4

---

_Referenced in [astral-sh/uv#14170](../../astral-sh/uv/pulls/14170.md) on 2025-06-20 20:10_

---

_Comment by @zanieb on 2025-06-20 20:48_

It fails despite an increased timeout 🤔 

---

_Comment by @Gankra on 2025-06-20 21:03_

Seems suspiciously timed with this PR that touched uninstall and merged an hour before I started hitting this

* https://github.com/astral-sh/uv/pull/13954

---

_Comment by @Gankra on 2025-06-20 21:04_

cc @jtfmumm 

---

_Comment by @jtfmumm on 2025-06-21 08:32_

EDIT: It looks like the interaction is between #14122 (Depot change) and #14126 (redirect change). When reverting just #14126, it's back to the post-14122 35-50s. Reverting just #14122 and it's back to 7-12s.

~~Interestingly, this appears to be an interaction between #14122 and #13954, which explains why I hadn't seen this on that PR in the past~~. I've looked at these tests in CI runs for the following:
* Commits on `main` prior to 14122 (Depot): Pass. 7-12s.
* `main` after 14122 but before 14126 (redirect changes): Pass. 35-50s.
* Current `main` (includes 14122 and 14126): Sometimes pass. Can be ~80s. 
* Reverting just 14122 (Depot): Pass. 7-12s.
* Reverting just 14126 (redirect changes): Pass. 35-50s.


---

_Referenced in [astral-sh/uv#14180](../../astral-sh/uv/pulls/14180.md) on 2025-06-21 09:29_

---

_Comment by @zanieb on 2025-06-21 12:54_

The failing test is for `pip_uninstall`, not `python_uninstall` — am I missing something?

---

_Comment by @zanieb on 2025-06-21 13:02_

Separately, I'm confused that https://github.com/astral-sh/uv/pull/13954 could significantly regress `uv pip` performance. We're going to need to look into that. Can we tell where all that time is going?

---

_Comment by @zanieb on 2025-06-21 13:10_

I'm poking at why the Depot runner would slow down the test.

---

_Comment by @zanieb on 2025-06-21 13:15_

In isolation, it runs in ~7s (I have it setup to fail on snapshot, ignore that)

> FAIL [   7.454s] uv::it pip_uninstall::uninstall_duplicate_by_path

https://github.com/astral-sh/uv/actions/runs/15796024085/job/44528137607?pr=14185

I suspect a locking issue?? though it could be load on the machine.

---

_Comment by @zanieb on 2025-06-21 13:36_

And here's the tracing log

https://gist.github.com/zanieb/edd017cddbc793c7d681b9dad49e3c0b

in which we spend 80s building the nearly empty package with Poetry

```
2.280060s  DEBUG uv_build_frontend Calling `poetry.core.masonry.api.build_wheel("V:\\uv-tmp\\.tmpiZzVXZ\\cache\\builds-v0\\.tmpvd1K8I", {}, None)` uv_build_frontend::run_python_script script="build_wheel", version_id="poetry-editable @ file:///V:/uv/scripts/packages/poetry_editable"
85.864381s DEBUG uv_distribution::source Finished building: poetry-editable @ file:///V:/uv/scripts/packages/poetry_editable
85.865997s DEBUG uv_fs Released lock at `V:\uv-tmp\.tmpiZzVXZ\cache\sdists-v9\path\23ac0958fa58eed4\.lock`
85.866236s TRACE uv_fs Checking lock for `V:\uv-tmp\.tmpiZzVXZ\cache\sdists-v9\path\23ac0958fa58eed4\0PTMleMNzPEn5RTgS7N4H\poetry_editable-0.1.0-py3-none-any.lock` at `V:\uv-tmp\.tmpiZzVXZ\cache\sdists-v9\path\23ac0958fa58eed4\0PTMleMNzPEn5RTgS7N4H\poetry_editable-0.1.0-py3-none-any.lock`
85.866302s DEBUG uv_fs Acquired lock for `V:\uv-tmp\.tmpiZzVXZ\cache\sdists-v9\path\23ac0958fa58eed4\0PTMleMNzPEn5RTgS7N4H\poetry_editable-0.1.0-py3-none-any.lock`
85.868945s DEBUG uv_fs Released lock at `V:\uv-tmp\.tmpiZzVXZ\cache\sdists-v9\path\23ac0958fa58eed4\0PTMleMNzPEn5RTgS7N4H\poetry_editable-0.1.0-py3-none-any.lock`
```

---

_Referenced in [astral-sh/uv#14188](../../astral-sh/uv/pulls/14188.md) on 2025-06-21 13:41_

---

_Referenced in [astral-sh/uv#14185](../../astral-sh/uv/pulls/14185.md) on 2025-06-21 13:41_

---

_Comment by @jtfmumm on 2025-06-21 13:51_

> Separately, I'm confused that [#13954](https://github.com/astral-sh/uv/pull/13954) could significantly regress `uv pip` performance. We're going to need to look into that. Can we tell where all that time is going?

Ok, you're right. I tried reverting just the upgrade changes and these tests were still taking ~80s.

---

_Comment by @jtfmumm on 2025-06-21 14:09_

It looks like the interaction is really with #14126. I've updated [the comment above](https://github.com/astral-sh/uv/issues/14158#issuecomment-2993461346).

---

_Comment by @zanieb on 2025-06-21 14:25_

For comparison, here's a log when it's quick https://gist.github.com/zanieb/444d6f02de61df5acee1900410d24e88

Still on the Depot runner.

---

_Referenced in [astral-sh/uv#14284](../../astral-sh/uv/pulls/14284.md) on 2025-06-26 16:54_

---

_Referenced in [astral-sh/uv#14285](../../astral-sh/uv/pulls/14285.md) on 2025-06-26 16:54_

---

_Comment by @zanieb on 2025-07-18 13:16_

I believe this was resolved by Flit and the the timeout change.

---

_Closed by @zanieb on 2025-07-18 13:16_

---
