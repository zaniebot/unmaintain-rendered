```yaml
number: 15732
title: "`uv` and caching"
type: issue
state: closed
author: stdedos
labels:
  - question
assignees: []
created_at: 2025-09-08T08:52:06Z
updated_at: 2025-11-03T02:52:05Z
url: https://github.com/astral-sh/uv/issues/15732
synced_at: 2026-01-12T16:02:16Z
```

# `uv` and caching

---

_@stdedos_

### Question

I was wondering how to do "correctly" uv and caching.

I was very happy to already find so much documentation already dedicated to that (https://docs.astral.sh/uv/concepts/cache/#caching-in-continuous-integration, https://docs.astral.sh/uv/guides/integration/)
Thank you for that 🙏 

I am thinking:

> With uv, it turns out that it's often faster to omit pre-built wheels from the cache (and instead re-download them from the registry on each run). On the other hand, caching wheels that are built from source tends to be worthwhile, since the wheel building process can be expensive, especially for extension modules.

Do you have any data for that? It is not necessarily a no-faith - but it is always nice to have visible results ✌️ 

> ```yaml
>  # GitLab CI creates a separate mountpoint for the build directory,
>  # so we need to copy instead of using hard links.
>  UV_LINK_MODE: copy
> ```

Isn't this something that uv can detect itself?

If default is _`auto`_ (`cow` on MacOS, `hardling` rest), and _hardlinking_ fails - shouldn't uv be able to fall back automatically selecting the next-best option that works and continue with that mode for the rest of the run?

If `UV_LINK_MODE` is hardcoded to something, then ofc it should fail and NOT fall back to anything.

> ```yaml
> ...
>   cache:
>     - key:
>         files:
>           - uv.lock
> ```

Is it worth to throw away the whole cache, for just a minuscule dependency change?

I get it that it is saner - but I was wondering if someone "compared it" with e.g. heavy dependabot usage.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @stdedos on 2025-09-08 08:52_

---

_Comment by @zanieb on 2025-09-08 13:18_

> Do you have any data for that?

We don't have anything to share here. We benchmarked it initially, but it's not something we're continuously benchmarking to publish data for.

> If default is auto (cow on MacOS, hardling rest), and hardlinking fails - shouldn't uv be able to fall back automatically selecting the next-best option that works and continue with that mode for the rest of the run?

We do fallback to copying on hardlink failures, but it emits a warning. Changing the mode silences the warning.

> Is it worth to throw away the whole cache, for just a minuscule dependency change?

On GitHub, we do

```
          key: uv-${{ runner.os }}-${{ hashFiles('uv.lock') }}
          restore-keys: |
            uv-${{ runner.os }}-${{ hashFiles('uv.lock') }}
            uv-${{ runner.os }}
```

so we'll fallback on inexact matches.

I'm not sure what the GitLab equivalent is — that integration guide is user contributed. Happy to change it if there's something better!

---

_Comment by @stdedos on 2025-09-08 13:24_

The _existing_ Gitlab suggestion that is what it does exactly.

My question is "is it worth it or not" (in cache metrics: speed, network, etc)

---

_Comment by @zanieb on 2025-09-08 13:28_

I'm not quite sure what you're saying there. My point is that the GitLab documentation seems less than ideal.

It seems like https://docs.gitlab.com/ci/caching/#per-cache-fallback-keys should be added

---

_Comment by @stdedos on 2025-09-08 14:22_

Ah, right - so I have missed

> so we'll fallback on inexact matches.

😅

I guess I'll try my hands on fallback-keys 🤝 

Thank you

---

_Closed by @charliermarsh on 2025-11-03 02:52_

---
