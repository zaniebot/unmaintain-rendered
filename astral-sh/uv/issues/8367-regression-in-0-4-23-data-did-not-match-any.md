```yaml
number: 8367
title: "Regression in 0.4.23: data did not match any variant of untagged enum CacheInfoWire"
type: issue
state: closed
author: KapJI
labels:
  - cache
assignees: []
created_at: 2024-10-19T16:34:31Z
updated_at: 2024-10-23T20:35:49Z
url: https://github.com/astral-sh/uv/issues/8367
synced_at: 2026-01-12T15:59:24Z
```

# Regression in 0.4.23: data did not match any variant of untagged enum CacheInfoWire

---

_@KapJI_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

My Github Actions CI started failing with this error:
```
error: Failed to build: `homeassistant-stubs @ file:///home/runner/work/homeassistant-stubs/homeassistant-stubs`
  Caused by: Failed to deserialize cache entry
  Caused by: data did not match any variant of untagged enum CacheInfoWire
  ```
  
 I bisected it to 0.4.23 here: https://github.com/KapJI/homeassistant-stubs/pull/496

This is an example workflow, `enable-cache: false`, other settings for `astral-sh/setup-uv` are default, it's using `ubuntu-latest` runner:
https://github.com/KapJI/homeassistant-stubs/actions/runs/11418948703/job/31772992617

All these tools in pre-commit hook are running via `uv run`. In this workflow no caches were materialized but `UV_CACHE_DIR` was set by `setup-uv`.

* I tried to remove all stored caches from the repo jsut in case with no result.
* Also I tried using different `cache-local-path`, also with no result.


---

_Comment by @KapJI on 2024-10-19 17:20_

Bisected to https://github.com/astral-sh/uv/pull/8259

* https://github.com/astral-sh/uv/commit/6a81d302bbc03bcb5bad4e6da44f4a0472f102b4 passes: https://github.com/KapJI/homeassistant-stubs/actions/runs/11419383821/job/31773917481
* https://github.com/astral-sh/uv/commit/7730861bc5c6fb58ad402a396479f2cba6946110 fails: https://github.com/KapJI/homeassistant-stubs/actions/runs/11419315350/job/31773778565

---

_Assigned to @charliermarsh by @zanieb on 2024-10-19 18:01_

---

_Label `bug` added by @zanieb on 2024-10-19 18:01_

---

_Label `cache` added by @zanieb on 2024-10-19 18:01_

---

_Comment by @zanieb on 2024-10-19 18:01_

Thanks for the bisect!

---

_Comment by @charliermarsh on 2024-10-19 19:25_

Are you somehow sharing a cache across multiple uv versions?

---

_Comment by @charliermarsh on 2024-10-19 19:25_

Like somehow writing to the cache from `0.4.23` and then reading from it on `0.4.22`?

---

_Comment by @charliermarsh on 2024-10-19 19:31_

I think that's very likely what's happening. Your `uv.lock` has:
```toml
[[package]]
name = "uv"
version = "0.4.15"
```

So the outer `uv run` for pre-commit is using 0.4.23 or whatever you've pinned, but the `uv run` within your pre-commit jobs is using the `uv` version locked in the project (0.4.15). It's not safe to write cache data with a later version and then read it with an earlier version unfortunately.

---

_Label `bug` removed by @charliermarsh on 2024-10-19 19:32_

---

_Comment by @charliermarsh on 2024-10-19 19:33_

I think you need to either use 0.4.15 in your project (to match home-assistant), or use different cache directories between your top-level `uv run` (or even `--no-cache`?) and the jobs in your `pre-commit`.

---

_Comment by @charliermarsh on 2024-10-19 19:34_

Alternatively we could decide to change our policy (written up [here](https://docs.astral.sh/uv/concepts/cache/#cache-versioning)) so that versions like 0.4.15 are expected to be able to read cache data from 0.4.23.

---

_Comment by @charliermarsh on 2024-10-19 19:35_

(I'm not opposed to changing the policy, it just means more frequent cache version bumps.)

---

_Comment by @KapJI on 2024-10-19 19:43_

Wow, that's a good catch! I don't know how it got into `uv.lock` though. Is there any reasonable case when this should be allowed?

Edit: I found it's a dependency of `homeassistant` itself.

---

_Comment by @KapJI on 2024-10-19 20:15_

Basically the issue here is that some downstream dependency may add different version of `uv` to its dependencies, like `homeassistant` in this case. This means it's not safe to use `uv` within `uv run` as outer and inner versions may mismatch.

I wonder if `uv` can detect such case and show some warning as this scenario may probably lead to many other issues.

---

_Comment by @AlexWaygood on 2024-10-19 23:32_

I actually just ran into this while working on typeshed.

Typeshed has `uv` listed as a test dependency in its `requirements-tests.txt` file (because we use it in our test scripts, but we don't want to mandate that users have to use `uv` for local development if they don't want to). But I also have `uv` installed globally for my local development, and the `uv` version I have installed globally is a much newer one than the `uv` pin in typeshed's `requirements-tests.txt` file that was installed into my typeshed-development virtual environment locally. Which led to `uv` crashing with this error message when one version of `uv` tried to read a cache entry written by another version of `uv`.

---

_Comment by @charliermarsh on 2024-10-20 16:07_

We should probably just change our cache versioning policy to make this less likely.

---

_Closed by @charliermarsh on 2024-10-20 16:48_

---

_Closed by @charliermarsh on 2024-10-20 16:48_

---

_Comment by @mishamsk on 2024-10-23 20:12_

@charliermarsh I may be reading the policy wrong, but it kinda implies that if two version have incompatible cache formats, then they will use different subfolders within the root cache folder to avoid errors. Which is what I'd expect. I am not concerned about extra gigabytes of duplicate data sitting around as long as it helps uv "just work". 

But now it seems that either I am reading the text wrong, or. the reality is different. I have pretty much the same case as @AlexWaygood - the most recent version of uv installed and used globally (e.g. for quick scripts), but locked older versions in many a project. The latter randomly fail to uv sync.

I imagine adding forward compatibility may be really hard. Wonder if you could just treat the UV_CACHE as root folder, but really place cache under versioned subfolders, maybe versioned to a specific uv version to keep it really simple and stupid.

Whatever is the case, the amount of reduced testing/ci/frustration time that uv already brought definitely worth these minor inconveniences. So no complains really! Absolutely lovely stuff you are all making!

---

_Comment by @charliermarsh on 2024-10-23 20:15_

Thank you @mishamsk ❤️ 

The cache policy going forward is what you described in your first paragraph: if two versions have incompatible cache formats, they use different subfolders within the cache. So they shouldn't error, but they may duplicate data.

But I _just_ changed the policy (see the linked PR here), so it's possible that you're seeing breakages when sharing the cache between older uv versions.


---

_Comment by @mishamsk on 2024-10-23 20:35_

> Thank you @mishamsk ❤️
> 
> The cache policy going forward is what you described in your first paragraph: if two versions have incompatible cache formats, they use different subfolders within the cache. So they shouldn't error, but they may duplicate data.
> 
> But I _just_ changed the policy (see the linked PR here), so it's possible that you're seeing breakages when sharing the cache between older uv versions.

ah, ok, that makes sense. you are just moving so fast, this PR is 4 days old, I haven't realized that it did include the change you mentioned in a comment ;-)

I'll go ahead and update uv version on one of my projects then, was just debating if there is value to do that. Now I know there is. Thanks!

---
