---
number: 5731
title: uv cache size limit
type: issue
state: open
author: FrancescElies
labels:
  - enhancement
  - help wanted
  - cache
  - needs-design
assignees: []
created_at: 2024-08-02T16:53:19Z
updated_at: 2025-12-21T23:47:10Z
url: https://github.com/astral-sh/uv/issues/5731
synced_at: 2026-01-10T01:23:51Z
---

# uv cache size limit

---

_Issue opened by @FrancescElies on 2024-08-02 16:53_

Unless I missed it, I think currently there is no way to limit the cache size.

I think this would be valuable in ci self-hosted runners where the cache can grow pretty big due to the need to support many python versions packages downloads, we have seen it growing up to ~40GB.

E.g. sccache has a [`SCCACHE_CACHE_SIZE`](https://github.com/mozilla/sccache/blob/0e6656243102299844cf344136b5e76f42a5a8ab/docs/Configuration.md#disk-local) that sets a maximum size of the cache.

At the moment, if it grows to big you run `uv cache clean` and start over the game again, but with a cache size limit one wouldn't have to worry.

What do you think?

PS.: thanks for making pip installs great again

---

_Comment by @charliermarsh on 2024-08-02 16:57_

Interesting idea. Would the cache be cleaned up on some sort of LRU basis?

---

_Label `cache` added by @charliermarsh on 2024-08-02 16:57_

---

_Comment by @FrancescElies on 2024-08-02 18:03_

That would be my first naive choice. 

---

_Comment by @samypr100 on 2024-08-03 00:41_

Interesting, I've been using `UV_CACHE_DIR` instead to be relative to the github workspace and cleaning the workspace at the end of a run using a `ACTIONS_RUNNER_HOOK_JOB_COMPLETED` (for self hosted runners).

---

_Comment by @zanieb on 2024-08-03 21:42_

@samypr100 do you think we should document that [here](https://docs.astral.sh/uv/guides/integration/github/#caching)?

---

_Comment by @samypr100 on 2024-08-03 23:48_

I think it makes sense if we want to include details for self hosted github runners caching. I can take a initial stab

---

_Referenced in [astral-sh/uv#5757](../../astral-sh/uv/pulls/5757.md) on 2024-08-04 04:53_

---

_Comment by @FrancescElies on 2024-08-06 16:45_

> Interesting, I've been using `UV_CACHE_DIR` instead to be relative to the github workspace and cleaning the workspace at the end of a run using a `ACTIONS_RUNNER_HOOK_JOB_COMPLETED` (for self hosted runners).

Doesn't cleaning the cache after each job defeat the purpose of using the cache? wouldn't in that case be the same as running `uv pip install` with  `--no-cache` flag?

Currently we took the approach of randomly calling `uv cache clean` 10% of the time, 10% of the jobs are slower but the cache won't grow wild.

---

_Comment by @samypr100 on 2024-08-06 16:50_

I think what's non obvious in my statement is that you could arguably store the cache in between using actions cache instead of relying on the runner to hold it for you and slowly filling up the disk. To clarify, I don't do that right now (saving it in actions/cache), so it's just an idea.

---

_Comment by @charlesnicholson on 2024-12-29 20:33_

Related: https://github.com/astral-sh/uv/issues/9790 - we have robots that automatically upgrade our packages every week, so our caches constantly fill up with older packages versions that are never used.

Being able to say "The cache has a maximum size of 4GB" and an LRU scheme that evicted cold entries would work very well for us.

---

_Label `enhancement` added by @zanieb on 2025-01-07 19:13_

---

_Label `help wanted` added by @zanieb on 2025-01-07 19:13_

---

_Label `needs-design` added by @zanieb on 2025-01-07 19:13_

---

_Referenced in [astral-sh/uv#11432](../../astral-sh/uv/issues/11432.md) on 2025-02-12 04:47_

---

_Referenced in [astral-sh/uv#8908](../../astral-sh/uv/issues/8908.md) on 2025-05-13 14:14_

---

_Referenced in [astral-sh/uv#13431](../../astral-sh/uv/issues/13431.md) on 2025-05-13 14:15_

---

_Comment by @axellpadilla on 2025-06-20 15:06_

I think aside from cache prune, a configurable soft approximate limit would be the best, even a default one, to avoid unlimited growing cache problems by default

---

_Comment by @charlesnicholson on 2025-06-20 15:21_

Between this issue and https://github.com/astral-sh/uv/issues/3518, we simply disable the uv cache fully on our dedicated test-runner PCs, and remind ourselves to regularly obliterate the uv caches on our workstations. We have a dependabot-like system that upgrades us to the latest versions of our third-party package dependencies every week, which is the worst case for uv's caching system.

---

_Comment by @SZubarev on 2025-07-31 13:13_

Just pruned 60GB of cache. It would be great to be able to setup size limit

---

_Comment by @jkobject on 2025-08-12 15:32_

would be great also to select where the cache gets stored

---

_Comment by @my1e5 on 2025-08-12 15:44_

> would be great also to select where the cache gets stored

You can. See https://docs.astral.sh/uv/concepts/cache/#cache-directory

---

_Comment by @liamjones-pw on 2025-09-12 03:22_

I have no idea how, but uv cache on my machine was taking up ~300GB. I only found when I started getting warnings about running out of disk. `uv cache clean` is slowly clawing it back.

~~I'm not even actively using uv day to day so, this feels very, very odd.~~ MCP servers running in background are likely the cause.



---

_Comment by @charlesnicholson on 2025-09-12 12:31_

The fact that this has been the cache behavior since the launch of uv makes me wonder if I'm just using it incorrectly. Every 1-2 weeks I manually clear my uv cache and see ~1GB get deleted, and it grows unbounded if I don't.

Is this what everyone does? Do people have tricks like scheduled prunes etc? It's not the worst price to pay for all of the benefits, but it's extremely surprising that the product is built on a grow-only unlimited caching system.

---

_Comment by @charliermarsh on 2025-09-12 15:04_

Hmm, I don't think it's particularly unusual to have an unbounded cache. Most tools I'm familiar with that cache to disk operate this way (I'm sure there are exceptions, or maybe it's different in other areas of software), though I of course agree that it would be nice to have a GC for it (like Cargo added recently) or an ability to bound it.

---

_Comment by @axellpadilla on 2025-09-12 15:05_

@charlesnicholson  It looks like it is, i think uv should at least run `uv cache prune` automatically after a certain space used ending a new sync for example, this clean a little at least and should be safe.

---

_Comment by @charlesnicholson on 2025-09-12 15:41_

We have a script in our project root that runs the git-sandboxed uv for us, so I'm just going to throw in a bit that looks at the cache size and prunes it if it's over 800MB or whatever. That way any time we run uv through our wrapper, it'll prune on size.

@charliermarsh maybe consider a stopgap where uv informs the user every now and then that their cache is > 2GB or something? It's clearly surprising at least a few users, even if it's working as intended :)

---

_Comment by @charlesnicholson on 2025-09-14 01:14_

I filed https://github.com/astral-sh/uv/issues/15821 requesting that uv provides `uv cache size` to make it easier to tool external cache pruning.

---

_Referenced in [astral-sh/uv#16761](../../astral-sh/uv/issues/16761.md) on 2025-11-17 15:29_

---

_Comment by @lucharo on 2025-12-16 12:41_

Hi, I'd be keen to see this implemented as I've ended up with a 99GB cache which crashed my laptop, auto clean up based on the last time a cached object was used/downloaded would be handy. I can of course set  cron jobs locally but `uv` handling this itself would be extra useful! 

---

_Referenced in [astral-sh/uv#17211](../../astral-sh/uv/pulls/17211.md) on 2025-12-21 22:31_

---

_Comment by @TrevorBurnham on 2025-12-21 23:47_

I've submitted a PR to add autopruning: https://github.com/astral-sh/uv/pull/17211

---
