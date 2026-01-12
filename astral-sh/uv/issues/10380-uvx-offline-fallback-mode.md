```yaml
number: 10380
title: uvx offline fallback mode?
type: issue
state: open
author: alexjbuck
labels:
  - enhancement
  - cache
assignees: []
created_at: 2025-01-07T23:35:13Z
updated_at: 2025-12-11T13:43:32Z
url: https://github.com/astral-sh/uv/issues/10380
synced_at: 2026-01-12T16:00:12Z
```

# uvx offline fallback mode?

---

_@alexjbuck_

I do a lot of work in offline environments with an alternate package index. That index goes down sometimes (much more often than the public pypi repo would).

A lot of routine uv/uvx tasks will check against network locations before running unless the `--offline` flag is passed. When the index goes offline I can continue working with the `--offline` flag and `uv/uvx` will use its local cache. This is great!

The issue is when I start having scripts or other tooling that invoke `uvx`. When the index is up, I don't want to run in `--offline` mode, because I want `uvx` to work normally. So I can write those scripts to not use `--offline`. However, when the index goes down, now I need to be running in `--offline` mode because a network failure doesn't automatically fallback to the local cache. That means I have to go edit all the callsites to use `uvx --offline`  for that time period.  

Example:

`uvx ruff check` fails when the package repo goes down and must be changed to `uvx --offline ruff check` to use the locally cached version of ruff. You have to have already run `uvx ruff ...` at least once with access to a package index to get it loaded into the cache for this to work but that's not something `uv` can fix 😆.

There are plenty of workarounds (such as maintaining a parallel set of scripts or accepting an "--offline" flag into the scripts) but this does seem like it could be behavior that `uv/uvx` could subsume.

I think this comes down to the fact that there's only two modes right now:
- **Online**: Default, `uvx` always tries to reach out to the package registry. e.g. `uv ruff format`
- **Offline**: `--offline` passed, `uvx` will never reach out to the package registry. e.g. `uv --offline ruff format`

I'm finding in my circumstances it would be great to have a third mode:
- **Online with Fallback**: Act like the default however, if a network request fails, fall back to `--offline` type behavior for that dependency and check if it exists in the local cache. If it doesn't, then fail just like how `uvx --offline <package not in cache>` would fail. e.g. `uv --offline-fallback ruff format`.

While my circumstances are probably fairly niche, the general idea is to let `uvx` have a "best effort" in case it can't reach the package index and not just fail when there's a valid local cached version.

I'm not sure if there are any pitfalls you could end up in with what could be a mixed online/offline dependency resolution. You can think of some weird cases where a specific dependency has a `git` source defined and that's accessible but transitive dependencies aren't online  because the package index is offline so they fallback to previously local cached versions. My first though is so long as the versions can be resolved satisfactorily it wouldn't be any different.

---

_Renamed from "uvx partial offline mode" to "uvx offline fallback mode" by @alexjbuck on 2025-01-07 23:38_

---

_Renamed from "uvx offline fallback mode" to "uvx offline fallback mode?" by @alexjbuck on 2025-01-07 23:38_

---

_Comment by @zanieb on 2025-01-08 00:09_

This seems reasonable to me. I think the problem might be that we can't respect the registry's cache options? I'm not sure though, @charliermarsh would know better.

---

_Label `enhancement` added by @zanieb on 2025-01-08 00:09_

---

_Label `cache` added by @zanieb on 2025-01-08 00:09_

---

_Comment by @ksamuel on 2025-02-20 00:26_

Doesn't `UV_OFFLINE` help with this use case ?

---

_Comment by @taranlu-houzz on 2025-05-30 23:56_

@zanieb This would be really helpful for my workflow as well. I have a service that uses `uv` in a subprocess in order to allow running different versions of an internal package that is pulled from an internal pypi server. When the pypi server is inaccessible (not a frequent occurrence, but it has happened a couple of times now), it causes jobs that call the service to fail because `uv` must make a successful connection. Having a fallback would be extremely handy.

On a related note: when looking into this further, I also noticed some oddities with the way the `--offline` flag and cached packages seem to work, so I create another issue here: https://github.com/astral-sh/uv/issues/13747

---

_Comment by @ajfriend on 2025-07-21 03:34_

+1. An "online, but best effort when offline" option would also be helpful to me. 

---

_Comment by @konstin on 2025-08-25 11:11_

I could see a `--never-refresh` [^1] option that skips over doing HTTP if there's something in the cache. A trickier but even more effective implementation would prioritize versions in the resolver that are cached, falling back to uncached versions.

[^1]: `--no-refresh` would give the default behavior.

---

_Comment by @J3ronimo on 2025-12-11 13:43_

+1 

Together with #16108 , in short we'd need "prefer cache" and "prefer latest" modes added, while currently we only have "strict cache" (--offline) and "strict latest" (default, fails if offline).

---
