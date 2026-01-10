```yaml
number: 12676
title: Update documentation about reusing cache across multiple containers
type: pull_request
state: open
author: potiuk
labels: []
assignees: []
base: main
head: update-docs-about-caching
created_at: 2025-04-04T17:12:24Z
updated_at: 2025-10-25T21:11:37Z
url: https://github.com/astral-sh/uv/pull/12676
synced_at: 2026-01-10T06:36:15Z
```

# Update documentation about reusing cache across multiple containers

---

_Pull request opened by @potiuk on 2025-04-04 17:12_

With #12359 I've learned about the power of mounting shared cache when you run multiple containers, I think it's worth mentioning it in the `uv` documentation so that others can discover it more easily

Closes: #12359

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Assigned to @zanieb by @zanieb on 2025-04-04 18:36_

---

_@kaxil reviewed on 2025-04-04 18:51_

---

_Review comment by @kaxil on `docs/concepts/cache.md`:165 on 2025-04-04 18:51_

```suggestion
your CI tests where you want to run `uv sync` as part of your CI tests. In such case you can share
```

---

_@jscheffl reviewed on 2025-04-04 20:22_

---

_Review comment by @jscheffl on `docs/concepts/cache.md`:169 on 2025-04-04 20:22_

```suggestion
In order to do it, the folder where the cache is stored must be mounted to the `~/.cache/uv` directory
```

---

_@zanieb reviewed on 2025-04-16 15:43_

---

_Review comment by @zanieb on `docs/concepts/cache.md`:159 on 2025-04-16 15:43_

Should this go in the Docker integration guide instead? We could keep a stub here, like 

```
## Sharing the cache

uv shares a cache across processes on the same machine by default.

See the [Docker integration guide](...) for details on how to share the cache across containers.
```

---

_Review comment by @zanieb on `docs/concepts/cache.md`:170 on 2025-04-16 15:44_

I'd probably share the Docker invocation for this too.

---

_@zanieb reviewed on 2025-04-16 15:44_

---

_@zanieb reviewed on 2025-04-16 15:44_

---

_Review comment by @zanieb on `docs/concepts/cache.md`:161 on 2025-04-16 15:44_

Please stylize as "uv" not "UV".

---

_@zanieb reviewed on 2025-04-16 15:45_

---

_Review comment by @zanieb on `docs/concepts/cache.md`:167 on 2025-04-16 15:45_

Is the parallel part particularly important here? Isn't it helpful that the cache is shared regardless of concurrency?

---

_@potiuk reviewed on 2025-06-22 17:35_

---

_Review comment by @potiuk on `docs/concepts/cache.md`:159 on 2025-06-22 17:35_

Good idea

---

_@potiuk reviewed on 2025-06-22 17:43_

---

_Review comment by @potiuk on `docs/concepts/cache.md`:170 on 2025-06-22 17:43_

absolutely.l

---

_@potiuk reviewed on 2025-06-22 17:43_

---

_Review comment by @potiuk on `docs/concepts/cache.md`:161 on 2025-06-22 17:43_

my bad

---

_@potiuk reviewed on 2025-06-22 17:46_

---

_Review comment by @potiuk on `docs/concepts/cache.md`:167 on 2025-06-22 17:46_

I think it's not obvious that you can do it for parallel docker containers but yes I think it makes sense to mention that until your machine is run the cache will also be used when running containers sequentially.

I reworded it a bit and in the "docker,md" I split caching strategy into two cases:

1) when you run containers locally or on long running VMs -> better to use cache mount in this case
2) when you run containers on CI (there it makes more sense to use mounted volume when you run parallel or sequential builds 

---

_Comment by @potiuk on 2025-06-22 17:52_

Sorry it took so long, but I hope I can finally contribute to uv :) . Happy to get more comments and apply them

---

_Comment by @potiuk on 2025-06-23 05:38_

I also updated the description to better separate two cases:

* one is cache_mount when you build image
* anotehr is mounting volume from host when you run containers

This is the **real** difference between those two cases - both of them using containers, and both can be used either locally or when running things in CI/ephemeral runners (but the second case only makes sense when you run more than one build/container per job).

---

_Comment by @cgravill on 2025-09-24 14:38_

The pointers in this PR were really useful, thanks!

Not sure it's quite the right place, but I wanted to share that we now do ephemeral GitHub builds self-hosted based on this. We initially had a pypi proxy but it was still slow. @zanieb suggested looking here, then I made a very direct adjustment to our CI runs:

```yaml
container:
  volumes:
    - uv-cache:/github/home/.cache/uv
```

which shares the cache on the host. We've been running it for a while with Ubuntu as the base container and it's worked great. You can be more explicit and pass a volume to the runners but this works straight in the actions.

---

_Comment by @potiuk on 2025-10-16 13:50_

> The pointers in this PR were really useful, thanks!

Maybe worth merging it then :) 

---

_Comment by @potiuk on 2025-10-21 11:58_

Shall we @zanieb :) ?

---

_Comment by @potiuk on 2025-10-25 21:11_

:) ? .. It hangs on my unmerged PRs

---
