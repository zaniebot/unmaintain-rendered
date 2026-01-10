```yaml
number: 14230
title: uv run does not remove extraneous packages
type: issue
state: closed
author: stewit
labels:
  - question
assignees: []
created_at: 2025-06-24T07:28:32Z
updated_at: 2026-01-09T13:20:44Z
url: https://github.com/astral-sh/uv/issues/14230
synced_at: 2026-01-10T03:11:34Z
```

# uv run does not remove extraneous packages

---

_Issue opened by @stewit on 2025-06-24 07:28_

### Summary

While `uv run` adds missing dependencies from the lock file, it does not remove extraneous packages that may have been installed manually into the venv. I would expect it to do a full sync (if necessary) so that extraneous packages are removed.

To reproduce
```
uv init
uv add pip
uv run python -c 'import requests'  # correctly throws ModuleNotFoundError

uv run pip install requests
uv run python -c 'import requests'  # no error: manually installed requests is not removed!

# Note: manually sync does a complete sync removing extraneous packages
uv sync
uv run python -c 'import requests'  # correctly throws ModuleNotFoundError
```

Note: The [documentation](https://docs.astral.sh/uv/guides/projects/#running-commands) on `uv run` explicitely states
> uv run guarantees that your command is run in a consistent, locked environment.

I interpret "consistent" to mean that exactly the locked dependencies are installed.


#### Why this is a problem:
A junior developer wants to try a new library. Not well-versed with dependency management tooling he manually installs it into the venv. Succeeding in what he wants to achieve he adds unit tests which are run with `uv run ...`.

Finally pushes his branch without detecting that a dependency is not declared correctly. Unit tests then fail somewhere in CI.



### Platform

Linux

### Version

0.7.14

### Python version

Python 3.13

---

_Label `bug` added by @stewit on 2025-06-24 07:28_

---

_Comment by @konstin on 2025-06-24 08:43_

Consistent in this case means that all your project requirements are fulfilled and there aren't clashes between their requirements. For `uv run`, we intentionally chose an inexact sync by default, so `uv run` doesn't do to many changes before running; We've put the convenience of an inexact sync higher than the potential problems with undeclared dependencies. You can use `uv run --exact` to get an exact sync. For running tests specifically, you could consider `uv run --isolated`.

---

_Label `bug` removed by @konstin on 2025-06-24 08:44_

---

_Label `question` added by @konstin on 2025-06-24 08:44_

---

_Comment by @stewit on 2025-06-24 09:18_

Hm, it is of course nice that explicit parameters like `--exact` for controlling what happens exists, but I certainly would expect this to be the default behaviour.

Maybe at least the documentation for `uv run` should then avoid the word "consistent" and be more explicit in describing what actually happens.

---

_Comment by @konstin on 2025-08-19 15:17_

The idea behind this is that it the packages that are installed are from a single, consistent resolution that is stored in `uv.lock`. We don't consider extra package a problem in that regard, we usually see that people run `uv sync --locked` in CI which catches this problem for them. 

---

_Comment by @zanieb on 2025-08-19 17:18_

I think in a world where we have a top-level `uv test` command I'd imagine it runs with `--exact` by default, but that's just not the right default for the majority of the `uv run` users.

---

_Comment by @stewit on 2025-08-25 07:39_

Yes, I understand that, thank you all. I would still kindly suggest being more precise about the term “consistent” in the linked documentation — but maybe I'm the only one who finds this confusing.

---

_Comment by @a9tavako on 2026-01-08 20:54_

I ran into the exact same issue as the original poster. 

I agree with @stewit; the documentation here (https://docs.astral.sh/uv/concepts/projects/sync/) is very **misleading**. To be precise, the documentation is actually wrong. 

It says 
- "when uv run is used, the project is locked and synced before invoking the requested command."
- "Syncing is "exact" by default, which means it will remove any packages that are not present in the lockfile."

The above two statements together means, `uv run` is an exact sync. The documentation needs to **explicitly** say that `uv run` uses the "--inexact" version of sync. 

As it is currently a user will trust `uv run` to sync, remove a dependency, and then still see it show up in their environment.

---

_Closed by @zanieb on 2026-01-09 13:20_

---
