```yaml
number: 6418
title: Add incremental resolution benchmark to codspeed
type: issue
state: open
author: konstin
labels:
  - help wanted
  - performance
assignees: []
created_at: 2024-08-22T09:45:52Z
updated_at: 2026-01-09T10:48:19Z
url: https://github.com/astral-sh/uv/issues/6418
synced_at: 2026-01-12T15:59:04Z
```

# Add incremental resolution benchmark to codspeed

---

_@konstin_

There are three scenarios for the uv resolver:
* Fresh resolution: We have a set of requirements, but no lockfile. We need to figure a resolution without any prior context. These are continuously tested by `resolve_warm_airflow`, `resolve_warm_jupyter` and `resolve_warm_jupyter_universal`.
* Incremental resolution: We have a lockfile that gives us preferences for versions (and help prefetching), but it's incompatible or incomplete with the requirements. This happens in a lot of cases, e.g. when `pyproject.toml` changed due to a version upgrade, with `uv lock --upgrade-package` or when using `uv run --with`. Our goal is to resolve versions that preserve most of the existing lockfile.
* We run `uv lock` and the lockfile matches: We only need to check against `package.metadata.requires-dist`.


While we have good coverage for the first case and the last case is fast beyond the need for optimization (<20ms even for transformers with all extras), we don't have good coverage for the middle case. We should add a scenario to codspeed, with a universal resolution where the requirements changes so that they don't  match `uv.lock` anymore.


---

_Label `help wanted` added by @konstin on 2024-08-22 09:45_

---

_Label `performance` added by @konstin on 2024-08-22 09:45_

---

_Comment by @LoukasPap on 2026-01-08 21:59_

@konstin Is this issue still open to work on? Probably, I am going to need to ask some questions on the way to understand it fully right.

---

_Comment by @konstin on 2026-01-09 10:48_

Yes this is still open, though I can only give limited guidance to implementing it.

---
