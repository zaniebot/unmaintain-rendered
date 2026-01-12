```yaml
number: 6654
title: "Improve cached package \"index of\" page for pinned versions which are already downloaded and cached."
type: issue
state: closed
author: albertferras-vrf
labels: []
assignees: []
created_at: 2024-08-26T20:28:40Z
updated_at: 2024-10-23T17:32:19Z
url: https://github.com/astral-sh/uv/issues/6654
synced_at: 2026-01-12T15:59:06Z
```

# Improve cached package "index of" page for pinned versions which are already downloaded and cached.

---

_@albertferras-vrf_

**Summary**
`uv` seems to cache the package's index-of page (example: https://pypi.org/simple/fastapi ) following cache-control headers, while `pip` doesn't seem to ever cache it.
If the repository returns a `no-cache` header (a private repository) or after cache expired (pypi's max-age=600s), then `uv` always has to redownload the package index-of page when installing this package.

For pinned versions which have already been downloaded locally, `uv` could resolve the packages without redownloading the 'index of' page.

This affects two cases:
- Private repositories with no-cache header
- Public pypi repository, after cache has expired


**Longer explanation**
Consider the following page: https://pypi.org/simple/fastapi/ I will name it "package page" because I don't know the proper name for it. This page is used by `uv` and `pip` to find which are the available versions of a given package (example: fastapi).

When requesting this 'package page', PyPi returns a `max-age: 600` (10min) which makes any `pip install` command not need to redownload the package page until 10min later when having to resolve, and `uv` would seem to do the same.

In my company we have a private pip repository where the package page response is set to `Cache-Control: no-cache`, I believe because we want to be able to install recently published versions right after they've been published. Anyway, wether or not no-cache is right or wrong doesn't matter here, let's just focus on the case where custom --index-url returns `no-cache` responses.


I have made a simple project that reproduces this here: https://github.com/albertferras-vrf/uv_cache_test/tree/main
- Start local pypi proxy with two base paths that can be used as `--index-url`: one with `cache-control: max-age=600` and one with `cache-control: no-cache`.
- Run `test_uv.sh` to run `uv pip install` on both cases, with and without cached downloads and package-pages, and with pinned versions.

The `test_uv.sh` script does this:
- Create new venv
- Clear all uv cache
- For every index url (pypi public, local proxy with max-age=600s, local proxy with no-cache):
  - Install packages normally (pinned versions)
  - Uninstall packages
  - Install packages again. Here the downloaded files should be already been cached. The 'package page' too, but that does not happen with `no-cache` header.

Logs:
```
LOCAL PYPI PROXY (keep cache-control header)
Resolved 9 packages in 1.53s
Prepared 8 packages in 135ms
Installed 8 packages in 5ms

Uninstalled 8 packages in 16ms

Reinstall
Resolved 9 packages in 5ms
Installed 8 packages in 9ms

Uninstalled 8 packages in 19ms

--------------------------------------------
LOCAL PYPI PROXY (cache-control: no-cache)
Resolved 9 packages in 1.50s  
Prepared 8 packages in 151ms
Installed 8 packages in 7ms

Uninstalled 8 packages in 29ms

Reinstall
Resolved 9 packages in 1.47s
Installed 8 packages in 5ms

Uninstalled 8 packages in 18ms
```

After reinstalling on each case, you can notice that `uv` honors the `cache-control` header:
- Reinstall when package page cached with `max-age=600s`: 5ms
- Reinstall when package page cached with `no-cache`: 1.5s

When running the same experiment with `pip` (`test_pip.sh`) then one can see that `pip` always redownloads the package page, even if it has already downloaded it seconds ago, in both cases with cache-control:max-age=600 and no-cache.

I don't know if this distinct behaviour is intentional in `uv`.

My question/suggestion is the following:
**Would it make sense to skip the 'package page' download if we already have a cached version of the package we're trying to download?**
This change could improve the speed to install packages from pinned versions in 2 cases: when public pypi package page was previously downloaded (but expired after `max-age=600`) OR when private repository has `no-cache` header. We would be saving some http requests.

In my case (private repository with `no-cache`), reinstalling packages I already have downloaded takes ~13s just to do the Resolve (my project has ~140 pinned packages)

---

_Comment by @albertferras-vrf on 2024-08-27 06:06_

I have updated the description with a `summary` section, easier to read

---

_Comment by @albertferras-vrf on 2024-08-28 21:23_

discussed in discord. Best solution would be to:
- enable cache on private repository's "indexof" page for all packages
- when we know there's a new recently released package, bypass the cache with `--refresh-package <name>` if doing `uv pip install` or `--refresh` when generating lock files

---

_Closed by @albertferras-vrf on 2024-08-28 21:24_

---
