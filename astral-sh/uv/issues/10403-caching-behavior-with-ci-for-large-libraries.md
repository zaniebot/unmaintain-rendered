---
number: 10403
title: Caching behavior with --ci for large libraries
type: issue
state: open
author: MattTheCuber
labels:
  - question
  - performance
  - cache
assignees: []
created_at: 2025-01-08T18:25:40Z
updated_at: 2025-10-01T15:21:44Z
url: https://github.com/astral-sh/uv/issues/10403
synced_at: 2026-01-10T01:24:53Z
---

# Caching behavior with --ci for large libraries

---

_Issue opened by @MattTheCuber on 2025-01-08 18:25_

According to [the docs](https://docs.astral.sh/uv/concepts/cache/#caching-in-continuous-integration), `uv cache prune --ci` "removes all pre-built wheels and unzipped source distributions from the cache".

Our project uses a few large libraries which take a long time to download from PyPi. For example, torch and it's dependencies (several nvidia packages) make up 2 GB of files that need to be downloaded (takes ~5 minutes to download and install with uv). Would it be possible to make the `--ci` flag not clear very large downloaded packages? Otherwise, an argument could be added to `uv cache prune` that doesn't clear libraries over a given size?

---

_Comment by @MattTheCuber on 2025-01-08 18:30_

Example:

```bash
$ uv venv
Using CPython 3.9.21 interpreter at: /usr/bin/python
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
$ source .venv/bin/activate
$ uv pip install torch --cache-dir .uv_cache
Resolved 22 packages in 528ms
Prepared 22 packages in 4m 58s
Installed 22 packages in 196ms
...
$ uv cache prune --ci --cache-dir .uv_cache
Pruning cache at: .uv_cache
Removed 13871 files (4.9GiB)
$ deactivate
$ rm -rf .venv
$ uv venv
Using CPython 3.9.21 interpreter at: /usr/bin/python
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
$ source .venv/bin/activate
$ uv pip install torch --cache-dir .uv_cache
Resolved 22 packages in 487ms
Prepared 22 packages in 5m 02s
Installed 22 packages in 202ms
...
```

---

_Label `performance` added by @Gankra on 2025-01-08 19:24_

---

_Label `cache` added by @Gankra on 2025-01-08 19:24_

---

_Comment by @Gankra on 2025-01-08 19:51_

If we do anything here, the CLI flag for a size limit is probably a good idea either way, as an override for any behaviour we pick. 

Out of curiosity, did you measure how long it takes to upload+download everything from your cache (e.g. if you don't use prune --ci)? That kind of information is the crux of whether this will actually improve performance to do this.

---

_Comment by @MattTheCuber on 2025-01-08 20:07_

Yes, I am currently using that method. It cuts off several minutes although it didn't measure the exact time difference.

---

_Comment by @MattTheCuber on 2025-01-09 14:34_

My current fastest solution is to build a custom image that simply pip installs torch that way uv doesn't have to download or install it.

---

_Comment by @fenuks on 2025-04-11 14:00_

I have somewhat related issue. I don't want `uv cache prune --ci` to remove
wheels at all, only stale/outdated ones.

Let's say I have cached wheels for `django 5.1.7`, I update to `5.2`. I would
like to remove old cached data for `v5.1.7`. Currently `uv cache prune --ci`
removes almost everything, and `uv sync` re-downloads every dependency, so
there is little advantage to having it. I get very similar times with `uv sync`
when I have no cache, and when I have cache pruned with `--ci`.

---

_Comment by @MattTheCuber on 2025-04-11 15:40_

@fenuks I think you are looking for just `uv cache prune` (without `--ci`) which only removes unused packages such as old versions (see the docs [here](https://docs.astral.sh/uv/concepts/cache/#clearing-the-cache)). The `--ci` removes all packages that were not built locally and would redownload them on next sync. I find this behavior a little strange as well, but [their docs](https://docs.astral.sh/uv/concepts/cache/#caching-in-continuous-integration) claims it is faster to redownload from PyPi than to save and load a larger cache file to the CI server.

---

_Comment by @fenuks on 2025-04-11 15:58_

Thank you for an answer. I use `uv cache prune` (even if getting dependencies would be faster that doing
so from cache, I think it's better to not expose unnecessary burden on public
infrastructure such as PyPI needlessly), but it doesn't seem to remove unused
versions from cache at all. Documentation is a bit vague, it doesn't state exactly what is
unused entry and how it is determined. It mentions only that `the cache
directory may contain entries created in previous uv versions that are no
longer necessary and can be safely removed`, so I would be grateful if somebody
could confirm that uv tries also to remove unused versions of packages.

I'm using the latest uv version at the moment (0.6.14) with `UV_LINK_MODE=copy`
and `UV_CACHE_DIR=.uv-cache`, after updating package, `uv cache prune` leaves
both versions of it in cache. Perhaps `uv` is reluctant to remove it, because
it cannot be assumed that this cache directory isn't used by many projects?

---

_Comment by @Gankra on 2025-04-11 18:14_

There are several different parts of the cache / ways in which a package can "be" cached. Which subdirs of the cache are you concerned with?

The prune command isn't really current-project-aware. If you don't pass the `--ci` flag its job is mostly to GC "dangling" entries in the cache that nothing *in the cache itself* references anymore. Specifically stuff in the `sdist-*` and `archive-*` dirs.

If you pass the `--ci` flag then we will also delete everything in the `wheels-*` dir, and most things from the `sdist-*` dir.

---

_Comment by @fenuks on 2025-04-12 10:24_

> Which subdirs of the cache are you concerned with?

From what I see, most data is stored in `archive-*`, `sdists-*`, and `wheels-*` dirs.

> The prune command isn't really current-project-aware.

That's what I observed, thank you for confirmation. Having fast cache service in local network, I would like uv to allow me to clean only data of unused dependencies in current project (or perhaps many projects, with option to provide paths to multiple `uv.lock` files, in case I have few separate projects in one repository).

---

_Label `question` added by @Gankra on 2025-04-14 18:21_

---

_Comment by @LSerranoPEReN on 2025-10-01 15:21_

I have a similar problem, as I also use very large machine learning libraries that take a long time to download. However, I don't think that not pruning the heaviest libraries completely solves the problem for CI.

I'm using Gitlab CI and building the CI cache requires zipping all the files to be saved which takes a lot of times when you have tens of thousands of files to save. My understanding of the uv cache is that most pre-built wheels are stored unzipped, so all the files in large libraries will need to be rezipped when building the CI cache.

One solution to both problems could be to have the possibility to configure a directory to keep all the downloaded wheels still zipped and save that particular directory for CI cache.

---
