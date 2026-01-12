```yaml
number: 10359
title: pkgs.dev.azure.com is not cached?
type: issue
state: closed
author: KalleDK
labels:
  - question
  - cache
assignees: []
created_at: 2025-01-07T12:56:57Z
updated_at: 2025-08-22T10:15:26Z
url: https://github.com/astral-sh/uv/issues/10359
synced_at: 2026-01-12T16:00:11Z
```

# pkgs.dev.azure.com is not cached?

---

_@KalleDK_

For some reason the cache is missed when using azure index.
I realized it when add / remove of packages that should be cache was redownloaded / refreshed everytime.

Two small examples showing the difference ( I don't know if Azure is denying cache and that is the problem )

```sh
mkdir test
cd test
uv init .
uv add typing-extensions
uv remove typing-extensions --offline
uv add typing-extensions --offline
# Now add azure index in pyproject.toml
# [tool.uv]
# index-url = "https://VssSessionToken@pkgs.dev.azure.com/XXXx/YYYYY/_packaging/ZZZZ/pypi/simple/"
uv add typing-extensions
uv remove typing-extensions --offline
uv add typing-extensions --offline
# Last here fails with missing in cache
× No solution found when resolving dependencies:
  ╰─▶ Because typing-extensions was not found in the cache and your project depends on typing-extensions, we can conclude that your project's requirements are unsatisfiable.

      hint: Packages were unavailable because the network was disabled. When the network is disabled, registry packages may only be read from the cache.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

Tried with a clean cache, and the package is in the cache.

An even more peculiar is that the remove is also not working, if you have more than one dependency.
```sh
mkdir test
cd test
uv init .
uv add typing-extensions
uv add tomlkit
uv remove typing-extensions --offline
uv add typing-extensions --offline
# Now add azure index in pyproject.toml
# [tool.uv]
# index-url = "https://VssSessionToken@pkgs.dev.azure.com/XXXx/YYYYY/_packaging/ZZZZ/pypi/simple/"
uv add typing-extensions
uv add tomlkit
uv remove typing-extensions --offline
# Already fails on the remove for some reason
× No solution found when resolving dependencies:
  ╰─▶ Because tomlkit was not found in the cache and your project depends on tomlkit>=0.13.2, we can conclude that your project's requirements are unsatisfiable.

      hint: Packages were unavailable because the network was disabled. When the network is disabled, registry packages may only be read from the cache.
```

---

_Comment by @charliermarsh on 2025-01-07 13:45_

You can run with `-vvv` and we'll give you logs about the caching behavior of the index. My guess is that `pkgs.dev.azure.com`  does not include any caching headers.

---

_Label `question` added by @charliermarsh on 2025-01-07 13:45_

---

_Label `cache` added by @charliermarsh on 2025-01-07 13:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-14 23:58_

---

_Comment by @thomasaarholt on 2025-08-04 20:47_

I can reproduce this. Here are some logs. An odd observation is that the _third_ time I add it, it caches. But if I remove it and try with `--offline`, it fails. 

With this [pyproject.toml](https://gist.github.com/thomasaarholt/b8aeac253d1df959730a1aee3829491b) - and running `uv tool install keyring --with artifacts-keyring==1.0.0rc1` in order to get a newer rust-based artifacts-keyring that doesn't require you installing dotnet. (note that you still won't be able to authenticate to my tracker unless I invite you).

This is what I did, with logs:

1. `uv add polars==1.30` log1 - I can try to provide this, but the over 100k lines broke gist.github.com.
2. `uv remove polars`
3. `uv add polars==1.30` [log2](https://gist.github.com/thomasaarholt/4a238f6c694fd1fff332081b4ea38aad)
4. `uv remove polars`
5. `uv add polars==1.30` [log3](https://gist.github.com/thomasaarholt/1407c789a641bd3009ddbfac8e451cfa)
6. `uv remove polars`
7. `uv add polars==1.30 --offline` [log4](https://gist.github.com/thomasaarholt/b802e31d9f384f594b68771749941538)

I know that pip doesn't cache ADO because ADO uses SAS URLs, which include time-based information in the query string, and pip insists on including the query string when it caches. So because you get a new SAS URL each time, pip will never fulfill it from the cache. 

---

_Comment by @charliermarsh on 2025-08-04 21:44_

You can set your own caching headers for that index if you want: https://docs.astral.sh/uv/concepts/indexes/#customizing-cache-control-headers. The behavior above seems right? In that as of now, they send `no-store`:

```
Cached request https://pkgs.dev.azure.com/thomasaarholt/thomasaarholt-python/_packaging/thomas-python-feed/pypi/simple/polars/ is not storable because its response has a 'no-store' cache-control directive
```

---

_Closed by @charliermarsh on 2025-08-21 18:30_

---

_Comment by @KalleDK on 2025-08-22 10:15_

@charliermarsh thanks - I've missed the possibility to change the headers

---
