---
number: 3468
title: Offline mode is not automatic fallback when offline
type: issue
state: closed
author: minusf
labels:
  - question
assignees: []
created_at: 2024-05-08T18:50:27Z
updated_at: 2024-05-09T01:37:37Z
url: https://github.com/astral-sh/uv/issues/3468
synced_at: 2026-01-10T01:23:28Z
---

# Offline mode is not automatic fallback when offline

---

_Issue opened by @minusf on 2024-05-08 18:50_

I am trying to understand the reason why `--offline` is not implied when network connections fail:

```
VIRTUAL_ENV=venv uv pip install -r requirements.txt
error: Could not connect, are you offline?
  Caused by: error sending request for url (https://pypi.org/simple/django-timezone-field/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
```

Is there one?  The great appeal of `devpi` was that no matter if I was online or offline `pip install` just worked. It makes the CI/CD pipeline somewhat more convoluted if a flag needs to be added to explicitly request offline mode.

If the reason is to fail loudly and early and make some noise so a human has a look, it would be really nice to have a "fallback to offline if can't connect anywhere" command line switch please.

---

_Renamed from "Offline not automatic failback" to "Offline mode is not automatic fallback when offline" by @minusf on 2024-05-08 18:50_

---

_Comment by @zanieb on 2024-05-08 18:55_

I'm a little confused by what you expect to happen here. Can you expand on what you think uv's behavior should be? With devpi, there's a package index for tools to query — how do you think uv should work without a package index available?

---

_Label `question` added by @zanieb on 2024-05-08 18:55_

---

_Comment by @minusf on 2024-05-08 21:50_

I am sorry, I didn't include the rest of the story: the same command with `--offline` succeeds as all these packages were already in the cache.

Obviously if the dependency graph cannot be successfully resolved and the packages installed only using the cache, the operation should fail.

---

_Comment by @zanieb on 2024-05-08 21:58_

Are all of your requirements pinned? I don't think we should attempt a network request unless it is needed, i.e. I would expect us to use the cache without the `--offline` flag here.

cc @charliermarsh

---

_Comment by @minusf on 2024-05-08 22:02_

This is a hobby project, so not all packages are pinned :D  Actually only django is and the rest "gets the latest" strategy.

---

_Comment by @zanieb on 2024-05-08 22:54_

If you just pin them i.e. use separate `requirements.in` and `requirements.txt` files with `uv pip compile` you shouldn't run into this issue. I'm not sure we should infer using offline for unpinned requirements, it's interesting to consider though.

---

_Comment by @minusf on 2024-05-08 23:22_

Thank you, I will check out `pip-compile` "flow", never used it before.

What are the risks of trying going offline mode here? I have a tunnel vision of my use case here (i.e. the `venv` was built completely successfully at least once and is cached), but it seems harmless to me to try and see if all packages can be installed from the local cache. What is there to lose? If I add the `--offline` parameter it just all works already...

---

_Comment by @zanieb on 2024-05-08 23:43_

I think the main problem is that package resolution should not change whether you have internet or not — that'd be a surprising behavior without opt-in.

cc @charliermarsh 

---

_Comment by @samypr100 on 2024-05-08 23:45_

> that'd be a surprising behavior without opt-in.

👆 100% agree

---

_Comment by @minusf on 2024-05-08 23:48_

So if I understand correctly, if all packages and subpackages were pinned, there could be no surprising behaviour and `--offline` could be a safe fallback?

---

_Comment by @zanieb on 2024-05-08 23:53_

That's my understanding, I don't think we'd need to change anything we'd just use the cache because we know what versions you want — there's no resolution to do.

---

_Comment by @minusf on 2024-05-09 00:06_

I tested the all pinned workflow and `--offline` indeed becomes redundant. 

This is great and maybe worth documenting. This means that for my uses cases I don't need `devpi` anymore and that's just toast. `pip` is also a marked man, all it does now is `pip list --outdated`.

I think this issue can be closed.

---

_Closed by @zanieb on 2024-05-09 01:37_

---

_Referenced in [astral-sh/uv#4728](../../astral-sh/uv/issues/4728.md) on 2024-07-02 12:49_

---
